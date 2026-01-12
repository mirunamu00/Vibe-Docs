# VibeDocs - GitHub 기반 인터랙티브 문서화 프레임워크

> 🎯 **한 줄 요약**: 아름답고 편집 가능한 문서 사이트를 만드는 프레임워크. 설정과 콘텐츠만 관리하면 됩니다.

---

## 1. Purpose (프로젝트 목적)

### 1.1 Problem Statement

현재 문서화 솔루션들의 한계:

| 문제점 | 기존 솔루션 |
|--------|------------|
| **밋밋한 디자인** | 기능 중심, 시각적 차별화 부족 |
| **읽기 전용** | 편집하려면 Git 클론 → 수정 → Push |
| **개발자 중심** | 비개발자 기여 진입장벽 높음 |

### 1.2 Solution

**VibeDocs**는 프레임워크로서:

- 사용자는 `docs/` 폴더와 `vibe.config.ts` 설정만 관리
- 내부 구현(컴포넌트, 라우팅, 빌드)은 프레임워크가 처리
- 테마 커스터마이징은 설정과 CSS 변수로 제공

```
사용자가 관리하는 것:
├── docs/           ← 마크다운 콘텐츠
├── public/         ← 정적 에셋 (로고, 이미지)
└── vibe.config.ts  ← 설정 파일

프레임워크가 처리하는 것:
├── 컴포넌트 (Header, Sidebar, DocPage...)
├── 라우팅
├── 빌드 시스템
├── GitHub OAuth
├── CRUD API
└── 애니메이션
```

### 1.3 Target Users

| 사용자 유형 | 니즈 | VibeDocs 제공 가치 |
|------------|------|-------------------|
| **오픈소스 메인테이너** | 예쁘고 관리 쉬운 문서 | 설정만으로 완성 |
| **스타트업** | 빠른 문서 구축 | CLI 한 줄로 시작 |
| **비개발자 팀원** | Git 없이 문서 기여 | 웹에서 직접 편집 |

### 1.4 Success Metrics

| 지표 | 목표 (MVP) |
|------|-----------|
| GitHub Stars | 500+ |
| npm 주간 다운로드 | 1,000+ |
| Lighthouse Performance | 90+ |

---

## 2. Core Value Proposition

### 2.1 경쟁사 비교

| 기능 | Docusaurus | GitBook | VibeDocs |
|------|------------|---------|----------|
| 정적 사이트 | ✅ | ❌ | ✅ |
| 웹 편집기 | ❌ | ✅ (유료) | ✅ (무료) |
| 모던 디자인 | 보통 | 보통 | ✨ 핵심 강점 |
| 무료 | ✅ | 제한적 | ✅ |
| Git 히스토리 | ✅ | ❌ | ✅ |

### 2.2 핵심 차별화

1. **Designful** - 모던 UI, 부드러운 애니메이션
2. **Editable** - 브라우저에서 바로 CRUD
3. **Zero Config** - 설정 파일 하나로 완성
4. **Hybrid** - 읽기는 정적(빠름), 쓰기는 동적(편리함)

---

## 3. User Experience

### 3.1 Quick Start

```bash
# 1. 프로젝트 생성
npx create-vibe-docs my-docs

# 2. 개발 서버 실행
cd my-docs
npm run dev

# 3. 배포
npm run build
```

### 3.2 사용자가 관리하는 파일 구조

```
my-docs/
├── docs/                    ← 마크다운 문서
│   ├── index.md
│   ├── getting-started.md
│   └── guides/
│       ├── installation.md
│       └── configuration.md
├── public/                  ← 정적 에셋
│   ├── logo.svg
│   └── og-image.png
├── vibe.config.ts           ← 설정 파일
└── package.json
```

### 3.3 Configuration

```typescript
// vibe.config.ts
import { defineConfig } from 'vibe-docs';

export default defineConfig({
  // 기본 설정
  title: 'My Documentation',
  description: 'Welcome to my docs',

  // GitHub 연동 (편집 기능)
  github: {
    repo: 'username/my-docs',
    branch: 'main',
    editEnabled: true,           // 웹 편집 활성화
    admins: ['username'],        // 편집 권한자
  },

  // 테마
  theme: {
    colorScheme: 'auto',         // 'light' | 'dark' | 'auto'
    primaryColor: 'blue',        // 프리셋 컬러
    accentColor: 'violet',
  },

  // 네비게이션
  nav: [
    { title: 'Docs', href: '/docs' },
    { title: 'GitHub', href: 'https://github.com/...' },
  ],

  // 기능 토글
  features: {
    search: true,
    tableOfContents: true,
    editButton: true,
    lastUpdated: true,
  },
});
```

