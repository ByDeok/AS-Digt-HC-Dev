# SRS 기반 MVP 개발 Task 통합 추출 계획

## 개요

- 목표: `SRS_V0.3.md`와 `11_SRS에서_상셰_TASK_추출하기.md`를 기반으로, **MVP 개발 완료까지 필요한 모든 Task를 누락 없이 도출**하고, 이를 **Epic → 하위 Task → WBS/그래프** 구조로 정리한다.
- 접근 방식: 먼저 **Epic 단위(대기능/Capability)** 를 정의·평가하고, 이후 **Epic별로 상세 개발 Task 리스트를 단계적으로 추출**한다.

## 1단계. 준비 및 공통 원칙 정리

- `SRS_V0.3.md`의 다음 섹션을 기준 레퍼런스로 삼는다.
- 1.6 MVP Phase Definition & Validation Scope
- 3.3 API Overview, 3.4 / 6.3 시퀀스 다이어그램
- 4.1 Functional Requirements (REQ-FUNC-xxx)
- 4.2 Non-Functional Requirements (REQ-NF-xxx)
- 5. Traceability Matrix, 6.1 API Endpoint List, 6.2 Entity & Data Model
- `11_SRS에서_상셰_TASK_추출하기.md`의 원칙을 전제한다.
- SRS 요구사항 1개 → 여러 개발 Task (Backend, Frontend, Infra, QA 등)
- Sub-Task 분해 축: 입력(Input) / 처리(Process) / 출력(Output) / 예외(Exception) / 설정(Config) / 테스트(Test)
- AC(수용 기준) = Task의 완료 조건(DoD)
- 산출물/도구를 미리 정한다.
- REQ 인벤토리 관리: 스프레드시트 or Notion DB (REQ ID, 제목, 타입, Epic 매핑 등)
- Task/WBS 관리: 이슈 트래커(Jira, GitHub Projects 등)
- 관계 시각화: Mermaid, Miro/Whimsical 등으로 Tree/Dependency Graph 작성

## 2단계. Epic(대기능) 정의 및 초기 분류

- SRS에서 자연스럽게 나오는 **기능 축 + Q1 Triplet** 기준으로 Epic 후보를 정의한다.
- Epic E1: 3분 온보딩 (REQ-FUNC-001~006, 019 + 관련 REQ-NF)
- Epic E2: 1장 의사용 요약 리포트 (REQ-FUNC-007~010 + 관련 REQ-NF)
- Epic E3: 오늘 1~3개 행동 코칭 (REQ-FUNC-011~014 + 관련 REQ-NF)
- Epic E4: 가족 보드 & 대리 접근 (REQ-FUNC-015~018 + 관련 REQ-NF)
- Epic E5: 디바이스 & 병원 포털 연동 (REQ-FUNC-003,004,019 + 관련 REQ-NF)
- Epic E6: 공통 Auth/Consent & 플랫폼/NFR (REQ-NF-001~019 중 횡단 요구, `/api/auth`, `/api/consent` 등)
- 각 Epic의 목적과 성공 기준을 1~2문장으로 정의하고, PRD의 Q1 Triplet 및 KPI와의 관계를 메모한다.
- 작업 산출물: "Epic 정의 표" (Epic ID, 이름, 관련 Story/Feature, 주요 REQ 목록, 비고).

## 3단계. REQ-FUNC / REQ-NF 인벤토리 구축 및 Epic 매핑

- `4.1 Functional Requirements` 테이블의 **REQ-FUNC-001~019 전체를 인벤토리로 수집**한다.
- 각 REQ-FUNC에 대해: ID, Title, Description, Feature, Priority, Source Story를 복사.
- `4.2 Non-Functional Requirements` 테이블의 **REQ-NF-001~019 전체를 인벤토리로 수집**한다.
- Category, Metric/Target, Source를 포함.
- 각 REQ를 상기 Epic들(E1~E6)에 매핑한다.
- 한 REQ가 여러 Epic에 걸치면, "Primary Epic"과 "Also Impacts" 필드를 분리하여 관리.
- Traceability Matrix(5장)를 참고해 Story↔REQ↔Epic 연계를 검산한다.
- 작업 산출물: "REQ 매핑 시트" (REQ ID, Type(FUNC/NF), Epic, Story, Feature, 영향 범위).

