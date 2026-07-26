# stability 상태 파일

> 이 팀원의 개인 기억. 오류 추적 이력을 기록하며, 다음 호출 시 이전 추적 맥락을 복원한다.

## 현재 상태

경고 발동 완료 — chief 응답 대기 중

## 오류 추적 이력

| 날짜 | 발생 위치 | 오류 유형 | 발생 횟수 | 처리 결과 |
|------|-----------|-----------|-----------|-----------|
| 2026-07-10 | joomidang-v2/.claude/hooks/tool-logger.ps1 (PostToolUse 로그 훅) | 구조적 | 세션 내 9건 중 7건 (checkout/page.tsx, payment-intent/route.ts, app/page.tsx, products/route.ts, products/page.tsx 등 5개 이상 파일·모듈) | 경고 발동 (아래 참조), 수정 여부 확인 대기 |

## 반복 오류 모니터링 (3회 기준)

| 오류 패턴 | 현재 발생 횟수 | 상태 |
|-----------|---------------|------|
| tool-logger.ps1 JSON 파싱 실패 → regex_fallback (Korean 경로 인코딩 손상, 무효 JSON 라인 기록) | 7 / 세션 내 9건 | 3회 기준 초과 — 구조적 오류로 확정, 경고 발동 |

## 경고 발동 이력

| 날짜 | 대상 | 오류 유형 | 처리 결과 |
|------|------|-----------|-----------|
| 2026-07-10 | chief | 구조적 (tool-logger.ps1 인코딩/JSON 무효화 버그) | chief 및 사용자 동시 보고 완료 — RCA 근거 첨부, 수용/반박 응답 대기 |

## 현재 적용 중인 판단 기준

- 반복 오류 3회 기준 엄수
- 레포 분리 구조(`project-state/{프로젝트명}-ai/...`) 경로 일관성 감시
- `.tool_log.jsonl`은 감시 세션의 1차 증거 파일 — 로그 자체의 무결성(유효 JSON 여부)도 감시 대상에 포함

## 다음 감시 예상

- CRITICAL-2 (페이지네이션 필터 누락) 수정 여부 확인
- CRITICAL-1 (검색 contains mode 미설정) — PostgreSQL 전환 시 재검증
- WARN-1 쿼리 중복 해소 여부 확인

---

## 2026-07-10 기술 감사 — ef0ba34 커밋 파일 9개

| 날짜 | 발생 위치 | 오류 유형 | 발생 횟수 | 처리 결과 |
|------|-----------|-----------|-----------|-----------|
| 2026-07-10 | products/page.tsx L182-214 | 신규 (CRITICAL) | 1 | 보고 완료 — 페이지네이션 URL에서 필터 파라미터 5개 누락 |
| 2026-07-10 | route.ts + page.tsx 검색 쿼리 | 신규 (CRITICAL) | 1 | 보고 완료 — contains mode 미설정, PostgreSQL 전환 시 발현 |
| 2026-07-10 | route.ts + page.tsx 전체 | 신규 (WARN) | 1 | 보고 완료 — 쿼리 로직/SortKey/getOrderBy 완전 중복 |
| 2026-07-10 | route.ts L24 | 신규 (WARN) | 1 | 보고 완료 — category 파라미터 검증 없이 타입 단언만 사용 |
| 2026-07-10 | FAQContent.tsx | 신규 (WARN) | 1 | 보고 완료 — ja/zh 로케일 FAQ 미구현 (영문 폴백) |
| 2026-07-10 | Footer.tsx L45 | 신규 (WARN) | 1 | 보고 완료 — locale을 keyof로 강제 캐스팅, 타입 안전성 부재 |

**반복 오류 모니터링 (3회 기준)**:
- 현재 모든 항목 1회 발생 — 반복 오류 없음, chief 경고 발동 없음
