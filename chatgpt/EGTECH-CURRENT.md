# egTEC AutoGen CURRENT

기준일: 2026-09-30
상태: CURRENT

## 현재 적용 지시문
- 기사 작성 Canonical: `chatgpt/K-ARTICLE-RULES.md`
- 기사 작성 Frozen Snapshot: `chatgpt/K-ARTICLE-RULES-v2.4.md`
- 대표이미지 Canonical: `chatgpt/K-ARTICLE-IMAGE-RULES.md`
- 대표이미지 Frozen Snapshot: `chatgpt/K-ARTICLE-IMAGE-RULES-v2.6.md`
- 생성형 AI 고지: `chatgpt/K-ARTICLE-RULES-AI-DISCLOSURE.md`

## 현재 버전
- 기사 작성 규칙: **v2.4 FROZEN**
- 대표이미지 규칙: **v2.6 FROZEN**

## 이번 업그레이드 핵심 — GITHUB-SAFE IMAGE PROFILE
1. GitHub 게시용 이미지는 고품질 원본과 분리된 경량 JPEG로 저장
2. 기본 1200×675 / RGB / 메타데이터 제거 / 4:2:0
3. JPEG Quality 약 35~40, 목표 파일 크기 **12 KB 이하**
4. 12 KB 초과 시 같은 이미지에 대해 압축 조정은 1회만 허용
5. 그래도 초과하면 960×540 fallback 1회
6. 품질값 반복 탐색 및 다중 `q*.jpg` 시험파일 생성 금지
7. GitHub 저장 전 SOI `FF D8` + EOI `FF D9` + 실제 decode + 해상도 확인
8. GitHub 저장 후 원격 바이트 길이 = 로컬 바이트 길이 확인
9. 원격에서도 SOI + EOI + 해상도를 확인해야 COMPLETE
10. SOI만 맞는 잘린 JPEG는 PASS 금지
11. 저장 실패를 이유로 이미지 재생성 금지
12. 성공 후 원격 확인 1회만 수행하고 즉시 종료

## HARD STOP
- 생성 최대 2회
- 안전 JPEG 변환은 기본 1회 + 압축 조정 1회 + 해상도 fallback 1회
- GitHub 저장 충돌 재시도 최대 1회
- 원격 완전성 검증 성공 즉시 종료
- 우회 저장 경로 탐색 금지

## 관리 원칙
- Canonical은 항상 최신 확정본을 유지한다.
- 새 버전은 Frozen Snapshot으로 별도 보존한다.
- 과거 Frozen Snapshot은 수정하지 않는다.
- 업데이트 순서는 `Frozen Snapshot 생성 → Canonical 갱신 → CURRENT 갱신 → 원격 재조회 1회`다.
- 검증 성공 후 추가 재조회하지 않는다.

## 현재 기준 한줄
**egTEC 대표이미지는 12 KB 이하의 GITHUB-SAFE JPEG로 경량화하고, 로컬·원격에서 JPEG 시작과 끝 및 바이트 길이를 확인한 뒤 즉시 종료한다.**
