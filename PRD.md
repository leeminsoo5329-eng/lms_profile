# PRD: 임재정 개인 포트폴리오 웹사이트

| 항목 | 내용 |
|------|------|
| 문서 버전 | v1.0 |
| 작성일 | 2026-10-13 |
| 원본 요구사항 | `docs/Profile_요구사항.txt` |
| 형태 | 원페이지(One-page) 반응형 웹사이트 |

---

## 1. 개요

### 1.1 목적
서울시립대학교 경영학부 임재정의 경력·활동·역량을 한눈에 보여주는 **개인 포트폴리오 사이트**를 구축한다.
여러 하위 페이지 이동 없이 **하나의 긴 페이지에서 스크롤**하며 모든 콘텐츠를 확인할 수 있어야 한다.

### 1.2 대상 사용자
- 채용 담당자 / 인턴십 · 대외활동 심사자
- 동아리 · 학회 · 창업 프로젝트 관계자
- 동문 및 네트워킹 대상자

### 1.3 성공 기준
- 방문자가 30초 안에 "누구인지 / 무엇을 했는지 / 어떻게 연락하는지" 파악할 수 있다.
- 모바일·데스크톱 모두에서 레이아웃이 깨지지 않는다.
- 콘텐츠가 BackEnd API로 제공되어, 향후 DB 연동 시 FrontEnd 수정이 최소화된다.

---

## 2. 프로필 콘텐츠 (요구사항 원문 기준)

### 2.1 개인정보
| 항목 | 값 |
|------|----|
| 이름 | 임재정 |
| 학교 | 서울시립대학교 |
| 전공 | 경영학부 |
| 학번 | 199526000 |
| Tel | 010-0000-0000 |
| E-Mail | limjj157@naver.com |

### 2.2 자기소개
> 안녕하세요!
> 서울시립대학교에서 경영학을 전공 중인 95학번 임재정 입니다.
> 데이터 분석과 MIS에 관심이 많아 시경연에서 2기 회장으로 활발히 활동하고 있습니다.
> 긍정적인 에너지와 논리적 사고로 팀의 성장을 돕는 인재입니다!!

### 2.3 학교 활동내역
1. 시립대경영정보연구회 2기 회장
2. 중앙 동아리 연극반 10기 연출 담당
3. 경영학부 학생회 부회장
4. 경영학부 사람사랑 소모임 창립멤버

### 2.4 주요 수상 및 외부 경력
| 기간 | 내용 | 구분 |
|------|------|------|
| 2025.01 ~ 2025.07 | (주)원스텝 인턴 | 경력 |
| 2025.08 | 2025 KOICA 글로벌 서포터즈 참여 | 대외활동 |
| 2026.07 | 2026 서울시립대학교 해커톤 경진대회 3위 입상 | 수상 |
| 2026.09 | 대한민국 모두의 창업 2기 프로젝트 지원 | 프로젝트 |

### 2.5 이미지 · 디자인 자산
- 프로필 사진: `docs/프로필사진.png`
- Figma 파일: `docs/Portfolio (Community).fig`
- Figma 링크: https://www.figma.com/design/isMqGYYV6k9IWap1589ORk/Portfolio--Community-?node-id=0-1&p=f&t=47WdNGMriZQpG8xD-0

---

## 3. 페이지 구성 (원페이지 섹션)

상단에 **고정 네비게이션 바**를 두고, 메뉴 클릭 시 해당 섹션으로 부드럽게 스크롤한다.

| 순서 | 섹션 (앵커) | 내용 |
|------|-------------|------|
| 0 | Header (고정) | 이름/로고, 메뉴: About · Education · Activities · Experience · Contact |
| 1 | Hero (`#home`) | 프로필 사진, 이름 "임재정", 한 줄 소개(예: "데이터 분석과 MIS에 관심 있는 경영학도"), CTA 버튼(연락하기) |
| 2 | About (`#about`) | 자기소개 원문, 관심 분야 키워드(데이터 분석, MIS, 리더십) |
| 3 | Education (`#education`) | 서울시립대학교 · 경영학부 · 학번 |
| 4 | Activities (`#activities`) | 학교 활동 4건을 카드 형태로 표시 (역할 강조) |
| 5 | Experience & Awards (`#experience`) | 수상/경력 4건을 날짜순 **타임라인** 형태로 표시, 구분 배지(경력/수상/대외활동/프로젝트) |
| 6 | Contact (`#contact`) | Tel, E-Mail(`mailto:` 링크), (2단계) 연락 폼 |
| 7 | Footer | © 2026 임재정, 이메일 링크 |

