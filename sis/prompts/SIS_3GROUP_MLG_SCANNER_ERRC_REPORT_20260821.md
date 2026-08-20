# SIS 공식 3종 그룹 — MLG / STC14 / G240 ERRC 회귀검증 보고서

- 기준일: 2026-08-21
- 검증대상: 1st / 2nd / last 공통 Overlay 및 단계별 Wrapper
- 상태: **SPEC REGRESSION PASS / MERGE PENDING**

## 1. ERRC

### Eliminate

- 1st가 2nd/last를 폐기한다는 과거 Registry 해석 제거
- Scanner 후보 = 전체 Universe라는 오해 제거
- Scanner 미검출 = 부정신호라는 오해 제거
- MLG-RISK_ON = 매입신호라는 오해 제거
- LAB/Scanner/MLG가 BUY READY를 단독 생성한다는 오해 제거
- 일반 Stock Wiki Prompt를 SIS 2nd로 대체하는 혼동 제거

### Reduce

- 동일 MLG 알고리즘을 3개 지시문에 복사·변형하지 않고 공통 FROZEN Gate를 참조
- 동일 Scanner 알고리즘을 각 지시문에서 재정의하지 않고 공통 Direct-Call Contract를 참조
- 기존 FROZEN 본문 변경을 최소화하고 Overlay/Wrapper 방식으로 연결

### Raise

- 1st / 2nd / last 역할 경계 명확성
- MLG 위험 제한 방향성
- Scanner 비결정성
- 3시장 freshness 독립성
- GitHub 보존·역사버전 보호
- Source Lock·활성평단·Track 구조 회귀안전성

### Create

- `SIS_3GROUP_COMMON_OVERLAY_v1.0-MLG-SCANNER-ERRC-CANDIDATE.md`
- 1st 무변경 Group-Link
- 2nd MLG/Scanner Overlay 후보
- last Scanner Overlay 후보
- 공식 3종 그룹 Registry

## 2. 회귀검증

| 검증항목 | 결과 | 비고 |
|---|---|---|
| 1st 보호 기준본 수정 없음 | PASS | 기존 v1.1은 그대로 유지 |
| 1st MLG v1.3 유지 | PASS | 알고리즘 변경 없음 |
| 1st Scanner v1.1 / STC14 v1.7.0 / G240 v1.4.0 유지 | PASS | 의미·순서 변경 없음 |
| 1st 전체 Universe Scanner 비종속 | PASS | 후보만으로 전체scan 대체 금지 유지 |
| 2nd GENERAL/CYCLE/LAB 3-Track 유지 | PASS | Track 삭제·통합 없음 |
| 2nd CYCLE 6종목 유지 | PASS | KR 3 + US 3 그대로 |
| 2nd M0/M1/M2/W/S1/S2/S3 유지 | PASS | 행동표준 변경 없음 |
| 2nd LAB SUPPORTING_ONLY | PASS | 단독 BUY READY 금지 유지 |
| 2nd BUY READY 기존 조건 유지 | PASS | MLG는 제한만 추가 |
| 2nd Wiki Source of Truth 유지 | PASS | 상세→대표 순서 유지 |
| last KIS/SIS Source Lock 유지 | PASS | Scanner가 덮어쓰기 금지 |
| last KR 활성평단 계산 유지 | PASS | 계산식 재정의 없음 |
| last US Large Buyer 원칙 유지 | PASS | 13F 원가 오용 금지 유지 |
| last JP Active Money 원칙 유지 | PASS | 변경 없음 |
| last R/R 및 최우선 매입가 유지 | PASS | Scanner는 산출 후 보조확인 |
| last MA5 실행타이밍 유지 | PASS | Scanner 뒤에 MA5 구조 유지 |
| MLG = 위험 제한기 | PASS | 상향 증폭 금지 |
| MLG-RISK_ON 단독 매입 생성 금지 | PASS | 세 단계 공통 |
| Scanner = DERIVED_TECHNICAL_EVIDENCE | PASS | 세 단계 공통 |
| Scanner 미검출 중립 | PASS | 탈락/매도 근거 금지 |
| Scanner stale/부재 시 분석 지속 | PASS | 자료 제한 표기 |
| Scanner polling 금지 | PASS | Background 완료 비대기 |
| `sis_scanner_refresh.php` 사용 금지 | PASS | 기존 계약 유지 |
| KR/US/JP freshness 독립 | PASS | completed-bar 기준 |
| 과거 FROZEN 파일 수정 금지 | PASS | Wrapper/Overlay만 신규 생성 |
| 3종 그룹 외 Prompt 대체 금지 | PASS | Registry에 명시 |

## 3. 중요한 보존 확인

### SIS 1st

현재 v1.1 자체가 이미 MLG/Scanner를 충분히 포함하므로 **분석규칙 변경은 0건**이다.

### SIS 2nd

`SIS_INSTRUCTION_v2.2.5.md`의 승인된 원문은 재작성하지 않는다. v2.2.6 후보는 additive overlay이며, 원문과 다른 문장으로 재구성한 대체본이 아니다.

따라서 2nd의 보호 기준본 원문 blob이 GitHub canonical로 확인되기 전에는 v2.2.6을 `현재 FROZEN 운영본`으로 선언하지 않는다.

### SIS last

v5.17에는 MLG와 시장 시계열이 이미 존재한다. 추가되는 것은 **R/R 이후 Scanner 보조확인**뿐이며, 활성평단/Source Lock/최우선 매입가 계산은 수정하지 않는다.

## 4. GitHub 보관상태

작업 브랜치:

` sis-3group-mlg-scanner-20260821 `

### wskimgit/k-article

- 공통 Overlay: 생성 완료
- 1st Group-Link: 생성 완료
- last Wrapper: 생성 완료
- 3종 Registry: 생성 완료
- 본 ERRC 보고서: 생성 완료

### wskimgit/stock-letter

- 2nd v2.2.6 Overlay: 생성 완료
- 3종 Pointer: 생성 완료

기존 main의 FROZEN 파일은 변경하지 않았다.

## 5. 잔여 경고

1. 2nd 보호 기준본 `SIS_INSTRUCTION_v2.2.5.md`의 exact GitHub canonical blob은 현재 검색에서 확인되지 않았다.
2. 따라서 해당 원문을 추정해 새로 만들지 않았다.
3. exact 원문이 확보되기 전까지 2nd 후보는 **원문 보호 Wrapper**로만 취급한다.
4. 본 검증은 지시문 논리 회귀검증이며 Scanner PHP 자체의 새 Alpha 변경 검증이 아니다.

## 6. 최종 판정

**SPEC REGRESSION: PASS**

- 기존 지시문 훼손: 없음
- 기존 개념 오염/혼합: 없음
- MLG 역할: 위험 제한으로 고정
- Scanner 역할: 기술 보조로 고정
- 1st/2nd/last 그룹 경계: 보존

단, GitHub main 병합 전이므로 새 후보를 현재 운영 FROZEN으로 승격했다고 표현하지 않는다.
