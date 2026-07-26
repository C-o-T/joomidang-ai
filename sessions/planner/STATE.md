# planner 상태 파일

## 현재 상태

대기 중 — Monitor 기반 A/B/C안 폐기, push(inbox) 모델 최종 설계안 chief 반환 완료 (2026-07-10)

## 완료한 작업 이력

| 날짜 | 작업 | 핵심 결정 |
|------|------|-----------|
| 2026-05-08 | dataverse v0.1 OKR KR4·KR5·KR7 확정 | KR4: 9개 도메인(SCI/ENG/MED/LAW/FIN/HIS/POL/PHI/GEN) + 판단 기준 / KR5: L1~L5 자동 측정 신호 정의 / KR7: bge-m3 채택 |
| 2026-05-11 | 감시 세션 자동 실행 구조 문제 분석 | hooks agent 타입 experimental + "cannot trigger tool calls" 제약으로 자동화 불완전 → 방향 2(STATE 기록) + 방향 3(체크리스트 강화) 조합 권고 |
| 2026-05-11 | 원칙 미준수 구조적 해결 방안 분석 | 5개 근본 원인 경로 도출 (이해 실패/실행 누락/구조 부재/감시 비활성/LLM 특성) → 즉시 적용: 각 세션 CLAUDE.md 인라인 체크 블록 + 완료 보고 형식 추가 / 장기: 원칙 시나리오 재작성 + HIGH 위험 작업 2단계 프로토콜 |
| 2026-05-11 | IPE 통합 프로토콜 설계 | 방안 1+2+3 통합: 인라인 체크(판단 1) + 역할별 시나리오 3개 내장(판단 3 경량화) + HIGH 판정 시 승인 게이트(판단 2 조건부 발동). 명칭: IPE(Inline Principle Enforcement). H1~H6 HIGH 판정 기준 6개 정의. developer/content-qa/planner 시나리오 예시 완성. AGENT_PRINCIPLES.md IPE 섹션 추가 + 각 역할 CLAUDE.md 시나리오 블록 추가 구현 계획 수립 |
| 2026-05-11 | 세션 시작 읽기 파일 최적화 분석 | 권고안: 방향 A+C+부분B 조합. AGENT_PRINCIPLES.md(844줄)→PRINCIPLES.md(130줄 보강 후) 교체로 세션당 ~10,620 토큰 절감(67%). 감시 세션 역할 정의 중복(175줄) 제거 + CLAUDE.md 슬림화(88→42줄). 구현 4단계 순서 정의. PRINCIPLES.md 5개 보완 항목 식별 |
| 2026-07-10 | Monitor 기반 서브 에이전트 시각화·실시간 공유 설계 | A안(기존 .tool_log.jsonl 하트비트 tail, 역할 식별 불가) / B안(신규 PROGRESS_LOG.md 역할별 append + Monitor tail, 실시간 role attribution 확보) / C안(B + tool_log 역할 식별자 삽입 + 교차검증 대시보드, 병렬 호출 시 식별 가능 여부 기술 검증 필요) 3안 설계. B안 권고. 핵심 발견: (1) 현재 .tool_log.jsonl에는 role/session 식별자 필드가 없어 병렬 실행 시 "누가" 안 하는지 구분 불가 (2) ACTIVE_CONTEXT.md는 세션종료 시점 사후 기록용 계약이 이미 있어 실시간 진행 공유에 재사용 시 동시쓰기 충돌 및 기존 overseer 기계적 트리거("위임 세션명 명시" 체크) 파손 위험 → 신규 파일 필요 (3) "감시 주체"는 chief가 아니라 sentinel/overseer가 맡아야 국세청 독립 원칙과 정합 — chief 자기감시는 이해충돌 |
| 2026-07-10 | Monitor A/B/C안 전면 폐기, push(inbox) 모델 최종안 설계 | developer PoC(동시쓰기+tail 충돌로 Windows에서 100% 쓰기 실패 재현, Mutex 직렬화는 안전하나 tail이 그 직렬화 자체를 깨뜨림)를 근거로 사용자가 "실시간 tail(수동 감시)"이 아닌 "chief가 쓰기 완료 후 명시적으로 읽으라고 지시하는 push(능동 전달)" 모델로 방향 전환 지시. 최종안: (1) 폴더구조 — project-state/{project}-ai/sessions/{role}/inbox/ 신설(그 역할만 읽는 개인 작업공간, 처리 완료 파일은 inbox/_processed/로 자가 이동) + sessions/_shared/DISPATCH_LOG.md 신설(chief 전용 append-only 전달 이력 허브, "모아두는 상위 폴더" 요구를 감사 인덱스로 구현) (2) push 경로 2개만 표준 채택 — (a)신규 Agent 호출 시 프롬프트에 inbox 읽기 단계 명시 (b)기존 실행중/완료 에이전트에는 SendMessage로 "inbox에 새 파일 추가했다, 읽어라" 재개 지시(단 agent id는 chief 런타임 세션 내에서만 유효 — chief 세션 재시작 시 레지스트리 소실되어 (a)로 자동 폴백). 자체 폴링은 "1회성 실행-반환 모델과 불합치 + 사용자가 거부한 수동감시로 회귀" 이유로 명시적 기각(3) 감시는 DISPATCH_LOG ↔ 대상 STATE.md 갱신시각 ↔ inbox/_processed 이동여부 ↔ (가능 시)tool_log Read 이벤트 4중 교차검증으로 overseer/stability가 수행, chief가 DISPATCH_LOG 기록 없이 inbox에만 써넣는 "은폐성 위임"도 별도 위반으로 sentinel이 포착하도록 설계 — 단 tool_log 교차검증은 tool-logger.ps1이 session_id/agent_id를 아직 파싱하지 않아(developer PoC 확인 완료) 부분적으로만 가능, 후속 구현 필요 (4) CLAUDE.md 4개 지점(chief/PRINCIPLES.md/각 역할/감시 3세션)별 추가 문구 초안 작성 완료. 실제 파일 수정은 미실행 — 사용자 승인 후 developer 위임 예정 |

