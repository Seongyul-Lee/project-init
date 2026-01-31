- 1차 평가와 2차 평가(재평가) 사이의 맥락
001-데일리셀프 PRD는 구버전 워크플로우에서 PRD 문서가 두개로 나뉘어져 생성됐고 두 문서를 합친 문서이다.
002-마음날씨는 최신 워크플로우에서 정상적으로 워크플로우 수행한 결과이다.
1차 평가는 001-데일리셀프의 PRD 문서로 진행했었고 대부분 항목들이 기준 미달이었다.



002-마음날씨 PRD(v3.0)를 기준으로 project-init 워크플로우를 재평가하겠습니다. 먼저 project-init의 워크플로우 파일을 확인합니다.​​​​​​​​​​​​​​​​



002-마음날씨 PRD(v3.0)를 기준으로 project-init 워크플로우를 재평가하겠습니다.



Project-Init 워크플로우 재평가 (마음날씨 PRD v3.0 기준)

1. 마음날씨 PRD와 app-idea-lab 워크플로우 정합성

PRD 구조 비교







|app-idea-lab stage-3 지침|마음날씨 PRD v3.0 실제 구조                                  |정합성      |

|-----------------------|-----------------------------------------------------|---------|

|7-1. 기술 스택             |✓ 7-1. 기술 스택 (레이어별 테이블)                              |**완전 일치**|

|7-2. 시스템 구조도           |✓ 7-2. 시스템 구조도 (ASCII 다이어그램)                         |**완전 일치**|

|7-3. 오프라인 우선 설계        |✓ 7-3. 오프라인 우선 설계 (Sync Queue, 충돌 해결)                |**완전 일치**|

|7-4. 핵심 설계 원칙          |✓ 7-4. 핵심 설계 원칙 (5개 원칙)                              |**완전 일치**|

|7-5. DB 스키마            |✓ 7-5. DB 스키마 (12개 테이블 CREATE TABLE + RLS + Function)|**완전 일치**|

|7-6. API 설계            |✓ 7-6. API 설계 (30+ 엔드포인트, TS 인터페이스 7종)               |**완전 일치**|

|7-7. 상태 관리 구조          |✓ 7-7. 상태 관리 구조 (Zustand Store 6개)                   |**완전 일치**|

|7-8. 기능-테이블-API 매핑표    |✓ 7-8. 기능-테이블-API 매핑표 (F-01~F-09)                    |**완전 일치**|

|7-9. 자체 점검 결과          |✓ 7-9. 자체 점검 결과 (9항목 통과)                             |**완전 일치**|



결론: 마음날씨 PRD는 app-idea-lab 워크플로우의 stage-3 지침을 완벽히 준수합니다.



2. 이전 평가 재검토 (데일리셀프 → 마음날씨)

이전 지적 사항별 재검토







|#|이전 지적 (데일리셀프 기준)        |마음날씨 PRD에서의 상태                                                    |재평가      |

|-|------------------------|------------------------------------------------------------------|---------|

|1|Section 7 하위 매핑 불명확     |7-1~7-9 모든 하위 섹션이 명확한 헤딩(`### 7-1.`)으로 구분됨                        |**문제 없음**|

|2|7-2 시스템 구조도 누락 가능성      |완전한 ASCII 아키텍처 다이어그램 포함 (Client/Backend/External 3계층)             |**문제 없음**|

|3|7-4 설계 원칙 누락 가능성        |5가지 설계 원칙 명시 (오프라인 우선, 최소 외부 의존, 점진적 복잡성, RLS 기반 보안, 낙관적 UI)      |**문제 없음**|

|4|7-8 기능-테이블-API 매핑 누락 가능성|F-01~F-09 전체 매핑표 존재 (관련 테이블, API 엔드포인트, 비고)                       |**문제 없음**|

|5|DB Function SQL 스텁만 존재  |`create_emotion_record` 55줄, `get_weekly_summary` 80줄 완전한 PL/pgSQL|**문제 없음**|

