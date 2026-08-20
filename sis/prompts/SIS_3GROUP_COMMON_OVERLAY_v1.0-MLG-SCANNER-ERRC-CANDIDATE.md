# SIS 3-Group Common Overlay v1.0 — MLG / STC14 / G240 / ERRC

- 기준일: 2026-08-21
- 상태: **IMPLEMENTATION_CANDIDATE**
- 성격: **ADDITIVE OVERLAY ONLY / BASE PROMPT IMMUTABLE**
- 목적: SIS 공식 3종 그룹에 MLG와 STC14/G240를 동일한 의미로 연결하되 기존 지시문의 역할·순서·금지규칙·계산법을 변경하지 않는다.

## 1. 공식 3종 그룹 — 이 범위만 적용

1. **SIS 1st(전체scan)**
   - 보호 기준본: `SIS_FULL_SCAN_PROMPT_v1.1-SELF-CONTAINED-3MARKET-SCANNER-ERRC-FROZEN.md`
2. **SIS 2nd 종목 분석**
   - 보호 기준본: `SIS_INSTRUCTION_v2.2.5.md`
3. **SIS last 개별 평단가 확인**
   - 보호 기준본: `SIS_ANALYSIS_PROMPT_v5.17-MARKET-TIMELINE-ERRC-FROZEN.md`

이 세 파일은 하나의 공식 SIS 분석 그룹이지만 서로 대체하지 않는다.

- 1st = 전체 Universe scan / 후보 선별
- 2nd = 선택 종목 상세 분석 / GENERAL + CYCLE + LAB 교차검증
- last = 개별 종목 활성 평단·Active Zone·최우선 매입가 확인

다른 Wiki 지시문, LAB 실험 Prompt, 일반 종목 누적분석 Prompt, 과거 v5.x 후보군, Scanner 개발 Prompt는 이 3종 그룹의 구성원이 아니다.

## 2. 절대 보존 규칙

1. 보호 기준본의 본문을 덮어쓰거나 문장을 재해석하여 바꾸지 않는다.
2. 기존 상태코드, 계산식, Source Lock, Track, Universe, LAB 역할, BUY READY 조건, Wiki 규칙을 삭제·축약·합성하지 않는다.
3. 새 규칙은 **명시된 교차계층만 추가**한다.
4. 기존 규칙과 새 보조계층이 충돌하면:
   - 원자료/계산/도메인 로직은 각 보호 기준본 유지
   - MLG는 신규진입 위험을 **제한하는 방향으로만** 작동
   - Scanner는 기술적 보조근거로만 작동하며 기존 판단을 덮어쓰지 못함
5. 과거 FROZEN 파일은 수정하지 않고 HISTORY로 보존한다.

## 3. 공통 MLG 계약

공통 시장 위험 Gate의 의미와 판정 로직은 다음 FROZEN 기준을 **변경 없이** 사용한다.

`SIS_MARKET_LEADING_GATE_v1.3-FULL-SCAN-LINK-ERRC-FROZEN.md`

핵심 불변조건:

- MLG는 **상향 증폭기가 아니라 위험 제한기**다.
- `MLG-RISK_ON`은 매입신호가 아니다.
- `MLG-RISK_ON`은 SIS·R/R·지지·활성평단·BUY READY 실패를 상향 복구하지 못한다.
- `MLG-RISK_OFF`와 `MLG-STRESS`는 신규진입을 더 보수적으로 제한할 수 있다.
- 자료가 부족하면 `MLG-NA`로 표시하고 억지 점수를 만들지 않는다.
- MLG와 시장 시계열 T1~T8은 별도 축이며 1:1 기계 매핑하지 않는다.

## 4. 공통 Scanner 계약

STC14/G240의 의미·신호·최신성·Direct Call은 다음 FROZEN 기준을 **변경 없이** 사용한다.

`SIS_SCANNER_DIRECT_CALL_CONTRACT_v1.1-3MARKET-FRESHNESS-ERRC-FROZEN.md`

