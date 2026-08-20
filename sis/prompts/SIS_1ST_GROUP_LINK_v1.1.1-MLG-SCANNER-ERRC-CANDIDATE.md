# SIS 1st(전체scan) Group-Link v1.1.1 — MLG / Scanner / ERRC

- 기준일: 2026-08-21
- 상태: **IMPLEMENTATION_CANDIDATE**
- 보호 기준본: `SIS_FULL_SCAN_PROMPT_v1.1-SELF-CONTAINED-3MARKET-SCANNER-ERRC-FROZEN.md`
- 공통 Overlay: `SIS_3GROUP_COMMON_OVERLAY_v1.0-MLG-SCANNER-ERRC-CANDIDATE.md`
- 성격: **NO-LOGIC-CHANGE WRAPPER**

## 1. 원문 보존

이 파일은 SIS 1st v1.1의 본문을 수정·대체·요약하지 않는다.

v1.1에는 이미 다음이 정식 반영되어 있다.

- MLG: `SIS_MARKET_LEADING_GATE_v1.3-FULL-SCAN-LINK-ERRC-FROZEN.md`
- Scanner 계약: `SIS_SCANNER_DIRECT_CALL_CONTRACT_v1.1-3MARKET-FRESHNESS-ERRC-FROZEN.md`
- STC14 v1.7.0
- G240 v1.4.0
- KR/US/JP 독립 freshness
- Scanner 비종속 전체 Universe scan

따라서 분석 로직의 추가·삭제·순서변경은 **0건**이다.

## 2. 유지되는 판단순서

`MLG → 시장 시계열 → 시장/섹터 → SIS → 활성평단/지지 → R/R → STC14/G240 → 최우선 매입가/행동판정 → MA5`

- MLG는 위험 제한기다.
- Scanner는 `DERIVED_TECHNICAL_EVIDENCE`다.
- Scanner 미검출은 부정 신호가 아니다.
- Scanner 후보만으로 추천을 확정하지 않는다.
- MLG-RISK_ON만으로 매입판정을 상향하지 않는다.

## 3. 이번 개선의 유일한 추가사항

SIS 1st를 다음 두 지시문과 함께 **공식 3종 그룹**의 첫 단계로 등록한다.

1. SIS 2nd 종목 분석
2. SIS last 개별 평단가 확인

이는 기능 통합이나 지시문 폐기를 의미하지 않는다. 세 지시문은 역할이 다른 독립 단계다.

## 4. 금지

- 보호 기준본 v1.1 본문 덮어쓰기
- 2nd 또는 last의 규칙을 1st에 흡수하여 대체
- 다른 SIS/Wiki/LAB Prompt를 1st의 대체본으로 지정
- 과거 Registry의 폐기문구를 이유로 2nd/last를 폐기

## 5. 승격 조건

Group Registry와 3-Group Overlay의 ERRC PASS 및 GitHub 보관 확인 후에만 본 Wrapper를 FROZEN 운영 링크로 승격한다.
