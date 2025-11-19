# REQ-FUNC-001-BE-01: 온보딩 세션 및 프로필 관리 백엔드 API

## 1. 목적 및 요약
- **목적**: 신규 사용자의 가입 요청을 처리하고, 온보딩 상태(단계, 진행률)를 추적하며 기본 프로필 정보를 저장하는 백엔드 API를 구현한다.
- **요약**: `/api/onboarding/start`, `/api/onboarding/verify` 등 온보딩 초기에 필요한 엔드포인트를 제공하고 `User`, `OnboardingSession` 엔티티를 관리한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - 온보딩 세션 생성 및 조회
  - 사용자 기본 프로필(이름, 나이, 성별, 질환) 저장
  - 본인인증(Mock 또는 실제) 결과 검증 및 회원 계정 생성
- **Out-of-Scope**:
  - 디바이스 연동(별도 Task)
  - 복잡한 약관 버전 관리

## 3. 기술적 요구사항
- **API 스펙**: RESTful API (OpenAPI 3.0 기반 문서화 필수)
- **데이터베이스**: MySQL `users`, `onboarding_sessions` 테이블
- **보안**: 세션 ID 및 임시 토큰을 통한 보안 유지

---

```yaml
task_id: "REQ-FUNC-001-BE-01"
title: "온보딩 세션 관리 및 프로필 등록 API 구현"
summary: >
  사용자 회원가입 및 온보딩 진행 상태를 관리하는 백엔드 기능을 구현한다.
  User 엔티티와 OnboardingSession 엔티티를 설계하고, 단계별 상태 저장 API를 제공한다.
type: "functional"

epic: "EPIC_ONBOARDING"
req_ids: ["REQ-FUNC-001", "REQ-FUNC-002", "REQ-FUNC-005", "REQ-FUNC-006"]
component: ["backend.api", "backend.db"]

context:
  srs_section: "3.4.1 핵심 온보딩 플로우"
  apis: ["/api/onboarding/start", "/api/onboarding/verify"]
  entities: ["User", "OnboardingSession"]

inputs:
  description: "클라이언트의 온보딩 시작 요청 및 프로필 데이터"
  fields:
    - name: "profile_data"
      type: "object"
      required: true

outputs:
  description: "생성된 세션 ID 및 현재 온보딩 단계 정보"
  success:
    http_status: 200
    body_example: "{ session_id: '...', step: 'DEVICE_CONNECT', eta: 120 }"

steps_hint:
  - "User 및 OnboardingSession JPA/MyBatis 엔티티 설계"
  - "POST /api/onboarding/start 구현: 세션 생성 및 DB 저장"
  - "POST /api/onboarding/verify 구현: 인증 확인 후 User 레코드 생성(또는 업데이트)"
  - "PUT /api/user/profile 구현: 나이/질환 정보 업데이트"
  - "온보딩 이탈 시 재진입을 위한 상태 조회 로직 구현"

preconditions:
  - "DB 스키마 마이그레이션 환경 구성 완료"

postconditions:
  - "신규 유저가 DB에 생성되고 온보딩 세션이 활성화됨"

dependencies: []

parallelizable: true
estimated_effort: "M"
priority: "Must"
agent_profile: ["backend"]

required_tools:
  languages: ["Java"]
  frameworks: ["Spring Boot", "JPA"]
  infra: ["MySQL"]
```

