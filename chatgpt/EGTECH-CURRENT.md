# egTEC AutoGen CURRENT

기준일: 2026-09-12
상태: CURRENT

## 현재 적용 지시문
- 기사 작성 Canonical: `chatgpt/K-ARTICLE-RULES.md`
- 기사 작성 Frozen Snapshot: `chatgpt/K-ARTICLE-RULES-v2.2.md`
- 대표이미지 Canonical: `chatgpt/K-ARTICLE-IMAGE-RULES.md`
- 대표이미지 Frozen Snapshot: `chatgpt/K-ARTICLE-IMAGE-RULES-v2.3.md`
- 생성형 AI 고지: `chatgpt/K-ARTICLE-RULES-AI-DISCLOSURE.md`

## 현재 버전
- 기사 작성 규칙: **v2.2 FROZEN**
- 대표이미지 규칙: **v2.3 FROZEN**

## 핵심 변경사항
대표이미지 작업이 무한 재생성·복잡한 저장 우회로 이어지지 않도록 다음을 고정한다.

1. 이미지 생성 기본 1회, 최대 2회
2. 차별화는 권고사항이며 강제 재생성 Gate가 아님
3. 기술 원리를 모두 도식화하지 않아도 기사와 연결된 실제 산업현장 대표사진이면 허용
4. 정상 이미지 확보 후 저장 실패를 이유로 재생성 금지
5. 저장 재시도 최대 1회
6. GitHub Actions, trigger, Base64 chunk, 임시 upload 폴더 방식 금지
7. `create_blob → create_tree → create_commit → update_ref → fetch_file` 직접 저장·검증을 기본 경로로 사용
8. 성공 또는 실패 상태를 명확하게 보고한 뒤 종료

## 관리 원칙
- `K-ARTICLE-RULES.md`와 `K-ARTICLE-IMAGE-RULES.md`는 항상 최신 Canonical을 유지한다.
- 새 버전 확정 시 버전 번호가 포함된 Frozen Snapshot을 별도로 보존한다.
- 과거 Frozen Snapshot은 수정하지 않는다.
- 이 파일 `EGTECH-CURRENT.md`는 최신 버전 포인터만 갱신한다.
