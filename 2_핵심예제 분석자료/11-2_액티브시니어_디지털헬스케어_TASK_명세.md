### E1. 3분 온보딩 (3-Minute Onboarding)

- **E1-T01**: 온보딩 단계/퍼널 설계 및 상태 정의 (Layer: Design, Type: Config, REQ: REQ-FUNC-001~006,019)  
- **E1-T02**: `/api/onboarding/start` Request DTO·Validation 설계 (Layer: Backend, Type: Input, REQ: REQ-FUNC-001,005)  
- **E1-T03**: 온보딩 세션 생성 Service 및 ETA/step 관리 로직 (Layer: Backend, Type: Process, REQ: REQ-FUNC-001,005,003)  
- **E1-T04**: 온보딩 진행률·예상 잔여시간(ETA) 계산 및 응답 스키마 (Layer: Backend, Type: Output, REQ: REQ-FUNC-005,006)  
- **E1-T05**: 온보딩 프로필 입력 Form UI 및 클라이언트 Validation (Layer: Frontend, Type: Input, REQ: REQ-FUNC-001, REQ-NF-008)  
- **E1-T06**: 인증 WebView/Redirect 처리 및 토큰 수신 UI 로직 (Layer: Frontend, Type: Process, REQ: REQ-FUNC-002)  
- **E1-T07**: `/api/onboarding/verify` Controller·Service 구현 (Layer: Backend, Type: Process, REQ: REQ-FUNC-002,006)  
- **E1-T08**: 병원 포털 미지원 지역 분기 및 예외 처리 로직 (Layer: Backend, Type: Exception, REQ: REQ-FUNC-019)  
- **E1-T09**: 온보딩 완료 조건 평가 및 첫 가치(리포트/행동 카드) 라우팅 (Layer: Backend, Type: Process, REQ: REQ-FUNC-006)  
- **E1-T10**: 온보딩 화면 접근성(A11y) 구현(라벨, 포커스, 오류 힌트) (Layer: Frontend, Type: Config, REQ: REQ-NF-008, Story4-AC3)  
- **E1-T11**: 온보딩 퍼널 E2E 테스트(성공/이탈/재시도 시나리오) (Layer: QA, Type: Test, REQ: REQ-FUNC-001~006,019)  
- **E1-T12**: 온보딩 p50 시간·완료율 모니터링 쿼리 및 대시보드 (Layer: DevOps, Type: Test, REQ: REQ-NF-001,003,012)  

---

### E2. 1장 의사용 요약 리포트 (One-Page Doctor Summary)

- **E2-T01**: 리포트 지표/섹션 설계(활동/심박/혈압/체중 등) (Layer: Design, Type: Config, REQ: REQ-FUNC-007)  
- **E2-T02**: Health 데이터 집계 Service(기간별 통계 계산) (Layer: Backend, Type: Process, REQ: REQ-FUNC-007, REQ-NF-002)  
- **E2-T03**: 검사 결과·디바이스 데이터 통합 어댑터 (Layer: Backend, Type: Process, REQ: REQ-FUNC-007,008, E5 연동)  
- **E2-T04**: 메타데이터(기간/기기/결측) 생성·태깅 로직 (Layer: Backend, Type: Process, REQ: REQ-FUNC-008)  
- **E2-T05**: `/api/reports/generate` Controller 및 Request/Response DTO (Layer: Backend, Type: Input/Output, REQ: REQ-FUNC-007,008)  
- **E2-T06**: 1장 리포트 일반 웹 뷰(UI) 구현 (Layer: Frontend, Type: Output, REQ: REQ-FUNC-007,009)  
- **E2-T07**: 접근성(A11y) 리포트 뷰(대글자/고대비/스크린리더) (Layer: Frontend, Type: Config, REQ: REQ-FUNC-010, REQ-NF-008)  
- **E2-T08**: PDF 렌더링 및 다운로드/인쇄 기능 구현 (Layer: Backend, Type: Output, REQ: REQ-FUNC-009, REQ-NF-002)  
- **E2-T09**: EMR 첨부용 링크/ID 생성 및 전달 API (Layer: Backend, Type: Output, REQ: REQ-FUNC-009, REQ-NF-011,016)  
- **E2-T10**: 리포트 생성 p95/필드 완결성 모니터링 지표·대시보드 (Layer: DevOps, Type: Test, REQ: REQ-NF-002,011,016)  
- **E2-T11**: 리포트 뷰/첨부 E2E 테스트(Story1 AC1~4) (Layer: QA, Type: Test, REQ: REQ-FUNC-007~010)  

