# 워크플로우 점검 보고서: 필수 의존성, 설치 명령어, 조건부 의존성 매핑, 로컬 DB / 오프라인, 인증

## 점검 기준

- app-idea-lab 프로젝트의 **기술 스택 제약**을 반영하여, stage-3가 생성하는 가상의 PRD 문서들을 입력 인자로 받아 project-init 워크플로우를 여러 차례 수행한다는 기준으로 점검한다.
- 001-데일리셀프-prd.md, 002-마음날씨-prd.md는 구버전 워크플로우 산출물이므로 제외한다.

### app-idea-lab 기술 스택 제약

```
- React Native + TypeScript
- Supabase (PostgreSQL)
- 외부 파트너십 없이 혼자 구현 가능해야 함
- 1인 개발 규모에 적합한 설계
- 3개월 MVP 출시 가능한 범위로 한정
```

### stage-2 기술 적합성 점수 기준

| 점수 | 기준 |
|------|------|
| 5 | RN + Supabase만으로 완전 구현 |
| 4 | 소수 외부 API 연동 필요 (지도 등) |
| 3 | 결제/인증 등 외부 서비스 필요 |
| 2 | 네이티브 모듈 또는 복잡한 외부 연동 필요 |
| 1 | RN으로 구현 부적합 (AR, 고성능 그래픽 등) |

---

## 근본 문제: 두 워크플로우 간 기술 선택 동기화 부재

app-idea-lab의 기술 스택 제약은 다음과 같다:

```
- React Native + TypeScript
- Supabase (PostgreSQL)
- 외부 파트너십 없이 혼자 구현 가능
- 1인 개발 규모에 적합
- 3개월 MVP 출시 가능한 범위
```

그런데 stage-3에서 Section 7-1을 작성할 때, **구체적 라이브러리 선택은 `sc:design` 스킬의 판단에 위임**된다:

> `stage-3.md` 줄 31: `7-1. 기술 스택: 레이어별(프론트엔드, 상태관리, 로컬저장, 백엔드, 결제, 알림, 분석, 빌드) 기술 선택 및 선정 사유`

stage-3는 **project-init의 고정 기술 스택(Expo Router, Zustand, React Native Paper, MMKV)이나 조건부 의존성 매핑 테이블을 참조하지 않는다.** 유일한 제약은 "React Native + TypeScript + Supabase 전용"뿐이다.

실제 PRD 001-데일리셀프가 이를 증명한다:

| 레이어 | stage-3가 선택한 기술 | project-init이 기대하는 기술 | 불일치 |
|---|---|---|---|
| 네비게이션 | React Navigation v7 | Expo Router | **불일치** |
| 결제 | RevenueCat v8 | react-native-iap | **불일치 (매핑 없음)** |
| 분석 | PostHog Cloud Free | Firebase / Aptabase | **불일치 (매핑 없음)** |
| UI | React Native Paper v5 | React Native Paper | 일치 |
| 상태 관리 | Zustand v5 | Zustand | 일치 |
| 로컬 KV | react-native-mmkv v3 | react-native-mmkv | 일치 |

> 001은 구버전 워크플로우이지만, **현재 stage-3에도 이 불일치를 방지하는 메커니즘이 없으므로** 동일한 문제가 재현된다.

---

## 영역별 재점검

### 1. 필수 의존성

project-init은 아래를 **고정**으로 항상 설치한다:
- Expo Router + 피어 6개 (expo-linking, expo-constants, @expo/metro-runtime, react-native-screens 등)
- Zustand
- React Native Paper + react-native-safe-area-context
- react-native-mmkv + react-native-nitro-modules

**문제**: stage-3가 이 고정 기술 대신 다른 기술을 선택하면, scaffold가 설치한 패키지와 PRD의 기술 스택이 어긋난다.

| 시나리오 | PRD가 선택 | scaffold가 설치 | 결과 |
|---|---|---|---|
| 네비게이션 | React Navigation | Expo Router + 피어 6개 | 불필요 패키지 설치 + 필요 패키지 누락 |
| UI | Tamagui / NativeBase | React Native Paper | 동일 |
| 상태 관리 | Jotai / Redux Toolkit | Zustand | 동일 |

