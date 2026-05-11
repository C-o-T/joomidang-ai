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
작업 내용  : IPE 시스템 설계·구현 + chief/CLAUDE.md 결함 4건 수정
완료       : - IPE(Inline Principle Enforcement) 원칙 I 추가 (AGENT_PRINCIPLES.md)
             - 전 세션 CLAUDE.md 12개 + PRINCIPLES.md IPE 블록 + 역할별 위반 시나리오 TOP3 삽입
             - chief/CLAUDE.md 설계 결함 4건 수정 (복수경고·git분기·불확실성·감시세션타이밍)
             - PRINCIPLES 레포 push: 29942c0, 77f5e88
             - joomidang-ai 레포 push: e66cb67, 0302ae8, a540a7f
미완료/보류: 없음
중요 결정  : IPE = 인라인 체크(판단1) + 역할 시나리오(판단3) + 조건부 승인게이트(판단2) 통합
             HIGH 기준 H1~H6, 면제 기준 명시
주의사항   : 다음 세션 시작 시 감시 세션(overseer/stability/sentinel) Agent 실행 필수
```

---

## 현재 진행 중인 작업

없음

---

## 미완료 / 보류

없음 — 전 세션 CLAUDE.md 경로 수정 완료 (6ccf244)

---

## 중요 결정 이력

| 날짜 | 결정 내용 | 이유 |
|------|-----------|------|
| 2026-05-08 | 팀원 영속성 모델 도입 — sessions/{role}/STATE.md | 서브 세션을 임시 용역이 아닌 포지션별 팀원으로 운영 |
| 2026-05-08 | 감시 세션 경고 대상 chief 전용 확정 | 구단 수뇌부는 감독(chief)에게만 경고 |
| 2026-05-09 | 레포 3분리 구조 확정 | PRINCIPLES(설정) / {프로젝트}-ai(상태) — 오염 원천 차단 |
