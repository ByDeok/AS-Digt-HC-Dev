# 액티브 시니어 디지털 헬스케어 MVP - Frontend Prototyping Prompt

이 문서는 **Firebase Studio** (또는 Cursor Composer, Windsurf 등의 AI 코딩 도구)를 사용하여 **"액티브 시니어 디지털 헬스케어"** 서비스의 MVP 프론트엔드 프로토타입을 빠르고 정확하게 구축하기 위한 프롬프트입니다.

이 프롬프트를 AI 에이전트에게 입력하여 프로젝트를 시작하세요.

---

## [Role Definition]
당신은 시니어 사용자 경험(UX)과 디지털 헬스케어 도메인에 특화된 **Senior Frontend Developer**입니다. 
가독성이 높고 직관적인 UI를 신속하게 프로토타이핑해야 하며, 실제 백엔드 연동 전 단계인 Mock Data 기반의 인터랙티브한 웹 애플리케이션을 구축해야 합니다.

## [Project Context]
- **서비스명**: 골든 웰니스 (Golden Wellness) - 가칭
- **타겟 유저**: IT 기기 사용이 낯선 65세 이상 액티브 시니어 및 그들의 자녀(보호자).
- **핵심 가치**: 
  1. **3분 온보딩**: 복잡한 절차 없이 빠르게 시작.
  2. **1장 요약 리포트**: 진료 시 의사에게 보여줄 수 있는 신뢰도 높은 데이터 요약.
  3. **오늘의 행동**: 하루 1~3개의 간단한 건강 미션 수행.
  4. **가족 연결**: 자녀가 부모님의 건강 상태를 모니터링.

## [Tech Stack]
- **Framework**: React (Vite)
- **Language**: TypeScript
- **Styling**: Tailwind CSS (필수: 모바일 퍼스트, 고대비/대글자 지원)
- **State Management**: Zustand (또는 React Context API)
- **Routing**: React Router DOM
- **Icons**: Lucide React
- **Charts**: Recharts (단순하고 직관적인 차트)
- **Motion**: Framer Motion (부드러운 전환 및 피드백)
- **Backend(Simulated)**: Firebase (Auth/Firestore) 구조를 따르는 Mock Data 함수

---

## [Design System Guidelines] (Senior-Friendly)
모든 UI 구현 시 아래 규칙을 **강제(Must)**로 적용하세요:
1. **Typography**: 본문 기본 폰트 크기 `16px` 이상, 주요 버튼 텍스트 `18px` 이상.
2. **Color**: 
   - Primary: 신뢰감을 주는 Blue/Teal 계열.
   - Text: `#111111` (Black) 또는 `#F7F7F7` (White) - 명도 대비 4.5:1 이상 유지.
   - Error/Alert: 명확한 Red/Orange.
3. **Layout**: 
   - 큼직한 버튼 (최소 높이 `56px`).
   - 한 화면에 하나의 주요 행동(One Action per Screen).
   - 복잡한 햄버거 메뉴 대신 하단 탭 바(Bottom Navigation) 사용.
4. **Feedback**: 버튼 클릭 시 확실한 시각적 피드백(색상 변경, 리플 효과) 제공.

---

## [Data Model Suggestion] (Mock Firestore Schema)
프로토타입 구동을 위해 아래와 같은 데이터 구조를 `mockData.ts`에 정의하고 사용하세요.

```typescript
// User Profile
interface User {
  uid: string;
  name: string; // "김철수"
  age: number;
  conditions: string[]; // ["고혈압", "당뇨"]
  role: 'senior' | 'caregiver';
  connectedDevices: string[]; // ["Galaxy Watch 6", "Omron BP"]
}

// Daily Mission
interface Mission {
  id: string;
  title: string; // "아침 약 드시기"
  isCompleted: boolean;
  type: 'medication' | 'exercise' | 'measure';
  date: string; // "2025-11-20"
}

// Health Metrics (Report)
interface HealthMetric {
  date: string;
  systolic?: number; // 수축기 혈압
  diastolic?: number; // 이완기 혈압
  glucose?: number; // 혈당
  steps?: number; // 걸음 수
  source: string; // "manual" | "device"
}
```