|6|TypeScript 인터페이스 부재     |7종 인터페이스 완전 정의 (Request/Response 포함)                              |**문제 없음**|



마음날씨 PRD의 추가 강점

데일리셀프(구버전) 대비 마음날씨(신버전)의 개선점:

	1.	User Story 커버리지: 7개 → 24개 (8 Epic), 모든 기능 영역 커버

	2.	Screen Map: 네비게이션 구조 트리 + 핵심 화면별 UI 구성 신규 추가

	3.	자체 점검 결과: 9개 항목 점검 결과 명시적 기록

	4.	오프라인 설계 상세도: Sync Queue 다이어그램, 지수 백오프(1s→2s→4s), Server Wins 충돌 해결 정책 등



3. project-init 워크플로우 재평가

강점 (마음날씨 PRD 기준)

1) PRD 섹션 번호 체계 완벽 호환



app-idea-lab stage-3: "### 7-1. 기술 스택" 형식 지침

마음날씨 PRD:        "### 7-1. 기술 스택" 형식 준수

project-init:        "## 7-1." 또는 "### 7-1." 헤딩 패턴 인식

                     ↓

                     완전 호환





2) PRD 품질 보장 파이프라인



stage-3 자체 점검 (9항목)

        ↓

stage-4 정합성 판별 (4영역: 사업성/리스크/기술/문서품질)

        ↓

    통과한 PRD만

        ↓

project-init 입력





마음날씨 PRD는 Minor 3건만 남기고 통과했으므로, project-init 입력 품질이 보장됩니다.

3) Task Master 투입 요건 충족

마음날씨 PRD v3.0 review 결과:







|Task Master 투입 요건                       |충족 여부|

|----------------------------------------|-----|

|기능 명세 (P0/P1/P2, AC)                    |✓    |

|DB 스키마 (CREATE TABLE, RLS, Function SQL)|✓    |

|API 설계 (엔드포인트, TS 인터페이스)                |✓    |

|아키텍처 상세도 (스택, 구조도, 상태 관리, 매핑표)          |✓    |

|화면 구조 / UI 명세                           |✓    |

|오프라인/동기화 설계                             |✓    |



개선이 여전히 필요한 항목

🔴 높음 (즉시 수정) -> 현재 해결 완료 (idia -> idea)







|#|항목                  |상태        |이유                      |

|-|--------------------|----------|------------------------|

|1|경로 오타 `app-idia-lab`|**여전히 문제**|PRD 위치 경로가 틀리면 파일을 찾지 못함| -> 현재 해결 완료 (idia -> idea)



🟡 중간 (품질 향상)







|#|항목                           |재평가 결과                                                     |

|-|-----------------------------|-----------------------------------------------------------|

|2|CLAUDE.md “설계 원칙” 섹션         |**여전히 권장** — PRD 7-4의 5가지 설계 원칙이 개발 의사결정 기준으로 유용           |

|3|CLAUDE.md “기능-테이블-API 매핑” 섹션 |**여전히 권장** — PRD 7-8의 매핑표가 개발 시 빈번 참조 자료                   |

|4|init-docs Section 7 하위 매핑 테이블|**강화 권장** — 7-1~7-8 각각이 CLAUDE.md 어느 섹션에 가는지 명시하면 파싱 정확도 향상|



🟢 낮음 (향후 개선)







|#|항목           |재평가 결과                                                                          |

|-|-------------|--------------------------------------------------------------------------------|

|5|“시스템 아키텍처” 섹션|**선택적** — 마음날씨 PRD 7-2의 ASCII 다이어그램을 CLAUDE.md에 포함하면 좋지만, 긴 다이어그램은 PRD 참조로 대체 가능|

|6|용어 사전 자동 연결  |**선택적** — 마음날씨 PRD에는 부록 A(용어 정의)가 없음. 있는 PRD만 해당                                |



4. 마음날씨 PRD 기반 CLAUDE.md 생성 시뮬레이션

