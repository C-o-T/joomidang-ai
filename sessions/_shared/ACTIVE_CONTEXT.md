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
작성 시각  : 2026-05-11
작업 내용  : PRINCIPLES 레포 재구성 (오염 제거) + 원칙 v2 준비 논의
완료       : - C-o-T/PRINCIPLES 레포 force push로 정리 (원칙·설정 파일만 남김)
             - C-o-T/joomidang-ai 레포 생성 (세션 상태 파일 분리 보관)
             - C-o-T/dataverse-ai 레포 생성 (dataverse 컨텍스트 분리)
             - START_HERE.md + CLAUDE.md 새 레포 구조 반영
             - .gitignore 보강 (project-state/, STATE.md 등 차단)
미완료/보류: - 원칙 v2: AGENT_PRINCIPLES.md 6.2 + git 추적 섹션 새 구조 반영
             - 원칙 v2: PRINCIPLES.md 자가 진단 표에 원칙 A 신호 행 추가
완료 후 누락: STATE.md/ACTIVE_CONTEXT 업데이트를 세션 종료 시 하지 않음 → 원칙 6 위반
중요 결정  : PRINCIPLES 레포는 pull 전용, 모든 프로젝트 상태는 {프로젝트}-ai 레포에 분리
주의사항   : 다음 세션은 원칙 v2 작업 전 이 내용 숙지 후 시작
```

---

## 현재 진행 중인 작업

없음 — 원칙 v2 준비 대기 중

---

## 미완료 / 보류

- **원칙 v2** (다음 세션에서 처리):
  - `AGENT_PRINCIPLES.md` 6.2 시작 체크리스트 → STATE.md가 프로젝트 레포에 있다는 내용 반영
  - `AGENT_PRINCIPLES.md` git 추적 규칙 → `project-state/` 구조 반영
  - `sessions/_shared/PRINCIPLES.md` 자가 진단 표 → 원칙 A 신호 행 추가

---

## 중요 결정 이력

| 날짜 | 결정 내용 | 이유 |
|------|-----------|------|
| 2026-05-08 | 팀원 영속성 모델 도입 — sessions/{role}/STATE.md | 서브 세션을 임시 용역이 아닌 포지션별 팀원으로 운영 |
| 2026-05-08 | 감시 세션 경고 대상 chief 전용 확정 | 구단 수뇌부는 감독(chief)에게만 경고 |
| 2026-05-09 | 레포 3분리 구조 확정 | PRINCIPLES(설정) / {프로젝트}-ai(상태) — 오염 원천 차단 |
