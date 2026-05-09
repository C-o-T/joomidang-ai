# joomidang V2 프로젝트 컨텍스트

> joomidang-ai 레포 전용 — 주미당 V2 기술 컨텍스트

---

## 프로젝트 개요

**목적**: 한국 전통주(막걸리·청주·소주·약주) 역직구 B2C 글로벌 이커머스 플랫폼
**핵심 차별점**: IP 기반 국가 감지 → 6개국 UX 테마 자동 전환
**로컬 경로**: `C:\Users\dntmd\OneDrive\바탕 화면\joomidang-v2\`

---

## 기술 스택

| 영역 | 기술 | 비고 |
|------|------|------|
| Framework | Next.js 16 (App Router) + TypeScript | Full-stack SSR |
| DB 호스팅 | Supabase (PostgreSQL) | 무료 시작 |
| ORM | Prisma 7 | adapter-pg 방식 |
| 인증 | NextAuth v5 (beta) | JWT 세션, 3역할 |
| 상태관리 | Zustand 5 | 장바구니·클라이언트 상태 |
| 결제 | Stripe | API v2026-04-22.dahlia |
| CSS | Tailwind CSS v4 | |
| 파일 스토리지 | Supabase Storage | 상품 이미지 |
| 배포 | Vercel | Geo 감지 내장 |

---

## Next.js 16 + Prisma 7 핵심 변경사항 (반드시 숙지)

| 항목 | 변경 내용 |
|------|-----------|
| `middleware.ts` | → `proxy.ts` (함수명도 `proxy`) |
| `request.geo`, `request.ip` | 완전 제거 → `x-vercel-ip-country` 헤더 사용 |
| `headers()`, `cookies()`, `params`, `searchParams` | 모두 async 필수 (`await headers()`) |
| Turbopack | 기본값, webpack config 사용 불가 |
| Prisma datasource `url` | schema.prisma에서 제거 → `prisma.config.ts`로 이동 |
| `PrismaClient` | `@prisma/adapter-pg` + `pg` Pool 어댑터 주입 필수 |
| Stripe API version | `"2026-04-22.dahlia"` |
| NextAuth v5 JWT 확장 | `declare module "next-auth"` 내부에만 선언 (next-auth/jwt 없음) |

---

## 프로젝트 구조

```
joomidang-v2/
├── app/
│   ├── (auth)/login/, register/
│   ├── (main)/products/, cart/
│   ├── (seller)/seller/
│   ├── (admin)/admin/
│   ├── api/
│   │   ├── auth/[...nextauth]/
│   │   ├── auth/register/
│   │   ├── geo/
│   │   ├── products/
│   │   ├── cart/
│   │   ├── orders/
│   │   └── webhooks/stripe/
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   ├── common/   Navbar, AgeGate, HeroBanner
│   ├── theme/    KR.ts JP.ts CN.ts US.ts EU.ts SEA.ts Default.ts index.ts
│   ├── products/ ProductGrid
│   ├── cart/
│   └── ui/
├── lib/
│   ├── prisma.ts    (Pool + PrismaPg 어댑터)
│   ├── geo.ts       (countryToTheme / fetchCountryCode)
│   └── stripe.ts
├── prisma/schema.prisma   (url 없음 — prisma.config.ts 사용)
├── prisma.config.ts       (defineConfig + datasource.url)
├── store/cartStore.ts     (Zustand persist → "jmd-cart")
├── types/index.ts         (ok()/fail() 헬퍼 + DTO 타입)
├── auth.ts                (NextAuth v5 설정)
└── proxy.ts               (middleware 대체 + Geo 헤더 주입)
```

---

## 테마 플로우

```
요청 → proxy.ts: x-vercel-ip-country → x-country-code 헤더
     → page.tsx (Server): await headers() → countryToTheme() → getTheme()
     → 컴포넌트에 theme prop 전달
```

| 국가 | 테마 | 레퍼런스 스타일 |
|------|------|----------------|
| KR | KR | 쿠팡 |
| JP | JP | 라쿠텐 |
| CN/HK/TW | CN | 타오바오 |
| US/CA | US | 아마존 |
| EU | EU | 잘란도 |
| SEA | SEA | 쇼피 |
| 기타 | DEFAULT | 다크 미니멀 |

---

## 코딩 컨벤션

```typescript
// API 응답
import { ok, fail } from "@/types";
return NextResponse.json(ok(data));
return NextResponse.json(fail("오류"), { status: 400 });

// 인증 체크
const session = await auth();
if (!session) return NextResponse.json(fail("로그인 필요"), { status: 401 });

// 금액: DB Decimal → JS Number(p.priceKrw)
```

---

## DB 스키마 개요

**모델**: User, Seller, Product, Cart, CartItem, Order, OrderItem, Review
**NextAuth 모델**: Account, Session, VerificationToken
**Enum**: Role (CONSUMER/SELLER/ADMIN), SellerStatus, Category, ProductStatus, OrderStatus

---

## 구현 현황 (2026-05-09 기준)

| 항목 | 상태 |
|------|------|
| Next.js 16 프로젝트 초기화 | 완료 |
| Prisma 7 스키마 + 어댑터 설정 | 완료 (prisma generate 성공) |
| NextAuth v5 + JWT 세션 | 완료 |
| 6개국 테마 시스템 | 완료 |
| Geo 감지 (proxy.ts + /api/geo) | 완료 |
| Zustand 장바구니 스토어 | 완료 |
| API 라우트 (products/cart/orders/webhooks) | 완료 |
| 인증 페이지 (login/register) | 완료 |
| 메인 홈 (Server Component + 테마) | 완료 |
| TypeScript 에러 0개 | 달성 |
| Supabase 프로젝트 연결 | **미완료** |
| `prisma migrate dev` 실행 | **미완료** |
| 상품 상세 페이지 | **미완료** |
| 장바구니 UI | **미완료** |
| Stripe 결제 플로우 | **미완료** |
| 셀러 대시보드 | **미완료** |
| 상품 이미지 업로드 | **미완료** |

---

## 환경변수 (필수)

```
DATABASE_URL=          # Supabase PostgreSQL 연결 문자열
NEXTAUTH_SECRET=       # openssl rand -base64 32
NEXTAUTH_URL=          # http://localhost:3000 (dev)
STRIPE_SECRET_KEY=     # sk_test_...
STRIPE_WEBHOOK_SECRET= # whsec_...
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY= # pk_test_...
```

---

## 업데이트 이력

| 날짜 | 내용 |
|------|------|
| 2026-05-09 | V2 기술 컨텍스트 최초 작성 (V1 Spring Boot 내용 교체) |
