# REQ-FUNC-003-BE-01: 디바이스 연동 및 OAuth 백엔드 API

## 1. 목적 및 요약
- **목적**: 웨어러블(워치) 및 혈압계 등 외부 디바이스 제조사의 API와 OAuth 2.0 기반으로 연동하여 액세스 토큰을 획득하고 저장하는 기능을 구현한다.
- **요약**: 벤더별(Samsung, Apple 등) OAuth 인증 흐름을 처리하고 `DeviceLink` 엔티티에 토큰을 암호화하여 저장한다. MVP에서는 최소 1종(워치)을 우선 구현한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - OAuth 인증 URL 생성 및 리다이렉트 처리
  - 콜백(Authorization Code) 수신 및 토큰 교환
  - 토큰 암호화 저장 (AES-256)
  - 초기 데이터(최근 30일치 활동/심박) 동기화 요청 트리거
- **Out-of-Scope**:
  - 실시간 데이터 스트림 처리 (배치/폴링 방식 사용)
  - 디바이스 펌웨어 관리

---

```yaml
task_id: "REQ-FUNC-003-BE-01"
title: "디바이스 OAuth 연동 및 토큰 관리 API 구현"
summary: >
  외부 헬스케어 플랫폼(Samsung Health/Google Fit 등)과 OAuth 연결을 처리한다.
  액세스 토큰의 안전한 저장(암호화)과 만료 시 갱신(Refresh) 로직을 포함한다.
type: "functional"

epic: "EPIC_ONBOARDING"
req_ids: ["REQ-FUNC-003", "REQ-NF-006"]
component: ["backend.api", "backend.integration"]

context:
  srs_section: "3.4.3 병원 포털/디바이스 연동 상태 동기화"
  apis: ["/api/onboarding/devices", "/api/devices/callback"]
  entities: ["DeviceLink"]

inputs:
  description: "사용자의 디바이스 연동 요청 및 OAuth 제공자의 콜백"
  fields:
    - name: "vendor"
      type: "string"
      allowed_values: ["samsung", "apple", "omron"]

outputs:
  description: "연동 성공 상태 및 초기 동기화 시작 응답"

steps_hint:
  - "DeviceLink 엔티티 및 토큰 암호화 컨버터 구현"
  - "Vendor별 OAuth 설정(ClientID, Secret) 관리 구조 설계"
  - "GET /auth/{vendor}/authorize: 인증 URL 리다이렉트 구현"
  - "GET /auth/{vendor}/callback: 코드 수신, 토큰 교환, DB 저장 구현"
  - "연동 성공 후 비동기 이벤트(DataSyncEvent) 발행"

preconditions:
  - "REQ-FUNC-001-BE-01 (User 존재)"
  - "외부 개발자 센터 앱 등록 및 키 발급 완료(또는 Mock)"

postconditions:
  - "DeviceLink 테이블에 유효한 액세스 토큰이 암호화되어 저장됨"

dependencies: ["REQ-FUNC-001-BE-01"]

parallelizable: true
estimated_effort: "M"
priority: "Must"
agent_profile: ["backend"]

required_tools:
  frameworks: ["Spring Security", "Spring WebClient"]
```