세부 시각 스타일(색상, 타이포그래피, 간격, 섹션 레이아웃)은 Figma 디자인을 따른다.

---

## 4. 기능 요구사항 (FR)

| ID | 요구사항 | 우선순위 |
|----|----------|----------|
| FR-01 | 모든 콘텐츠를 단일 페이지(`/`)에 섹션으로 표시한다. 하위 라우트를 만들지 않는다. | 필수 |
| FR-02 | 고정 네비게이션 메뉴 클릭 시 해당 섹션으로 스무스 스크롤한다. | 필수 |
| FR-03 | 스크롤 위치에 따라 현재 섹션 메뉴를 하이라이트한다. | 권장 |
| FR-04 | 모바일에서는 햄버거 메뉴로 네비게이션을 제공한다. | 필수 |
| FR-05 | 프로필·활동·수상 데이터를 BackEnd API에서 받아 렌더링한다. | 필수 |
| FR-06 | E-Mail 클릭 시 메일 클라이언트(`mailto:`)를 연다. 모바일에서 Tel 클릭 시 전화(`tel:`)를 연다. | 필수 |
| FR-07 | 섹션 진입 시 페이드인 등 가벼운 스크롤 애니메이션을 적용한다. | 권장 |
| FR-08 | "맨 위로" 버튼을 제공한다. | 선택 |
| FR-09 | 연락 폼(이름·이메일·메시지) 제출 → `POST /api/contact` 저장. (DB 연동 단계에서 구현) | 2단계 |
| FR-10 | API 실패 시 사용자에게 오류 상태를 표시하고 페이지 전체가 깨지지 않게 한다. | 필수 |

---

## 5. 비기능 요구사항 (NFR)

| 구분 | 요구사항 |
|------|----------|
| 성능 | Lighthouse Performance 90 이상, 이미지 `next/image` 최적화 |
| 반응형 | 모바일(≥360px) · 태블릿 · 데스크톱 대응, 가로 스크롤 없음 |
| 접근성 | 시맨틱 태그, 이미지 `alt`, 키보드 탐색, 명도 대비 WCAG AA |
| SEO | `title`, `description`, Open Graph 메타 태그 |
| 유지보수 | TypeScript strict, 데이터 접근 계층 분리 |
| 보안 | BackEnd CORS로 FrontEnd 도메인만 허용, 비밀값은 환경변수로 관리 |
| 기술 제약 | Java 계열 사용 금지, 순수 정적 HTML 금지, Vercel 배포 가능 스택 |

---

## 6. 시스템 아키텍처

```
[Browser]
    │
    ▼
[FrontEnd: Next.js on Vercel]  ── fetch ──▶  [BackEnd: Hono API on Vercel Functions]
                                                     │
                                                     ▼
                                       [1단계: 정적 TS 데이터]
                                       [2단계: Neon Postgres + Drizzle ORM]
```

- **FrontEnd**: Next.js (App Router) + TypeScript + Tailwind CSS. 서버 컴포넌트에서 API 호출 후 렌더링(ISR/캐시 활용).
- **BackEnd**: Node.js + TypeScript + Hono, Vercel Functions로 배포. `routes → repositories → data` 계층 구조.
- **DB (향후)**: Neon Postgres (Vercel Marketplace). repository 구현만 교체하여 전환.

### 6.1 API 명세

Base URL: `NEXT_PUBLIC_API_BASE_URL`

| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/api/profile` | 기본 정보 + 자기소개 |
| GET | `/api/activities` | 학교 활동 목록 |
| GET | `/api/awards` | 수상 및 외부 경력 목록 |
| POST | `/api/contact` | 연락 메시지 저장 (2단계) |
| GET | `/api/health` | 헬스 체크 |

**GET `/api/profile`**
```json
{
  "name": "임재정",
  "school": "서울시립대학교",
  "major": "경영학부",
  "studentId": "199526000",
  "phone": "010-0000-0000",
  "email": "limjj157@naver.com",
  "headline": "데이터 분석과 MIS에 관심 있는 경영학도",
  "introduction": "안녕하세요! 서울시립대학교에서 경영학을 전공 중인 ...",
  "profileImage": "/profile.png"
}
```

**GET `/api/activities`**
```json
[
  { "id": 1, "organization": "시립대경영정보연구회", "role": "2기 회장", "order": 1 },
  { "id": 2, "organization": "중앙 동아리 연극반", "role": "10기 연출 담당", "order": 2 },
  { "id": 3, "organization": "경영학부 학생회", "role": "부회장", "order": 3 },
  { "id": 4, "organization": "경영학부 사람사랑 소모임", "role": "창립멤버", "order": 4 }
]
```

**GET `/api/awards`**
```json
[
  { "id": 1, "title": "(주)원스텝 인턴", "category": "경력", "startDate": "2025-01", "endDate": "2025-07" },
  { "id": 2, "title": "2025 KOICA 글로벌 서포터즈 참여", "category": "대외활동", "startDate": "2025-08", "endDate": null },
  { "id": 3, "title": "2026 서울시립대학교 해커톤 경진대회 3위 입상", "category": "수상", "startDate": "2026-07", "endDate": null },
  { "id": 4, "title": "대한민국 모두의 창업 2기 프로젝트 지원", "category": "프로젝트", "startDate": "2026-09", "endDate": null }
]
```

**POST `/api/contact`** (2단계)
```json
// Request
{ "name": "홍길동", "email": "hong@example.com", "message": "안녕하세요..." }
// Response 201
{ "id": 1, "createdAt": "2026-10-13T00:00:00Z" }
```

---

## 7. 향후 DB 스키마 초안

| 테이블 | 주요 컬럼 |
|--------|-----------|
| `profile` | id, name, school, major, student_id, phone, email, headline, introduction, profile_image |
| `activities` | id, organization, role, display_order |
| `awards` | id, title, category, start_date, end_date, display_order |
| `contact_messages` | id, name, email, message, created_at |

---

## 8. 배포 계획

| 대상 | Vercel 프로젝트 | Root Directory | 환경변수 |
|------|----------------|----------------|----------|
| FrontEnd | portfolio-frontend | `frontend` | `NEXT_PUBLIC_API_BASE_URL` |
| BackEnd | portfolio-backend | `backend` | `CORS_ORIGIN`, (향후) `DATABASE_URL` |

배포 순서: BackEnd 배포 → API URL을 FrontEnd 환경변수에 설정 → FrontEnd 배포 → BackEnd `CORS_ORIGIN`에 FrontEnd 도메인 등록.

---

## 9. 마일스톤

| 단계 | 내용 | 산출물 |
|------|------|--------|
| M1 | 프로젝트 스캐폴딩 (frontend/backend 초기화, 린트/TS 설정) | 로컬 실행 가능한 빈 FE/BE |
| M2 | 섹션 UI 구현 (Figma 반영, 반응형, 네비게이션) | 목업 데이터로 완성된 원페이지 |
| M3 | BackEnd API 구현 및 FE 연동 | API 기반 데이터 렌더링 |
| M4 | Vercel 배포 (FE/BE) | 운영 URL |
| M5 | DB 연동 (Neon Postgres) 및 연락 폼 | DB 기반 데이터 + 연락 메시지 저장 |

---

## 10. 범위 외 및 미결 사항

### 범위 외 (현 단계)
- 관리자(Admin) 페이지 및 콘텐츠 편집 기능
- 다국어(영문) 지원
- 로그인 / 회원 기능

### 미결 사항
- [ ] Figma 디자인의 세부 스펙(색상 팔레트, 폰트, 섹션 레이아웃) 확인 및 반영 범위 확정
- [ ] 전화번호·학번의 공개 여부 (개인정보 노출 범위)
- [ ] Hero 한 줄 소개 문구 확정
- [ ] 활동/경력 항목별 상세 설명(기간, 성과) 추가 여부
- [ ] 다크모드 지원 여부
- [ ] 커스텀 도메인 사용 여부
