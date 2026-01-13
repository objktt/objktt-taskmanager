# Airtable 실제 데이터 수집 양식

**Base URL**: https://airtable.com/appFYcydeTxQ5dP7f/tblCIK64A6GZbowza/viw8doG6iR3SvA7aQ

---

## 📋 데이터 입력 가이드

### 📌 필수 입력 순서
1. **People** (팀 멤버) 먼저 입력
2. **Projects** (프로젝트) 입력
3. **Grants** (지원금) 입력
4. **Vendors** (공급업체) 입력
5. **Tasks** (작업) 입력 (다른 테이블 참조 필요)
6. **Decisions** (의사결정) 입력

---

## 1️⃣ People (팀 멤버)

### 입력 필드

| 필드명 | 타입 | 필수 여부 | 예시 |
|--------|------|----------|------|
| Name | Single Line Text | ✅ 필수 | 황효상 |
| Role | Single Select | ✅ 필수 | CEO / Strategy (옵션 중 선택) |
| Email | Single Line Text | 선택 | ceo@objktt.com |
| Phone Number | Single Line Text | 선택 | 010-1234-5678 |
| Photo | Attachments | 선택 | 프로필 이미지 파일 |

### Role 옵션 (현재 설정된 값)
- CEO / Strategy
- Developer
- Marketing / Grants

### ✅ 입력 예시
```
Name: 황효상
Role: CEO / Strategy
Email: ceo@objktt.com
Phone Number: 010-1234-5678
```

---

## 2️⃣ Projects (프로젝트)

### 입력 필드

| 필드명 | 타입 | 필수 여부 | 예시 |
|--------|------|----------|------|
| Project Name | Single Line Text | ✅ 필수 | Objktt Studio 웹사이트 개편 |
| Goal | Single Line Text | 선택 | 2026년 상반기까지 완전한 웹사이트 리뉴얼 |
| Category | Single Select | ✅ 필수 | Product / Brand / Marketing / Grant / Ops |
| Status | Single Select | ✅ 필수 | Planning / Active / Paused / Done |
| Start Date | Date | 선택 | 2024-01-15 |
| End Date | Date | 선택 | 2024-06-30 |
| Owner | Link to People | 선택 | 황효상 (People 테이블에서 선택) |
| Related Tasks | Link to Tasks | 선택 | 나중에 Tasks 입력 후 연결 |
| Related Grants | Link to Grants | 선택 | 나중에 Grants 입력 후 연결 |

### ✅ 입력 예시
```
Project Name: Objktt Studio 웹사이트 개편
Goal: 2026년 상반기까지 완전한 웹사이트 리뉴얼
Category: Product
Status: Active
Start Date: 2024-01-15
End Date: 2024-06-30
Owner: 황효상
```

---

## 3️⃣ Grants (지원금)

### 입력 필드

| 필드명 | 타입 | 필수 여부 | 예시 |
|--------|------|----------|------|
| Grant Name | Single Line Text | ✅ 필수 | 2026 중소기업 디지털 전환 지원 |
| 기관명 | Single Line Text | ✅ 필수 | 중소벤처기업부 |
| 지원 목적 | Single Line Text | ✅ 필수 | SaaS 플랫폼 고도화 및 시장 확장 |
| 총 사업비 | Currency | ✅ 필수 | 100,000,000 |
| 지원금 | Currency | ✅ 필수 | 80,000,000 |
| 기간 시작일 | Date | ✅ 필수 | 2026-01-01 |
| 기간 종료일 | Date | ✅ 필수 | 2026-12-31 |
| Status | Single Select | ✅ 필수 | Researching / Applying / Accepted / Running / Closed |
| 담당자 | Link to People | 선택 | 이재영 (People에서 선택) |
| Related Projects | Link to Projects | 선택 | 프로젝트와 연결 |
| Related Vendors | Link to Vendors | 선택 | 공급업체와 연결 |
| Related Tasks | Link to Tasks | 선택 | 나중에 Tasks 입력 후 연결 |

### ✅ 입력 예시
```
Grant Name: 2026 중소기업 디지털 전환 지원
기관명: 중소벤처기업부
지원 목적: SaaS 플랫폼 고도화 및 시장 확장
총 사업비: ₩100,000,000
지원금: ₩80,000,000
기간 시작일: 2026-01-01
기간 종료일: 2026-12-31
Status: Applying
담당자: 이재영
```

---

## 4️⃣ Vendors (공급업체)

### 입력 필드

| 필드명 | 타입 | 필수 여부 | 예시 |
|--------|------|----------|------|
| Vendor Name | Single Line Text | ✅ 필수 | 아주개발소 |
| Type | Single Select | ✅ 필수 | Software Supplier / Design / Marketing / Development / Other |
| Contact Person | Single Line Text | 선택 | 김철수 |
| Contact Email | Single Line Text | 선택 | cs@ajudev.com |
| Contact Phone | Single Line Text | 선택 | 010-9876-5432 |
| Logo | Attachments | 선택 | 벤더 로고 이미지 |
| Related Grants | Link to Grants | 선택 | 관련 지원금과 연결 |
| Related Tasks | Link to Tasks | 선택 | 나중에 Tasks 입력 후 연결 |

### ✅ 입력 예시
```
Vendor Name: 아주개발소
Type: Development
Contact Person: 김철수
Contact Email: cs@ajudev.com
Contact Phone: 010-9876-5432
```

---

## 5️⃣ Tasks (업무 작업)

### 입력 필드

