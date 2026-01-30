# Project Init

PRD 문서를 기반으로 React Native + Supabase 프로젝트를 자동 생성하고 초기화하는 Claude Code 워크플로우 도구.

## 개요

[app-idia-lab](https://github.com/Seongyul-Lee/app-idia-lab)에서 채택(adopted)된 PRD 문서를 입력받아, 표준화된 Expo + Supabase 프로젝트 구조를 자동으로 스캐폴딩한다.

이 프로젝트는 **코드 개발을 하지 않는다.** PRD 기반 프로젝트 초기화 자동화 스킬만 제공한다.

## 워크플로우

```
/init-scaffold NNN-아이디어명 프로젝트명  →  프로젝트 생성 + 폴더 구조 + 의존성 설치
/init-docs NNN-아이디어명 프로젝트명      →  CLAUDE.md + KNOWLEDGE.md 생성 + Git 초기화
```

### `/init-scaffold`

PRD를 분석하여 다음을 수행한다:

1. Expo 프로젝트 생성 (`create-expo-app`)
2. 표준 폴더 구조 생성 (Expo Router 기반)
3. PRD의 P0 기능에 따른 기능별 폴더 확장
4. 공통 의존성 설치 (`npx expo install`)
5. PRD에 명시된 조건부 의존성 추가 설치
6. `.env.example`, `.gitignore` 등 설정 파일 생성
7. 커스텀 Claude Code 명령어 복사 (`next.md`, `plan.md`)

### `/init-docs`

PRD 각 섹션에서 정보를 추출하여 다음 문서를 생성한다:

- **CLAUDE.md** — 프로젝트별 개발 지침 (기술 스택, 코딩 컨벤션, DB/API 규칙, 검증 프로토콜 등)
- **KNOWLEDGE.md** — 프로젝트별 도메인 지식 (비즈니스 컨텍스트, 페르소나, 핵심 기능, 용어 사전 등)

## 기술 스택 (생성되는 프로젝트)

| 레이어 | 기술 |
|---|---|
| 프레임워크 | Expo Managed Workflow (최신 SDK) |
| 언어 | TypeScript (strict mode) |
| 라우팅 | Expo Router (파일 기반) |
| UI | React Native Paper |
| 상태 관리 | Zustand |
| 백엔드 | Supabase (PostgreSQL + Auth + Edge Functions + Storage) |
| 로컬 저장소 | react-native-mmkv |

## 생성되는 프로젝트 구조

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
│   ├── utils/                 # 유틸리티 함수
│   ├── types/                 # TypeScript 타입 정의
│   ├── constants/             # 상수
│   └── assets/                # 정적 리소스
├── .claude/commands/          # Claude Code 커스텀 명령어
├── docs/plans/                # 구현 계획 문서
├── supabase/
│   ├── migrations/            # DB 마이그레이션 SQL
│   └── functions/             # Edge Functions
├── CLAUDE.md                  # 개발 지침
└── KNOWLEDGE.md               # 도메인 지식
```

## 조건부 의존성

PRD에 명시된 기술에 따라 추가 패키지를 자동으로 설치한다:

- **로컬 DB**: op-sqlite, WatermelonDB
- **인증**: Kakao OAuth, Google OAuth, Apple Auth
- **알림**: expo-notifications
- **분석**: Firebase Analytics, Aptabase
- **결제**: react-native-iap
- **차트**: Gifted Charts, Victory Native
- **UI**: Calendar, DateTimePicker, SVG
- **미디어**: Image Picker, Camera, Haptics
- **애니메이션**: Lottie

## 사전 요구사항

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- Node.js 18+
- Git