### 3.4 Frontmatter

```markdown
---
title: Getting Started
description: Learn how to set up VibeDocs
order: 1
---

# Getting Started

Your content here...
```

### 3.5 커스터마이징 레벨

| 레벨 | 방법 | 자유도 |
|------|------|--------|
| **L1. 설정** | `vibe.config.ts` 수정 | 낮음 |
| **L2. 스타일** | CSS 변수 오버라이드 | 중간 |
| **L3. 슬롯** | 특정 영역에 커스텀 컴포넌트 삽입 | 중간 |
| **L4. 테마** | 커뮤니티 테마 적용 | 중간 |
| **L5. Eject** | 전체 소스 추출 후 직접 수정 | 높음 |

#### L2. CSS 변수 오버라이드

```css
/* public/custom.css */
:root {
  --vibe-primary: #3b82f6;
  --vibe-background: #fafafa;
  --vibe-sidebar-width: 280px;
  --vibe-font-sans: 'Pretendard', sans-serif;
}
```

#### L3. 슬롯 시스템

```typescript
// vibe.config.ts
export default defineConfig({
  slots: {
    // 헤더 우측에 커스텀 버튼 추가
    'header-right': './components/CustomButton.tsx',

    // 문서 하단에 피드백 위젯
    'doc-footer': './components/FeedbackWidget.tsx',

    // 사이드바 상단에 로고
    'sidebar-top': './components/Logo.tsx',
  },
});
```

---

## 4. Technical Architecture

### 4.1 하이브리드 아키텍처

