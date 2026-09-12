# egTEC AutoGen CURRENT

기준일: 2026-09-12
상태: CURRENT

## 현재 적용 지시문
- 기사 작성 Canonical: `chatgpt/K-ARTICLE-RULES.md`
- 기사 작성 Frozen Snapshot: `chatgpt/K-ARTICLE-RULES-v2.4.md`
- 대표이미지 Canonical: `chatgpt/K-ARTICLE-IMAGE-RULES.md`
- 대표이미지 Frozen Snapshot: `chatgpt/K-ARTICLE-IMAGE-RULES-v2.5.md`
- 생성형 AI 고지: `chatgpt/K-ARTICLE-RULES-AI-DISCLOSURE.md`

## 현재 버전
- 기사 작성 규칙: **v2.4 FROZEN**
- 대표이미지 규칙: **v2.5 FROZEN**

## 이번 업그레이드 핵심 — 무한루프 제거 / 초기 직선 처리 복원
1. 이미지 처리 기본 경로를 `DIRECT ONE-SHOT`으로 고정
2. 정상 이미지 확정 후 작업용 최종 JPG는 1개만 유지
3. `test*.jpg`, `q*.jpg` 등 품질 시험용 다중 파일 생성 금지
4. 정규화 1회, AI 고지 후처리 1회만 수행
5. 최고 vN과 기존 GitHub blob SHA는 1회만 확인
6. 최종 JPG의 로컬 Git blob SHA를 계산해 기존 최고 SHA와 직접 비교
7. 동일 SHA이면 새 버전을 만들지 않고 기존 최고 버전 재사용 후 즉시 종료
8. 새 이미지이면 `create_blob → create_tree → create_commit → update_ref → fetch_file` 직선 경로 사용
9. 이미지 확정 후 정상 GitHub 작업은 최대 7-call로 제한
10. 같은 HEAD·tree·파일·blob을 반복 조회하지 않음
11. GitHub 도구 discovery는 실제 필요한 도구가 없을 때만 최대 1회
12. non-fast-forward 충돌만 동일 blob으로 1회 재시도
13. 저장 실패를 이유로 이미지 재생성 금지
14. 저장 성공 후 원격 확인은 1회만 수행하고 즉시 COMPLETE 종료
15. GitHub Actions, trigger, Base64 chunk, `_upload_*`, placeholder 신규 사용 금지
16. 성공 후 추가 확인 금지, 실패 후 새로운 우회 경로 탐색 금지

## HARD STOP
다음 중 하나면 추가 작업 없이 종료한다.
- 이미지 생성 2회 소진
- 동일 SHA 확인
- GitHub 저장 및 원격 재조회 성공
- 저장 재시도 1회 소진
- ERRC 재검증 1회 소진

## 관리 원칙
- Canonical은 항상 최신 확정본을 유지한다.
- 새 버전은 별도 Frozen Snapshot으로 보존한다.
- 과거 Frozen Snapshot은 수정하지 않는다.
- 업데이트 순서는 `Frozen Snapshot 생성 → Canonical 갱신 → CURRENT 갱신 → 원격 재조회 1회`다.
- 검증 성공 후 추가 재조회하지 않는다.

## 현재 기준 한줄
**egTEC는 최종 이미지 한 장이 확보되면 더 시험하지 않고 GitHub에 직선 경로로 한 번 저장하고 한 번 확인한 뒤 반드시 종료한다.**