**현실적 발생 가능성**: stage-2의 기술 적합성 점수(3-5점 범위)를 통과한 아이디어는 대부분 "RN + Supabase 기본 생태계"를 사용한다. Expo Router, Zustand, Paper는 이 생태계의 주류 선택이므로, sc:design이 **같은 기술을 선택할 확률이 높긴 하다.** 하지만 보장되지는 않는다.

**해결 방향**: stage-3에서 sc:design 호출 시, project-init의 고정 기술 스택을 컨텍스트로 전달하여 선택을 제약해야 한다. 또는 stage-3 자체에 고정 기술 목록을 명시해야 한다.

---

### 2. 설치 명령어 / 조건부 의존성 매핑

init-scaffold 2단계:
> "읽은 내용에서 CLAUDE.md의 '조건부 의존성 매핑' 테이블에 해당하는 기술을 식별한다."

**문제**: stage-3가 매핑 테이블에 없는 기술을 PRD에 쓰면, init-scaffold가 이를 식별할 수 없어 설치를 건너뛴다. 에러나 경고도 발생하지 않는다.

현재 조건부 매핑 테이블에 **없는** 기술 중, stage-3가 선택할 수 있는 현실적 후보들:

| 레이어 | 매핑에 없는 기술 | 선택 가능성 |
|---|---|---|
| 결제 | RevenueCat | **높음** — 1인 개발자에게 IAP보다 진입장벽 낮음 |
| 분석 | PostHog, Mixpanel, Amplitude | **높음** — Firebase보다 프라이버시 친화적 |
| 에러 추적 | Sentry, Bugsnag | **중간** — PRD에 Non-functional Requirements로 명시될 수 있음 |
| 지도 | react-native-maps, expo-location | **중간** — stage-2에서 "소수 외부 API(지도 등)" 언급 |
| 딥링크/공유 | expo-sharing, Branch.io | **중간** |
| 폰트/아이콘 | @expo/vector-icons 이외의 아이콘 라이브러리 | **낮음** |

**핵심**: stage-2의 기술 적합성 4점("소수 외부 API 연동 필요") 또는 3점("결제/인증 등 외부 서비스 필요")으로 채택된 아이디어는, project-init의 조건부 매핑으로 커버되지 않는 기술을 포함할 가능성이 있다.

**해결 방향**: 두 가지 접근이 가능하다.
- **(A)** stage-3에 project-init의 조건부 매핑 테이블을 전달하여, 매핑에 있는 기술만 선택하도록 제약
- **(B)** init-scaffold에서 매핑에 없는 기술을 감지하면, 사용자에게 수동 설치를 안내하는 경고 단계 추가

---

### 3. 로컬 DB / 오프라인

stage-3 줄 33:
> `7-3. 오프라인 우선 설계: 로컬 저장소 선택 및 근거(WatermelonDB, MMKV, AsyncStorage 등)`

stage-3가 로컬 저장소 후보로 **예시를 나열**한다. sc:design은 이 중에서 선택하되, app-idea-lab의 제약("1인 개발 규모에 적합한 설계")에 의해 복잡한 선택을 할 가능성이 낮다.

**현실적 시나리오 재구성:**

| 기술 적합성 점수 | 로컬 저장소 선택 가능성 | 비고 |
|---|---|---|
| 5점 (RN+Supabase만) | MMKV (필수 설치)만으로 충분 | 조건부 설치 불필요 |
| 4점 (소수 외부 API) | MMKV + op-sqlite | 구조화 데이터 캐싱 필요 시 |
| 3점 (결제/인증 등) | MMKV + op-sqlite | 오프라인 필요도에 따라 |

- **WatermelonDB**: 1인 개발 / 3개월 MVP 제약 하에서 선택될 가능성이 **매우 낮음**. RN 0.76+ 호환 이슈 + 커뮤니티 플러그인 의존성 + 학습 곡선을 고려하면, sc:design이 op-sqlite를 선호할 것으로 예상된다.
- **AsyncStorage**: MMKV가 필수 의존성으로 항상 설치되므로, sc:design이 AsyncStorage를 별도로 선택할 이유가 거의 없음.

**결론**: "WatermelonDB Expo 플러그인 누락" 이슈는 app-idea-lab의 제약을 고려하면 **실질적 발생 가능성이 매우 낮다.** 다만 매핑 테이블에 남겨두려면 피어 칼럼을 정확하게 채우는 것이 원칙적으로 맞다.

