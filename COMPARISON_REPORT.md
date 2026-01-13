# Airtable 테이블 구조 비교 보고서

**Base ID**: appFYcydeTxQ5dP7f
**분석 일자**: 2026-01-14

---

## 📊 전체 요약

### 기본 정보
- 총 6개 테이블 구조 분석 완료
- 모든 목업 데이터 삭제 완료 (28개 레코드)
- 가이드라인과 실제 구조 간 차이점 확인

---

## 1. People 테이블 (tblCIK64A6GZbowza)

### ✅ 일치하는 필드
| 필드명 | 타입 | 상태 |
|--------|------|------|
| Name | singleLineText | ✅ 일치 |
| Email | singleLineText | ✅ 유사 (가이드: Email 타입) |
| Owned Projects | multipleRecordLinks | ✅ 일치 |
| Related Tasks | multipleRecordLinks | ⚠️ 유사 (가이드: Assigned Tasks) |

### ❌ 가이드에는 없는 필드 (현재만 존재)
| 필드명 | 타입 | 설명 |
|--------|------|------|
| Role | singleSelect | 현재 설정: CEO, Strategy, Product Planning, Design, Development, Engineering, Marketing, Business, Grants, Other |
| Photo | multipleAttachments | 프로필 사진 |
| Phone Number | singleLineText | 연락처 |
| Managed Grants | multipleRecordLinks | 관리하는 지원금 |
| Task Count | count | 할당된 작업 수 |
| Project Count | count | 소유한 프로젝트 수 |
| Grant Count | count | 관리하는 지원금 수 |
| Active Project Names | rollup | 진행 중인 프로젝트 이름 |
| Summary of Responsibilities | aiText | 책임 요약 (AI) |
| Suggested Focus Area | aiText | 제안된 초점 영역 (AI) |
| Decisions | multipleRecordLinks | 관련 의사결정 |

### ❌ 가이드에는 있으나 현재 없는 필드
| 필드명 | 타입 | 설명 |
|--------|------|------|
| Type | Single Select | Core, External |
| Total Tasks | Count | Assigned Tasks 수 |
| Active Tasks | Rollup | 진행 중인 작업 수 |
| Backlog | Rollup | 백로그 작업 수 |
| Tasks Completed | Rollup | 완료된 작업 수 |
| Active Projects | Rollup | 진행 중인 프로젝트 수 |
| Workload | Formula | `IF({Active Tasks} > 0, CONCATENATE({Active Tasks}, " tasks in progress"), "No active tasks")` |

### 🔍 주요 차이점
1. **Role 타입**: 가이드는 Single Line Text, 실제는 Single Select
2. **필드명 차이**: Assigned Tasks vs Related Tasks, Owned Grants vs Managed Grants
3. **계산 필드 부족**: Active Tasks, Backlog, Tasks Completed, Workload 필드 없음

---

## 2. Projects 테이블 (tblVT6Qm1wQAzp3gA)

### ✅ 일치하는 필드
| 필드명 | 타입 | 상태 |
|--------|------|------|
| Project Name | singleLineText | ✅ 일치 |
| Goal | singleLineText | ✅ 일치 |
| Category | singleSelect | ✅ 일치 |
| Status | singleSelect | ✅ 일치 |
| Start Date | date | ✅ 일치 |
| End Date | date | ✅ 일치 |
| Related Tasks | multipleRecordLinks | ✅ 일치 |
| Related Grants | multipleRecordLinks | ✅ 일치 |

### ❌ 가이드에는 있으나 현재 없는 필드
| 필드명 | 타입 | 설명 |
|--------|------|------|
| Owner | Link to People | Single record |
| Related Vendors | Link to Vendors | Allow multiple records |
| Total Tasks | Count | Related Tasks 수 |
| Tasks Completed | Rollup | 완료된 작업 수 |
| Progress % | Formula | `IF({Total Tasks} > 0, ROUND(({Tasks Completed} / {Total Tasks}) * 100, 0), 0)` |
| Duration (Days) | Formula | `IF(AND({Start Date}, {End Date}), DATETIME_DIFF({End Date}, {Start Date}, 'days'), "")` |

### 🔍 주요 차이점
1. **Owner 필드 부족**: 현재는 관리를 위해 필요
2. **계산 필드 부족**: Progress %, Total Tasks, Tasks Completed 없음
3. **Duration (Days) vs Project Duration (days)**: 이름 차이

---

## 3. Tasks 테이블 (tblNLk5LzsOEgHFwn)

### ✅ 일치하는 필드
| 필드명 | 타입 | 상태 |
|--------|------|------|
| Task Name | singleLineText | ✅ 일치 |
| Status | singleSelect | ✅ 일치 |
| Notes | multilineText | ✅ 일치 |

