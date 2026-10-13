# CLAUDE.md

이 파일은 이 저장소에서 작업하는 Claude Code를 위한 가이드입니다.
제품 요구사항 전체는 [PRD.md](./PRD.md)를, 원본 요구사항은 `docs/Profile_요구사항.txt`를 참고합니다.

## Project Overview

임재정(서울시립대학교 경영학부)의 **개인 포트폴리오 원페이지(One-page) 웹사이트**.
여러 하위 페이지 이동 없이 하나의 긴 페이지에서 스크롤로 모든 콘텐츠(소개 · 학력 · 활동 · 수상/경력 · 연락처)를 보여준다.
향후 Database 연동을 전제로 **FrontEnd / BackEnd를 분리**하여 개발하고, 둘 다 **Vercel**에 배포한다.

## Directory Structure

```
/
├── CLAUDE.md
├── PRD.md
├── docs/                       # 요구사항·디자인 원본 (수정하지 않음)
│   ├── Profile_요구사항.txt
│   ├── 프로필사진.png
│   └── Portfolio (Community).fig
├── frontend/                   # Next.js 앱 (Vercel 프로젝트 #1, Root Directory = frontend)
│   ├── src/app/                # App Router (page.tsx 하나에 모든 섹션 조합)
│   ├── src/components/sections/  # Hero, About, Education, Activities, Awards, Contact
│   ├── src/components/layout/    # Header(고정 네비), Footer
│   ├── src/lib/api.ts          # BackEnd API 호출 유틸
│   └── public/                 # 프로필 이미지 등 정적 자산
└── backend/                    # Hono API 서버 (Vercel 프로젝트 #2, Root Directory = backend)
    ├── api/                    # Vercel Functions 엔트리
    ├── src/routes/             # profile, activities, awards, contact
    ├── src/repositories/       # 데이터 접근 계층 (정적 데이터 → 추후 DB로 교체)
    └── src/data/               # 초기 정적 데이터 (TS)
```

## Tech Stack

| 영역 | 기술 |
|------|------|
| FrontEnd | Next.js (App Router), React, TypeScript, Tailwind CSS |
| BackEnd | Node.js, TypeScript, Hono (Vercel Functions) |
| Database (향후) | Neon Postgres (Vercel Marketplace) + Drizzle ORM |
| 배포 | Vercel (FE/BE 각각 별도 프로젝트) |
| 패키지 매니저 | npm |

### 반드시 지킬 제약
- **Java 계열 기술(Java, Spring, JSP 등) 사용 금지.**
- **순수 정적 HTML 단일 파일로 개발 금지** — DB 연동이 가능한 FE/BE 구조를 유지한다.
- **원페이지 구성 유지** — 새로운 라우트(하위 페이지)를 만들지 말고 섹션 + 앵커(`#about` 등)로 이동한다.
- 모든 기술은 Vercel 배포가 가능해야 한다.

## Commands

각 폴더에서 실행한다.

```bash
# frontend (http://localhost:3000)
cd frontend && npm install
npm run dev
npm run build
npm run lint

# backend (http://localhost:4000)
cd backend && npm install
npm run dev
npm run build
```

## Coding Conventions

- TypeScript `strict` 모드 사용, `any` 지양.
- 섹션 컴포넌트는 `src/components/sections/` 아래 섹션 단위로 분리한다 (`Hero.tsx`, `About.tsx`, `Education.tsx`, `Activities.tsx`, `Awards.tsx`, `Contact.tsx`).
- **프로필 콘텐츠(이름, 소개, 활동, 수상 등)는 FrontEnd에 하드코딩하지 않는다.** BackEnd API에서 받아와 렌더링한다 (DB 전환 대비).
- BackEnd는 `routes → repositories → data(또는 DB)` 계층을 지킨다. DB 도입 시 repository 구현만 교체한다.
- FE/BE 간 응답 타입은 PRD.md의 API 명세와 일치시킨다.
- 스타일은 Tailwind 유틸리티 클래스 사용, 디자인 토큰(색상·폰트)은 `tailwind` 설정/CSS 변수로 관리한다.
- 반응형(모바일 우선)과 접근성(시맨틱 태그, `alt`, 키보드 포커스)을 기본으로 한다.
- UI 텍스트는 한국어.

## Design Reference

- Figma: https://www.figma.com/design/isMqGYYV6k9IWap1589ORk/Portfolio--Community-?node-id=0-1&p=f&t=47WdNGMriZQpG8xD-0
- 로컬 Figma 파일: `docs/Portfolio (Community).fig`
- 프로필 이미지: `docs/프로필사진.png` → `frontend/public/profile.png`로 복사하여 사용 (`next/image`)
- 레이아웃: 상단 고정 네비게이션 + 세로로 이어지는 섹션들의 원페이지 스크롤 방식

## Environment Variables

| 위치 | 변수 | 설명 |
|------|------|------|
| frontend | `NEXT_PUBLIC_API_BASE_URL` | BackEnd API 주소 (로컬: `http://localhost:4000`) |
| backend | `CORS_ORIGIN` | 허용할 FrontEnd 도메인 |
| backend | `DATABASE_URL` | (향후) Neon Postgres 연결 문자열 |

`.env*` 파일은 커밋하지 않는다. Vercel 환경변수는 `vercel env`로 관리한다.

## Deployment

> **현재(임시)**: 루트 `vercel.json` + `public/index.html`(= `docs/임재정 포트폴리오.html` 디자인 시안 사본)을 정적 사이트로 배포 중.
> `frontend/`·`backend/` 구현 후에는 이 임시 설정(`vercel.json`, `public/`)을 제거하고 아래 구성으로 전환한다.

- Vercel 프로젝트 2개: `frontend`(Root Directory: `frontend`), `backend`(Root Directory: `backend`).
- BackEnd 먼저 배포 → URL을 FrontEnd의 `NEXT_PUBLIC_API_BASE_URL`에 설정 → FrontEnd 배포.
- BackEnd `CORS_ORIGIN`에 FrontEnd 배포 도메인을 등록한다.

## Privacy Note

전화번호·이메일·학번 등 개인정보가 포함된다. 요구사항 파일의 값을 그대로 사용하되,
공개 범위(특히 전화번호·학번 노출 여부)는 PRD.md의 미결 사항을 확인한 뒤 결정한다.