## 현재 적용 중인 판단 기준

- 범용 AI 에이전트 목적 최우선 — 단일 도메인이 아닌 폭넓은 지식 파이프라인
- 로컬 실행 필수 (비용 제약) — 클라우드 API 임베딩 제외
- 자동화 가능한 기준 우선 — 사람이 매번 판단해야 하는 기준은 회피
- 보수적 레벨 배정 — 불확실할 때는 하위 레벨 배정
- GEN 15% 상한 — GEN 초과 시 분류 체계 재검토 트리거
- 원칙 시스템 설계: "기술적 강제" 불가 시 "위반 가시성 극대화"를 차선으로 채택
- 파일 최적화 방향: 정보 손실 없이 토큰 절감 = 계층 분리 우선(A+C), AGENT_PRINCIPLES.md 자체 압축은 후속(B)
- PRINCIPLES.md는 세션 시작 필수 읽기 파일로 격상 — 단, 5개 보완 항목 추가 후에만 AGENT_PRINCIPLES.md 대체 가능
- 실시간 진행 공유는 사후 기록용 파일(STATE.md/ACTIVE_CONTEXT.md)과 별개 계층으로 분리 — 목적·쓰기 시점·동시성 요구가 다르면 파일도 분리
- "감시"와 "조율"은 항상 다른 주체가 맡아야 한다 — chief가 자신이 위임한 서브 에이전트의 태만 여부를 스스로 판정하는 구조는 국세청 독립 원칙과 충돌
- 자가보고(서브 에이전트가 스스로 진행상황을 기록) 기반 신호는 항상 기계적 신호(.tool_log.jsonl 실제 도구 호출)와 교차검증 없이는 신뢰도 낮음 — "말로만 보고"를 걸러내는 장치 필요
- 수동적 감시(tail/폴링)보다 능동적 전달(push)이 이 하네스의 실행 모델(에이전트는 1회 호출-작업-반환, 상주 프로세스 아님)에 근본적으로 더 부합 — 기술적 우회가 아니라 프로세스 모델에 맞는 설계를 우선한다
- SendMessage로 기존 에이전트를 재개하는 것은 같은 chief 런타임 세션 내에서만 유효(agent id가 세션에 귀속) — 세션 경계를 넘는 영속적 레지스트리가 없다는 전제 하에 설계해야 함
- 동일 파일에 대한 "쓰기 후 재수정"은 이미 읽었을 수도 있는 대상과 경합할 수 있어 금지 — 추가 지시는 항상 새 파일로, 기존 파일은 불변으로 취급

## chief와 협의한 사항

- 2026-05-08: KR4·KR5·KR7 결정 작업 지시 수령 및 완료
- KR4: 초안 8개 → 확정 9개 (PHI 추가, SCI/ENG 경계 기준 명확화)
- KR5: L1~L5 유지, 각 레벨 자동 판단 신호 3개 이상 충족 방식으로 구체화
- KR7: bge-m3 채택 (KR-ELECTRA → bge-m3 마이그레이션 필요, v0.2에서 처리)
- 2026-05-11: 감시 세션 자동 실행 구조 문제 분석 지시 수령 및 완료
  - hooks agent 타입 experimental, command에서 Agent 도구 호출 불가 확인
  - 방향 2(STATE 기록) + 방향 3(체크리스트) 조합 권고
  - AGENT_PRINCIPLES.md 수정 대상 구체화 (체크리스트 1항목 추가)
  - chief/STATE.md 감시 세션 상태 블록 표준화 제안
