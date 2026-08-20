# SIS 공식 3종 그룹 Registry — 2026-08-21 v1

- 상태: **IMPLEMENTATION_CANDIDATE**
- 목적: SIS 1st / 2nd / last를 하나의 공식 그룹으로 고정하고 MLG·STC14/G240 공통계층의 비파괴 적용관계를 기록한다.

## 1. 공식 그룹

| 단계 | 역할 | 보호 기준본 | 개선 후보 | GitHub canonical |
|---|---|---|---|---|
| SIS 1st | 전체scan | `SIS_FULL_SCAN_PROMPT_v1.1-SELF-CONTAINED-3MARKET-SCANNER-ERRC-FROZEN.md` | `SIS_1ST_GROUP_LINK_v1.1.1-MLG-SCANNER-ERRC-CANDIDATE.md` | `wskimgit/k-article/sis/prompts/` |
| SIS 2nd | 종목 분석 | `SIS_INSTRUCTION_v2.2.5.md` | `SIS_INSTRUCTION_v2.2.6-MLG-SCANNER-ERRC-CANDIDATE.md` | `wskimgit/stock-letter/chatgpt/` |
| SIS last | 개별 평단가 확인 | `SIS_ANALYSIS_PROMPT_v5.17-MARKET-TIMELINE-ERRC-FROZEN.md` | `SIS_LAST_ACTIVE_COST_PROMPT_v5.17.1-MLG-SCANNER-ERRC-CANDIDATE.md` | `wskimgit/k-article/sis/prompts/` |

세 지시문은 하나의 그룹이나 역할은 독립적이며 서로를 폐기·대체하지 않는다.

## 2. 공통 교차계층

- MLG 기준: `SIS_MARKET_LEADING_GATE_v1.3-FULL-SCAN-LINK-ERRC-FROZEN.md`
- Scanner 계약: `SIS_SCANNER_DIRECT_CALL_CONTRACT_v1.1-3MARKET-FRESHNESS-ERRC-FROZEN.md`
- Scanner: STC14 v1.7.0 / G240 v1.4.0
- 공통 Overlay: `SIS_3GROUP_COMMON_OVERLAY_v1.0-MLG-SCANNER-ERRC-CANDIDATE.md`

MLG와 Scanner의 기존 알고리즘·임계값·상태 정의는 변경하지 않는다.

## 3. 단계별 불변 역할

### SIS 1st

- `trade_list.php enabled=true` KR/US/JP 전체 Universe scan
- Scanner 후보에 종속되지 않음
- MLG 및 Scanner 이미 정식 반영
- 이번 개선에서 분석 기능 변경 없음

### SIS 2nd

- `GENERAL FIND + CYCLE TRACK + LAB TRACK`
- 독립 Track → CROSS VALIDATION
- CYCLE 6종목 및 M0~S3 유지
- LAB SUPPORTING_ONLY 유지
- BUY READY/Wiki 규칙 유지
- MLG는 위험 제한 Overlay
- Scanner는 PRICE/RR 이후 ACT/BUY READY 전 기술 보조확인

### SIS last

- 개별 활성평단/Active Zone 중심
- KR KIS/SIS Source Lock, US Large Buyer, JP Active Money 규칙 유지
- MLG 의미 유지
- Scanner는 R/R 이후 MA5 전 기술 보조확인
- Scanner로 활성평단/최우선 매입가 재계산 금지

## 4. 충돌 우선순위

1. 사용자의 가장 최근 명시적 3종 그룹 지시
2. 본 3종 그룹 Registry
3. 3-Group Common Overlay
4. 각 단계의 보호 기준본 — 해당 단계 고유 계산/Track/Source/금지규칙
5. 공통 MLG — 위험 제한 범위
6. 공통 Scanner — 기술 보조 범위
7. 과거 FROZEN / Candidate / Wiki 보조 Prompt

단, 4~6은 역할범위가 다르므로 본 Registry가 보호 기준본의 도메인 로직을 삭제·변경하는 근거가 될 수 없다.

## 5. 역사 Registry 충돌 처리

과거 `SIS_1ST_FULL_SCAN_FROZEN_REGISTRY_20260821_v2.md`에는 `SIS Last 개별 평단가 확인`을 폐기 대상으로 적은 문구가 존재한다.

이 과거 Registry 파일 자체는 **수정하지 않는다.**

다만 2026-08-21의 최신 명시 지시에 의해 그 폐기문구는 **현재 3종 그룹 운영정책에는 적용되지 않는 역사적 기록**으로만 본다.

같은 이유로 SIS 2nd나 SIS last를 1st에 흡수·폐기하는 해석을 금지한다.

## 6. 타 지시문 혼동 금지

다음은 공식 3종 그룹 구성원이 아니다.

- `chatgpt/INSTRUCTION_v4.7.2.md` 일반 종목 누적분석
- Volume Cycle 단독 Prompt
- LAB 실험엔진 Prompt
- 과거 SIS Analysis v5.x Candidate
- Wiki 자동갱신 Prompt
- Scanner 개발 Prompt

이들은 자료·보조규칙으로 참조할 수 있으나 1st/2nd/last의 정체성을 대체하지 않는다.

## 7. FROZEN 승격 규칙

각 개선 후보는 다음을 모두 통과한 뒤에만 현재 FROZEN으로 승격한다.

- 보호 기준본 원문 무변경
- 단계별 핵심 회귀 PASS
- MLG 위험 제한 방향성 PASS
- Scanner 비결정성 PASS
- GitHub 해당 저장소 보관 PASS
- 이전 FROZEN HISTORY 보존
- Registry 최신본 갱신

GitHub 저장/병합이 완료되지 않은 후보를 `현재 FROZEN 운영본`이라고 표현하지 않는다.