잘 추출되는 섹션

기술 스택 (PRD 7-1 → CLAUDE.md)



## 기술 스택

| 계층 | 기술 | 선택 이유 |

|------|------|----------|

| 프론트엔드 | React Native + TypeScript + Expo | 크로스 플랫폼, 1인 개발 효율 |

| 상태 관리 | Zustand + TanStack Query | 경량 전역 상태 + 서버 상태 캐싱 |

| 로컬 저장소 | WatermelonDB + MMKV | 오프라인 우선, Observable |

| 백엔드 | Supabase | BaaS, RLS 내장 |

| 결제 | RevenueCat | 크로스 플랫폼 IAP 통합 |

...





✓ 완전 추출 가능

DB 규칙 (PRD 7-5 → CLAUDE.md)



## DB 규칙

### 테이블 목록

users, emotion_categories, emotions, activities, emotion_records, 

emotion_record_emotions, emotion_record_activities, selfcare_cards, 

selfcare_emotion_map, selfcare_feedback, subscriptions, push_tokens, 

daily_card_usage, report_cache (14개)



### RLS 정책 요약

- 모든 테이블 RLS 활성화

- 기본: auth.uid() = user_id

- 마스터 데이터(emotion_categories, emotions, selfcare_cards): 인증 사용자 전체 읽기



### 핵심 DB Function

- create_emotion_record: 감정 기록 트랜잭션 생성 (emotion_records + M:N 테이블)

- get_weekly_summary: 주간 기본 요약 (무료)





✓ 완전 추출 가능 (CREATE TABLE 전문은 PRD 참조)

API 규칙 (PRD 7-6 → CLAUDE.md)



## API 규칙

### REST vs Edge Function 기준

- REST: 단순 CRUD

- DB Function (RPC): 트랜잭션 필요

- Edge Function: 복합 비즈니스 로직, 외부 API 호출, 구독 검증



### 주요 엔드포인트

| Method | 경로 | 설명 | 구현 |

|--------|------|------|------|

| POST | /rpc/create_emotion_record | 감정 기록 생성 | DB Function |

| GET | /emotion_records?... | 기간별 조회 | REST |

| POST | /functions/v1/recommend-selfcare | 셀프케어 추천 | Edge Function |

...





✓ 완전 추출 가능

오프라인/동기화 규칙 (PRD 7-3 → CLAUDE.md)



## 오프라인/동기화 규칙

### 저장소 분담

- WatermelonDB: emotion_records, 피드백 등 구조화 데이터

- MMKV: JWT 토큰, 설정, 구독 상태 캐시



### Sync Queue

- 최대 3회 재시도

- 지수 백오프: 1s → 2s → 4s



### 충돌 해결

- CREATE: UUID 기반, 충돌 없음

- UPDATE: Server Wins (server_updated_at 우선)

- DELETE: 소프트 삭제 병합

- 원칙: "사용자의 감정 기록은 절대 유실하지 않는다"





✓ 완전 추출 가능

상태 관리 패턴 (PRD 7-7 → CLAUDE.md)



### 상태 관리 패턴

| Store | 역할 | 주요 상태 | 지속성 |

|-------|------|----------|--------|

| AuthStore | 인증 상태 | user, isAuthenticated | Supabase Auth |

| EmotionRecordStore | 감정 기록 CRUD | todayRecords, selectedDate | WatermelonDB |

| MasterDataStore | 마스터 데이터 캐싱 | categories, emotions, activities | MMKV |

| ReportStore | 리포트 데이터 | weeklySummary, weeklyReport | TanStack Query |

| SubscriptionStore | 구독 상태 | isPremium, expiresAt | MMKV + RevenueCat |

| UIStore | 전역 UI 상태 | isDarkMode, isOnline, syncStatus | 메모리 |





✓ 완전 추출 가능

추가 권장 섹션

설계 원칙 (PRD 7-4 → CLAUDE.md 신규)



## 설계 원칙