---

### E3. 오늘 1~3개 행동 코칭 (Action Coaching)

- **E3-T01**: 행동 카드 후보 규칙/룰셋 정의 (Layer: Design, Type: Config, REQ: REQ-FUNC-011)  
- **E3-T02**: 사용자 상태 평가 배치/트리거(Job/스케줄러) 구현 (Layer: Backend, Type: Process, REQ: REQ-FUNC-011, REQ-NF-001)  
- **E3-T03**: 오늘의 행동 카드 생성 알고리즘(1~3개 제약 포함) (Layer: Backend, Type: Process, REQ: REQ-FUNC-011,012)  
- **E3-T04**: `/api/actions/today` Controller·DTO (Layer: Backend, Type: Input/Output, REQ: REQ-FUNC-011,012)  
- **E3-T05**: 알림 스케줄러 및 야간(22~07시) 자동 침묵 로직 (Layer: Backend, Type: Config, REQ: REQ-FUNC-012, REQ-NF-005)  
- **E3-T06**: 행동 카드 목록/상태 UI 및 필터 (Layer: Frontend, Type: Output, REQ: REQ-FUNC-011~013)  
- **E3-T07**: `/api/actions/{id}/complete` 완료 처리 로직 (Layer: Backend, Type: Process, REQ: REQ-FUNC-013,014)  
- **E3-T08**: 오프라인 큐잉 및 재시도(지수 백오프) 모듈 (Layer: Backend, Type: Exception, REQ: REQ-FUNC-014)  
- **E3-T09**: D1/W1 완료율 및 오경보율 계산·대시보드 (Layer: DevOps, Type: Test, REQ: REQ-NF-012,015)  
- **E3-T10**: 행동 코칭 플로우 E2E 테스트(Story2 AC1~4) (Layer: QA, Type: Test, REQ: REQ-FUNC-011~014)  

---

### E4. 가족 보드 & 대리 접근 (Family Board & Delegation)

- **E4-T01**: 가족 보드 도메인 모델 및 역할 체계 설계 (Layer: Design, Type: Config, REQ: REQ-FUNC-015,016)  
- **E4-T02**: `/api/family-board` 가족 보드 요약 조회 API (Layer: Backend, Type: Output, REQ: REQ-FUNC-015,017)  
- **E4-T03**: `/api/family-board/invite` 초대 발송 API (Layer: Backend, Type: Input, REQ: REQ-FUNC-016)  
- **E4-T04**: `/api/family-board/role` 역할 부여·변경 API (Layer: Backend, Type: Process, REQ: REQ-FUNC-016,018)  
- **E4-T05**: 초대→동의→역할 부여 3단계 상태 머신 구현 (Layer: Backend, Type: Process, REQ: REQ-FUNC-016, REQ-NF-006,007)  
- **E4-T06**: 가족 보드 UI(일정/약/위험 알림) 및 역할별 뷰 (Layer: Frontend, Type: Output, REQ: REQ-FUNC-015~017)  
- **E4-T07**: 가족 보드 변경사항 동기화(폴링 기반 p95 ≤60초) (Layer: Backend, Type: Process, REQ: REQ-FUNC-017, REQ-NF-005)  
- **E4-T08**: 민감 행위(역할 변경 등)에 대한 2FA 적용 (Layer: Backend, Type: Config, REQ: REQ-FUNC-018, REQ-NF-007)  
- **E4-T09**: 가족 보드 관련 감사 로그 기록·조회 처리 (Layer: Backend, Type: Process, REQ: REQ-NF-006,007)  
- **E4-T10**: 가족 보드/대리 접근 플로우 E2E 테스트 (Layer: QA, Type: Test, REQ: REQ-FUNC-015~018)  

