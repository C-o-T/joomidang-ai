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

## 현재 진행 중인 작업

없음 — 버그 수정 완료, 배포 대기 중

---

## 미완료 / 보류

- Supabase / AWS 프로젝트 생성 + DATABASE_URL 설정 (사용자 액션 필요)
- schema.prisma provider를 "postgresql"로 전환 (DB URL 설정 후)
- Vercel 또는 AWS 배포 (위 완료 후)

---

## 중요 결정 이력

| 날짜 | 결정 내용 | 이유 |
|------|-----------|------|
| 2026-05-08 | 팀원 영속성 모델 도입 — sessions/{role}/STATE.md | 서브 세션을 임시 용역이 아닌 포지션별 팀원으로 운영 |
| 2026-05-08 | 감시 세션 경고 대상 chief 전용 확정 | 구단 수뇌부는 감독(chief)에게만 경고 |
| 2026-05-09 | 레포 3분리 구조 확정 | PRINCIPLES(설정) / {프로젝트}-ai(상태) — 오염 원천 차단 |
| 2026-05-13 | sentinel hook 자동화 — UserPromptSubmit 블로킹 방식 | 구조적 모순(chief가 sentinel을 수동 실행) 해소 |
