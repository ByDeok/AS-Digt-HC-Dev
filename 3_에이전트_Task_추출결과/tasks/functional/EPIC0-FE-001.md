# EPIC0-FE-001: 온보딩 - 랜딩 및 프로필 입력 UI PoC

## 1. 목적 및 요약
- **목적**: 신규 사용자가 앱 설치 후 처음 마주하는 랜딩 페이지와 프로필(이름, 나이, 성별, 질환) 입력 과정을 검증하기 위한 프론트엔드 프로토타입을 구현한다.
- **요약**: 3분 온보딩의 첫 단계로, 사용자 이탈을 최소화하는 직관적인 UI와 접근성(큰 글씨)을 고려한 입력 폼을 제공한다. 실제 백엔드 연동 대신 Mock API를 사용한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - 앱 랜딩 화면 (로그인/회원가입 진입)
  - 소셜 로그인/본인인증 시뮬레이션 UI (버튼 및 단순 상태 전환)
  - 기본 프로필 입력 폼 (이름, 생년월일, 성별, 주요 질환 선택)
  - 접근성 모드 토글 (UI 확인용)
- **Out-of-Scope**:
  - 실제 소셜 로그인/본인인증 연동 (OAuth/SMS)
  - 백엔드 프로필 저장 API 연동 (Local State/Context 사용)
  - 약관 상세 내용 팝업 (더미 텍스트 처리)

## 3. 주요 화면 및 흐름 (Wireframe Hint)
1. **랜딩**: 로고, "시작하기" 버튼 (큰 글씨).
2. **인증 선택**: 카카오/네이버/휴대폰 아이콘 (클릭 시 "인증 완료" 토스트 후 다음 단계).
3. **프로필 입력 1**: 이름, 생년월일 입력 (숫자 키패드).
4. **프로필 입력 2**: 성별, 질환(고혈압/당뇨 등 체크박스) 선택.
5. **완료/다음**: "다음 단계로" 버튼 활성화.

## 4. 개발 주안점
- **접근성**: 모든 텍스트는 기본 1.2배 크기로 설정, 명도 대비 4.5:1 이상 준수.
- **UX**: 입력 폼은 한 화면에 하나씩(Step-by-Step) 노출하여 인지 부하 감소.
- **Mock Data**: 사용자 입력 데이터는 전역 상태(Zustand/Context)에 임시 저장하여 다음 Task(연동)로 전달.

---

```yaml
task_id: "EPIC0-FE-001"
title: "온보딩 - 랜딩 및 프로필 입력/인증 시뮬레이션 UI PoC"
summary: >
  앱의 첫 진입점인 랜딩 페이지와 사용자 프로필(나이/성별/질환) 입력 폼을 구현한다.
  실제 인증 없이 상태만 시뮬레이션하며, 시니어 친화적인 큰 글씨 UI를 적용한다.
type: "functional"

epic: "EPIC_0_FE_PROTOTYPE"
req_ids: ["REQ-FUNC-001", "REQ-FUNC-002"]
component: ["frontend.ui", "frontend.state"]

context:
  srs_section: "3.4.1 핵심 온보딩 플로우"
  wireframes: ["WF-ONBOARD-01", "WF-ONBOARD-02"]

inputs:
  description: "사용자 터치 입력 (버튼 클릭, 텍스트 입력)"
  fields:
    - name: "auth_provider"
      type: "enum"
    - name: "user_profile"
      type: "object"
      properties: ["name", "age", "gender", "conditions"]

outputs:
  description: "입력된 프로필 데이터가 메모리상에 저장되고 다음 단계(디바이스 연동)로 라우팅됨"
  success:
    state_update: "onboardingStep = DEVICE_CONNECT"

steps_hint:
  - "React Router를 사용하여 온보딩 단계별 라우트(/onboarding/intro, /profile) 설정"
  - "공통 레이아웃 컴포넌트(큰 폰트, 고대비 스타일 적용) 구현"
  - "Zustand 또는 Context API로 온보딩 상태 관리 스토어 생성"
  - "인증 버튼 클릭 시 1초 딜레이 후 인증 완료 상태로 변경하는 Mock 로직 구현"
  - "프로필 입력 폼 구현 및 유효성 검사(필수 항목 확인)"

preconditions:
  - "React + Vite 프로젝트 기본 세팅 완료"
  - "Tailwind CSS 또는 스타일링 라이브러리 설정 완료"

postconditions:
  - "사용자가 앱 실행 후 프로필 입력까지 막힘없이 진행 가능"
  - "입력된 데이터가 스토어에 유지됨"

config:
  feature_flags:
    - "mock_auth_mode: true"

dependencies: []

parallelizable: true
estimated_effort: "S"
priority: "Must"
agent_profile: ["frontend"]

required_tools:
  frameworks: ["React", "Vite", "TailwindCSS"]
```