---

### E5. 디바이스 & 병원 포털 연동 (Device & Portal Connect)

- **E5-T01**: 디바이스 커넥터(워치 1종) 인터페이스 설계 (Layer: Design, Type: Config, REQ: REQ-FUNC-003, REQ-NF-017,019)  
- **E5-T02**: 디바이스 OAuth 콜백 처리 `/api/onboarding/devices` (Layer: Backend, Type: Input, REQ: REQ-FUNC-003)  
- **E5-T03**: 초기 데이터 동기화 및 `DeviceLink` 저장 로직 (Layer: Backend, Type: Process, REQ: REQ-FUNC-003)  
- **E5-T04**: 병원 포털 연동 설정 `/api/onboarding/portal` 구현 (Layer: Backend, Type: Input, REQ: REQ-FUNC-004,019)  
- **E5-T05**: 포털 미지원 시 업로드 채널/CS 티켓 생성 예외 처리 (Layer: Backend, Type: Exception, REQ: REQ-FUNC-019)  
- **E5-T06**: 디바이스/포털 연동 상태 조회 및 재동기화 스케줄러 (Layer: Backend, Type: Process, REQ: REQ-NF-005,019)  
- **E5-T07**: 연동 상태 UI(연결/에러/재연결 안내) (Layer: Frontend, Type: Output, REQ: REQ-FUNC-003,004,019)  
- **E5-T08**: 연동 에러 코드·로그·모니터링 지표 구현 (Layer: DevOps, Type: Test, REQ: REQ-NF-004,006,019)  

---

### E6. 플랫폼 / Auth & 공통 NFR (Platform & Cross-Cutting)

- **E6-T01**: Auth/Session/RBAC 설계 및 정책 문서화 (Layer: Design, Type: Config, REQ: REQ-NF-006,007,018)  
- **E6-T02**: `/api/auth/login` 구현(OAuth/OIDC 연동 포함) (Layer: Backend, Type: Input, REQ: REQ-FUNC-002, REQ-NF-006,007)  
- **E6-T03**: `/api/consent` 동의/위임 기록 생성 API (Layer: Backend, Type: Process, REQ: REQ-FUNC-001,002,016; REQ-NF-006)  
- **E6-T04**: `AuditLog` 기록 미들웨어(행위/리소스/메타데이터) (Layer: Backend, Type: Process, REQ: REQ-NF-006,007)  
- **E6-T05**: 공통 에러 핸들링 및 에러 코드 스키마 정의 (Layer: Backend, Type: Exception, REQ: 모든 REQ-FUNC)  
- **E6-T06**: 모니터링 대시보드(지연/오류/비용) 구성 (Layer: DevOps, Type: Config, REQ: REQ-NF-001~005,009,010)  
- **E6-T07**: 온콜 알림(이메일/슬랙) 룰 설정 및 에스컬레이션 (Layer: DevOps, Type: Config, REQ: REQ-NF-010)  
- **E6-T08**: NFR·보안·부하 테스트 시나리오 설계 및 실행 (Layer: QA, Type: Test, REQ: REQ-NF-001~010,017,018)  

---

### 데이터 모델 / DB·ORM·Migration Task (DM-*)