적용 Scanner:

- STC14 v1.7.0
- G240 v1.4.0

Scanner는 항상 `DERIVED_TECHNICAL_EVIDENCE`다.

### STC14
- CROSS/MATCH = 저점권 반전 보조
- WATCH = 관찰
- EXIT_ZONE = 신규진입 신호 아님

### G240
- CROSS/MATCH = MA5/MA240 장기이평 회복 보조
- BELOW_2W = Cross 대기
- 자체 매입신호 아님

### 절대 금지
Scanner는 다음을 덮어쓰지 못한다.

- KIS/SIS Publisher Source Lock
- 가격/OHLCV/수급 원자료
- 활성평단·Active Zone
- MLG
- 시장 시계열 T1~T8
- 시장/섹터 레짐
- SIS 상태
- GENERAL/CYCLE/LAB Track 결과
- R/R
- 최우선 매입가
- BUY READY
- 최종 매입·매도 행동

`Scanner 미검출 = 부정 신호`로 해석하지 않는다.
`Scanner stale/부재 = 기술 스캐너 자료 제한`으로 두고 본 분석은 계속한다.

## 5. 실행 공통규칙

분석 시작 직후 필요 시:

- `https://k-bizpost.myds.me/stc14.php`
- `https://k-bizpost.myds.me/g240.php`

를 Direct Call ensure한다.

단:

1. Background 전체 Scan 완료를 기다리지 않는다.
2. polling하지 않는다.
3. 현재 저장된 Snapshot으로 분석을 계속한다.
4. `sis_scanner_refresh.php`를 사용하지 않는다.
5. 호출 순서와 판단 우선순위를 혼동하지 않는다. Scanner 호출이 먼저 일어나도 **판단상 MLG가 상위**다.
6. KR/US/JP freshness는 각각 최근 완료 거래일 기준으로 독립 검증한다.

## 6. 1st 적용 — 기존 본문 변경 없음

SIS 1st v1.1은 이미 MLG v1.3과 Scanner Direct-Call v1.1, STC14 v1.7.0/G240 v1.4.0을 본문에 완전히 포함한다.

따라서 1st에는 **분석 로직을 추가하거나 재배열하지 않는다.**

기존 판단 순서 그대로:

`MLG → 시장 시계열 → 시장/섹터 → SIS → 활성평단/지지 → R/R → STC14/G240 → 최우선 매입가/행동 → MA5`

본 Overlay는 오직 3종 그룹 소속과 공통계층 연결을 명시한다.

## 7. 2nd 적용 — 3-Track 불변

SIS 2nd v2.2.5의 다음 핵심은 절대 변경하지 않는다.

- `GENERAL FIND + CYCLE TRACK + LAB TRACK`
- 세 Track 독립 보존 후 `CROSS VALIDATION`
- CYCLE 고정 6종목 및 M0/M1/M2/W/S1/S2/S3
- LAB = SUPPORTING_ONLY / READ-ONLY / actual_buy=false
- LAB 단독 BUY READY 금지
- stale/current-session 규칙
- PRICE / Invalidation / Target / R/R
- ACT
- BUY READY 조건
- DETAIL MEMORY → REPRESENTATIVE WIKI SYNC

추가되는 것은 두 가지뿐이다.

### 7-1. MLG 위험 Overlay

기존 `DATA / RISK` 자료확인 후, MARKET/SECTOR 판단에서 MLG를 별도 축으로 표시한다.

- 기존 SIS 시장 `ON/WAIT/OFF`를 MLG로 치환하지 않는다.
- 기존 SIS+LAB 교차판정을 삭제하지 않는다.
- `MLG-RISK_ON`으로 BUY READY를 새로 만들지 않는다.
- `MLG-RISK_OFF/STRESS`는 기존 ACT/BUY READY를 제한하는 방향으로만 작동한다.
- `MLG-NA`이면 기존 분석은 계속하되 신규매입 강도를 보수적으로 제한한다.

