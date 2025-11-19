# EPIC0-FE-003: 1장 의사용 요약 리포트 UI PoC

## 1. 목적 및 요약
- **목적**: 액티브 시니어 헬스케어의 핵심 가치인 '의사에게 보여주는 1장 요약 리포트'를 웹/모바일 화면에서 렌더링하고, PDF 형식으로 미리보는 기능을 구현한다.
- **요약**: 수집된(Mock) 데이터를 바탕으로 주요 지표(혈압, 혈당, 보행 등)를 그래프와 표로 시각화하고, 의사가 보기 편한 레이아웃을 구성한다.

## 2. 범위 (Scope)
- **In-Scope**:
  - 리포트 메인 대시보드 (카드 형태의 요약 정보)
  - 상세 리포트 뷰 (기간별 추세 그래프, A4 비율 레이아웃)
  - PDF 내보내기/공유 버튼 (실제 생성 대신 UI 및 Mock 동작)
  - 데이터 출처/결측치 태그 표시 UI
- **Out-of-Scope**:
  - 실제 PDF 생성 라이브러리 연동 (브라우저 인쇄 기능 활용 정도로 갈음)
  - 복잡한 차트 인터랙션 (Zoom/Pan)

## 3. 주요 화면 및 흐름
1. **메인 홈**: "김철수님의 11월 건강 요약" 카드 클릭.
2. **리포트 뷰어**: 
   - 상단: 환자 정보, 조회 기간, 병원명.
   - 중단: 주요 바이탈(혈압/당뇨) 추세선 그래프, 이상 수치 하이라이트.
   - 하단: "의사 선생님께 보여주세요" 버튼.
3. **공유/인쇄**: 버튼 클릭 시 시스템 공유 시트 또는 브라우저 인쇄 창 호출.

## 4. 개발 주안점
- **레이아웃**: 모바일에서도 잘 보이지만, 의사에게 보여줄 때는 태블릿/PC 포맷(A4)과 유사한 밀도로 정보를 보여주는 반응형 처리.
- **데이터 시각화**: Chart.js 또는 Recharts 등을 활용하여 시니어/의사가 직관적으로 이해할 수 있는 단순한 차트 구현.
- **신뢰성**: 데이터의 출처(예: Galaxy Watch, Omron)와 측정 시간 표시.

---

```yaml
task_id: "EPIC0-FE-003"
title: "1장 의사용 요약 리포트 뷰 및 데이터 시각화 UI PoC"
summary: >
  수집된 건강 데이터를 의사가 확인하기 좋은 A4 형태의 1장 리포트 UI로 렌더링한다.
  그래프와 표를 포함하며, 결측치 및 출처 태그를 표시한다.
type: "functional"

epic: "EPIC_0_FE_PROTOTYPE"
req_ids: ["REQ-FUNC-007", "REQ-FUNC-008", "REQ-FUNC-009", "REQ-FUNC-010"]
component: ["frontend.ui", "frontend.chart"]

context:
  srs_section: "3.4.2 진료 전 1장 리포트 생성 및 사용"
  wireframes: ["WF-REPORT-01"]

inputs:
  description: "Mock 건강 데이터 (JSON)"
  fields:
    - name: "health_metrics"
      type: "json"
      example: "{ blood_pressure: [{sys: 120, dia: 80, date: '2025-11-01'}], ... }"

outputs:
  description: "리포트 화면 렌더링 및 인쇄 트리거"

steps_hint:
  - "Mock Data(JSON) 정의 (지난 3개월 혈압/혈당/보행 데이터)"
  - "Recharts/Chart.js 설치 및 추세선 그래프 컴포넌트 구현"
  - "CSS Grid를 활용하여 A4 비율을 고려한 리포트 레이아웃(Header, Body, Footer) 구성"
  - "측정 기기 아이콘 및 결측 데이터('측정 안됨') 표시 로직 추가"
  - "상단 'PDF 저장/인쇄' 버튼에 `window.print()` 연결하여 브라우저 기본 인쇄 기능 테스트"

preconditions:
  - "EPIC0-FE-002 완료 (메인 진입 가능)"

postconditions:
  - "사용자가 리포트 화면을 열고 그래프를 확인할 수 있음"
  - "인쇄 버튼 클릭 시 인쇄 미리보기 창이 뜸"

dependencies: ["EPIC0-FE-002"]

parallelizable: true
estimated_effort: "M"
priority: "Must"
agent_profile: ["frontend"]

required_tools:
  frameworks: ["React", "Recharts"]
```

