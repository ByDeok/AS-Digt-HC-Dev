# REQ-FUNC-007-BE-01: 1장 리포트 생성 엔진 및 데이터 집계

## 1. 목적 및 요약
- **목적**: 수집된 다양한 건강 데이터(활동, 수면, 심박, 혈압 등)를 기간별(3개월/6개월)로 집계하고 표준화하여 'HealthReport' 엔티티를 생성한다.
- **요약**: 이기종 데이터 포맷을 통일하고, 의사에게 의미 있는 통계(평균, 최대/최소, 추세)를 계산하여 JSON 형태로 저장한다. 필요 시 PDF 생성 서비스와 연동한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - 기간별 데이터 쿼리 및 집계 (Average, Max, Min, Count)
  - 결측 구간 식별 및 태깅 (Missing Data Tagging)
  - 리포트 스코어링 알고리즘 (간단한 규칙 기반)
  - 리포트 생성 API (`/api/reports/generate`)
- **Out-of-Scope**:
  - 복잡한 질환 예측 AI 모델
  - 실시간 스트리밍 데이터 처리

---

```yaml
task_id: "REQ-FUNC-007-BE-01"
title: "1장 요약 리포트 데이터 집계 및 생성 로직 구현"
summary: >
  사용자의 건강 로그를 조회하여 의사용 리포트에 필요한 통계 지표를 계산한다.
  데이터 결측 여부를 판단하고, 최종 결과를 HealthReport 엔티티로 저장한다.
type: "functional"

epic: "EPIC_REPORT"
req_ids: ["REQ-FUNC-007", "REQ-FUNC-008", "REQ-FUNC-009"]
component: ["backend.service", "data.pipeline"]

context:
  srs_section: "3.4.2 진료 전 1장 리포트 생성 및 사용"
  apis: ["/api/reports/generate", "/api/reports/{id}"]
  entities: ["HealthReport", "HealthLog"]

inputs:
  description: "리포트 생성 요청 (기간, 대상 사용자)"
  fields:
    - name: "period_months"
      type: "integer"
      default: 3

outputs:
  description: "생성된 리포트 ID 및 요약 데이터 JSON"

steps_hint:
  - "HealthLog(원시 데이터) 테이블에서 기간별 데이터 조회 쿼리 최적화"
  - "데이터 정규화 및 결측치 처리 로직(MissingValueHandler) 구현"
  - "주요 지표(혈압, 보행 등) 통계 계산 서비스 구현"
  - "HealthReport 엔티티에 JSON 형태로 요약 결과 저장"
  - "리포트 생성 완료 알림 트리거"

preconditions:
  - "사용자 건강 데이터가 일부 수집되어 있어야 함 (또는 Mock Data)"

postconditions:
  - "요청된 기간에 대한 HealthReport 레코드가 생성됨"

dependencies: ["REQ-FUNC-001-BE-01"]

parallelizable: true
estimated_effort: "L"
priority: "Must"
agent_profile: ["backend", "data"]

required_tools:
  frameworks: ["Spring Boot", "QueryDSL"]
  infra: ["MySQL"]
```