## 4단계. MVP 범위 컷 및 Dummy/축소 허용 범위 표시

- `1.6 MVP Phase Definition & Validation Scope`를 기준으로, 각 REQ에 대해 아래 속성을 표시한다.
- Must-Prove in MVP: MVP에서 반드시 증명해야 하는 요구 (1.6.1에 해당).
- Hypothesis/Long-Term: MVP에서는 가설로 두고, 계측/로깅만 필수 (1.6.2에 해당).
- Dummy/축소 구현 허용: MVP에서 축소 구현으로 인정 (1.6.3에 해당).
- Epic별로 "MVP에서 실제 구현 대상 REQ″와 "계측/로깅 중심 REQ″를 구분한다.
- 작업 산출물: "MVP Scope 표" (REQ ID, Epic, MVP Status, 구현 수준, 비고).

## 5단계. Epic 단위 상위 WBS 설계 (코드-구현 전 Task 프레임)

- 각 Epic(E1~E6)에 대해, **아직 세부 구현 이름을 쓰지 않고 상위 WBS 구조만** 먼저 잡는다.
- 공통 상위 WBS 레벨 예시:
- L1: Epic (예: E1 3분 온보딩)
- L2: Capability/Flow 단위 (예: 프로필 등록, 인증, 디바이스 연동, 병원 포털 연동, 온보딩 완료 라우팅)
- L3: 구현 레이어 단위 (API, Service, Data/DB, Frontend/View, DevOps, QA/Test)
- 이 단계에서는 **아직 "/api/onboarding/start Controller 구현"처럼 구체화하지 말고**, "온보딩 시작 API 레이어 구현" 수준으로만 쪼갠다.
- 작업 산출물: Epic별 상위 WBS 다이어그램 (문서 or Mermaid graph TD 기반 Tree).

## 6단계. Epic별 상세 Functional Task 추출 (입력/처리/출력/예외/설정/테스트)

- 각 Epic에 대해 다음 순서로 작업한다 (Epic 단위 워크숍처럼 반복).

1. **연관 REQ-FUNC 목록 정리**: 해당 Epic에 매핑된 REQ-FUNC-xxx 리스트를 정리.
2. **행동(Behavior) 단위로 분해**: 각 REQ를 `입력 / 처리 / 출력 / 예외 / 설정 / 테스트` 축으로 Sub-Task 후보를 쪼갠다.
3. **시퀀스 다이어그램 반영**: `3.4`, `6.3`의 시퀀스 다이어그램을 보며, 빠진 Sub-Task가 없는지 체크.
4. **API 레벨 매핑**: `6.1 API Endpoint List`를 참고해, Sub-Task를 구체 API(`/api/...`)와 연결.
5. **Acceptance Criteria → DoD 변환**: `4.1.2`의 AC를 각 Task의 완료 조건으로 체크리스트화.

- 각 Task는 Issue 템플릿(11번 문서 상단 템플릿)을 사용해 다음 필드를 포함하도록 설계한다.
- Summary: 기능명 + REQ ID (예: [E1][REQ-FUNC-001] 기본 프로필 생성 API 구현)
- Description: SRS 섹션 링크(REQ, 시퀀스, 데이터모델), 맥락 설명
- Acceptance Criteria (GWT / 체크리스트)
- Non-Functional Constraints (관련 REQ-NF 요약)
- Labels: Epic, Backend/Frontend/DevOps/Test, priority 등
- 작업 산출물: Epic별 Functional Task 리스트(이슈 초안) + 각 Task의 DoD 정의.

## 7단계. 데이터 모델 관점 Task 추출 (DB/ORM/Migration)

- `6.2 Entity & Data Model`을 기준으로, 주요 엔터티별 Task를 정리한다.
- User, OnboardingSession, HealthReport, ActionCard, FamilyBoard, ConsentRecord, AuditLog, DeviceLink, PortalConnection 등.
- 각 엔터티에 대해 공통 Task 세트를 정의한다.
- 테이블/스키마 설계 및 검토
- 인덱스/FK/제약조건 설계
- Migration 스크립트 작성
- ORM Entity/Repository 구현
- Seed/Fixture 데이터 작성
- 앞에서 정의한 Epic/REQ와 연결시켜, 중복 Task를 제거하고 참조를 추가한다.
- 예: `HealthReport` 관련 Task는 E2 (리포트 Epic)의 하위 Data/DB 작업으로 편입.
- 작업 산출물: 엔터티별 DB/ORM/Migration Task 목록 + Epic/REQ 매핑.