### ❌ 가이드에는 있으나 현재 없는 필드
| 필드명 | 타입 | 설명 |
|--------|------|------|
| Description | Multi-line Text | 작업 상세 설명 |
| Assignee | Link to People | Single record |

### ❌ 가이드에는 없는 필드 (현재만 존재)
| 필드명 | 타입 | 설명 |
|--------|------|------|
| Owner | multipleRecordLinks | 담당자 (다수) |
| Domain | singleSelect | 작업 도메인 |
| Priority | singleSelect | 우선순위 |
| Due Date | date | 마감일 |
| Related Project | Link to Projects | Single record |
| Related Grant | Link to Grants | Single record |
| Related Vendor | Link to Vendors | Single record |
| Days Until Due | formula | 마감일까지 남은 일수 |
| Is Overdue? | formula | 기한 초과 여부 |
| Project Status | multipleLookupValues | 프로젝트 상태 |
| Grant Status | multipleLookupValues | 지원금 상태 |
| Vendor Type | multipleLookupValues | 벤더 유형 |
| Owner Role | multipleLookupValues | 소유자 역할 |
| Summary (AI) | aiText | 작업 요약 (AI) |
| Risk Assessment (AI) | aiText | 리스크 평가 (AI) |
| Decisions | multipleRecordLinks | 관련 의사결정 |

### 🔍 주요 차이점
1. **Assignee 필드 차이**: 가이드는 단일, 현재는 다중
2. **연결 필드**: Related Project, Grant, Vendor가 현재는 단일 연결, 가이드는 다중
3. **AI 필드**: 가이드에는 없으나 현재에 존재

---

## 4. Grants 테이블 (tblVCOv166jJkSR1W)

### ✅ 일치하는 필드
| 필드명 | 타입 | 상태 |
|--------|------|------|
| Grant Name | singleLineText | ✅ 일치 |
| Status | singleSelect | ✅ 일치 |
| Related Projects | multipleRecordLinks | ✅ 일치 |
| Related Tasks | multipleRecordLinks | ✅ 일치 |

### ❌ 가이드에는 있으나 현재 없는 필드
| 필드명 | 타입 | 설명 |
|--------|------|------|
| Granting Organization | Single Line Text | 지원 기관 |
| Application Purpose | Single Line Text | 지원 목적 |
| Total Budget | Currency | 총 사업비 |
| Grant Amount | Currency | 지원금 |
| Start Date | Date | 기간 시작일 |
| End Date | Date | 기간 종료일 |
| Owner | Link to People | 담당자 |
| Related Vendors | Link to Vendors | 관련 공급업체 |
| Total Tasks | Count | 관련 작업 수 |
| Tasks Completed | Rollup | 완료된 작업 수 |
| Grant Utilization | Formula | 지원금 사용률 |
| Duration (Days) | Formula | 프로젝트 기간 |

### ❌ 가이드에는 없는 필드 (현재만 존재)
| 필드명 | 타입 | 설명 |
|--------|------|------|
| 기관명 | singleLineText | 기관명 (한글) |
| 지원 목적 | singleLineText | 지원 목적 (한글) |
| 총 사업비 | currency | 총 사업비 (한글) |
| 지원금 | currency | 지원금 (한글) |
| 기간 시작일 | date | 기간 시작일 (한글) |
| 기간 종료일 | date | 기간 종료일 (한글) |
| 담당자 | multipleRecordLinks | 담당자 (다수) |
| Number of Related Tasks | count | 관련 작업 수 |
| Number of Related Vendors | count | 관련 공급업체 수 |
| Number of Related Projects | count | 관련 프로젝트 수 |
| Total Related Task Statuses | rollup | 관련 작업 상태 |
| All Related Vendor Types | rollup | 관련 공급업체 유형 |
| Grant Utilization Rate (%) | formula | 지원금 사용률 (%) |
| Grant Duration (days) | formula | 지원금 기간 (일) |
| Grant Summary (AI) | aiText | 지원금 요약 (AI) |
| Grant Risk Assessment (AI) | aiText | 지원금 리스크 평가 (AI) |

### 🔍 주요 차이점
1. **필드명 국어화**: 기관명, 지원 목적, 총 사업비 등 한글 필드명 사용
2. **Owner vs 담당자**: 명칭 차이
3. **AI 필드 추가**: Grant Summary, Grant Risk Assessment

---

## 5. Vendors 테이블 (tblq123AXwauwEWDI)