---

### 4. 인증

stage-3는 인증 방식을 구체적으로 제약하지 않는다. sc:design이 PRD의 Target Users(한국 시장)를 고려하여 선택한다.

**한국 시장 타겟 앱에서 현실적인 인증 조합:**

| 인증 방식 | 선택 가능성 | project-init 매핑 |
|---|---|---|
| Supabase Email/Password | **높음** (기본) | 별도 패키지 불필요 (supabase-js 포함) |
| Kakao OAuth | **높음** (한국 시장) | `@react-native-seoul/kakao-login` — **매핑 있음** |
| Google OAuth | **높음** | `@react-native-google-signin/google-signin` — **매핑 있음** |
| Apple Auth | **높음** (iOS 필수 정책) | `expo-apple-authentication` — **매핑 있음** |
| Naver OAuth | **중간** (한국 시장) | **매핑 없음** |
| Supabase 내장 OAuth (브라우저) | **중간** | **매핑 없음** (expo-web-browser 필요) |

**문제**:
- **Naver OAuth**: 한국 시장 타겟 앱에서 네이버 로그인은 카카오 다음으로 흔한 선택이다. `@react-native-seoul/naver-login` 패키지가 존재하지만 조건부 매핑에 없다.
- **Supabase 내장 OAuth**: sc:design이 네이티브 SDK 대신 Supabase의 브라우저 기반 OAuth를 선택하면 `expo-web-browser` + `expo-auth-session`이 필요하지만 매핑에 없다.

**app-idea-lab 제약 고려 시**: "외부 파트너십 없이 혼자 구현 가능"이라는 제약이 있으므로, OAuth 제공자의 개발자 콘솔 등록은 가능하지만 별도 심사가 필요한 인증 방식(법인 필수 등)은 제외된다. Kakao/Google/Apple/Naver 모두 개인 개발자 등록이 가능하므로 이 제약에 걸리지 않는다.

---

## 종합

### 이전 점검 대비 변경된 판단

| 이전 이슈 | 이전 심각도 | 재평가 | 사유 |
|---|---|---|---|
| WatermelonDB Expo 플러그인 누락 | 높음 | **낮음** | 1인 개발/3개월 MVP 제약상 선택 가능성 극히 낮음 |
| 네이티브 모듈 config plugin 미설정 | 중간 | **중간** (유지) | Kakao/Google OAuth는 현실적으로 빈번 |
| Supabase 내장 OAuth 미매핑 | 중간 | **중간** (유지) | sc:design이 선택할 수 있는 현실적 옵션 |
| Firebase 추가 설정 미안내 | 중간 | **낮음** | sc:design이 PostHog 등 대안 선택 가능성이 더 높음 |
| `.env.example` 조건부 확장 없음 | 중간 | **중간** (유지) | OAuth는 항상 추가 env 필요 |

### 신규 이슈

| 이슈 | 심각도 | 설명 |
|---|---|---|
| **stage-3 ↔ project-init 기술 동기화 부재** | **높음** | stage-3가 project-init의 고정/조건부 기술 목록을 모름 → PRD와 scaffold 불일치 가능 |
| **RevenueCat 매핑 누락** | **중간** | 1인 개발 결제에서 RevenueCat은 react-native-iap보다 현실적 선택 |
| **PostHog 등 경량 분석 도구 매핑 누락** | **중간** | Firebase는 config plugin 복잡성이 높아 sc:design이 대안 선택 가능 |
| **Naver OAuth 매핑 누락** | **중간** | 한국 시장 타겟에서 네이버 로그인은 주요 인증 수단 |
| **지도/위치 서비스 매핑 부재** | **낮음** | stage-2에서 "소수 외부 API(지도 등)"을 4점으로 평가, 채택 가능 |

### 가장 근본적인 해결책

stage-3의 `sc:design` 호출 시, project-init의 고정 기술 스택과 조건부 의존성 매핑 테이블을 입력 컨텍스트로 전달하여, PRD에 작성되는 기술 선택이 project-init이 처리할 수 있는 범위 안에 들어오도록 제약하는 것이다. 이렇게 하면 두 워크플로우 간의 기술 선택이 자연스럽게 동기화된다.
