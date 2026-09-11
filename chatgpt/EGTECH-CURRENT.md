# egTEC AutoGen CURRENT

기준일: 2026-09-12
상태: CURRENT

## 현재 적용 지시문
- 기사 작성 Canonical: `chatgpt/K-ARTICLE-RULES.md`
- 기사 작성 Frozen Snapshot: `chatgpt/K-ARTICLE-RULES-v2.3.md`
- 대표이미지 Canonical: `chatgpt/K-ARTICLE-IMAGE-RULES.md`
- 대표이미지 Frozen Snapshot: `chatgpt/K-ARTICLE-IMAGE-RULES-v2.4.md`
- 생성형 AI 고지: `chatgpt/K-ARTICLE-RULES-AI-DISCLOSURE.md`

## 현재 버전
- 기사 작성 규칙: **v2.3 FROZEN**
- 대표이미지 규칙: **v2.4 FROZEN**

## 이번 업그레이드 핵심
1. 조사 범위 확장은 최대 1회로 제한하고, 적합 주제가 없으면 `HOLD`로 정상 종료
2. 특정 기업·제품·기술의 핵심 주장은 1차 자료를 우선 확인하는 Evidence Gate 적용
3. 게시일과 실제 사건·발표일을 분리하여 오래된 사건의 최신 뉴스 오인 방지
4. 기존 기사 중복 검사를 제목뿐 아니라 핵심 기술·기업·적용사례까지 확대
5. ERRC 실패 수정·재검증도 1회로 제한하여 검증 무한루프 방지
6. 기사 작업 상태를 `COMPLETE / PARTIAL / HOLD / ERROR`로 명확히 구분
7. 새 대표이미지의 SHA가 기존 최고 버전과 같으면 `N+1` 중복 파일을 만들지 않음
8. GitHub 이미지 저장은 현재 `main`의 base tree를 보존한 직접 commit 방식 사용
9. `main` ref는 force update하지 않으며 동시 변경 충돌 시 동일 바이너리로 1회만 재시도
10. 정상 이미지 확보 후 저장 실패를 이유로 이미지 재생성 금지 유지
11. GitHub Actions, trigger, Base64 chunk, 임시 upload 폴더 방식 신규 사용 금지 유지
12. AI 이미지 고지는 생성 모델의 그림문자보다 정확한 후처리 적용을 우선

## 관리 원칙
- `K-ARTICLE-RULES.md`와 `K-ARTICLE-IMAGE-RULES.md`는 항상 최신 Canonical을 유지한다.
- 새 버전 확정 시 버전 번호가 포함된 Frozen Snapshot을 별도로 보존한다.
- 과거 Frozen Snapshot은 수정하지 않는다.
- `EGTECH-CURRENT.md`는 최신 Canonical/Frozen 버전 포인터와 핵심 변경사항만 관리한다.
- 동일 내용이면 불필요한 update commit을 만들지 않는다.
- 향후 업그레이드 시 **Frozen Snapshot 생성 → Canonical 갱신 → CURRENT 갱신 → 원격 재조회 검증** 순서를 따른다.

## 현재 기준 한줄
**egTEC는 최신 해외 근거가 있는 새로운 FAB 운영 주제만 기사화하고, 기사·이미지 생성과 GitHub 저장을 유한 횟수로 끝내며, 동일 이미지 중복 버전과 우회 저장을 만들지 않는다.**
