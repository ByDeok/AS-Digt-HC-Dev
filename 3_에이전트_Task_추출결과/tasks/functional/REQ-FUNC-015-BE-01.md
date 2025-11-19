# REQ-FUNC-015-BE-01: 가족 보드 및 권한 관리 API

## 1. 목적 및 요약
- **목적**: 시니어와 보호자 간의 데이터 공유를 위한 가족 보드를 생성하고, 초대/수락 및 역할(Role) 기반 접근 제어를 처리한다.
- **요약**: `FamilyBoard`와 `AccessRole` 엔티티를 통해 N:M 관계를 관리하고, 초대 코드 발급 및 검증 로직을 구현한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - 가족 보드 생성 (시니어 가입 시 자동 또는 수동)
  - 초대 링크(토큰) 생성 및 유효성 검증
  - 멤버 추가 및 역할(Viewer, Admin) 변경
  - 접근 제어 인터셉터/필터 (보드 멤버만 데이터 조회 가능)
- **Out-of-Scope**:
  - 복잡한 위임장 법적 효력 관리
  - 그룹 채팅

---

```yaml
task_id: "REQ-FUNC-015-BE-01"
title: "가족 보드 멤버십 및 역할 기반 접근 제어(RBAC) 구현"
summary: >
  가족 구성원을 초대하고 역할을 부여하는 백엔드 로직을 구현한다.
  데이터 접근 시 해당 유저가 가족 보드의 적절한 권한을 가졌는지 검증하는 보안 로직을 포함한다.
type: "functional"

epic: "EPIC_FAMILY_BOARD"
req_ids: ["REQ-FUNC-015", "REQ-FUNC-016", "REQ-FUNC-018"]
component: ["backend.api", "backend.security"]

context:
  srs_section: "1.2.1 Family Board"
  apis: ["/api/family-board/invite", "/api/family-board/role"]
  entities: ["FamilyBoard", "BoardMember"]

inputs:
  description: "초대 요청 및 역할 변경 요청"

outputs:
  description: "초대 링크 토큰 또는 멤버십 상태 변경"

steps_hint:
  - "FamilyBoard, BoardMember 엔티티 설계 (Composite Key 등 고려)"
  - "초대 토큰 생성(JWT 또는 Random String) 및 만료 관리 로직 구현"
  - "초대 수락 API 구현: 토큰 검증 후 Member 추가"
  - "Spring Security와 연동하여 보드 ID 기반 접근 제어(AOP/Filter) 구현"

preconditions:
  - "User 인증 시스템이 구축되어 있어야 함"

postconditions:
  - "초대를 통해 다른 유저가 내 가족 보드에 접근 가능해짐"

dependencies: ["REQ-FUNC-001-BE-01"]

parallelizable: true
estimated_effort: "M"
priority: "Must"
agent_profile: ["backend"]

required_tools:
  frameworks: ["Spring Security"]
```

