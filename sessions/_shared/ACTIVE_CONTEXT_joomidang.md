# joomidang V2 작업 컨텍스트

---

## 마지막 업데이트

```
역할       : chief → developer 위임 / overseer 감시
작성 시각  : 2026-05-09
작업 내용  : 셀러 대시보드 + 관리자 패널 구현
완료       : [API] seller/register, seller/products, seller/products/[id]
             [API] admin/sellers, admin/sellers/[id], admin/products
             [페이지] seller/page, seller/register, seller/products, seller/products/new
             [페이지] seller/products/[id]/edit
             [페이지] admin/page, admin/sellers, admin/products
             [컴포넌트] SellerActionButton, ProductStatusButton
             TypeScript 에러 0 유지
미완료/보류: - Supabase 연결 (.env.local 실제 값 입력 필요)
             - npx prisma migrate dev --name init
             - Stripe Payment Intent 연동 (결제 완성)
             - 상품 이미지 업로드 (Supabase Storage, 현재는 URL 입력으로 대체)
중요 결정  : 상품 삭제 시 CartItem 선삭제 (FK 제약)
             셀러 정지 시 User.role → CONSUMER 강등
             클라이언트 버튼은 router.refresh() 패턴 (Server Component 재실행)
발견한 문제: overseer 경고 — STATE.md 갱신 지시 프롬프트 누락(MEDIUM) → 교정 완료
주의사항   : 새 상품은 status=PENDING → 관리자 ACTIVE 승인 필요
             이미지 업로드는 Supabase Storage 연결 후 별도 구현
```

---

## 현재 진행 중인 작업

없음 — 다음 지시 대기

---

## 미완료 / 보류

| 우선순위 | 작업 |
|----------|------|
| 1 | Supabase .env.local 연결 + prisma migrate |
| 2 | Stripe Payment Intent 연동 |
| 3 | 상품 이미지 업로드 (Supabase Storage) |

---

## 중요 결정 이력

| 날짜 | 결정 내용 | 이유 |
|------|-----------|------|
| 2026-05-09 | Prisma 7 adapter-pg 방식 채택 | 서버리스 커넥션 풀 최적화 |
| 2026-05-09 | middleware.ts → proxy.ts | Next.js 16 breaking change |
| 2026-05-09 | 단일 MainPage + 7 테마 config 패턴 | V1의 6배 코드 중복 제거 |
| 2026-05-09 | Zustand persist hydration → mounted 패턴 | SSR/CSR 불일치 방지 |
| 2026-05-09 | 셀러 정지 → User.role CONSUMER 강등 | 권한 누수 방지 |
