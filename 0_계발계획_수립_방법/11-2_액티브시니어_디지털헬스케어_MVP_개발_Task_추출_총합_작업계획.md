# **MVP 개발 Task 추출 총합 작업계획**

# MVP 개발 Task 추출 총합 작업계획 (액티브시니어 디지털헬스케어)

## 목표

- `GPT-SRS/SRS_V0.3.md` SRS와 `0_계발계획_수립_방법/11-2_AI_에이전틱_프로그래밍을_위한_풀버전_Task_정의서_작성하기.md`를 **공동 기준 문서**로 삼아,
- **액티브시니어 디지털헬스케어 플랫폼** **MVP 범위의 모든 개발 Task**를 추출·구조화한다.
- Task는 **SRS 기반 REQ-FUNC/REQ-NF 정합성**을 유지하면서도, 동시에 **AI 에이전트가 바로 쓸 수 있는 Task 템플릿(YAML 메타데이터 포함)**을 따른다.
- 최종적으로는 이 문서에서 추출된 Task들을 기반으로 **통합 Task Tree(WBS)**와 **통합 의존성 그래프(DAG)**(6, 7단계)를 구성한다.

---

## 단계 0. 기준·참조 문서 및 ID 체계 정리

- **기준 문서 재확인**
    - SRS: `GPT-SRS/SRS_V0.3.md` (REQ-FUNC / REQ-NF / 시퀀스 / API / 엔티티)
    - AI 에이전틱 Task 템플릿: `0_계발계획_수립_방법/11-2_AI_에이전틱_프로그래밍을_위한_풀버전_Task_정의서_작성하기.md`
    - 핵심 분석 자료: `2_핵심예제 분석자료/` 내 페르소나, JTBD, AOS-DOS 매트릭스 등
- **ID 및 태깅 규칙 정합**
    - SRS의 `REQ-FUNC-XXX`, `REQ-NF-XXX`와 일관되게 연동되는 `task_id` 규칙 정의
        - 예: `REQ-FUNC-001-BE-01`, `REQ-NF-003-FE-01`, `EPIC0-FE-001` 등
    - `epic`, `feature`, `agent_profile`, `parallelizable`, `priority` 등 메타데이터 값 체계를 템플릿 문서 기준으로 확정.
- **파일/디렉터리 구조 합의**
    - 기능 요구사항 Task: `tasks/functional/REQ-FUNC-XXX.md`
    - 비기능 요구사항 Task: `tasks/non-functional/REQ-NF-XXX.md`
    - 프론트엔드 PoC(Epic 0) Task: `tasks/functional/EPIC0-FE-XXX.md` 등으로 정리.

---

## 단계 1. MVP 범위 SRS 요구사항 선별 및 매핑 기초 작업

- **MVP 범위 REQ-FUNC/REQ-NF 목록화**
    - `GPT-SRS/SRS_V0.3.md`의 **1.6 MVP Phase Definition**에 따라 MVP 필수 포함 항목을 선별.
        - **3분 온보딩**: REQ-FUNC-001~006, 019 (F1)
        - **1장 의사용 요약 리포트**: REQ-FUNC-007~010 (F2, F7, F10)
        - **오늘 1~3개 행동 코칭**: REQ-FUNC-011~014 (F3, F9)
        - **가족 보드 및 대리 접근**: REQ-FUNC-015~018 (F4, F8)
    - Post-MVP 대상(예: 고급 AI 코치, 완전한 EMR 연동, 워치 2종 동시 정합성 검증 등)은 별도 테이블로 분리.
- **요구사항-도큐먼트 매핑 시트 생성**
    - 각 REQ별로 PRD User Story, SRS 섹션(3.4 Sequence), API 엔드포인트(6.1), 엔티티(6.2)를 열로 두고 1차 매핑 테이블 작성.
    - 이 매핑 테이블은 이후 Task 정의서의 `req_ids`, `context.srs_section`, `context.apis`, `context.entities` 필드의 입력 소스로 활용.
- **Epic/Feature 거시 구조 스케치**
    - SRS의 Traceability Matrix(5.0)를 참고하여 MVP의 핵심 4대 Epic(온보딩, 리포트, 행동코칭, 가족보드)을 정의.
    - 각 Epic에 속하는 REQ-FUNC/REQ-NF 그룹핑.

---

## 단계 2. AI 에이전틱 Task 템플릿 적용 및 스키마 검증

- **기능(REQ-FUNC)용 템플릿 적용**
    - Task 템플릿의 필수 필드(`inputs`, `outputs`, `steps_hint`, `preconditions`)를 액티브시니어 헬스케어 도메인에 맞게 구체화.
        - 예: `inputs`에 "사용자 최근 6개월 검사 데이터(CSV/JSON)", `outputs`에 "1장 리포트 PDF URL" 등 구체적 명시.
- **비기능(REQ-NF)용 템플릿 적용**
    - 헬스케어/시니어 도메인 특화 카테고리/레이블 정의.
        - `accessibility:senior_ui` (대글자/고대비), `security:phi` (개인건강정보 보호), `performance:latency` (3분 온보딩 타임아웃 방지) 등.
- **Task 메타데이터 규칙 정리**
    - `agent_profile`: `["frontend"]` (React/Vite), `["backend"]` (Spring Boot), `["devops"]`, `["data"]` (Data/Python)
    - `estimated_effort`: `S/M/L` (Story Point 1/3/5 기준)
    - `priority`: `P0` (MVP Must), `P1` (Should), `P2` (Could)

---

## 단계 3. Epic 0 – 프론트엔드 PoC 프로토타입 Task 추출

