# K-ARTICLE 대표이미지 생성·저장 규칙 v2.6 — FROZEN

기준일: 2026-09-30
상태: FROZEN / K-ARTICLE-RULES v2.4에 대한 필수 이미지 규칙

## 1. 목적
대표이미지는 기사 주제와 직접 연결되는 사실적인 산업현장 대표사진 1장을 빠르게 생성하고, GitHub 전송 한도 때문에 JPEG가 잘리거나 깨지는 문제 없이 안정적으로 저장하는 것을 목표로 한다.

핵심 원칙:
**이미지 확정 → GITHUB-SAFE JPEG 1개 생성 → 로컬 완전성 확인 → GitHub 직접 저장 → 원격 완전성 확인 1회 → 즉시 종료**

## 2. 기본 처리 방식 — DIRECT ONE-SHOT
기본 경로는 다음으로 고정한다.

`이미지 생성 → 필요 시 1회 재생성 → AI 고지 후처리 → GITHUB-SAFE JPEG 변환 → 로컬 검증 → 최고 vN/SHA 1회 확인 → create_blob → create_tree → create_commit → update_ref → fetch_file 1회 → 종료`

저장 오류 때문에 이미지를 다시 생성하지 않는다.
workflow·trigger·Base64 chunk·임시 upload 폴더 같은 우회 저장도 사용하지 않는다.

## 3. 이미지 제작 기준
1. 사실적인 반도체 FAB·Subfab·클린룸·Utility·장비·정비·검사·물류·연구환경 사진형을 기본으로 한다.
2. 기사 핵심 장면 하나만 표현한다.
3. 기본 화면비는 16:9다.
4. 기사 제목, 큰 글자, 차트, 표, 화살표, 설명 패널, 뉴스페이지, 포스터, 인포그래픽을 넣지 않는다.
5. 실제 기업 로고·브랜드명·제품명·읽을 수 있는 UI 문구를 넣지 않는다.
6. 인터넷 사진을 복제하지 않는다.
7. `생성형 AI 제작 이미지` 문구는 생성 후 우측 하단에 작은 후처리 텍스트로 1회 적용한다.

## 4. 생성 횟수 — 최대 2회
- 기본 생성: 1회
- 기사와 전혀 무관하거나 심한 왜곡·포스터형·큰 글자·기업 로고가 있는 경우에만 추가 1회
- 한 기사당 최대 2회

2회 모두 부적합하면:
`기사 저장 성공 / 이미지 생성 실패 / PARTIAL`
로 종료한다.

저장 실패는 이미지 재생성 사유가 아니다.

## 5. GITHUB-SAFE IMAGE PROFILE — 필수
GitHub에 저장하는 최종 게시용 이미지는 아래 프로파일을 적용한다.

### 1차 프로파일
- 형식: JPEG
- 해상도: **1200×675**
- 색상: RGB
- 메타데이터: 제거
- Chroma subsampling: 4:2:0
- JPEG Quality: **약 35~40**
- 최종 파일 목표 크기: **12 KB 이하**

### 2차 압축 — 1회만
1차 변환 결과가 12 KB를 초과할 때만 같은 이미지에 대해 **압축 조정 1회**를 허용한다.
- 이미지 재생성 금지
- 별도 시험 파일 생성 금지
- 같은 최종 작업파일을 다시 인코딩
- JPEG Quality를 약 30 전후로 낮춘다.

### 크기 축소 fallback
2차 압축 후에도 12 KB를 초과하면:
- 해상도를 **960×540**으로 한 번 축소
- JPEG Quality 약 30 전후
- 12 KB 이하를 목표로 저장

여기까지 수행한 뒤에도 안전 크기를 만들 수 없으면:
`이미지 저장 준비 실패 / PARTIAL`
로 종료한다.

**절대로 품질값을 여러 단계로 반복 탐색하지 않는다.**

## 6. 최종 작업파일 — 반드시 1개
정상 이미지가 선택된 뒤 작업용 최종 파일은 딱 하나만 유지한다.

`<기사 stem>.vN.jpg`

금지:
- `test*.jpg`
- `q*.jpg`
- `fix_q*.jpg`
- 품질 비교용 다중 파일
- Base64 조각 파일

압축 조정은 같은 최종 작업파일을 덮어쓰는 방식으로만 수행한다.

## 7. 로컬 저장 전 검증 — 반드시 수행
GitHub로 보내기 전에 최종 JPEG에 대해 다음을 한 번 확인한다.

