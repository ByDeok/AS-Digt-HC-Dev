# AI Agent Task Usage Guide

## 1. 개요
본 문서는 `3_Agent_Task_Results` 디렉토리에 저장된 Task 정의서들을 **AI 에이전틱 프로그래밍(AI Agentic Programming)** 워크플로우에서 어떻게 활용해야 하는지 설명합니다. 이 Task 정의서들은 사람 개발자와 AI 에이전트가 협업하기 위한 **공통 프로토콜** 역할을 합니다.

## 2. Task 정의서 구조 (Schema)
모든 Task 파일(`.md`)은 두 부분으로 나뉩니다.

1. **Human-Readable Section (상단)**
   - 개발자나 PM이 읽고 검토하는 영역입니다.
   - 목적, 범위, 화면 흐름, 주요 개발 포인트가 자연어로 기술되어 있습니다.
   - 에이전트에게 "이 Task의 맥락(Context)"을 이해시키는 용도로 프롬프트에 포함될 수 있습니다.

2. **Machine-Readable YAML Block (하단)**
   - AI 에이전트 오케스트레이터가 파싱하여 **자동화된 실행 계획**을 수립하는 데이터입니다.
   - `task_id`, `req_ids` (SRS 추적), `inputs/outputs`, `steps_hint` (구현 가이드), `dependencies` (실행 순서)가 포함됩니다.

## 3. 에이전트 활용 시나리오

### 시나리오 A: 단일 Task 구현 요청
개발자가 특정 기능 하나를 에이전트에게 위임할 때 사용합니다.

> **User Prompt 예시**:
> "REQ-FUNC-001-BE-01.md 파일의 내용을 바탕으로, Spring Boot 프로젝트에 회원가입 API를 구현해줘. YAML 블록의 steps_hint를 준수해야 해."

### 시나리오 B: 워크플로우 자동화 (Orchestrator)
여러 에이전트가 협업하는 경우, 메인 에이전트(Manager)는 YAML 블록을 읽어 작업을 분배합니다.

1. **Task Loading**: `tasks/**/*.md` 파일을 스캔하여 YAML 데이터를 로드합니다.
2. **Dependency Resolution**: `dependencies` 필드를 분석하여 실행 가능한(Blocked되지 않은) Task 목록을 추출합니다. (DAG 참조)
3. **Agent Routing**: `agent_profile` 필드를 보고 적절한 에이전트에게 할당합니다.
   - `["frontend"]` -> React/Vite 전문 에이전트
   - `["backend"]` -> Spring Boot/Java 전문 에이전트
4. **Execution & Verification**:
   - 에이전트는 `steps_hint`에 따라 코드를 작성합니다.
   - 작업 완료 후 `postconditions` 항목을 체크리스트로 사용하여 자가 검증(Self-Correction)을 수행합니다.

## 4. 버전 관리 및 동기화
- **SRS 변경 시**: SRS 요구사항이 변경되면 관련 `req_ids`를 가진 Task 파일을 찾아 수정해야 합니다.
- **Code 변경 시**: 구현 중 발견된 제약사항이나 기술적 변경점은 Task 파일의 `risk_notes`나 `steps_hint`에 업데이트하여 기록으로 남깁니다.

## 5. 디렉토리 구조
- `tasks/functional/`: 기능 요구사항(REQ-FUNC, EPIC) 기반 Task
- `tasks/non-functional/`: 비기능 요구사항(REQ-NF) 기반 Task
- `docs/`: WBS, DAG, 가이드 문서

## 6. 주의사항
- **Mock Data 활용**: 초기 개발 단계(특히 Epic 0)에서는 실제 외부 연동 대신 Task 정의서에 명시된 Mock Data 구조를 적극 활용하십시오.
- **한글 경로 이슈**: 파일 시스템 호환성을 위해 디렉토리명은 영문(`3_Agent_Task_Results`)을 권장합니다.
```

