# K-ARTICLE 대표이미지 생성·저장 규칙 v2.5 — FROZEN

기준일: 2026-09-12
상태: FROZEN / K-ARTICLE-RULES v2.4에 대한 필수 이미지 규칙

## 1. 목적
대표이미지는 기사 주제와 직접 연결되는 사실적인 산업현장 대표사진 1장을 빠르게 생성하고 저장하는 것을 목표로 한다.

이 버전은 이미지 생성·GitHub 저장 과정이 길어지거나 같은 확인을 반복하는 문제를 제거한다.

핵심 원칙:
**이미지 확정 → 최종 JPG 1개 → GitHub 직접 저장 1회 → 원격 확인 1회 → 즉시 종료**

## 2. 복원할 기본 처리 방식 — DIRECT ONE-SHOT
초기 정상 처리처럼 이미지 한 장을 하나의 완성 바이너리로 만든 뒤 GitHub에 직접 한 번 저장한다.

다음 흐름을 기본 경로로 고정한다.

`이미지 생성 → 필요 시 1회 재생성 → 1200×675 JPG 1개 확정 → 최고 vN/SHA 확인 → 새 이미지이면 create_blob → create_tree → create_commit → update_ref → fetch_file 1회 → 종료`

저장 과정 중 새로운 이미지를 다시 만들거나, 별도 workflow·trigger·Base64 chunk·임시 upload 폴더를 만드는 우회 처리는 하지 않는다.

## 3. 이미지 제작 기준
1. 사실적인 반도체 FAB·Subfab·클린룸·Utility·장비·정비·검사·물류·연구환경 사진형을 기본으로 한다.
2. 기사 핵심 장면 하나만 표현한다.
3. 16:9 비율로 생성하고 최종 JPG는 정확히 1200×675로 정규화한다.
4. 기사 제목, 큰 글자, 차트, 표, 화살표, 설명 패널, 뉴스페이지, 포스터, 인포그래픽을 넣지 않는다.
5. 실제 기업 로고·브랜드명·제품명·읽을 수 있는 UI 문구를 넣지 않는다.
6. 인터넷 사진을 복제하지 않는다.
7. `생성형 AI 제작 이미지` 고지가 필요하면 이미지 생성 후 우측 하단에 작은 후처리 텍스트로 1회 적용한다.

## 4. 생성 횟수 — 최대 2회
- 기본 생성: 1회
- 기사와 전혀 무관하거나 심한 왜곡·포스터형·큰 글자·기업 로고가 있는 경우에만 추가 1회
- 최대 2회 후에는 더 생성하지 않는다.

2회 모두 부적합하면:
`기사 저장 성공 / 이미지 생성 실패 / PARTIAL`
로 즉시 종료한다.

저장 실패는 이미지 재생성 사유가 아니다.

## 5. 최종 작업파일 — 반드시 1개
정상 이미지가 선택되면 작업용 최종 파일을 딱 하나만 만든다.

`<기사 stem>.vN.jpg`

규칙:
- 1200×675 JPEG
- 추가 품질 시험용 `test*.jpg`, `q*.jpg`, 임시 변형본을 만들지 않는다.
- 같은 이미지를 여러 JPEG 품질값으로 반복 저장하지 않는다.
- 정규화는 1회만 수행한다.
- AI 고지 후처리도 1회만 수행한다.

## 6. 버전 및 동일 이미지 판정
기존 파일을 덮어쓰지 않는다.

- 최초: `.v1.jpg`
- 수정: `.v2.jpg`
- 이후: `.v3.jpg` …
- 무버전 이미지는 레거시 `v0`로 취급한다.

저장 전 repository root에서 동일 기사 최고 `vN`을 1회만 확인한다.

최종 JPG의 **로컬 Git blob SHA**를 계산한다.
Git blob SHA는 JPEG 바이너리에 대해 Git 표준 `SHA1("blob <length>\0" + data)` 방식으로 계산한다.

- 로컬 Git blob SHA = 현재 최고 버전 GitHub blob SHA → 새 파일을 만들지 않고 기존 최고 버전을 재사용하여 COMPLETE 종료
- 서로 다름 → `N+1`로 저장

SHA 비교를 위해 기존 이미지를 다시 다운로드하거나 디코딩하지 않는다.

## 7. GitHub 저장 — 7-call 직선 경로
저장소:
- repository: `wskimgit/k-article`
- branch: `main`
- 위치: repository root

최종 이미지가 확정된 뒤 GitHub 작업은 정상 경로에서 최대 7회 호출만 허용한다.