## 8단계. NFR(성능/보안/운영/접근성) 기반 횡단 Task 추출

- `4.2 Non-Functional Requirements`를 카테고리별로 다시 그룹화한다.
- 성능(Onboarding/Reports/Sync/KPI), 보안(Auth/암호화/감사로그), 모니터링/비용, 접근성(A11y), 확장성/유지보수성 등.
- 각 카테고리별로 **플랫폼/DevOps/QA Task 집합**을 정의한다.
- 성능: APM 설정, 부하 테스트 시나리오/스크립트, 캐싱/쿼리 최적화 Task
- 보안: TLS 설정, 암호화 모듈 적용, RBAC 설계, 감사 로그 파이프라인 구현
- 모니터링/비용: 대시보드 구성, 알람 룰, 비용 계측 태그/메트릭 정의
- 접근성: 스크린리더 라벨 점검, 포커스 관리, A11y 테스트 케이스 작성
- 각 NFR Task를 주요 Epic에 연결한다.
- 예: REQ-NF-001(성능) → E1/E2/E3, REQ-NF-006(보안) → E4/E6 등.
- 작업 산출물: NFR 기반 Cross-cutting Task 리스트 + Epic/REQ-NF 매핑.

## 9단계. Epic 내부/간 WBS Tree 및 Dependency Graph 확정

- 각 Epic별로, 앞에서 도출된 Functional + Data + NFR Task를 모두 포함하는 **상세 WBS Tree**를 만든다.
- L1 Epic → L2 Capability/Flow → L3 레이어(API/서비스/DB/FE/DevOps/QA) → L4 개별 Task.
- 동시에, **Task 간 선후관계 및 참조 관계 그래프**를 생성한다.
- 노드: 개별 Task (이슈 ID)
- 엣지: "A → B" = B는 A 완료 후 시작 가능 / 강한 의존 관계
- Epic 간 공통 인프라(예: Auth/Consent, Monitoring)는 별도 노드로 두고 다수 Epic에서 참조.
- Mermaid 등으로 대표 그래프를 남겨 개발/PM이 한눈에 볼 수 있게 한다.
- 작업 산출물: Epic별 WBS Tree 문서 + 전체 Dependency Graph.

## 10단계. 통합 MVP Backlog 구성 및 우선순위/마일스톤 설정

- 모든 Epic에서 도출된 Task를 하나의 **통합 MVP Backlog**로 모은다.
- 필수 메타데이터: Task ID, Epic, REQ-FUNC/REQ-NF, API, Entity, Layer, Type(입력/처리/출력/예외/설정/테스트), Story, Priority.
- Q1 Triplet 및 KPI, 기술 리스크를 기준으로 **우선순위와 마일스톤**을 정의한다.
- 예: M1(온보딩 + 리포트 최소 경로), M2(행동 코칭 + 가족 보드 베타), M3(NFR 하드닝 및 리포트/온보딩 튜닝).
- Backlog 상에서 Epic/Story/REQ 단위로 필터링이 가능하도록 뷰를 설계한다.
- 작업 산출물: 통합 MVP Backlog 뷰 + 마일스톤/릴리즈 계획.

## 11단계. Traceability & 변경 관리 체계 정리

- SRS ↔ REQ ↔ Epic ↔ Task ↔ Test Case 간 추적성을 유지하기 위한 규칙을 정의한다.
- 네이밍 규칙: Summary/Description에 REQ ID, Epic ID, Story ID 포함.
- 링크 규칙: SRS 섹션 앵커, API/ERD 문서 링크 통일.
- SRS 버전업(예: SRS_V0.3 → V0.4) 시 **어떤 절차로 Task/Backlog를 업데이트할지** 정의한다.
- 변경된 REQ만 Diff → 영향받는 Epic/Task 재평가.
- 작업 산출물: "요구사항 추적 및 변경 관리 가이드" 문서.