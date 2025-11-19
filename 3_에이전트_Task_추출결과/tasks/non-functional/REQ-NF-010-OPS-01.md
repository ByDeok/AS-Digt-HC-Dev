# REQ-NF-010-OPS-01: 통합 모니터링 및 로깅 파이프라인 구축

## 1. 목적 및 요약
- **목적**: 시스템의 안정적인 운영을 위해 핵심 메트릭(지연 시간, 에러율)과 애플리케이션 로그를 수집하고 시각화한다.
- **요약**: Prometheus/Grafana 및 ELK(또는 Loki) 스택을 구성(또는 Cloud Native 서비스 활용)하여 REQ-NF-010 요구사항을 충족한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - Spring Boot Actuator 연동 및 메트릭 노출
  - 주요 API 요청/응답 로깅 (구조화된 JSON 포맷)
  - 에러 발생 시 알림 채널(Slack/Email) 연동
  - MVP 핵심 KPI 대시보드 (온보딩 이탈률, 리포트 생성 수)
- **Out-of-Scope**:
  - 분산 트레이싱(Jaeger) 상세 구현 (Post-MVP)

---

```yaml
task_id: "REQ-NF-010-OPS-01"
title: "운영 모니터링 대시보드 및 중앙 집중식 로깅 구축"
summary: >
  서버 상태와 비즈니스 지표를 실시간으로 감시할 수 있는 모니터링 환경을 구축한다.
  로그를 중앙에서 수집하여 장애 분석을 용이하게 한다.
type: "non_functional"

epic: "EPIC_OPS"
req_ids: ["REQ-NF-004", "REQ-NF-010"]
component: ["infra.monitoring", "backend.logging"]

category: "observability"
labels: ["monitoring:application", "logging:centralized"]

context:
  srs_section: "4.2 Non-Functional Requirements"

requirements:
  description: "모든 API 에러율 0.5% 미만 유지 감시, 온콜 알림 5분 내 발송"
  kpis:
    - "에러율 < 0.5%"
    - "로그 검색 지연 < 1분"

steps_hint:
  - "Spring Boot Actuator 설정 및 Prometheus Endpoint 노출"
  - "Logback 설정을 통한 JSON 포맷 로그 파일 생성"
  - "Docker Compose(또는 K8s Helm)로 Prometheus, Grafana, Loki 스택 배포"
  - "Grafana 대시보드 생성 (시스템 리소스 + 비즈니스 메트릭)"
  - "AlertManager 설정: 5xx 에러 급증 시 Slack 알림 전송"

preconditions:
  - "백엔드 애플리케이션이 Docker 컨테이너화 되어 있어야 함"

postconditions:
  - "Grafana에서 서버 CPU/Memory 및 API 요청 수를 볼 수 있음"

dependencies: ["REQ-FUNC-001-BE-01"]

parallelizable: true
estimated_effort: "M"
priority: "Should"
agent_profile: ["devops"]

required_tools:
  infra: ["Prometheus", "Grafana", "Loki"]
```