1. **오프라인 우선**: 감정 기록은 WatermelonDB에 즉시 저장 → 네트워크 복구 시 동기화

2. **최소 외부 의존**: MVP에서 AI/ML 모델 불필요, SQL 집계로 패턴 분석

3. **점진적 복잡성**: 규칙 기반 → 통계 기반 → (향후) LLM 기반 단계적 고도화

4. **RLS 기반 보안**: 모든 테이블에 Row Level Security 적용

5. **낙관적 UI**: 저장/수정 시 로컬 상태 즉시 반영





→ 개발 의사결정 기준으로 매우 유용

기능-테이블-API 매핑 (PRD 7-8 → CLAUDE.md 신규)



## 기능-테이블-API 매핑

| 기능 ID | 기능명 | 테이블 | API |

|---------|--------|--------|-----|

| F-01 | 감정 태깅 | emotion_records, emotion_record_emotions/activities | POST /rpc/create_emotion_record |

| F-02 | 캘린더 & 통계 | emotion_records, emotions | GET /emotion_records, GET /rpc/get_weekly_summary |

| F-03 | 셀프케어 카드 | selfcare_cards, selfcare_feedback, daily_card_usage | POST /functions/v1/recommend-selfcare |

...





→ 기능 구현 시 즉시 참조 가능



5. 종합 점수 (마음날씨 PRD 기준 재평가)







|평가 항목          |이전 점수 (데일리셀프)|재평가 점수 (마음날씨)|변화|

|---------------|-------------|-------------|--|

|워크플로우 연계성      |8/10         |**9/10**     |↑ |

|PRD 파싱 정확성     |6/10         |**9/10**     |↑↑|

|CLAUDE.md 품질   |7/10         |**8/10**     |↑ |

|KNOWLEDGE.md 품질|8/10         |**8/10**     |= |

|정보 손실          |7/10         |**9/10**     |↑ |



점수 상승 이유

	1.	PRD 파싱 정확성 6→9: 마음날씨 PRD가 app-idea-lab stage-3 지침을 완벽 준수하여 섹션 번호 체계(7-1~7-9)가 명확함

	2.	워크플로우 연계성 8→9: PRD 품질 보장 파이프라인(stage-3 자체점검 + stage-4 정합성 판별) 효과 입증

	3.	정보 손실 7→9: DB Function SQL 본문, TypeScript 인터페이스, 상태 관리 Store 등 모든 정보가 PRD에 완전 명시됨

남은 개선 사항







|우선순위|항목                                        |영향도       |

|----|------------------------------------------|----------|

|🔴 높음|경로 오타 수정 (`app-idia-lab` → `app-idea-lab`)|파일 접근 불가  | -> 현재 해결 완료 (idia -> idea)

|🟡 중간|CLAUDE.md “설계 원칙” 섹션 추가                   |개발 의사결정 기준|

|🟡 중간|CLAUDE.md “기능-테이블-API 매핑” 섹션 추가           |개발 효율     |

|🟢 낮음|init-docs Section 7 하위 매핑 테이블 강화          |파싱 명확성    |



6. 결론

마음날씨 PRD v3.0은 app-idea-lab 워크플로우를 완벽히 준수한 결과물로, project-init 워크플로우와의 호환성이 매우 높습니다.

이전 평가(데일리셀프 기준)에서 지적한 문제점들:

	∙	Section 7 하위 매핑 불명확 → 해결됨 (마음날씨 PRD가 명확한 헤딩 사용)

	∙	7-2, 7-4, 7-8 정보 손실 가능성 → 해결됨 (모든 섹션 완전 작성)

	∙	DB Function 스텁 문제 → 해결됨 (55줄, 80줄 완전한 SQL)

남은 개선 사항은 대부분 “있으면 좋은” 수준이며, 경로 오타 수정만이 즉시 필요한 문제입니다.

최종 평가: app-idea-lab → project-init 연계 워크플로우는 실무 사용 가능 수준입니다.​​​​​​​​​​​​​​​​