# AI 멀티 세션 원칙 시스템 — 인수인계 문서

> 최종 업데이트: 2026-05-11  
> 작성: chief 세션  
> 목적: 다음 세션이 컨텍스트 없이도 v2 작업을 바로 시작할 수 있도록

---

## 1. 이 시스템이 무엇인가

한 AI 세션이 혼자 생각하면 맹점이 생긴다.  
여러 세션이 각자의 전문 관점으로 독립적으로 검토할 때 더 객관적이고 깊이 있는 결과물이 나온다.  
이것이 이 시스템을 만든 이유다.

- **chief**: 사용자와 직접 소통, 작업 조율 (실행 안 함)
- **서브 세션 7개**: planner / developer / rca / okr / data / content-qa / perf
- **감시 세션 3개**: overseer / stability / sentinel (chief에게만 경고)

---

## 2. 레포 구조

| 레포 | 역할 | push 여부 |
|------|------|-----------|
| `C-o-T/PRINCIPLES` | 원칙·설정 전용 | **절대 push 금지** |
| `C-o-T/joomidang-ai` | joomidang 프로젝트 AI 세션 상태 | push/pull 모두 |
| `C-o-T/dataverse-ai` | dataverse 프로젝트 AI 세션 상태 | push/pull 모두 |

**로컬 디렉터리 구조:**
```
PRINCIPLES/                        ← git: C-o-T/PRINCIPLES
├── AGENT_PRINCIPLES.md
├── CLAUDE.md
├── START_HERE.md
├── sessions/{role}/CLAUDE.md
└── project-state/                 ← .gitignore 등록
    ├── joomidang-ai/              ← git: C-o-T/joomidang-ai
    │   ├── HANDOFF.md
    │   ├── sessions/{role}/STATE.md
    │   └── sessions/_shared/ACTIVE_CONTEXT.md
    └── dataverse-ai/              ← git: C-o-T/dataverse-ai
        ├── sessions/{role}/STATE.md
        └── sessions/_shared/ACTIVE_CONTEXT.md
```

---

## 3. v1 완료 현황

| 항목 | 상태 |
|------|------|
| 7가지 운영 원칙 수립 (원칙 A 포함) | ✅ 완료 |
| 11개 역할 CLAUDE.md 작성 | ✅ 완료 |
| 팀원 영속성 모델 (STATE.md) | ✅ 완료 |
| 감시 세션 구조 (overseer/stability/sentinel) | ✅ 완료 |
| PRINCIPLES 레포 오염 제거 + 레포 3분리 | ✅ 완료 |
| START_HERE.md + CLAUDE.md 새 구조 반영 | ✅ 완료 |

---

## 4. v2 작업 목록 (다음 세션에서 처리)

### [HIGH] AGENT_PRINCIPLES.md — 레포 분리 구조 전면 반영

현재 AGENT_PRINCIPLES.md는 단일 레포 기준으로 작성되어 있다.  
CLAUDE.md / START_HERE.md는 이미 업데이트됐지만, AGENT_PRINCIPLES.md는 아직이다.

수정 대상:
- **6.2 세션 시작 체크리스트**: STATE.md 읽기를 "프로젝트 레포(`project-state/{project}-ai/`)에서 읽기"로 수정
- **6.4 컨텍스트 파일 위치 테이블**: PRINCIPLES 레포 파일 vs 프로젝트 레포 파일로 분리 표기
- **git 추적 규칙 섹션**: `project-state/` 구조 및 "상태 파일은 프로젝트 레포에만 push" 규칙 반영

### [HIGH] sessions/chief/CLAUDE.md — 서브 세션 호출 프롬프트 업데이트

현재 서브 세션에게 전달하는 시작 프롬프트:
```
3. sessions/{role}/STATE.md  ← 없으면 최초 투입
```
수정 후:
```
3. project-state/{프로젝트명}-ai/sessions/{role}/STATE.md  ← 프로젝트 레포에서 읽기
```

### [MEDIUM] sessions/_shared/PRINCIPLES.md — 자가 진단 표 원칙 A 신호 행 추가

현재 자가 진단 표에 원칙 A 관련 신호가 없다.  
추가할 행:
```
| "내가 빠르게 처리하면 되지" / 서브 세션 없이 완료 | 원칙 A (위임 의무) |
```

### [MEDIUM] 감시 세션 실제 활성화 검증 구조 개선

현재 문제: 원칙에 "작업 시작 시 overseer/stability/sentinel을 Agent로 실행"이라고 되어 있지만,  
실제로 매 세션마다 실행되는지 확인할 방법이 없다.  
sentinel이 이걸 감시해야 하는데 sentinel도 chief가 실행해야 하는 구조적 모순이 존재한다.

검토 방향:
- sentinel을 Claude Code hooks로 자동 실행하는 방법 검토
- 또는 chief 시작 시 감시 세션 실행 여부를 STATE.md에 기록하는 방식

### [LOW] TEAM_STATUS.md 생성

팀원 현황판 — 각 세션의 마지막 작업, 현재 상태를 한눈에 보는 파일.  
위치: `project-state/{프로젝트}-ai/TEAM_STATUS.md`

---

## 5. 다음 세션 시작 방법

```bash
# PRINCIPLES 최신 상태 확인
cd PRINCIPLES && git pull

# 프로젝트 상태 최신화
cd project-state/joomidang-ai && git pull

# Claude Code 실행 (PRINCIPLES 디렉터리에서)
# → CLAUDE.md 자동 로드
# → START_HERE.md 읽고 준비 완료
```

세션에게 전달할 멘트:
> "깃허브에 있는 START_HERE.md 읽고 작업 준비해줘. project-state/joomidang-ai도 클론돼있어."

---

## 6. 주의사항

- **PRINCIPLES 레포에는 절대 push하지 않는다** — `.gitignore`로 차단되어 있지만 `git add -f` 등으로 우회하지 않는다
- **작업 종료 시 반드시 STATE.md + ACTIVE_CONTEXT.md 업데이트 후 push** — 이번 v1 세션에서 이 단계를 빠뜨려서 이력 누락이 발생했다
- **원칙 수정은 content-qa에 위임** — chief가 직접 원칙 파일을 수정하는 것은 원칙 A 위반