- **대상 플로우 및 화면 선정**
    - MVP의 핵심 가치인 **'3분 온보딩'**과 **'1장 의사용 리포트'** 화면을 최우선 PoC 대상으로 선정.
    - 사용자 여정 지도(`06_...clean.md`)의 "가입/연동" 및 "진료실 제시" 단계를 커버하는 UI 스켈레톤.
- **Epic 0 Task 헤더 목록 작성**
    - `task_id`: `EPIC0-FE-XXX`
    - `title`: 예) "온보딩-프로필 입력 및 약관 동의 UI", "1장 리포트 미리보기 및 PDF 렌더링 UI"
- **PoC 수준 Task 상세 정의 작성**
    - Out of Scope: 실제 병원 API 연동, 복잡한 예외 처리, 실제 OAuth 토큰 교환(Mocking 처리).
    - In Scope: 화면 레이아웃, 대글자/고대비 모드 전환(접근성), 주요 버튼 인터랙션, Mock Data 바인딩.
    - YAML 필드 설정:
        - `epic: "EPIC_0_FE_POC"`
        - `agent_profile: ["frontend"]`

---

## 단계 4. REQ-FUNC 기반 전체 개발 Task 추출

- **REQ-FUNC별 Task 묶음 설계**
    - SRS의 각 REQ-FUNC(001~019)를 구현하기 위한 End-to-End Task 묶음 정의.
    - **Input/상태 준비**: 예) "사용자 프로필 및 동의 정보 수신"
    - **핵심 처리 로직**: 예) "건강 데이터 집계 및 리포트 스코어 계산 로직 구현"
    - **Output/저장**: 예) "리포트 엔티티 저장 및 PDF 생성 비동기 요청"
- **시퀀스/엔드포인트/엔티티 매핑 반영**
    - SRS 3.4.1(온보딩), 3.4.2(리포트), 3.4.3(연동) 시퀀스 다이어그램의 각 Step을 Task Step으로 변환.
    - API 엔드포인트(6.1)와 엔티티 모델(6.2 User, HealthReport, DeviceLink 등)을 Task 컨텍스트에 명시.
- **컴포넌트 축 분리**
    - **Frontend**: React, PWA, Accessibility UI 구현
    - **Backend**: Spring Boot, REST API, DB 트랜잭션, 배치 스케줄러(알림)
    - **Infra/DevOps**: CI/CD, DB 구축, 로깅/모니터링 환경

---

## 단계 5. REQ-NF 및 횡단 관심사 기반 Task 추출

- **REQ-NF 목록화 및 REQ-FUNC 연계**
    - SRS 4.2 REQ-NF(001~019)를 기반으로 성능, 보안, 접근성, KPI 측정 Task 도출.
    - 예: `REQ-NF-003`(온보딩 3분 제한) → "온보딩 단계별 체류 시간 로깅 및 이탈 지점 추적 구현"
- **카테고리/레이블 설계 및 적용**
    - **접근성(A11y)**: `REQ-NF-008` 대응, "스크린리더 호환성 테스트 및 ARIA 라벨 적용"
    - **보안(Security)**: `REQ-NF-006, 007` 대응, "민감정보(PHI) 암호화 저장 및 2FA 로직 구현"
    - **모니터링/KPI**: `REQ-NF-010~016` 대응, "핵심 KPI(D1/W1 완료율, 리포트 생성률) 대시보드 구성"
- **DevOps/QA/운영 Task 도출**
    - 부하 테스트(k6) 스크립트 작성 (Report 생성 부하 등)
    - 개인정보 보호 감사 로그 시스템 구축

---

## 단계 6. 통합 Task Tree(WBS) 작성

- 추출된 Task를 구조화하여 WBS(Work Breakdown Structure) 작성:
    - **Level 1 (Epic)**: 온보딩 / 리포트 / 행동코칭 / 가족보드 / 인프라&기타
    - **Level 2 (Feature)**: 프로필 설정, 디바이스 연동, 리포트 생성, 알림 발송 등
    - **Level 3 (Task)**: [BE] API 구현, [FE] UI 구현, [DB] 스키마 설계, [QA] TC 작성

---

## 단계 7. 통합 의존성 그래프(DAG) 작성

- Task 간 선후행 관계 정의:
    - `TASK-DB-SCHEMA` → `TASK-BE-API` → `TASK-FE-UI` → `TASK-E2E-TEST`
    - "온보딩(User/DeviceLink) 구현" → "리포트 생성(데이터 수집) 구현"
- 의존성 시각화(Mermaid)를 통해 병렬 개발 가능 구간(예: FE UI와 BE API 병렬 진행, Stub 활용) 식별.
- Critical Path(최장 경로) 도출: 온보딩 → 데이터 수집 → 리포트 생성 → 리포트 뷰.

---

## 단계 8. Task 정의 품질 점검

- **스키마 및 일관성 점검**
    - 모든 Task가 템플릿의 필수 필드(`task_id`, `req_ids`, `context`)를 포함하는지 확인.
    - SRS v0.3의 최신 변경 사항(MVP 범위 축소 등)이 Task에 올바르게 반영되었는지 검토.
- **Human-Agent 가독성 점검**
    - Task 설명이 AI 에이전트가 이해하고 코드를 생성할 수 있을 만큼 구체적인지(Steps Hint 제공 여부) 확인.

---

## 단계 9. 도구 연동 및 PRD-SRS-Task 변경 관리

- **실제 작업 도구와 연동**
    - GitHub Issues/Projects 등에 Label/Milestone과 함께 Task 업로드.
    - SRS 버전 업데이트 시 Task 수정 플로우(Change Request) 수립.
- **변경 추적 관리**
    - SRS의 `REQ-FUNC` 변경 시 관련 `req_ids`를 가진 Task를 찾아 영향도 분석 및 수정.