1. 파일 크기 ≤ 12 KB 목표 충족
2. JPEG 시작 시그니처 = `FF D8`
3. JPEG 종료 시그니처 = `FF D9`
4. 실제 JPEG decode 성공
5. 해상도 = 1200×675, fallback 사용 시 960×540
6. AI 고지 문구가 잘리지 않고 식별 가능
7. 최종 파일은 하나뿐임

이 검증을 통과하지 않은 바이너리는 GitHub에 저장하지 않는다.

## 8. 버전 및 동일 이미지 판정
기존 이미지는 덮어쓰지 않는다.

- 최초: `.v1.jpg`
- 수정: `.v2.jpg`
- 이후: `.v3.jpg` …
- 무버전 이미지는 레거시 `v0`

저장 전 동일 기사 최고 `vN`과 blob SHA를 1회만 확인한다.

최종 JPEG의 로컬 Git blob SHA를 계산한다.
Git 표준:
`SHA1("blob <length>\0" + data)`

- 로컬 SHA = 현재 최고 버전 SHA → 새 버전 만들지 않고 기존 최고 버전 재사용 후 COMPLETE
- 다름 → `N+1`로 저장

## 9. GitHub 저장 — 직선 경로
저장소:
- repository: `wskimgit/k-article`
- branch: `main`
- 위치: repository root

정상 경로:
1. 현재 main HEAD/base tree 확인 — 1회
2. 동일 기사 최고 vN/blob SHA 확인 — 1회
3. `create_blob(base64)`
4. 현재 base tree를 유지한 `create_tree`
5. 현재 HEAD를 parent로 `create_commit`
6. `update_ref(main, force=false)`
7. 저장 파일 `fetch_file(..., base64)` — 1회

이미 확보된 HEAD/tree 정보는 다시 조회하지 않는다.

## 10. 동시 변경 충돌 — 1회만
non-fast-forward 충돌인 경우에만:
1. 최신 HEAD/base tree 재조회 1회
2. 기존 blob SHA 재사용
3. create_tree → create_commit → update_ref

create_blob 재호출 금지.
이미지 재생성·재압축 금지.
두 번째 ref update도 실패하면 PARTIAL로 종료한다.

## 11. 원격 저장 후 완전성 검증 — 1회만
최종 `fetch_file(..., base64)`에서 다음을 확인한다.

1. 경로가 정확함
2. 원격 blob SHA = 저장한 blob SHA
3. Base64 전체 decode 성공
4. 원격 바이트 수 = 로컬 바이트 수
5. 시작 시그니처 = `FF D8`
6. 종료 시그니처 = `FF D9`
7. JPEG 구조에서 해상도가 1200×675 또는 fallback 960×540
8. 가능하면 실제 decode 성공

**SOI(`FF D8`)만 확인하고 PASS하지 않는다.**
EOI(`FF D9`)와 바이트 길이까지 일치해야 COMPLETE다.

검증 성공 즉시 종료한다. 추가 재조회하지 않는다.

## 12. 절대 금지
- GitHub Actions workflow 신규 생성
- trigger 파일 신규 생성
- `.jpg.b64` 최종 저장
- Base64 chunk 저장
- `_upload_*` 폴더 생성
- placeholder 생성
- main force update
- base tree 없는 create_tree
- 저장 실패 때문에 이미지 재생성
- 성공 후 추가 검증
- 실패 후 새로운 우회 경로 탐색
- 동일 SHA 이미지의 N+1 중복 저장
- 12 KB를 맞추기 위한 무한 품질 탐색

## 13. HARD STOP
다음 중 하나면 즉시 종료한다.
- 이미지 생성 2회 소진
- GITHUB-SAFE 변환 한도 소진
- 동일 SHA 확인
- GitHub 저장 및 원격 완전성 검증 성공
- 저장 재시도 1회 소진

## 14. egtech.php 표시 규칙
1. TXT와 같은 논리 stem의 이미지 후보를 찾는다.
2. 가장 높은 `vN`을 표시한다.
3. 버전 이미지가 없을 때만 레거시 `v0`를 사용한다.
4. 같은 버전은 `jpg → jpeg → png → webp → gif` 순으로 우선한다.
5. 필요 시 `mtime-size` query로 브라우저 캐시를 방지한다.

## 15. 한줄 운영규칙
**대표이미지는 고품질 원본을 그대로 GitHub에 밀어 넣지 않고, 12 KB 이하의 GITHUB-SAFE JPEG로 변환한 뒤 시작·종료 시그니처와 바이트 길이를 확인하여 한 번 저장하고 한 번 검증한 뒤 끝낸다.**

**상태: FROZEN v2.6**
**기준일: 2026-09-30**
