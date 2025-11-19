# REQ-NF-006-SEC-01: 보안 및 개인정보 보호(PHI) 체계 구현

## 1. 목적 및 요약
- **목적**: 민감한 건강 정보(PHI)를 다루는 헬스케어 서비스로서 데이터 암호화, 감사 로그, 보안 인증 체계를 갖춘다.
- **요약**: DB 컬럼 단위 암호화, 중요 행위(조회/수정)에 대한 감사 로그(Audit Log) 기록, 그리고 위험 기반 2FA 기초를 구현한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - DB 내 개인정보(이름, 전화번호) 및 건강정보(진단명) 암호화 저장
  - 모든 데이터 접근 트랜잭션에 대한 감사 로그 테이블 기록
  - HTTPS(TLS 1.2+) 강제 적용 설정
  - 가족 보드 중요 설정 변경 시 재인증(Password/OTP) 로직
- **Out-of-Scope**:
  - 복잡한 키 관리 시스템(KMS) 연동 (MVP는 환경변수/Secret 파일 기반)

---

```yaml
task_id: "REQ-NF-006-SEC-01"
title: "데이터 암호화 및 감사 로그(Audit Trail) 구현"
summary: >
  개인건강정보(PHI)의 기밀성을 보장하기 위해 저장 시 암호화를 적용하고,
  누가 언제 데이터에 접근했는지 추적할 수 있는 감사 로그를 남긴다.
type: "non_functional"

epic: "EPIC_SECURITY"
req_ids: ["REQ-NF-006", "REQ-NF-007"]
component: ["backend.security", "backend.db"]

category: "security"
labels: ["security:encryption", "security:audit"]

context:
  srs_section: "6.2.7 AuditLog"

requirements:
  description: "AES-256 암호화 및 모든 조회/변경 이력 기록"
  kpis:
    - "감사 로그 누락율 0%"

steps_hint:
  - "JPA Converter 또는 Hibernate Listener를 활용한 투명한 암호화/복호화 구현"
  - "AuditLog 엔티티 및 Async Logging 서비스 구현"
  - "Spring Security Filter Chain에 HTTPS 리다이렉트 설정"
  - "민감 API(@AuditLogging 어노테이션 등) 호출 시 자동으로 로그 남기는 AOP 구현"

preconditions:
  - "DB 스키마에 AuditLog 테이블 존재"

postconditions:
  - "DB 조회 시 민감 데이터가 평문으로 보이지 않아야 함"
  - "API 호출 시 AuditLog 테이블에 행우 기록이 쌓여야 함"

dependencies: ["REQ-FUNC-001-BE-01"]

parallelizable: true
estimated_effort: "M"
priority: "Must"
agent_profile: ["backend", "security"]

required_tools:
  frameworks: ["Spring Security", "Bouncy Castle"]
```

