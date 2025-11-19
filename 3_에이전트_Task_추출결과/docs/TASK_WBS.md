# MVP Development Task WBS & Dependency Graph

본 문서는 SRS V0.3 및 Task 추출 결과를 바탕으로 구성된 작업 분할 구조(WBS)와 의존성 그래프(DAG)를 기술합니다.

## 1. Work Breakdown Structure (WBS)

### 🌟 Epic 0: Frontend PoC Prototype (Breadth-First)
제품 전체의 주요 흐름을 빠르게 시각화하고 UX를 검증하기 위한 프로토타입 개발입니다.
- **EPIC0-FE-001**: 온보딩 - 랜딩 및 프로필 입력/인증 시뮬레이션
- **EPIC0-FE-002**: 온보딩 - 디바이스/병원 연동 시뮬레이션 및 완료
- **EPIC0-FE-003**: 1장 의사용 요약 리포트 뷰 및 데이터 시각화
- **EPIC0-FE-004**: 오늘의 행동 카드 리스트 및 완료 인터랙션
- **EPIC0-FE-005**: 가족 보드 뷰 및 초대/역할 관리 UI

### 🚀 Epic 1: Core Onboarding & Identity (MVP Backend)
사용자 식별, 인증, 그리고 외부 디바이스/병원 연결의 기초를 다집니다.
- **REQ-FUNC-001-BE-01**: 온보딩 세션 관리 및 프로필 등록 API (User, Session)
- **REQ-FUNC-003-BE-01**: 디바이스 OAuth 연동 및 토큰 관리 API (DeviceLink)

### 📊 Epic 2: Health Report Engine (MVP Backend)
수집된 데이터를 분석하여 의사에게 제공할 가치를 창출합니다.
- **REQ-FUNC-007-BE-01**: 1장 요약 리포트 데이터 집계 및 생성 로직 (HealthReport)

### 🏃 Epic 3: Daily Action Coach (MVP Backend)
매일 사용자에게 행동을 제안하고 습관 형성을 돕습니다.
- **REQ-FUNC-011-BE-01**: 일일 행동 카드 생성 스케줄러 및 관리 API (ActionCard)

### 👨‍👩‍👧 Epic 4: Family Board & Access Control (MVP Backend)
보호자 연결 및 데이터 공유 보안을 처리합니다.
- **REQ-FUNC-015-BE-01**: 가족 보드 멤버십 및 역할 기반 접근 제어 (FamilyBoard)

### 🛡️ & ⚙️ Non-Functional / Operations
- **REQ-NF-006-SEC-01**: 데이터 암호화 및 감사 로그(Audit Trail) 구현 (Security)
- **REQ-NF-010-OPS-01**: 운영 모니터링 대시보드 및 중앙 집중식 로깅 구축 (Ops)

---

## 2. Dependency Graph (DAG)

아래 그래프는 Backend 구현 시의 논리적/데이터 의존성을 나타냅니다. Frontend PoC는 독립적으로 병렬 수행 가능합니다.

```mermaid
graph TD
    %% Nodes
    U[REQ-FUNC-001-BE-01<br/>(User/Profile/Session)]
    D[REQ-FUNC-003-BE-01<br/>(Device OAuth)]
    F[REQ-FUNC-015-BE-01<br/>(Family Board)]
    R[REQ-FUNC-007-BE-01<br/>(Report Engine)]
    A[REQ-FUNC-011-BE-01<br/>(Action Scheduler)]
    
    S[REQ-NF-006-SEC-01<br/>(Security/Audit)]
    O[REQ-NF-010-OPS-01<br/>(Monitoring/Ops)]

    %% Frontend PoC (Parallel)
    FE_POC[Epic 0: FE PoC Prototype]

    %% Edges
    U --> D
    U --> F
    U --> A
    
    D --> R
    U --> R
    
    %% Cross-cutting concerns (Apply to all)
    S -.-> U
    S -.-> D
    S -.-> F
    S -.-> R
    S -.-> A
    
    O -.-> U
    O -.-> D
    O -.-> R
```

### 실행 순서 가이드

1. **Phase 1 (Parallel Start)**:
   - **Frontend Team (Agent)**: Epic 0의 모든 Task (`EPIC0-FE-XXX`)를 수행하여 UI/UX를 확정합니다.
   - **Backend Team (Agent)**: `REQ-FUNC-001-BE-01` (User Core)를 최우선 구현합니다. 동시에 `REQ-NF-006` (Security Base) 설계를 잡습니다.

2. **Phase 2 (Integration)**:
   - User Core가 완료되면 `REQ-FUNC-003` (Device), `REQ-FUNC-015` (Family), `REQ-FUNC-011` (Action)을 병렬로 진행합니다.
   - 이 단계에서 DB 스키마가 확장됩니다.

3. **Phase 3 (Value Creation)**:
   - 데이터 수집(Device)이 준비되면 `REQ-FUNC-007` (Report Engine)을 구현하여 실제 가치를 창출합니다.

4. **Phase 4 (Operation Ready)**:
   - `REQ-NF-010` (Monitoring)을 적용하여 배포 준비를 마칩니다.
```

