---
description: Plan Mode 진입 + 구현 계획 문서 생성
argument-hint: [feature-name]
allowed-tools: Read, Grep, Glob, Write, EnterPlanMode
---

$ARGUMENTS 기능의 구현 계획을 수립하고 문서화한다.

## 실행 순서

1. **Plan Mode 진입** (EnterPlanMode 도구 사용)
2. 관련 코드베이스 분석
3. 영향 범위 파악
4. 계획 파일 작성 (`.claude/plans/`에 Plan Mode 기본 방식대로)
5. 사용자 승인 (ExitPlanMode)
6. **Plan Mode에서 작성한 계획을 그대로** `docs/plans/PLAN_$ARGUMENTS.md`로 복사

## 중요

- Plan Mode에서 작성한 계획 내용을 **정형화하지 않고 그대로** 저장
- 탐색 과정, 의사결정 근거, 검토한 대안 등 맥락 정보 보존
- 형식 강제 없음 - Plan Mode 출력이 곧 최종 문서

## 참고

- Claude가 자동으로 Plan Mode에 진입할 때는 문서를 생성하지 않음
- `/plan`을 명시적으로 호출하면 Plan Mode + 문서 저장까지 수행
