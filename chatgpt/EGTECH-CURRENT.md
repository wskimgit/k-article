# egTEC AutoGen CURRENT

기준일: 2026-11-01
상태: CURRENT

## 현재 적용 지시문
- 기사 작성 Canonical: `chatgpt/K-ARTICLE-RULES.md`
- 기사 작성 Frozen Snapshot: `chatgpt/K-ARTICLE-RULES-v2.5.md`
- 대표이미지 Canonical: `chatgpt/K-ARTICLE-IMAGE-RULES.md`
- 대표이미지 Frozen Snapshot: `chatgpt/K-ARTICLE-IMAGE-RULES-v2.7.md`
- 생성형 AI 고지: `chatgpt/K-ARTICLE-RULES-AI-DISCLOSURE.md`

## 현재 버전
- 기사 작성 규칙: **v2.5 FROZEN**
- 대표이미지 규칙: **v2.7 FROZEN**

## 이번 업그레이드 핵심 — 저작권 검증 필수
1. 기사 v2.5 §6.1과 이미지 v2.7 §1.1의 Copyright Gate를 필수 선행조건으로 적용한다.
2. 자료를 초안·참조 프롬프트에 이용하기 전과 최종 TXT/JPG 게시 전에 실제 이용 방식의 허용 근거와 조건 준수를 확인한다.
3. 사실 확인 후 독자 서술과 원문·이미지의 복제·인용·번역·변형을 구분해 검증한다. 공개 열람·공식자료·출처 표시·AI 고지는 이용 허락을 대신하지 않는다.
4. PASS만 사용한다. 권리 불명확은 HOLD, 침해 우려·조건 위반은 EXCLUDE로 판정하여 해당 자료·표현·이미지를 제거한다.
5. 제외 후 핵심 근거가 부족하면 기사 게시를 HOLD한다. 기사만 PASS이고 이미지만 실패하면 해당 이미지 없이 기사만 저장하고 PARTIAL로 종료할 수 있다.
6. 자료별 근거·판정·제외 조치를 `chatgpt/copyright-checks/YYMMDD.<식별자>.json`에 기록하여 기사 stem과 연결한다. 기록 저장 없이 COMPLETE를 보고하지 않는다.
7. 무참조 AI 생성 및 기존 이미지 동일 SHA 재사용도 저작권 검증을 면제하지 않는다.
8. 검증은 확인 가능한 이용 근거를 기록하는 절차이며 법적 무침해를 보증한다고 보고하지 않는다.

## 유지되는 이미지 전송 규칙 — GITHUB-SAFE IMAGE PROFILE
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
**egTEC는 저작권 검증 PASS 자료만 독자적으로 기사화하고, PASS한 기사·이미지를 검증표와 함께 게시한다. 침해 우려·권리 불명확 자료는 제거하며, 핵심 근거가 부족하면 HOLD한다. 기존 이미지 전송·완전성·유한 실행 규칙을 유지한다.**