1. `main` HEAD와 base tree 확인 — 1회
2. repository tree에서 최고 `vN`과 기존 blob SHA 확인 — 1회
3. 새 이미지일 때만 `create_blob(base64)` — 1회
4. 현재 base tree를 보존하여 새 이미지 경로만 `create_tree` — 1회
5. 현재 HEAD를 parent로 `create_commit` — 1회
6. `update_ref(main, force=false)` — 1회
7. 저장된 `.vN.jpg`를 `fetch_file(..., base64)`로 재조회 — 1회

이미 `HEAD/tree/최고 vN` 정보가 현재 작업에서 확보되어 있으면 다시 조회하지 않고 재사용한다.

## 8. 동시 변경 충돌 — 단 한 번만
`update_ref`가 non-fast-forward 등 동시 변경 때문에 실패한 경우에만 예외적으로 한 번 재시도한다.

재시도 절차:
1. 최신 HEAD/base tree 재조회 1회
2. **기존에 만든 동일 blob SHA를 재사용**하여 create_tree
3. create_commit
4. update_ref(force=false)

`create_blob`을 다시 하지 않는다.
이미지를 다시 생성·정규화하지 않는다.
두 번째 ref update도 실패하면:
`이미지 저장 실패 / PARTIAL`
로 종료한다.

## 9. 반복 조회 금지
다음 반복을 금지한다.
- 같은 branch HEAD 반복 조회
- 같은 tree 반복 조회
- 같은 파일 존재 여부 반복 조회
- 같은 blob SHA 재확인
- 저장 전에 workflow/commit history를 다시 검색
- 이미 알고 있는 GitHub 도구를 다시 discovery
- 동일 이미지에 대한 여러 품질 JPEG 생성
- 검증 성공 후 추가 검증

도구 스키마가 실제로 없는 경우에만 discovery를 **최대 1회** 허용한다.

## 10. 저장 후 검증 — 1회만
최종 `fetch_file(..., base64)` 한 번으로 다음만 확인한다.
- 정확한 `.vN.jpg` 경로
- GitHub blob SHA가 방금 저장한 blob SHA와 일치
- JPEG Base64 조회 성공

이미 로컬에서 1200×675 JPEG, 시작/종료 시그니처, AI 고지를 확정했으므로 GitHub 저장 후 동일 검사를 반복하지 않는다.

검증 성공 즉시 `COMPLETE`로 종료한다.

## 11. 절대 금지
- 이미지 저장을 위해 GitHub Actions workflow 생성
- trigger 파일 생성
- `.jpg.b64` 최종 저장
- Base64 chunk 파일 생성
- `_upload_*` 임시 폴더 생성
- placeholder 생성
- `main` force update
- base tree 없는 create_tree
- 저장 실패를 이유로 이미지 재생성
- 검증 실패 후 무한 재검증
- 동일 SHA 이미지를 N+1 버전으로 중복 저장

기존 repository에 남아 있는 과거 workflow·trigger·chunk는 레거시이며, 새 작업에서는 사용하지 않는다.

## 12. 실행시간보다 중요한 종료 규칙
작업이 오래 걸리는 경우 더 많이 확인하는 것이 아니라 더 빨리 종료한다.

- 생성 한도 도달 → 종료
- 저장 재시도 한도 도달 → 종료
- 동일 SHA 확인 → 기존 이미지 재사용 후 종료
- 저장 검증 성공 → 즉시 종료
- 우회 경로가 필요해 보임 → 우회하지 않고 PARTIAL 종료

## 13. 상태
### COMPLETE
- 정상 이미지 확정
- 새 vN 저장 및 1회 재조회 성공, 또는 동일 SHA 기존 최고 버전 재사용

### PARTIAL
- 기사 TXT는 정상이나 이미지 생성 또는 저장 실패

### ERROR
- 기사와 이미지 모두 정상 처리할 수 없는 구조적 오류

## 14. egtech.php 표시 규칙
1. TXT와 같은 논리 stem의 이미지 후보를 찾는다.
2. 가장 높은 `vN`을 표시한다.
3. 버전 이미지가 없을 때만 무버전 레거시 `v0`를 사용한다.
4. 같은 버전은 `jpg → jpeg → png → webp → gif` 순으로 우선한다.
5. 필요 시 `mtime-size` query로 캐시를 방지한다.

## 15. 한줄 운영규칙
**최종 이미지 한 장이 확보되면 더 조사하거나 시험하지 말고, GitHub에 한 번 직접 저장하고 한 번 확인한 뒤 끝낸다.**

**상태: FROZEN v2.5**
**기준일: 2026-09-12**