| 필드명 | 타입 | 필수 여부 | 예시 |
|--------|------|----------|------|
| Task Name | Single Line Text | ✅ 필수 | 홈페이지 디자인 개선 |
| Domain | Single Select | ✅ 필수 | Strategy / Product / Design / Dev / Marketing / Biz / Grant |
| Status | Single Select | ✅ 필수 | Backlog / Planned / In Progress / Review / Done |
| Priority | Single Select | ✅ 필수 | High / Medium / Low |
| Due Date | Date | 선택 | 2024-02-15 |
| Owner | Link to People | ✅ 필수 | 최태석 (People에서 선택) |
| Related Project | Link to Projects | 선택 | Objktt Studio 웹사이트 개편 |
| Related Grant | Link to Grants | 선택 | 2026 중소기업 디지털 전환 지원 |
| Related Vendor | Link to Vendors | 선택 | 아주개발소 |
| Notes | Multi-line Text | 선택 | 레이아웃 적용, 모바일 최적화 필요 |

### ✅ 입력 예시
```
Task Name: 홈페이지 디자인 개선
Domain: Design
Status: In Progress
Priority: High
Due Date: 2024-02-15
Owner: 최태석
Related Project: Objktt Studio 웹사이트 개편
Related Grant: 2026 중소기업 디지털 전환 지원
Related Vendor: 아주개발소
Notes: 레이아웃 적용, 모바일 최적화 필요
```

---

## 6️⃣ Decisions (의사결정)

### 입력 필드

| 필드명 | 타입 | 필수 여부 | 예시 |
|--------|------|----------|------|
| Decision Name | Single Line Text | ✅ 필수 | 기술 스택 결정: React vs Vue |
| Decision Date | Date | ✅ 필수 | 2024-01-20 |
| Description | Multi-line Text | 선택 | 팀 내부 논의 결과, 개발 효율성을 고려하여 React 선택 |
| Decision Maker | Link to People | ✅ 필수 | 황효상 (People에서 선택) |
| Related Task | Link to Tasks | 선택 | 관련 작업 연결 |
| Related Project | Link to Projects | 선택 | 관련 프로젝트 연결 |
| Supporting Document | Attachments | 선택 | 기술 스택 비교 문서 |
| Decision Status | Single Select | ✅ 필수 | Proposed / Approved / Rejected / Revised |

### ✅ 입력 예시
```
Decision Name: 기술 스택 결정: React vs Vue
Decision Date: 2024-01-20
Description: 팀 내부 논의 결과, 개발 효율성을 고려하여 React 선택
Decision Maker: 황효상
Decision Status: Approved
Supporting Document: 기술 스택 비교 문서 (파일 첨부)
```

---

## 🎯 데이터 입력 체크리스트

### People
- [ ] 최소 3명 이상의 팀 멤버 입력
- [ ] 각 멤버의 Role 지정
- [ ] 연락처 (이메일, 전화번호) 입력

### Projects
- [ ] 각 프로젝트의 목표(Goal) 명확히 기술
- [ ] Category 분류 (Product, Brand, Marketing, Grant, Ops)
- [ ] 시작일/종료일 설정
- [ ] 담당자(Owner) 지정

### Grants
- [ ] 기관명(한글) 입력
- [ ] 지원 목적 명확히 기술
- [ ] 총 사업비와 지원금 정확히 입력
- [ ] 기간(시작일/종료일) 설정
- [ ] 담당자 지정

### Vendors
- [ ] 각 공급업체의 Type 분류
- [ ] 담당자 정보 입력
- [ ] 연락처(이메일, 전화) 입력

### Tasks
- [ ] 모든 작업에 담당자(Owner) 지정
- [ ] 우선순위(Priority) 설정
- [ ] 마감일(Due Date) 설정
- [ ] 관련 프로젝트/지원금/벤더 연결

### Decisions
- [ ] 의사결정 일자 기록
- [ ] 의사결정자 지정
- [ ] 의사결정 상태(Proposed/Approved/Rejected/Revised) 기록

---

## 🔗 유용한 링크

- [Airtable Base 메인 페이지](https://airtable.com/appFYcydeTxQ5dP7f/pagq5kHdOHDac0tfg)
- [People 테이블](https://airtable.com/appFYcydeTxQ5dP7f/tblCIK64A6GZbowza/viw8doG6iR3SvA7aQ)
- [Projects 테이블](https://airtable.com/appFYcydeTxQ5dP7f/tblVT6Qm1wQAzp3gA/viw2wWNlEla0pe8LZ)
- [Grants 테이블](https://airtable.com/appFYcydeTxQ5dP7f/tblVCOv166jJkSR1W/viwqt1LDFEffbESU9)
- [Vendors 테이블](https://airtable.com/appFYcydeTxQ5dP7f/tblq123AXwauwEWDI/viwWaRb3OFYUy3d3S)
- [Tasks 테이블](https://airtable.com/appFYcydeTxQ5dP7f/tblNLk5LzsOEgHFwn/viwCkDhfCamFMQf8F)
- [Decisions 테이블](https://airtable.com/appFYcydeTxQ5dP7f/tblKuijXteGdbbG4I/viwBdZAPzamyNuJr5)

---

## 💡 팁

1. **입력 순서 준수**: People → Projects → Grants → Vendors → Tasks → Decisions 순서로 입력
2. **연결 필드 활용**: Tasks, Grants, Projects는 서로 연결되어야 데이터가 의미가 있음
3. **정기적 업데이트**: 작업 상태, 프로젝트 진행상황을 최신으로 유지
4. **AI 필드 활용**: 자동 생성되는 요약/리스크 평가를 참고하여 의사결정에 활용
5. **백업**: 주기적으로 데이터를 내보내기(Export)하여 백업

---

**마지막 업데이트**: 2026-01-14
**문서 버전**: 1.0
