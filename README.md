<a id="readme-top"></a>

# Project Init

app-idea-lab에서 채택된 PRD 문서를 기반으로, React Native + Supabase 프로젝트를 자동 생성하고 초기화하는 워크플로우 도구.

<!-- TABLE OF CONTENTS -->
<details>
  <summary>목차</summary>
  <ol>
    <li><a href="#프로젝트-소개">프로젝트 소개</a></li>
    <li><a href="#워크플로우">워크플로우</a></li>
    <li><a href="#프로젝트-구조">프로젝트 구조</a></li>
    <li><a href="#시작하기">시작하기</a></li>
    <li><a href="#사용법">사용법</a></li>
    <li><a href="#기술-스택">기술 스택</a></li>
    <li><a href="#생성되는-프로젝트-구조">생성되는 프로젝트 구조</a></li>
    <li><a href="#의존성-매핑">의존성 매핑</a></li>
    <li><a href="#연관-프로젝트">연관 프로젝트</a></li>
  </ol>
</details>

## 프로젝트 소개

이 프로젝트는 **코드 개발을 하지 않는다.** PRD 기반 프로젝트 초기화 자동화 스킬만 제공한다.

**하는 일:**
- PRD를 읽고 필요한 폴더 구조 생성
- 공통 + 조건부 의존성 설치
- CLAUDE.md, KNOWLEDGE.md 등 개발 지침 문서 자동 생성
- Git 초기화

**하지 않는 일:**
- 앱 기능 코드 작성
- PRD 작성이나 평가 (app-idea-lab의 역할)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## 워크플로우

2개의 슬래시 커맨드로 구성된다. 순서대로 실행한다.

| 순서 | 커맨드 | 설명 |
|:---:|---|---|
| 1 | `/init-scaffold NNN-아이디어명 프로젝트명` | 프로젝트 생성 + 폴더 구조 + 의존성 설치 |
| 2 | `/init-docs NNN-아이디어명 프로젝트명` | CLAUDE.md + KNOWLEDGE.md 생성 + Git 초기화 |

```
app-idea-lab/ideas/adopted/NNN-아이디어명-prd.md
        │
        ▼
  /init-scaffold  →  프로젝트 폴더 + 의존성 설치
        │
        ▼
  /init-docs      →  CLAUDE.md + KNOWLEDGE.md + Git init
        │
        ▼
  ~/프로젝트명/   →  개발 착수 가능 상태
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## 프로젝트 구조

```
project-init/
├── .claude/
│   └── commands/
│       ├── init-scaffold.md    # 프로젝트 생성 커맨드
│       └── init-docs.md        # 문서 생성 커맨드
├── templates/
│   └── commands/
│       ├── next.md             # 생성 프로젝트에 복사할 /next 커맨드
│       └── plan.md             # 생성 프로젝트에 복사할 /plan 커맨드
├── CLAUDE.md                   # 프로젝트 지침 (스택, 의존성 매핑 등)
└── README.md
```

### 경로 상수

| 항목 | 경로 |
|---|---|
| app-idea-lab | `~/app-idea-lab` |
| PRD 파일 위치 | `~/app-idea-lab/ideas/adopted/NNN-아이디어명-prd.md` |
| 프로젝트 생성 기본 경로 | `~/` |
| 커맨드 템플릿 | `~/project-init/templates/commands/` |

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## 시작하기

### 사전 요구사항

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI 설치
- [app-idea-lab](../app-idea-lab)에서 채택된 PRD 문서 (최소 1개)

### 설치

```sh
git clone <repo-url> ~/project-init
cd ~/project-init
```

별도의 의존성 설치는 필요 없다. Claude Code 세션에서 슬래시 커맨드를 실행하면 된다.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## 사용법

Claude Code에서 project-init 프로젝트를 열고 커맨드를 실행한다.

```
# 1. 프로젝트 스캐폴드 생성
/init-scaffold 009-복약케어 medi-care

