# rca 상태 파일

> 이 팀원의 개인 기억. 작업 완료 시 갱신하며, 다음 호출 시 이전 맥락을 복원한다.

## 현재 상태
대기 중

## 완료한 작업 이력
| 날짜 | 작업 | 핵심 결정 |
|------|------|-----------|
| 2026-08-30 | joomidang-v2 전체 현황 재감사 (개발완성도/보안/배포인프라/TODO 재검증) | `npx next build` 재현 결과 lib/email.ts의 `new Resend(RESEND_API_KEY)` 모듈 최상단 즉시생성이 원인으로 프로덕션 빌드 여전히 실패 확인 — 서비스 오픈 전 유일한 필수(blocking) 개발 항목으로 결론. tsc --noEmit은 통과, app/api 라우트에 스텁 없음, admin/seller 권한체크 샘플 전부 정상. id-documents 버킷 퍼블릭 문제·FAQ 사업자정보 placeholder는 여전히 미해결로 재확인. .env는 git에 커밋돼있으나 실제 값은 전부 placeholder라 즉각적 유출 위험 아님(다만 관례상 이례적이라 주의 필요). 테스트/CI 전무, seed 스크립트(ts-node 미설치)도 여전히 결함. docs/TODO.md에 위시리스트 서버연동 관련 모순된 두 줄(완료 표시 vs 옛 미해결 항목 잔존) 발견 — 문서 정리 필요. 배포 인프라(Dockerfile/ecosystem.config.js/nginx.conf)는 내용상 정상, vercel.json은 Gabia 전환 이후 죽은 설정으로 판단. |

## 현재 적용 중인 판단 기준
(이번 프로젝트에서 정립된 원칙·방향 — 최초엔 비워둠)

## chief와 협의한 사항
(중요 결정 요약 — 최초엔 비워둠)

## 다음 작업 예상
lib/email.ts Resend lazy-init 수정(developer 위임) 후 next build 재검증. docs/TODO.md 위시리스트 항목 모순 정리.