- 2026-05-11: 원칙 미준수 구조적 해결 방안 분석 지시 수령 및 완료
  - 5개 근본 원인 경로 도출 (이해 실패 / 실행 단계 누락 / 구조적 강제 부재 / 감시 세션 비활성 / LLM Attention 특성)
  - 즉시 적용 권고: 각 세션 CLAUDE.md에 "작업 직전 원칙 확인" 인라인 체크 블록 삽입 + 완료 보고 형식에 원칙 준수 확인 항목 추가
  - 장기 개선 권고: 원칙 파일 시나리오 재작성 + HIGH 위험 작업 2단계 실행 프로토콜
  - chief 판단 요청 3개: (1) 즉시 적용 여부, (2) 원칙 재작성 예약 여부, (3) 2단계 프로토콜 속도 손실 감수 여부
- 2026-05-11: IPE 통합 프로토콜 설계 완료
  - 명칭: IPE (Inline Principle Enforcement)
  - 4단계 흐름: 단계0(시나리오 내장) → 단계1(인라인 체크) → 단계2(위험도 판정) → 단계3(HIGH 시 승인 게이트)
  - HIGH 판정 기준 H1~H6 6개 조건 정의
  - developer/content-qa/planner 역할별 시나리오 3개씩 예시 완성
  - 구현 위치: AGENT_PRINCIPLES.md IPE 섹션 + 각 역할 CLAUDE.md 시나리오 블록
  - 구현 순서 6단계 권고 (AGENT_PRINCIPLES.md 먼저 → 역할 파일 → content-qa 검토)
  - chief 판단 요청 4개: IPE 체크 출력 여부 / HIGH 에스컬레이션 범위 / 나머지 역할 시나리오 작성 시기 / 감시 세션 IPE 적용 여부

## 다음 작업 예상

- [폐기] Monitor B안 기반 구현 계획 — push(inbox) 모델로 대체됨, 아래 항목으로 교체
- [대기] push(inbox) 모델 사용자 승인 대기 — 승인 시 구현 순서: (1) project-state/{project}-ai/sessions/{role}/inbox/ + inbox/_processed/ 디렉터리 신설(전 역할) + sessions/_shared/DISPATCH_LOG.md 신설(개발 위임) (2) sessions/chief/CLAUDE.md에 "MD 푸시 절차" 절 추가(Agent 호출 프롬프트 템플릿에 inbox 읽기 단계 삽입 + SendMessage 재개 절차 + DISPATCH_LOG 기록 의무) (3) sessions/_shared/PRINCIPLES.md에 inbox/DISPATCH_LOG 표준 형식 추가 (4) 역할별 CLAUDE.md(developer/planner/rca/okr/data/content-qa/perf) "시작 시 필수 확인"에 inbox 확인 단계 추가 (5) overseer/stability/sentinel CLAUDE.md에 DISPATCH_LOG↔STATE.md↔inbox/_processed↔tool_log 4중 교차검증 트리거 추가 — 단 tool_log 부분은 tool-logger.ps1의 session_id/agent_id 파싱 보강(developer 별도 구현 필요)이 선행되어야 완전해짐, 그 전까지는 3중 교차검증으로 부분 가동
- [대기] tool-logger.ps1 session_id/agent_id/agent_type 파싱 보강 — developer PoC에서 raw stdin에 필드 실존 확인됨, push 모델의 tool_log 교차검증 완전 가동을 위한 선행 구현 (chief 승인 후 developer 위임)
- [대기] IPE 구현: AGENT_PRINCIPLES.md IPE 섹션 추가 — chief 승인 후 developer 세션에 위임
- [대기] IPE 구현: 각 역할 CLAUDE.md 시나리오 블록 추가 (developer/content-qa/planner/rca/okr/data/perf)
- [대기] content-qa 세션의 IPE 설계 전체 검토 — 구현 완료 후
- [선택] 나머지 역할(rca/okr/data/perf) 시나리오 작성 — chief 지시 후 처리
- [대기] 세션 시작 파일 최적화 구현 — chief 승인 후 4단계 순서로 진행 (1단계: CLAUDE.md 읽기 순서 교체 / 2단계: CLAUDE.md 슬림화 / 3단계: AGENT_PRINCIPLES.md 감시 세션 중복 175줄 제거 / 4단계: AGENT_PRINCIPLES.md 추가 압축)
- [필수] PRINCIPLES.md 보완 5개 항목: IPE 계획 명세서 형식 + 감시 세션 원칙2 예외 + 세션 종료 기록 형식 + STATE.md 표준 템플릿 + H1~H6 기준 표
- PROJECT_CONTEXT_dataverse.md 갱신 (KR4·KR5·KR7 확정 내용 반영) — 별도 지시 후 처리
- v0.2 파이프라인 구현 시 bge-m3으로 기존 186개 청크 재임베딩
- GEN 비율 모니터링 기준 구현 (전체 수집 문서의 15% 상한)
