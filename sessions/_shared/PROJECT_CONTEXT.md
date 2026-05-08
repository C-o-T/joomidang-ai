# 프로젝트 컨텍스트

> 이 파일은 워크스페이스 내 모든 프로젝트의 기술 컨텍스트를 담는다.
> 새 프로젝트 시작 시 chief가 업데이트한다.

---

## 워크스페이스 구조

```
C:\Users\wptmd\Desktop\joomidang\
  ├── joomidang-platform\   ← 역직구 전통주 이커머스 플랫폼 (메인)
  ├── joomidang-v2\         ← v2 개발 중
  ├── autotrader\           ← 자동매매 시스템
  ├── dataverse\            ← AI 에이전트 데이터 수집·가공 인프라
  ├── news-tracker\         ← 뉴스 대시보드 + 실시간 주가
  └── sessions\             ← AI 세션 파일 (이 폴더)
```

---

## 프로젝트 1: joomidang-platform (메인)

**목적**: 한국 전통주(막걸리·청주·소주·약주) 역직구 B2C 이커머스 플랫폼

### 기술 스택

| 영역 | 기술 |
|------|------|
| Backend | Spring Boot, Java 21, MyBatis, MariaDB |
| Frontend | React 18 + Vite, React Router v6 |
| 인증 | BCrypt (at.favre.lib) |
| 유틸 | Lombok (@Data, @RequiredArgsConstructor) |

### 포트 및 접속

| 서비스 | 포트 |
|--------|------|
| Backend API | 8080 |
| Frontend (dev) | 5173 / 5174 |
| MariaDB | 3306 (DB: joomidang, user: root, pw: mariadb) |

### Backend 아키텍처

```
Controller → Service → Mapper(인터페이스) → MyBatis XML → MariaDB
```

- CORS: localhost:5173, 5174, 3000 허용
- Soft Delete: `is_deleted` 컬럼
- 에러 처리: try-catch + e.printStackTrace() + 500 + 한국어 메시지
- 응답 규칙: POST=201, GET=객체직접, PUT/DELETE=200

### 패키지 구조

```
com.joomidang.backend.{모듈}.{controller|service|mapper|dto}
```

### 주요 REST API

| 모듈 | 경로 |
|------|------|
| 사용자 | `/users`, `/users/login`, `/users/check-email` |
| 상품 | `/products`, `/products/{id}`, `/products/seller/{id}` |
| 장바구니 | `/cart`, `/cart/user/{userId}` |
| 주문 | `/orders`, `/orders/{id}/status` |
| 판매자 | `/sellers`, `/sellers/user/{userId}` |

### Frontend 라우트

```
/           → MainPage (상품 목록)
/login      → LoginPage
/join       → JoinPage
/products/:id → ProductDetailPage
/cart       → CartPage
/order      → OrderPage
/export-guide → ExportGuidePage (SELLER 전용)
```

### 다국어 지원

- `translations.js`: ko/en/ja/zh 100+ 번역 키
- `GeoContext`: IP → 국가 → 언어 자동 감지
- 헬퍼: `getProductName(product, lang)`, `t('key')`

### 주요 비즈니스 로직

- 주문: @Transactional, 재고 감소 실패 시 전체 롤백
- 장바구니: upsert (동일 상품 → 수량 증가, 없으면 insert)
- 로그인: BCrypt 검증, 응답 시 password 필드 null 처리

### DTO 주요 필드 (camelCase ↔ DB snake_case)

| DTO | 주요 필드 |
|-----|-----------|
| UserDTO | id, email, password, name, role(CONSUMER\|SELLER\|ADMIN), country |
| ProductDTO | id, sellerId, nameKo/En/Ja, category, price(price_krw), stock, status |
| OrderDTO | id, userId, totalPrice, currency, shippingCountry, status |
| SellerDTO | id, userId, breweryName, businessNumber, status(PENDING\|APPROVED\|...) |

---

## 프로젝트 2: autotrader

**목적**: 자동매매 시스템

> 상세 컨텍스트: `sessions/_shared/PROJECT_CONTEXT_autotrader.md` (작성 필요)

---

## 프로젝트 3: dataverse

**목적**: 범용 AI 에이전트용 데이터 수집·가공 인프라

> 상세 컨텍스트: `sessions/_shared/PROJECT_CONTEXT_dataverse.md` (작성 필요)

---

## 프로젝트 4: news-tracker

**목적**: 뉴스 대시보드 + 실시간 주가 모니터링

> 상세 컨텍스트: 필요 시 chief가 추가

---

## 업데이트 이력

| 날짜 | 내용 |
|------|------|
| 2026-05-08 | 최초 작성 — joomidang-platform 기술 컨텍스트 |