# 2. 개발 문서 생성 + Git 초기화
/init-docs 009-복약케어 medi-care
```

실행 후 `~/medi-care/`에 개발 착수 가능한 프로젝트가 생성된다.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## 기술 스택

생성되는 프로젝트의 고정 기술 스택. app-idea-lab의 기술 스택 제약과 동기화된다.

| 레이어 | 기술 |
|---|---|
| 프레임워크 | Expo Managed Workflow (SDK 최신 안정 버전) |
| 언어 | TypeScript (strict mode) |
| 라우팅 | Expo Router (파일 기반) |
| UI | React Native Paper |
| 상태 관리 | Zustand |
| 백엔드 | Supabase (PostgreSQL + Auth + Edge Functions + Storage) |
| 로컬 KV | react-native-mmkv |
| 로컬 DB | op-sqlite (조건부) |
| 타겟 플랫폼 | iOS / Android |

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## 생성되는 프로젝트 구조

init-scaffold가 생성하는 Expo Router 기반 프로젝트의 표준 폴더 구조.

```
[프로젝트루트]/
├── src/
│   ├── app/                    # Expo Router 파일 기반 라우팅
│   │   ├── (auth)/            # 인증 관련 화면 그룹
│   │   ├── (tabs)/            # 메인 탭 네비게이터
│   │   ├── _layout.tsx        # 루트 레이아웃
│   │   └── index.tsx          # 엔트리 포인트
│   ├── components/            # 재사용 컴포넌트
│   │   └── ui/               # 공통 UI
│   ├── hooks/                 # 커스텀 훅
│   ├── stores/                # Zustand 스토어
│   ├── services/              # 외부 서비스 연동
│   │   └── supabase.ts       # Supabase 클라이언트 초기화
│   ├── utils/                 # 유틸리티 함수
│   ├── types/                 # TypeScript 타입 정의
│   ├── constants/             # 상수
│   └── assets/                # 정적 리소스
├── .claude/
│   └── commands/              # /next, /plan 커맨드
├── docs/
│   └── plans/                 # 구현 계획 문서
├── supabase/
│   ├── migrations/            # DB 마이그레이션 SQL
│   └── functions/             # Edge Functions
├── .env.example
├── .gitignore
├── CLAUDE.md                  # 프로젝트별 개발 지침
└── KNOWLEDGE.md               # 프로젝트별 도메인 지식
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## 의존성 매핑

### 공통 의존성 (모든 프로젝트에 설치)

`@supabase/supabase-js`, `react-native-paper`, `react-native-safe-area-context`, `zustand`, `react-native-mmkv`, `react-native-nitro-modules`, `expo-router`, `expo-linking`, `expo-constants`, `@expo/metro-runtime`, `react-native-screens`, `react-native-reanimated`, `react-native-gesture-handler`, `expo-status-bar`, `expo-splash-screen`, `expo-secure-store`, `expo-font`, `expo-system-ui`, `expo-dev-client`

### 조건부 의존성 (PRD Section 7-1에 명시된 경우만)

| 카테고리 | PRD 기술명 | 패키지 |
|---|---|---|
| 로컬 DB | op-sqlite | `@op-engineering/op-sqlite` |
| 인증 | Kakao OAuth | `@react-native-seoul/kakao-login` |
| 인증 | Google OAuth | `@react-native-google-signin/google-signin` |
| 인증 | Apple Auth | `expo-apple-authentication` |
| 알림 | expo-notifications | `expo-notifications` |
| 분석 | Firebase Analytics | `@react-native-firebase/analytics` + `app` |
| 분석 | Aptabase | `@aptabase/react-native` |
| 결제 | react-native-iap | `react-native-iap` |
| 결제 | RevenueCat | `react-native-purchases` |
| 차트 | Gifted Charts | `react-native-gifted-charts` + `react-native-svg` |
| 차트 | Victory Native | `victory-native` + `@shopify/react-native-skia` |
| UI | react-native-calendars | `react-native-calendars` |
| UI | DateTimePicker | `@react-native-community/datetimepicker` |
| 미디어 | expo-image-picker | `expo-image-picker` |
| 애니메이션 | Lottie | `lottie-react-native` |

상세 피어 의존성 및 주의사항은 `CLAUDE.md`를 참조.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## 연관 프로젝트

### app-idea-lab

본 프로젝트의 입력 소스. 아이디어 발굴 → 평가 → PRD 작성 → 검증의 4단계를 거쳐 채택된 PRD를 산출한다.

```
app-idea-lab (PRD 산출) → project-init (프로젝트 생성) → 실제 개발
```

### 동기화 항목

아래 항목이 app-idea-lab에서 변경되면 본 프로젝트의 대응 섹션도 함께 수정해야 한다.

| app-idea-lab | project-init |
|---|---|
| 기술 스택 제약 | 기술 스택, 공통/조건부 의존성 |
| 개발 환경 | 개발 환경, 경로 상수 |
| 타겟 플랫폼 | 타겟 플랫폼 |
| PRD 최종 목표 / Task Master 투입 요건 | CLAUDE.md 골격, KNOWLEDGE.md 골격 |

<p align="right">(<a href="#readme-top">back to top</a>)</p>
