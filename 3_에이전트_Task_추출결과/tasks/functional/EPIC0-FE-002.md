# EPIC0-FE-002: 온보딩 - 디바이스/병원 연동 및 완료 UI PoC

## 1. 목적 및 요약
- **목적**: 온보딩 과정 중 가장 이탈률이 높을 수 있는 외부 기기(워치, 혈압계) 및 병원 포털 연동 과정을 시각화하고, 3분 내 완료 경험을 제공하는 UI를 구현한다.
- **요약**: OAuth 연동 및 데이터 수집 대기 시간을 시각적으로 지루하지 않게 표현하고, 연동 실패/건너뛰기 처리를 포함한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - 디바이스 선택 및 연동 요청 UI (삼성/애플 워치 등 아이콘)
  - 병원 포털 연동 및 데이터 조회 로딩 애니메이션 (Progress Bar/Spinner)
  - 연동 성공/실패 모의 결과 표시
  - 온보딩 완료 축하 화면 및 메인(리포트) 진입 버튼
- **Out-of-Scope**:
  - 실제 제조사/병원 API 통신
  - 블루투스 페어링 로직
  - 실제 데이터 파싱

## 3. 주요 화면 및 흐름
1. **기기 연동**: "사용 중인 기기를 선택하세요" -> 워치/혈압계 아이콘 선택 -> "연동하기" 버튼.
2. **연동 시뮬레이션**: 팝업 창 뜨고 2초 후 "연동 성공" 체크 표시.
3. **병원 연동**: "다니시는 병원 기록을 불러올까요?" -> "불러오기" or "건너뛰기".
4. **로딩/대기**: "최근 건강 기록을 분석 중입니다..." (진행률 표시, 3~5초).
5. **완료**: "준비가 완료되었습니다! 첫 리포트를 확인해보세요."

## 4. 개발 주안점
- **피드백**: 사용자가 기다리는 동안 시스템이 작업 중임을 명확히 인지하도록 애니메이션/문구 제공.
- **예외 처리 UI**: 연동 실패 시 "다음에 하기" 또는 "수기 입력" 옵션 노출 확인.
- **Mocking**: `setTimeout`을 활용하여 API 지연 시간을 시뮬레이션.

---

```yaml
task_id: "EPIC0-FE-002"
title: "온보딩 - 디바이스/병원 연동 시뮬레이션 및 완료 UI PoC"
summary: >
  온보딩 후반부의 외부 연동 흐름을 구현한다.
  비동기 로딩 상태, 성공/실패 피드백, 스킵 처리를 포함하여 사용자가 최종 완료 화면에 도달하게 한다.
type: "functional"

epic: "EPIC_0_FE_PROTOTYPE"
req_ids: ["REQ-FUNC-003", "REQ-FUNC-004", "REQ-FUNC-005", "REQ-FUNC-006"]
component: ["frontend.ui", "frontend.state"]

context:
  srs_section: "3.4.1 핵심 온보딩 플로우"

inputs:
  description: "사용자의 연동 동의 및 기기 선택 액션"
  fields:
    - name: "selected_devices"
      type: "array"
    - name: "hospital_consent"
      type: "boolean"

outputs:
  description: "연동 상태 업데이트 및 온보딩 완료 플래그 설정"
  success:
    state_update: "is_onboarding_completed = true"

steps_hint:
  - "디바이스 선택 그리드 UI 컴포넌트 구현"
  - "가짜 비동기 함수(mockAsyncOperation)를 만들어 연동 로딩 상태(Loading/Success/Error) 관리"
  - "로딩 중 진행률(Progress Bar) 및 친절한 안내 문구 표시"
  - "병원 연동 건너뛰기 시 대체 흐름(수기 입력 안내 팝업 등) 구현"
  - "최종 완료 화면에서 메인 대시보드로 이동하는 라우팅 처리"

preconditions:
  - "EPIC0-FE-001에서 프로필 입력 데이터가 스토어에 존재해야 함"

postconditions:
  - "사용자가 온보딩을 마치고 메인 화면으로 진입 가능"
  - "연동된 기기 목록이 상태 관리 스토어에 저장됨"

dependencies: ["EPIC0-FE-001"]

parallelizable: false
estimated_effort: "S"
priority: "Must"
agent_profile: ["frontend"]

required_tools:
  frameworks: ["React", "Vite"]
```