```
┌─────────────────────────────────────────────────────────┐
│                    VibeDocs Architecture                 │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  [일반 방문자]                [Admin]                    │
│       │                          │                       │
│       ▼                          ▼                       │
│  정적 파일 (CDN)           GitHub OAuth                  │
│       │                          │                       │
│       ▼                          ▼                       │
│  docs-data.json             GitHub API                   │
│  (빌드 시 생성)             (실시간 CRUD)                │
│       │                          │                       │
│       └──────────┬───────────────┘                       │
│                  ▼                                       │
│           VibeDocs Runtime                               │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### 4.2 패키지 구조

```
@vibe-docs/
├── create-vibe-docs     # CLI (npx create-vibe-docs)
├── vibe-docs            # 코어 프레임워크
├── @vibe-docs/theme-default   # 기본 테마
└── @vibe-docs/plugin-*        # 공식 플러그인
```

### 4.3 Tech Stack

| 영역 | 기술 | 이유 |
|------|------|------|
| Framework | React + Vite | 빠른 빌드, 풍부한 생태계 |
| Styling | Tailwind CSS | 테마 시스템과 호환 |
| Animation | Framer Motion | 선언적 애니메이션 |
| Markdown | unified ecosystem | 확장성 |
| Auth | GitHub OAuth | 타겟 사용자 친화적 |
| Hosting | Vercel/Netlify | 무료, 간편 배포 |

### 4.4 Data Flow

#### Read (모든 사용자)
```
빌드: docs/*.md → docs-data.json
런타임: CDN → docs-data.json → 렌더링
✅ API 호출 없음, 초고속
```

#### Write (Admin)
```
편집 → GitHub API → 커밋 생성 → Actions 트리거 → 재배포
```

---

## 5. Milestones

### Phase 0: Foundation
**목표**: 프로젝트 기반 구축

- 모노레포 구조 (pnpm workspace)
- `create-vibe-docs` CLI 스캐폴딩
- `vibe-docs` 코어 패키지 구조
- TypeScript + ESLint + Prettier 설정

**산출물**: `npx create-vibe-docs my-docs` 실행 가능

---

### Phase 1: Static Docs
**목표**: 정적 문서 사이트 생성

- Markdown → HTML 빌드 파이프라인
- 기본 레이아웃 (Header, Sidebar, Content)
- 파일 기반 라우팅
- Frontmatter 파싱
- 자동 사이드바 생성
- 기본 테마

**산출물**: 읽기 전용 문서 사이트

---

### Phase 2: Design System
**목표**: 차별화된 디자인

- CSS 변수 기반 테마 시스템
- 다크모드
- 페이지 전환 애니메이션
- 코드 블록 스타일링
- 반응형 디자인
- 타이포그래피

**산출물**: 아름다운 문서 사이트

---

### Phase 3: GitHub Integration
**목표**: 인증 및 CRUD

- GitHub OAuth 플로우
- Admin 권한 체크
- 문서 Create/Update/Delete
- 커밋 메시지 입력
- 배포 상태 표시

**산출물**: 브라우저에서 문서 편집 가능

---

### Phase 4: Editor
**목표**: 편집 경험

- 마크다운 에디터 (CodeMirror)
- 실시간 프리뷰
- 툴바 (Bold, Link, Image...)
- 이미지 업로드
- 자동 저장

**산출물**: 생산적인 편집 환경

---

### Phase 5: Features
**목표**: 고급 기능

- 검색 (Cmd+K)
- Table of Contents
- 슬롯 시스템
- 플러그인 API
- 다국어 (i18n)

**산출물**: 완성도 높은 프레임워크

---

### Phase 6: Polish & Launch
**목표**: 출시 준비

- 성능 최적화
- SEO
- 문서 사이트 (dogfooding)
- npm 배포
- 랜딩 페이지

**산출물**: 오픈소스 공개

---

## 6. Key Decisions

### 6.1 프레임워크 vs 템플릿

| 옵션 | 장점 | 단점 |
|------|------|------|
| 템플릿 | 완전한 자유도 | 유지보수 부담, 업데이트 어려움 |
| **프레임워크** ✅ | 간편함, 자동 업데이트 | 커스터마이징 제한 |

**결론**: 프레임워크 방식 채택. 대부분의 사용자는 설정만으로 충분하며, 고급 사용자는 슬롯/eject 옵션 활용.

### 6.2 커스터마이징 전략

1. **설정으로 해결 가능한 것** → `vibe.config.ts`
2. **스타일 변경** → CSS 변수
3. **부분적 UI 변경** → 슬롯 시스템
4. **완전한 자유** → Eject

### 6.3 왜 GitHub API 직접 사용?

- ✅ 서버 비용 0원
- ✅ Git 히스토리 자동 생성
- ✅ GitHub가 인프라 관리
- ⚠️ Rate Limit → 읽기는 정적이라 OK

---

## 7. Security

| 위협 | 대응 |
|------|------|
| client_secret 노출 | Serverless Function에서만 사용 |
| XSS | DOMPurify로 마크다운 sanitize |
| 권한 상승 | GitHub API가 repo 권한 검증 |

---

## 8. Risks

| 리스크 | 영향 | 대응 |
|--------|------|------|
| GitHub API 변경 | 높음 | API 버전 명시, 래퍼 추상화 |
| 경쟁 솔루션 | 중간 | 디자인/UX 차별화 집중 |
| 커스터마이징 요구 | 중간 | 슬롯 시스템으로 확장성 제공 |

---

## 9. Open Questions

- [ ] 테마 마켓플레이스 지원 여부?
- [ ] 블로그 기능 포함 vs 별도 패키지?
- [ ] 플러그인 API 설계?
- [ ] Vercel 외 호스팅 지원 범위?

---

## 10. Appendix

### 10.1 참고 자료

- [Docusaurus](https://docusaurus.io/) - 구조 참고
- [VitePress](https://vitepress.dev/) - Vite 기반 참고
- [Nextra](https://nextra.site/) - Next.js 기반 참고
- [Mintlify](https://mintlify.com/) - 디자인 참고

### 10.2 용어 정의

| 용어 | 정의 |
|------|------|
| Admin | config에 등록된 GitHub 사용자 (편집 권한) |
| Slot | 사용자 컴포넌트를 삽입할 수 있는 지정 영역 |
| Eject | 프레임워크 내부 코드를 추출하여 직접 관리 |

---

**문서 버전**: 3.0
**최종 수정**: 2026-01-12
**상태**: 기획 완료