---

## [Implementation Steps] (Task by Task)

아래 순서대로 기능을 구현해주세요. 각 단계가 끝날 때마다 사용자가 테스트할 수 있는 상태여야 합니다.

### Step 1: 온보딩 (EPIC0-FE-001, FE-002)
**목표**: 앱 진입부터 메인 화면 도달까지의 과정을 구현합니다.
1. **Landing Page**: 앱 로고와 "시작하기" 큰 버튼.
2. **Auth Simulation**: 카카오/네이버 로그인 버튼 (클릭 시 1초 후 로그인 성공 처리).
3. **Profile Form**:
   - 이름 입력 -> 생년월일 입력 -> 성별/질환 선택 (Step-by-Step UI).
   - 입력 폼은 크고 명확하게(Input 높이 56px+).
4. **Device Connect**:
   - "사용 중인 기기를 선택하세요" (워치, 혈압계 아이콘 그리드).
   - 연동 클릭 시 `setTimeout`으로 2초간 "연동 중..." 로딩 표시 후 성공 체크.
   - 병원 연동은 "건너뛰기" 버튼 제공.
5. **Complete**: "준비 완료" 화면 후 메인으로 이동.

### Step 2: 메인 대시보드 & 오늘의 행동 (EPIC0-FE-004)
**목표**: 사용자가 매일 접하는 홈 화면을 구현합니다.
1. **Layout**: 상단 헤더(안녕하세요 김철수님), 메인 컨텐츠, 하단 GNB(홈, 리포트, 가족, 설정).
2. **Today's Mission Card**:
   - "오늘 할 일 3개 남았어요" 문구.
   - 카드 리스트: [아침 약], [혈압 측정], [30분 걷기].
   - 카드 클릭 시: 체크박스 활성화 + 축하 애니메이션(Confetti or Motion) + "참 잘했어요!" 토스트 메시지.
3. **Notification Mock**: 우상단 벨 아이콘 클릭 시 "지금 혈압 잴 시간입니다" 알림 리스트 노출.

### Step 3: 1장 의사용 리포트 (EPIC0-FE-003)
**목표**: 의사에게 보여줄 데이터를 시각화합니다.
1. **Entry**: 하단 '리포트' 탭 클릭.
2. **UI Structure**: A4 용지 비율을 고려한 반응형 컨테이너.
   - **Header**: 환자 정보, 조회 기간(최근 3개월).
   - **Body**: 
     - 혈압/혈당 추세 그래프 (Recharts LineChart 사용, X축: 날짜, Y축: 수치).
     - 정상 범위 이탈 시 붉은색 포인트로 강조.
   - **Footer**: "의사 선생님께 보여주세요" 문구.
3. **Actions**: "PDF 저장" 또는 "인쇄하기" 버튼 클릭 시 `window.print()` 호출.

### Step 4: 가족 공유 보드 (EPIC0-FE-005)
**목표**: 보호자 관점의 화면을 구현합니다.
1. **Entry**: 하단 '가족' 탭 클릭.
2. **Member List**: 나(관리자), 어머니(대상자), 동생(뷰어) 프로필 리스트.
3. **Invite UI**: "가족 초대하기" 버튼 -> 클릭 시 "초대 링크가 복사되었습니다" 토스트.
4. **Activity Feed**: 타임라인 형태로 "어머니가 방금 혈압을 측정하셨습니다(120/80)", "아침 약 복용 완료" 로그 표시.

---

## [Execution Command]
위 내용을 바탕으로 지금 바로 프로젝트 구조를 잡고, **Step 1: 온보딩** 단계부터 코드를 작성해주세요.
필요한 라이브러리(`lucide-react`, `recharts`, `framer-motion`, `clsx`, `tailwind-merge` 등)는 설치 명령어를 제안해주세요.
