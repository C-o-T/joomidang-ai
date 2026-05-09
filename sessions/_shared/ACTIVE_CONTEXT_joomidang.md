# joomidang V2 작업 컨텍스트

> 이 파일은 joomidang V2 프로젝트 전용 작업 상태 파일이다.
> 세션 종료 시 반드시 업데이트한다.

---

## 마지막 업데이트

```
역할       : chief
작성 시각  : 2026-05-09
작업 내용  : joomidang-v2 프로젝트 기초 구축
완료       : - Next.js 16 + Prisma 7 + NextAuth v5 + Stripe + Tailwind v4 초기화
             - 6개국 테마 시스템 (KR/JP/CN/US/EU/SEA/DEFAULT) 구현
             - Geo 감지: proxy.ts + /api/geo (Vercel Edge + ip-api.com 폴백)
             - DB 스키마: User/Seller/Product/Cart/Order/Review + NextAuth 모델
             - API 라우트: products, cart, orders, auth/register, webhooks/stripe
             - 인증: login/register 페이지 + NextAuth v5 Credentials provider
             - 메인 홈 (Server Component, 테마 자동 적용)
             - TypeScript 에러 0개 달성
             - CLAUDE.md 업데이트 (Next.js 16 + Prisma 7 변경사항)
미완료/보류: - Supabase 프로젝트 생성 및 .env.local 설정 필요
             - npx prisma migrate dev --name init 실행 필요
             - 상품 상세 페이지 (/products/[id])
             - 장바구니 UI (cart/page.tsx)
             - Stripe 결제 플로우 (checkout)
             - 셀러 대시보드 (seller/)
             - 상품 이미지 업로드 (Supabase Storage)
중요 결정  : Next.js 16 + Prisma 7 어댑터 방식 채택 — 서버리스 환경에서 안정적인 커넥션 풀 관리
             Stripe API version: 2026-04-22.dahlia (node_modules에서 직접 확인)
             NextAuth v5 JWT 타입 확장: next-auth 모듈 내부에만 선언 (next-auth/jwt 없음)
발견한 문제: 없음 (TypeScript 에러 0 달성 후 클린)
주의사항   : prisma.config.ts에 DATABASE_URL 설정됨 — schema.prisma에는 url 없음
             proxy.ts가 middleware.ts 대신 사용됨 — Next.js 16 breaking change
```

---

## 현재 진행 중인 작업

없음 — 기초 구축 완료, 다음 지시 대기

---

## 미완료 / 보류

| 우선순위 | 작업 |
|----------|------|
| 1 | Supabase 연결 + prisma migrate |
| 2 | 상품 상세 페이지 |
| 3 | 장바구니 UI |
| 4 | Stripe 결제 플로우 |
| 5 | 셀러 대시보드 + 이미지 업로드 |

---

## 중요 결정 이력

| 날짜 | 결정 내용 | 이유 |
|------|-----------|------|
| 2026-05-09 | Prisma 7 adapter-pg 방식 채택 | 서버리스 커넥션 풀 최적화 |
| 2026-05-09 | middleware.ts → proxy.ts | Next.js 16 breaking change |
| 2026-05-09 | 단일 MainPage + 7 테마 config 패턴 | V1의 6배 코드 중복 제거 |
