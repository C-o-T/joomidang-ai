# joomidang V2 작업 컨텍스트

> 이 파일은 joomidang V2 프로젝트 전용 작업 상태 파일이다.
> 세션 종료 시 반드시 업데이트한다.

---

## 마지막 업데이트

```
역할       : chief → developer 위임
작성 시각  : 2026-05-09
작업 내용  : 미구현 페이지 + API 라우트 전체 구현
완료       : - app/api/products/[id]/route.ts (단일 상품 조회 + 리뷰)
             - app/api/orders/route.ts (GET 주문목록 / POST 주문생성 + 재고감소 transaction)
             - app/(main)/products/page.tsx (상품 목록, 카테고리 필터, 검색, 페이지네이션)
             - app/(main)/products/[id]/page.tsx (상품 상세, locale별 이름/설명, 리뷰)
             - app/(main)/cart/page.tsx (장바구니, Zustand, 수량 조절)
             - app/(main)/checkout/page.tsx (배송지 폼, 국가→통화, POST /api/orders)
             - app/(main)/orders/page.tsx (주문 내역, auth redirect, 상태 배지)
             - components/products/CategoryFilter.tsx
             - components/products/SearchBar.tsx
             - components/products/AddToCartButton.tsx
             - next.config.ts — Supabase Storage 이미지 도메인 추가
             - TypeScript 에러 0 유지
미완료/보류: - Supabase 연결 (.env.local 실제 값 입력 필요)
             - npx prisma migrate dev --name init (DB 연결 후 실행)
             - Stripe 실제 결제 테스트 (Payment Intent 연동)
             - 셀러 대시보드 + 상품 등록 페이지
             - 관리자 페이지
             - 상품 이미지 업로드 (Supabase Storage)
중요 결정  : Prisma $transaction + include 타입 추론 제약 → transaction 내 재조회 패턴 사용
             Zustand persist hydration 이슈 → mounted state + useEffect 패턴으로 해결
             카테고리 enum: schema는 OTHER (CLAUDE.md의 ETC 아님)
발견한 문제: 없음
주의사항   : DB 연결 없이도 페이지는 빌드됨. 실제 데이터는 Supabase 연결 후 동작.
```

---

## 현재 진행 중인 작업

없음 — 다음 지시 대기

---

## 미완료 / 보류

| 우선순위 | 작업 |
|----------|------|
| 1 | Supabase .env.local 연결 + prisma migrate |
| 2 | Stripe Payment Intent 연동 (checkout 완성) |
| 3 | 셀러 대시보드 + 상품 등록 + 이미지 업로드 |
| 4 | 관리자 페이지 |

---

## 중요 결정 이력

| 날짜 | 결정 내용 | 이유 |
|------|-----------|------|
| 2026-05-09 | Prisma 7 adapter-pg 방식 채택 | 서버리스 커넥션 풀 최적화 |
| 2026-05-09 | middleware.ts → proxy.ts | Next.js 16 breaking change |
| 2026-05-09 | 단일 MainPage + 7 테마 config 패턴 | V1의 6배 코드 중복 제거 |
| 2026-05-09 | Zustand persist hydration → mounted 패턴 | SSR/CSR 불일치 방지 |
