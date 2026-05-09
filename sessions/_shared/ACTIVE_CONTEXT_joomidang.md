# joomidang V2 작업 컨텍스트

---

## 마지막 업데이트

```
역할       : chief → developer/stability 세션 위임
작성 시각  : 2026-05-09
작업 내용  : Stripe Payment Intent 연동 + 전체 빌드 테스트
완료       : - @stripe/react-stripe-js 설치
             - app/api/stripe/payment-intent/route.ts 생성
             - checkout/page.tsx 2단계 결제 UI로 재작성 (배송지 → Stripe Elements)
             - npm run build 전체 빌드 통과 ✅ (32개 라우트 전부 정상)
             - TypeScript 에러 0 ✅
             - 페이지 13개 / API 12개 / proxy 1개 전부 구조 확인 ✅
             - SELLER/ADMIN 보안 패턴 전수 확인 ✅
미완료/보류: - .env.local 실제 값 입력 필요 (5개 모두 placeholder)
             - Supabase 연결 후 prisma migrate dev
             - Stripe 실제 결제 E2E 테스트 (키 설정 필요)
             - 상품 이미지 업로드 (Supabase Storage)
중요 결정  : KRW/JPY는 zero-decimal 통화 → Stripe amount 그대로 (× 100 없음)
             confirmPayment 후 Stripe가 return_url로 직접 redirect
             장바구니 clear()는 주문 생성 성공 직후 호출
발견한 문제: 없음 — 빌드 에러 0, TypeScript 에러 0
주의사항   : 런타임 동작은 Supabase + Stripe 실제 키 설정 후 가능
```

---

## 현재 진행 중인 작업

없음 — 환경변수 설정 대기 중

---

## 미완료 / 보류

| 우선순위 | 작업 |
|----------|------|
| 1 | .env.local 실제 값 설정 + prisma migrate |
| 2 | Stripe E2E 테스트 (stripe CLI webhook 포워딩) |
| 3 | 상품 이미지 업로드 (Supabase Storage) |

---

## 구현 완료 라우트 현황 (2026-05-09)

| 라우트 | 타입 | 상태 |
|--------|------|------|
| / | Dynamic (SSR) | ✅ |
| /login, /register | Static | ✅ |
| /products | Dynamic | ✅ |
| /products/[id] | Dynamic | ✅ |
| /cart, /checkout | Static | ✅ |
| /orders | Dynamic | ✅ |
| /seller/* (5개) | Dynamic/Static | ✅ |
| /admin/* (3개) | Dynamic | ✅ |
| /api/* (12개) | Dynamic | ✅ |