### 7-2. Scanner 기술 Overlay

기존 `PRICE / RR`을 계산한 뒤 ACT/BUY READY 확정 전에 STC14/G240를 **보조 확인**한다.

- Scanner는 GENERAL/CYCLE/LAB 후보를 만들거나 삭제하지 못한다.
- 미검출은 중립이다.
- 동시 CROSS/MATCH는 기술적 합치로 표시할 수 있으나 BUY READY를 만들지 못한다.
- stale/부재는 `기술 스캐너: 자료 제한`이다.

2nd 확장 실행흐름:

`DATA / RISK → MLG(위험 Overlay) → MARKET / SECTOR → GENERAL FIND + CYCLE TRACK + LAB TRACK → CROSS VALIDATION → STRUCTURE / PHASE / SIGNAL → SCENARIO → PRICE / RR → STC14/G240 보조확인 → ACT → BUY READY → DETAIL MEMORY → REPRESENTATIVE WIKI SYNC → 한 줄 결론`

이는 v2.2.5의 기존 흐름을 삭제·대체하는 것이 아니라 두 교차확인 지점을 추가한 것이다.

## 8. last 적용 — 활성평단 로직 불변

SIS last v5.17의 다음은 절대 변경하지 않는다.

- KR GitHub/KIS/SIS Snapshot 및 Freshness Gate
- Request Bridge/Mirror 진단
- Source Lock
- 외국인/기관 활성 매집 사이클
- US Large Buyer Active Cost Basis
- JP Large Buyer/Active Money 방식
- SIS 상태
- 주요 지지
- R/R
- 최우선 매입가
- 추격 Gate
- MA5 실행 타이밍

MLG는 v5.17에 이미 존재하는 위험 제한 의미를 그대로 유지한다.

Scanner는 **R/R 검증 후 MA5 실행 타이밍 전** 기술적 보조확인으로만 추가한다.

last 판단계층:

`MLG → 시장 시계열 → 시장/섹터 → SIS → 활성평단/지지 → R/R → STC14/G240 → 최우선 매입가/행동 → MA5`

Scanner는 활성평단·최우선 매입가를 다시 계산하거나 변경하지 않는다.

## 9. 그룹 간 전달 원칙

- 1st 후보는 2nd 상세분석의 입력이 될 수 있다.
- 2nd의 상세판정은 last의 개별 활성평단 확인 대상으로 이어질 수 있다.
- 그러나 각 단계의 결과를 다음 단계에서 **자동 확정값으로 승계하지 않는다.** 다음 단계의 고유 Source/계산/Gate를 다시 확인한다.
- 1st·2nd·last 중 어느 하나도 다른 하나를 폐기하거나 대체하지 않는다.

## 10. 타 지시문 혼동 금지

다음은 이 3종 그룹의 대체본이 아니다.

- 일반 Stock Wiki 누적분석 지시문
- Volume Cycle 단독 지시문
- LAB 실험엔진 지시문
- 과거 SIS Analysis 후보 Prompt
- Scanner 개발/운영 Prompt
- Wiki 자동갱신 규칙

이들의 유용한 자료를 참조할 수는 있으나 공식 1st/2nd/last 역할을 바꾸지 않는다.

## 11. 동결 승격 조건

이 Overlay를 FROZEN으로 승격할 때:

1. 세 보호 기준본의 원문 무변경 확인
2. 1st 회귀 PASS
3. 2nd 3-Track/LAB/CYCLE/BUY READY 회귀 PASS
4. last Source Lock/활성평단/R/R/MA5 회귀 PASS
5. MLG 위험 제한 방향성 PASS
6. Scanner 비결정성 PASS
7. 각 최신 지시문을 해당 GitHub에 보관
8. 이전 FROZEN은 HISTORY로 보존
9. 그룹 Registry 갱신

을 모두 만족해야 한다.