- **DM-T01**: `User`/Profile 테이블 설계 및 DDL 작성 (Epic: E1,E2,E3,E4,E6, REQ: REQ-FUNC-001,007,011,015)  
- **DM-T02**: `User`/Profile ORM Entity 및 Repository 구현 (Epic: E6, REQ: 공통)  
- **DM-T03**: `OnboardingSession` 테이블 및 인덱스/제약 설계 (Epic: E1, REQ: REQ-FUNC-001~006)  
- **DM-T04**: `OnboardingSession` Migration 스크립트 및 기본 seed 데이터 (Epic: E1)  
- **DM-T05**: `HealthReport` 테이블 및 metrics/context JSON 스키마 설계 (Epic: E2, REQ: REQ-FUNC-007~010)  
- **DM-T06**: `HealthReport` ORM 및 조회/저장 Repository 구현 (Epic: E2)  
- **DM-T07**: `ActionCard` 테이블/인덱스(D1/W1 집계 고려) 설계 (Epic: E3, REQ: REQ-FUNC-011~013)  
- **DM-T08**: `FamilyBoard`/`AccessRole` 테이블 설계(FK·제약 포함) (Epic: E4, REQ: REQ-FUNC-015~017)  
- **DM-T09**: `ConsentRecord` 테이블 및 consent_scope JSON 구조 설계 (Epic: E4,E6, REQ: REQ-FUNC-016; REQ-NF-006)  
- **DM-T10**: `AuditLog` 테이블 및 메타데이터 컬럼 설계 (Epic: E6, REQ: REQ-NF-006,007)  
- **DM-T11**: `DeviceLink` 테이블 설계(토큰 암호화 컬럼 포함) (Epic: E5, REQ: REQ-FUNC-003)  
- **DM-T12**: `PortalConnection` 테이블 설계 (Epic: E5, REQ: REQ-FUNC-004,019)  
- **DM-T13**: 테스트용 User/Report/ActionCard 등 Seed/Fixture 데이터 스크립트 (Epic: All, REQ: REQ-NF-011~016 계측 지원)  

---

### NFR 기반 Cross-cutting Task (NFR-T*)

- **NFR-T01**: 앱 초기 로드/주요 화면 전환 성능 측정 스크립트 및 지표 정의 (REQ: REQ-NF-001, Epic: E1,E3,E4,E2)  
- **NFR-T02**: 온보딩 퍼널 성능 튜닝(쿼리/캐싱/네트워크) (REQ: REQ-NF-003, Epic: E1)  
- **NFR-T03**: 리포트 생성/ PDF 렌더링 성능 최적화 (REQ: REQ-NF-002, Epic: E2)  
- **NFR-T04**: SLA·오류율 대시보드 및 알람 룰 정의 (REQ: REQ-NF-004,010, Epic: E6)  
- **NFR-T05**: 가족 보드/알림 동기화 지연 측정 및 폴링 주기 튜닝 (REQ: REQ-NF-005, Epic: E3,E4)  
- **NFR-T06**: TLS1.2+/AES-256 적용 및 키 관리 정책 수립 (REQ: REQ-NF-006, Epic: E6)  
- **NFR-T07**: 고위험 접근에 대한 2FA 플로우 및 정책 설정 (REQ: REQ-NF-007, REQ-FUNC-018, Epic: E4,E6)  
- **NFR-T08**: 온보딩/리포트에 대한 접근성 체크리스트 및 자동 테스트 구성 (REQ: REQ-NF-008, Epic: E1,E2,E6)  
- **NFR-T09**: 인프라/스토리지 비용 메트릭 수집 및 대시보드 구성 (REQ: REQ-NF-009, Epic: E6)  
- **NFR-T10**: 공통 로그 스키마·Trace ID·코릴레이션 설계 (REQ: REQ-NF-010, Epic: E6)  
- **NFR-T11**: 10만 MAU 수평 확장을 고려한 아키텍처 설계 메모/가이드 (REQ: REQ-NF-017, Epic: E6)  
- **NFR-T12**: 주요 기능 모듈(F1~F5,F10)의 모듈 경계/배포 단위 설계 (REQ: REQ-NF-018, Epic: E6)  
- **NFR-T13**: 기관 채널/커넥터 활성 상태 트래킹 지표·뷰 설계 (REQ: REQ-NF-019, Epic: E5,E6)  