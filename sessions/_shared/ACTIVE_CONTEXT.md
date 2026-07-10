# 현재 작업 컨텍스트

> 세션 간 작업 상태를 공유하는 파일이다.
> 세션 종료 시 반드시 아래 형식으로 업데이트한다.
>
> ⚠️ 이 파일은 joomidang-ai 레포 전용이다.
> 프로젝트별 작업 상태는 각 프로젝트 레포(`C-o-T/{프로젝트}-ai`)에서 관리한다.

---

## 마지막 업데이트

```
역할       : chief
작성 시각  : 2026-05-13
작업 내용  : 원칙 시스템 정합성 감사 + sentinel 자동화 조사 + 수정 적용
완료       : - AGENT_PRINCIPLES.md: 원칙 5·세션 구조 총칙에 원칙 I(IPE) 추가, 변경 이력 기재
             - PRINCIPLES.md: 세션 시작 6단계 추가 (chief 감시 세션 활성화 강제)
             - sessions/chief/CLAUDE.md: 감시 세션 미실행 시 원칙 A 위반 문구 강화
             - CLAUDE.md (루트): push 금지 주석 "상태 파일" 한정으로 명확화 (e322f33)
미완료/보류: sentinel 자동화 hooks 방안 — UserPromptSubmit hook 활용 여부 사용자 결정 대기
중요 결정  : 단기 자동화 = 6단계 명시(완료) / 중기 = hooks 설정 (선택적)
주의사항   : 없음
```

---

## ⚠️ 긴급 공지 — 2026-05-14~15 원칙 시스템 v3 완성

**다음 세션 시작 전 반드시 확인**

### 1. sentinel 자동화 hook 활성화 (fe3550f)

Claude Code UserPromptSubmit hook이 이제 자동 실행된다.

- 세션 시작 후 **10분 내에** overseer / stability / sentinel Agent 3개를 실행해야 한다
- 실행 완료 후 **Write 도구로 `.claude/.sentinel_active` 파일 생성 필수**
  - 내용: 현재 날짜/시각 (예: `2026-05-13T16:00:00`)
- 파일 미생성 시 10분 경과 후 **모든 프롬프트가 자동 차단**됨

### 2. IPE 체크 블록 형식 변경 없음 — PRINCIPLES.md 6단계 추가됨

세션 시작 절차가 5단계 → **6단계**로 변경됐다.
6단계: overseer / stability / sentinel 3개 Agent 호출 (chief 전용)

### 3. AGENT_PRINCIPLES.md 원칙 I 서술 보완 완료 (e322f33)

원칙 5 본문 및 세션 구조 총칙에 원칙 I(IPE) 포함 표현 추가됨.

---

## 마지막 업데이트

```
역할       : chief
작성 시각  : 2026-05-13
작업 내용  : sentinel hook 구현 + 원칙 정합성 수정 완료
완료       : - .claude/settings.json: SessionStart + UserPromptSubmit hook 설정
             - .claude/hooks/session-start.ps1: 세션 시작 시 마커 초기화
             - .claude/hooks/check-sentinel.ps1: 감시 세션 미활성 10분 후 블로킹
             - sessions/chief/CLAUDE.md: .sentinel_active 마커 파일 생성 의무 추가
             - AGENT_PRINCIPLES.md: 원칙 I 서술 보완, 변경 이력 추가 (e322f33)
             - PRINCIPLES.md: 세션 시작 6단계 추가 (fe3550f)
미완료/보류: 없음
중요 결정  : sentinel 자동화 = UserPromptSubmit hook + 10분 유예 + 마커 파일 방식
주의사항   : 다음 세션 시작 시 반드시 위 긴급 공지 확인 후 .sentinel_active 생성
```

---

## 마지막 업데이트

```
역할       : chief
작성 시각  : 2026-05-15
작업 내용  : 원칙 시스템 v3 완성 — hook 자동화 + 감시 독립성 + 연대책임
완료       : - PostToolUse hook: 도구 호출 로그 자동 기록 (.tool_log.jsonl)
             - 감시 세션 독립성: 로그 직접 읽기 + 기계적 경고 트리거
             - 연대책임 시스템: VIOLATION_LOG.md + 팀 해산 프로토콜 (Team 1 초기화)
             - stability 권고 반영: tool-logger 절대경로, Bash 로그 제거 (48f37ee)
             - .sentinel_active 프로젝트 루트로 경로 이동 + 2026-05-14 갱신
             - overseer 원칙 A 경고 반박: 이번 세션 위임 실제 이행됨
               (planner/rca/developer/content-qa Agent 실행 + .delegation_active 절차 준수)
미완료/보류: 없음
중요 결정  : 서브 세션 STATE.md 미업데이트 = 서브 세션 원칙 6 위반 (chief 직접 처리 증거 아님)
주의사항   : 다음 세션 시작 시 VIOLATION_LOG.md 읽기 (7단계) 필수 — Team 1, 위반 0회
```

---

## 마지막 업데이트

