# SIS last 개별 평단가 확인 v5.17.1 — MLG / STC14 / G240 / ERRC

- 기준일: 2026-08-21
- 상태: **IMPLEMENTATION_CANDIDATE**
- 보호 기준본: `SIS_ANALYSIS_PROMPT_v5.17-MARKET-TIMELINE-ERRC-FROZEN.md`
- 공통 Overlay: `SIS_3GROUP_COMMON_OVERLAY_v1.0-MLG-SCANNER-ERRC-CANDIDATE.md`
- 성격: **BASE v5.17 IMMUTABLE + SCANNER ADDITIVE OVERLAY**

## 1. 목적

v5.17의 활성 평단·Active Zone·Source Lock·R/R·최우선 매입가·MA5 규칙을 그대로 유지하면서 STC14/G240를 최종 기술 보조확인으로 추가한다.

본 파일은 v5.17 본문을 재작성하지 않는다. 명시되지 않은 모든 조항은 v5.17을 그대로 따른다.

## 2. 절대 불변 영역

다음은 변경하지 않는다.

- 한국: GitHub Snapshot 직접조회 → Freshness Gate → Request Bridge → 재확인 → Mirror/Bridge 진단 → 공개자료 보완
- `wskimgit/k-article/sis/{code}.json` Source Lock
- SUFFICIENT/LIMITED/INSUFFICIENT 처리
- 외국인·기관 활성 매집 사이클 계산
- `순매수금액 ÷ 순매수수량`을 실제 매입단가로 보지 않는 규칙
- 미국 Large Buyer Active Cost Basis
- `13F market value ÷ shares`를 원가로 사용하지 않는 규칙
- 일본 Large Buyer / Active Money 방식
- M0/M1/M2/W/S1/S2/S3
- MLG 및 T1~T8
- 주요 지지대
- R/R 기준
- 최우선 매입가
- 추격매수 Gate
- MA5 실행 타이밍
- 자료 제한 시 산출 보류

## 3. MLG

v5.17에 이미 존재하는 MLG 의미와 로직을 그대로 사용한다.

- `MLG-RISK_ON` = 매입신호 아님
- `MLG-RISK_OFF/STRESS` = 위험 제한
- `MLG-NA` = 자료부족, 억지 점수 금지
- MLG와 시장 시계열은 별도 축

공통 의미는 `SIS_MARKET_LEADING_GATE_v1.3-FULL-SCAN-LINK-ERRC-FROZEN.md`와 정합하게 해석하되 v5.17의 원자료·활성평단 계산을 변경하지 않는다.

## 4. STC14 / G240 추가 위치

Scanner는 **R/R 검증이 끝난 뒤, 최우선 매입가/행동 및 MA5 실행 타이밍을 확정하기 전** 기술 보조확인으로만 사용한다.

판단계층:

`MLG → 시장 시계열 → 시장/섹터 → SIS → 활성평단/지지 → R/R → STC14/G240 → 최우선 매입가/행동 → MA5`

### STC14
- CROSS/MATCH = 저점권 반전 보조
- WATCH = 관찰
- EXIT_ZONE = 신규진입 신호 아님

### G240
- CROSS/MATCH = MA5/MA240 장기이평 회복 보조
- BELOW_2W = Cross 대기
- 자체 매입신호 아님

## 5. Scanner 불변 제약

Scanner는 절대 다음을 하지 못한다.

- 활성평단 재계산
- Active Zone 재정의
- KIS/SIS Source Lock 변경
- 현재가/OHLCV/수급 덮어쓰기
- MLG 변경
- SIS 상태 상향
- R/R 상향
- 최우선 매입가 생성 또는 변경
- BUY/SELL 행동 단독 생성

`STC14/G240 미검출 = 중립`

`STC14/G240 stale/부재 = 기술 스캐너: 자료 제한`

두 Scanner가 동시 CROSS/MATCH여도 기존 v5.17 Gate를 통과하지 못하면 매입판정을 상향하지 않는다.

## 6. Scanner Direct Call

분석 시작 직후 필요 시:

- `https://k-bizpost.myds.me/stc14.php`
- `https://k-bizpost.myds.me/g240.php`

를 parameterless ensure 방식으로 호출한다.

- Background 완료를 기다리지 않는다.
- polling하지 않는다.
- 현재 Snapshot으로 분석을 계속한다.
- `sis_scanner_refresh.php`를 사용하지 않는다.
- KR/US/JP freshness를 독립 확인한다.

세부 의미는 `SIS_SCANNER_DIRECT_CALL_CONTRACT_v1.1-3MARKET-FRESHNESS-ERRC-FROZEN.md`를 변경 없이 따른다.

## 7. 출력 추가

기존 v5.17 기본 출력은 유지하고 다음 한 줄을 추가할 수 있다.

`기술 스캐너: STC14 {상태/없음/자료제한} · G240 {상태/없음/자료제한}`

Scanner 때문에 기존 필드를 제거·축약하지 않는다.

## 8. 3종 그룹 위치

본 지시문은 공식 3종 그룹의 마지막 단계다.

`SIS 1st 전체scan → SIS 2nd 종목 분석 → SIS last 개별 평단가 확인`

그러나 1st/2nd의 결과를 자동 확정값으로 승계하지 않고 v5.17의 고유 Source Lock과 계산을 다시 수행한다.

## 9. 승격 조건

- v5.17 원문 무변경
- 활성평단 계산 회귀 PASS
- Source Lock 회귀 PASS
- R/R/MA5 회귀 PASS
- Scanner 비결정성 PASS
- 공통 MLG 방향성 PASS
- GitHub 보관 PASS

후에만 FROZEN으로 승격한다.
