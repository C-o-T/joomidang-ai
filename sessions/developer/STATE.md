# developer 상태 파일

> 이 팀원의 개인 기억. 작업 완료 시 갱신하며, 다음 호출 시 이전 맥락을 복원한다.

## 현재 상태

대기 중

## 완료한 작업 이력

| 날짜 | 작업 | 핵심 결정 |
|------|------|-----------|
| 2026-05-08 | dataverse — arXiv 수집기 구현 | requests 라이브러리, verify=False (Windows TLS 이슈) |
| 2026-05-09 | joomidang V2 — 미구현 페이지 + API 라우트 전체 구현 | Prisma $transaction + findUniqueOrThrow 재조회 패턴, Zustand mounted 패턴 |
| 2026-05-09 | joomidang V2 — 셀러 대시보드 + 관리자 패널 전체 구현 | seller.status APPROVED 체크 후 상품 등록, CartItem 선삭제 후 Product 삭제, router.refresh() 패턴으로 Server Component 갱신 |

## 현재 적용 중인 판단 기준 (joomidang V2)

- Next.js 16: `await params`, `await headers()`, `await searchParams` 필수
- Prisma Decimal → `Number()` 변환 필수
- `$transaction` 내 include 타입 추론 깨짐 → transaction 후 `findUniqueOrThrow` 재조회
- Zustand persist hydration → `mounted` state + `useEffect` 패턴
- 카테고리 enum: schema에서 `OTHER` (ETC 아님)
- API 응답: `ok()` / `fail()` 헬퍼 일관 사용
- TypeScript `any` 금지 — `as unknown as T` 패턴으로 대체
- 셀러 상품 권한: `sellerId` 직접 비교로 타 셀러 접근 차단
- 관리자 상태 변경: SUSPENDED 시 User.role → CONSUMER 강등, APPROVED 시 SELLER 확인
- 클라이언트 상태 변경 버튼: `useRouter().refresh()` 로 Server Component 재실행

## chief와 협의한 사항

- joomidang V2 스택: Next.js 16 + Prisma 7 + Supabase + NextAuth v5 + Stripe
- Stripe API version: 2026-04-22.dahlia

## 다음 작업 예상

- Stripe Payment Intent 연동
