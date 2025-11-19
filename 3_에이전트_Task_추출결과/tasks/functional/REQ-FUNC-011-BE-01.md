# REQ-FUNC-011-BE-01: 행동 카드 생성 및 스케줄링

## 1. 목적 및 요약
- **목적**: 사용자에게 매일 제공될 1~3개의 '오늘의 행동'을 생성하고 관리하는 백엔드 로직을 구현한다.
- **요약**: 사용자의 상태(질환, 최근 활동)를 기반으로 미리 정의된 행동 템플릿에서 적절한 카드를 선택하여 `ActionCard`를 생성하는 일일 배치 작업을 포함한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - 행동 템플릿 관리 (예: "10분 걷기", "약 복용", "혈압 측정")
  - 일일 배치 스케줄러 (매일 새벽 04시 실행)
  - 행동 카드 조회 및 상태 변경 API
  - D1/W1 완료율 집계 로직
- **Out-of-Scope**:
  - 강화학습 기반 개인화 추천 (단순 룰 기반으로 시작)

---

```yaml
task_id: "REQ-FUNC-011-BE-01"
title: "일일 행동 카드 생성 스케줄러 및 관리 API 구현"
summary: >
  매일 새벽 사용자의 상태에 맞는 행동 카드를 생성하는 배치 작업을 구현한다.
  생성된 카드의 상태(대기/완료/취소)를 관리하는 REST API를 제공한다.
type: "functional"

epic: "EPIC_ACTION_COACH"
req_ids: ["REQ-FUNC-011", "REQ-FUNC-012", "REQ-FUNC-013"]
component: ["backend.service", "backend.batch"]

context:
  srs_section: "6.3.2 행동 카드 생성 및 완료 플로우"
  apis: ["/api/actions/today", "/api/actions/{id}/complete"]
  entities: ["ActionCard", "ActionTemplate"]

inputs:
  description: "배치 실행 트리거 또는 API 호출"

outputs:
  description: "사용자별 금일 ActionCard 레코드 생성"

steps_hint:
  - "ActionTemplate(행동 은행) 테이블 설계 및 초기 데이터 시딩"
  - "Spring Batch 또는 Scheduler를 이용한 일일 생성 작업 구현"
  - "단순 룰 엔진(예: 고혈압 유저 -> 혈압 측정 카드 필수) 적용"
  - "ActionCard 완료 처리 API 구현 및 통계(완료율) 업데이트 로직"

preconditions:
  - "User 프로필(질환 정보)이 존재해야 함"

postconditions:
  - "매일 지정된 시간에 타겟 유저들의 ActionCard가 생성됨"

dependencies: ["REQ-FUNC-001-BE-01"]

parallelizable: true
estimated_effort: "M"
priority: "Must"
agent_profile: ["backend"]

required_tools:
  frameworks: ["Spring Batch", "Spring Boot"]
```

