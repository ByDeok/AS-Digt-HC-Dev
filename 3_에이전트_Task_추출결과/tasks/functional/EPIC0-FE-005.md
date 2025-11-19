# EPIC0-FE-005: 가족 보드 및 초대 UI PoC

## 1. 목적 및 요약
- **목적**: 보호자(자녀)가 시니어의 건강 정보를 공유받고 역할을 설정하는 '가족 보드'의 프론트엔드 뷰를 구현한다.
- **요약**: 가족 구성원 목록 표시, 초대 링크 생성(UI만), 구성원별 역할(보기/수정) 표시 기능을 포함한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - 가족 보드 메인 화면 (연동된 시니어 프로필, 구성원 리스트)
  - "가족 초대하기" 버튼 및 공유 UI
  - 구성원 역할 변경 드롭다운 (뷰어/관리자)
  - 시니어 활동 로그 피드 (예: "어머니가 아침 약을 드셨습니다")
- **Out-of-Scope**:
  - 실제 초대 링크 발송 (카카오톡 API 등)
  - 실시간 동기화 (WebSocket)

## 3. 주요 화면 및 흐름
1. **가족 보드 진입**: 메뉴 -> 가족 관리.
2. **리스트**: "나(관리자)", "어머니(대상자)", "동생(뷰어)" 목록 표시.
3. **초대**: "초대하기" -> "링크 복사됨" 알림.
4. **활동 피드**: 하단에 시니어의 주요 행동(약 복용, 걷기 완료) 로그가 타임라인으로 표시됨.

## 4. 개발 주안점
- **정보 계층**: 시니어의 상태가 가장 잘 보여야 하며, 가족 구성원 관리는 부가적인 기능으로 배치.
- **프라이버시 느낌**: 누가 내 정보를 보고 있는지 명확하게 인지할 수 있는 UI.

---

```yaml
task_id: "EPIC0-FE-005"
title: "가족 보드 뷰 및 초대/역할 관리 UI PoC"
summary: >
  시니어와 보호자가 정보를 공유하는 가족 보드 화면을 구현한다.
  구성원 목록, 역할 표시, 초대 버튼, 그리고 시니어 활동 로그 피드를 포함한다.
type: "functional"

epic: "EPIC_0_FE_PROTOTYPE"
req_ids: ["REQ-FUNC-015", "REQ-FUNC-016", "REQ-FUNC-017"]
component: ["frontend.ui"]

context:
  srs_section: "1.2.1 Family Board"

inputs:
  description: "가족 구성원 및 활동 로그 Mock Data"

outputs:
  description: "가족 보드 화면 렌더링"

steps_hint:
  - "가족 구성원 리스트 컴포넌트 (Avatar, Name, Role Badge) 구현"
  - "초대하기 버튼 클릭 시 클립보드 복사(navigator.clipboard) 시뮬레이션"
  - "활동 로그(Timeline) 컴포넌트 구현"
  - "역할 변경 드롭다운 UI 구현 (실제 권한 체크는 제외)"

preconditions:
  - "EPIC0-FE-002 완료"

postconditions:
  - "가족 보드에서 구성원 목록과 활동 로그를 확인할 수 있음"

dependencies: ["EPIC0-FE-002"]

parallelizable: true
estimated_effort: "S"
priority: "Should"
agent_profile: ["frontend"]

required_tools:
  frameworks: ["React"]
```