### ✅ 일치하는 필드
| 필드명 | 타입 | 상태 |
|--------|------|------|
| Vendor Name | singleLineText | ✅ 일치 |
| Type | singleSelect | ✅ 일치 |

### ❌ 가이드에는 있으나 현재 없는 필드
| 필드명 | 타입 | 설명 |
|--------|------|------|
| Contact Person | Single Line Text | 담당자 |
| Contact Email | Email | 이메일 |
| Contact Phone | Phone | 연락처 |
| Related Projects | Link to Projects | 관련 프로젝트 |
| Related Grants | Link to Grants | 관련 지원금 |
| Related Tasks | Link to Tasks | 관련 작업 |
| Total Tasks | Count | 관련 작업 수 |
| Task Statuses | Rollup | 작업 상태들 |

### ❌ 가이드에는 없는 필드 (현재만 존재)
| 필드명 | 타입 | 설명 |
|--------|------|------|
| Logo | multipleAttachments | 로고 |
| Related Grants | multipleRecordLinks | 관련 지원금 |
| Related Tasks | multipleRecordLinks | 관련 작업 |
| Notes | multilineText | 메모 |
| # of Related Tasks | count | 관련 작업 수 |
| # of Related Grants | count | 관련 지원금 수 |
| Earliest Task Due Date | rollup | 가장 빠른 작업 마감일 |
| Latest Task Due Date | rollup | 가장 늦은 작업 마감일 |
| All Task Statuses | rollup | 작업 상태들 |
| All Grant Statuses | rollup | 지원금 상태들 |
| Vendor Summary (AI) | aiText | 벤더 요약 (AI) |
| Vendor Reputation (AI) | aiText | 벤더 평판 (AI) |

### 🔍 주요 차이점
1. **연결 필드 차이**: Related Projects, Related Grants 현재 단일 연결, 가이드는 다중
2. **AI 필드**: Vendor Summary, Vendor Reputation

---

## 6. Decisions 테이블 (tblKuijXteGdbbG4I)

### ✅ 일치하는 필드
| 필드명 | 타입 | 상태 |
|--------|------|------|
| Decision Name | singleLineText | ✅ 일치 |
| Decision Date | date | ✅ 일치 |
| Description | multilineText | ✅ 일치 |
| Decision Status | singleSelect | ✅ 일치 |

### ❌ 가이드에는 있으나 현재 없는 필드
| 필드명 | 타입 | 설명 |
|--------|------|------|
| Decision Maker | Link to People | 의사결정자 |
| Related Task | Link to Tasks | 관련 작업 |
| Related Project | Link to Projects | 관련 프로젝트 |

### ❌ 가이드에는 없는 필드 (현재만 존재)
| 필드명 | 타입 | 설명 |
|--------|------|------|
| Supporting Document | multipleAttachments | 지원 문서 |
| Days to Decision | formula | 의사결정까지 일수 |
| Decision Summary (AI) | aiText | 의사결정 요약 (AI) |
| Related Task Status | multipleLookupValues | 관련 작업 상태 |
| Related Project Status | multipleLookupValues | 관련 프로젝트 상태 |
| Decision Category (AI) | aiText | 의사결정 분류 (AI) |

### 🔍 주요 차이점
1. **Decision Maker 명칭 차이**
2. **AI 필드 추가**: Decision Summary, Decision Category

---

## 📋 전체 결론

### 주요 차이점 요약

#### 1. 언어 차이
- Grants 테이블에서 한글 필드명 사용 (기관명, 지원 목적 등)

#### 2. 필드 타입 차이
- Role: 가이드는 Single Line Text, 현재는 Single Select

#### 3. 연결 필드 개수 차이
- 가이드는 다중 연결을 권장
- 현재는 단일/다중 혼재

#### 4. 계산 필드 부족
- People: Active Tasks, Backlog, Tasks Completed, Workload
- Projects: Total Tasks, Tasks Completed, Progress %
- Vendors: Total Tasks, Task Statuses

#### 5. AI 필드 추가
- 현재 구조에 다수의 AI 필드 존재
- 가이드에는 없음

---

## 💡 권장 사항

### 옵션 A: 가이드라인 준수 (권장)
- 가이드에 있는 필드 추가
- 현재만 있는 필드 유지
- 두 구조 모두 지원

### 옵션 B: 현재 구조 유지
- 현재 구조가 더 최신이거나 실제 업무 환경에 맞음
- 가이드는 참고용으로 사용

### 옵션 C: 하이브리드 접근
- 가이드의 핵심 필드 유지
- 현재의 AI/계산 필드 추가
- 두 구조의 장점 통합

---

**다음 단계**: 사용자의 요청에 따라 구조 조정 진행