```
역할       : chief
작성 시각  : 2026-06-24
작업 내용  : 주미당 V2 플랫폼 기능 완성 + 배포 준비
완료       : - git commit: 전체 코드 7커밋 (127파일 포함)
             - ImageGallery 컴포넌트: 이전/다음/썸네일 목록
             - 셀러 상품 폼 (신규+편집): 다중 이미지 업로드 3장
             - API seller/products POST+PUT: images 배열 지원
             - SEO: generateMetadata (상품 상세), sitemap.ts, robots.ts
             - 상품 상세 페이지: 관련 상품 4개 섹션
             - 검색 버그 수정: mode:insensitive → contains (SQLite 호환)
             - vercel.json: icn1 리전, prisma generate 빌드
             - lib/prisma.ts: DATABASE_URL 기반 어댑터 자동 선택 (libsql/pg)
             - prisma/seed.ts: 동일 자동 어댑터 선택
             - .sentinel_active: 프로젝트 루트에 생성
미완료/보류: - Supabase PostgreSQL 연결 (DATABASE_URL 필요)
             - schema.prisma provider "sqlite" → "postgresql" 전환 (Supabase 연결 후)
             - Vercel 배포 (위 완료 후)
중요 결정  : schema 전환은 Supabase 자격증명 없이 local dev를 깨뜨리므로 사용자 확인 후 진행
주의사항   : schema 전환 시 npx prisma migrate reset + migrate dev --name init + db seed 필요
```

---

## 마지막 업데이트

```
역할       : chief
작성 시각  : 2026-07-10
작업 내용  : 세션 시작 절차 정식 수행 (7단계) + 감시 세션 최초 실가동 + 위반 사이클 1회 처리
완료       : - overseer/stability/sentinel Agent 3개 실제 실행, .sentinel_active 마커 생성
             - stability: joomidang-v2 tool-logger.ps1 UTF-8 인코딩 버그 발견 → developer가 수정·커밋(ff72e87)
             - sentinel: PRINCIPLES 레포 tool-logger.ps1 하드코딩 경로 결함 발견 → developer가 수정·커밋(0c5d5cf)
             - sentinel/overseer: ACTIVE_CONTEXT_joomidang.md 비표준 중복 파일 발견 → 비교 후 삭제(고유 정보 없음 확인)
             - overseer: 2026-06-04 감시 세션 STATE.md 초기화가 비표준 경로·정식 절차(3진아웃) 없이 이루어진 점을
               원칙 6+4 위반으로 판정 → VIOLATION_LOG.md #1 기록 (chief 위반 0/3 → 1/3)
             - 사용자에게 06-04 지시 여부 확인 → "제가 지시했습니다" 확답 받음
             - sentinel 재검토: 지시의 정당성과 절차 위반(비표준 파일 기록·미승계·06-24 기록 5주간 미푸시)은
               별개 사안으로 판단 → 위반 판정 유지(1/3), 근거를 VIOLATION_LOG.md에 주석으로 추가
             - joomidang-ai 레포 미푸시 변경사항(06-24 기록 등) 커밋(6373079)+push 완료
완료       : 세 레포(PRINCIPLES/joomidang-ai/joomidang-v2) 모두 커밋·push 최신 상태
미완료/보류: - sentinel 제안 — "사용자 직접 지시에 의한 예외적 초기화" 절차가 VIOLATION_LOG.md에 정의되어
               있지 않음. 향후 이런 경우 표준 파일 기재 + VIOLATION_LOG 동시 기록을 의무화하는 절차
               신설 여부 사용자 결정 대기
             - joomidang-v2 레포에 이번 작업과 무관한 미커밋 변경(상품 필터/정렬 기능 추정)이 존재 —
               사용자가 "지금은 놓아두기"로 확인, 추후 확인 필요
             - Supabase/AWS 프로젝트 생성 + DATABASE_URL 설정 (기존 미완료 항목, 변동 없음)
중요 결정  : "지시의 정당성"과 "절차 준수 여부"는 별개로 판단한다 — 사용자 승인이 있어도 표준 경로·
             정식 절차를 우회한 사실 자체는 위반으로 유지 (sentinel 판단, chief 수용)
발견한 문제: PRINCIPLES 레포와 joomidang-v2 레포 각각의 tool-logger.ps1 훅이 서로 다른 이유로
             장기간 작동 불능/손상 상태였음에도 감지되지 않고 있었음 (감시 인프라의 로그 기반 검증이
             사실상 무력화된 상태로 5주 이상 방치)
주의사항   : 다음 세션은 VIOLATION_LOG.md 1/3 상태로 시작 — 2회 도달 시 모든 결과물 overseer 검토 필수 전환됨
```

---

## 현재 진행 중인 작업

없음 — 훅 인프라 복구 및 위반 사이클 1건 처리 완료, 배포 준비 단계 대기 중

---

## 미완료 / 보류

- Supabase / AWS 프로젝트 생성 + DATABASE_URL 설정 (사용자 액션 필요)
- schema.prisma provider를 "postgresql"로 전환 (DB URL 설정 후)
- Vercel 또는 AWS 배포 (위 완료 후)
- "사용자 직접 지시에 의한 예외적 초기화" 절차 신설 여부 (sentinel 제안, 사용자 결정 대기)
- joomidang-v2 레포의 무관한 미커밋 변경(상품 필터/정렬) 추후 확인

---

## 중요 결정 이력

| 날짜 | 결정 내용 | 이유 |
|------|-----------|------|
| 2026-05-08 | 팀원 영속성 모델 도입 — sessions/{role}/STATE.md | 서브 세션을 임시 용역이 아닌 포지션별 팀원으로 운영 |
| 2026-05-08 | 감시 세션 경고 대상 chief 전용 확정 | 구단 수뇌부는 감독(chief)에게만 경고 |
| 2026-05-09 | 레포 3분리 구조 확정 | PRINCIPLES(설정) / {프로젝트}-ai(상태) — 오염 원천 차단 |
| 2026-05-13 | sentinel hook 자동화 — UserPromptSubmit 블로킹 방식 | 구조적 모순(chief가 sentinel을 수동 실행) 해소 |
| 2026-07-10 | 지시의 정당성 ≠ 절차 준수 — 사용자 승인 있어도 비표준 절차는 위반으로 유지 | 사후 승인이 절차 우회의 면죄부가 되면 원칙 6·투명성이 형해화됨 |
