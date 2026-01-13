# Objktt Studio - Airtable Implementation Guide

**Base Name**: Objktt Studio Operations
**Purpose**: Operational database for task, project, and grant management
**Team**: 황효상 (CEO/Strategy), 최태석 (Dev), 이재영 (Marketing/Grants)

---

## QUICK START

### 1. Create the Base
1. Go to [Airtable](https://airtable.com)
2. Click "Create a base" → "Start from scratch"
3. Name it: **"Objktt Studio Operations"**

### 2. Create Tables (in this order)
Create tables with exact names below:

1. **People** (Create first - needed for relationships)
2. **Projects**
3. **Tasks**
4. **Grants**
5. **Vendors**
6. **Decisions** (Optional)

### 3. Add Initial People
Create these 3 people first:

| Name | Role | Type |
|------|------|------|
| 황효상 | CEO / Strategy | Core |
| 최태석 | Developer | Core |
| 이재영 | Marketing / Grants | Core |

---

## TABLE CREATION DETAILS

### TABLE 1: People

**Primary Field**: Name

#### Fields to Create:

| Field Name | Type | Options/Formula |
|------------|------|-----------------|
| Role | Single Line Text | - |
| Type | Single Select | Core, External |
| Email | Email | - |
| Assigned Tasks | Link to Tasks | Allow multiple records |
| Owned Projects | Link to Projects | Allow multiple records |
| Owned Grants | Link to Grants | Allow multiple records |
| Total Tasks | Count | Count Assigned Tasks |
| Active Tasks | Rollup (Assigned Tasks → Status) | `SUM(IF(values = "In Progress", 1, 0))` |
| Backlog | Rollup (Assigned Tasks → Status) | `SUM(IF(values = "Backlog", 1, 0))` |
| Tasks Completed | Rollup (Assigned Tasks → Status) | `SUM(IF(values = "Done", 1, 0))` |
| Active Projects | Rollup (Owned Projects → Status) | `SUM(IF(values = "Active", 1, 0))` |
| Workload | Formula | `IF({Active Tasks} > 0, CONCATENATE({Active Tasks}, " tasks in progress"), "No active tasks")` |

---

### TABLE 2: Projects

**Primary Field**: Project Name

#### Fields to Create:

| Field Name | Type | Options/Formula |
|------------|------|-----------------|
| Goal | Single Line Text | One-sentence description |
| Category | Single Select | Product, Brand, Marketing, Grant, Ops |
| Status | Single Select | Planning, Active, Paused, Done |
| Start Date | Date | Date only |
| End Date | Date | Date only |
| Owner | Link to People | Single record |
| Related Tasks | Link to Tasks | Allow multiple records |
| Related Grants | Link to Grants | Allow multiple records |
| Related Vendors | Link to Vendors | Allow multiple records |
| Total Tasks | Count | Count Related Tasks |
| Tasks Completed | Rollup (Related Tasks → Status) | `SUM(IF(values = "Done", 1, 0))` |
| Progress % | Formula | `IF({Total Tasks} > 0, ROUND(({Tasks Completed} / {Total Tasks}) * 100, 0), 0)` |
| Duration (Days) | Formula | `IF(AND({Start Date}, {End Date}), DATETIME_DIFF({End Date}, {Start Date}, 'days'), "")` |

---

### TABLE 3: Tasks

**Primary Field**: Task Name

#### Fields to Create:

| Field Name | Type | Options/Formula |
|------------|------|-----------------|
| Owner | Link to People | Single record |
| Domain | Single Select | Strategy, Product, Design, Dev, Marketing, Biz, Grant |
| Status | Single Select | Backlog, Planned, In Progress, Review, Done |
| Priority | Single Select | High, Medium, Low |
| Due Date | Date | Include time: Yes |
| Related Project | Link to Projects | Single record (REQUIRED) |
| Related Grant | Link to Grants | Single record (Optional) |
| Related Vendor | Link to Vendors | Single record (Optional) |
| Notes | Long Text | - |
| Completed Date | Date | Date only |
| Is Overdue | Formula | `IF(AND({Due Date} < NOW(), {Status} != "Done"), "⚠️ Overdue", "")` |
| Days Until Due | Formula | `IF({Due Date}, DATETIME_DIFF({Due Date}, NOW(), 'days'), "")` |

---

### TABLE 4: Grants

**Primary Field**: Grant Name

#### Fields to Create:

| Field Name | Type | Options/Formula |
|------------|------|-----------------|
| 기관명 | Single Line Text | - |
| 지원 목적 | Long Text | - |
| 총 사업비 | Currency | Symbol: ₩, KRW |
| 지원금 | Currency | Symbol: ₩, KRW |
| 지원금 비율 % | Formula | `IF({총 사업비} > 0, ROUND(({지원금} / {총 사업비}) * 100, 1), 0)` |
| Start Date | Date | Date only |
| End Date | Date | Date only |
| Status | Single Select | Researching, Applying, Accepted, Running, Closed |
| 담당자 | Link to People | Single record, Default: 이재영 |
| Related Projects | Link to Projects | Allow multiple records |
| Related Vendors | Link to Vendors | Allow multiple records |
| Related Tasks | Link to Tasks | Allow multiple records |
| Projects | Count | Count Related Projects |
| Vendors | Count | Count Related Vendors |
| Tasks | Count | Count Related Tasks |
| Tasks Completed | Rollup (Related Tasks → Status) | `SUM(IF(values = "Done", 1, 0))` |
| Execution Progress % | Formula | `IF({Tasks} > 0, ROUND(({Tasks Completed} / {Tasks}) * 100, 0), 0)` |
| Duration (Days) | Formula | `IF(AND({Start Date}, {End Date}), DATETIME_DIFF({End Date}, {Start Date}, 'days'), "")` |
| Days Remaining | Formula | `IF({End Date}, DATETIME_DIFF({End Date}, TODAY(), 'days'), "")` |

---

### TABLE 5: Vendors

**Primary Field**: Vendor Name

#### Fields to Create:

| Field Name | Type | Options/Formula |
|------------|------|-----------------|
| Type | Single Select | Software Supplier, Design, Marketing, Consulting |
| 사업자 형태 | Single Select | 개인, 법인 |
| 지원사업용 역할 설명 | Long Text | - |
| 계약 상태 | Single Select | Planned, Signed |
| Related Grants | Link to Grants | Allow multiple records |
| Related Projects | Link to Projects | Allow multiple records |
| Actual Tasks | Link to Tasks | Allow multiple records |
| Tasks | Count | Count Actual Tasks |
| Tasks Completed | Rollup (Actual Tasks → Status) | `SUM(IF(values = "Done", 1, 0))` |
| Has Active Tasks | Formula | `IF({Tasks} > 0, "✅ Yes", "❌ No")` |

---

### TABLE 6: Decisions (Optional)

**Primary Field**: Decision Title

#### Fields to Create:

| Field Name | Type | Options/Formula |
|------------|------|-----------------|
| Decision | Long Text | Final decision statement |
| Context | Long Text | Why this decision was needed |
| Impact | Long Text | What this decision affects |
| Made By | Link to People | Single record |
| Decision Date | Date | Include time: Yes |
| Related Task | Link to Tasks | Single record (Optional) |
| Related Project | Link to Projects | Single record (Optional) |

---

## VIEWS SETUP

### Tasks Table Views

1. **All Tasks** (Default)
   - Group by: Status
   - Sort: Due Date (ascending)
   - Fields: Task Name, Owner, Domain, Status, Priority, Due Date, Related Project

2. **By Domain**
   - Group by: Domain
   - Filter: Status ≠ Done
   - Sort: Priority (High → Low), then Due Date
   - Fields: Task Name, Owner, Status, Priority, Due Date, Related Project, Is Overdue

3. **By Owner**
   - Group by: Owner
   - Filter: Status ≠ Done
   - Sort: Due Date

4. **Overdue & Upcoming**
   - Filter Formula: `OR({Due Date} < NOW(), AND({Due Date} <= TODAY() + 7, {Status} != "Done"))`
   - Sort: Due Date (ascending)
   - Group by: Status

5. **Dev Tasks** (for 최태석)
   - Filter: Domain = Dev, Status ≠ Done
   - Sort: Priority (High → Low), Due Date
   - Fields: Task Name, Status, Priority, Due Date, Related Project, Related Grant, Related Vendor

6. **Blocked Tasks**
   - Filter Formula: `AND({Priority} = "High", {Due Date} < TODAY() + 3, {Status} != "Done")`
   - Sort: Due Date (ascending)
   - Fields: Task Name, Owner, Domain, Related Project, Related Grant, Notes

---

### Projects Table Views

1. **All Projects** (Default)
   - Group by: Status
   - Sort: Start Date (descending)
   - Fields: Project Name, Goal, Category, Status, Start Date, End Date, Owner, Progress %

2. **Active Projects**
   - Filter: Status = Active
   - Sort: Progress % (ascending)
   - Fields: Project Name, Goal, Category, Owner, Progress %

3. **By Category**
   - Group by: Category
   - Filter: Status ≠ Done
   - Sort: End Date (ascending)

---

### Grants Table Views

1. **All Grants** (Default)
   - Group by: Status
   - Sort: Start Date (descending)
   - Fields: Grant Name, 기관명, Status, 지원금, Execution Progress %, 담당자

2. **Grant Pipeline**
   - Filter: Status ≠ Closed
   - Group by: Status
   - Sort: Start Date (descending)
   - Fields: Grant Name, 기관명, Status, Execution Progress %, Days Remaining

3. **Active Grants**
   - Filter Formula: `OR({Status} = "Accepted", {Status} = "Running")`
   - Sort: End Date (ascending)
   - Fields: Grant Name, 기관명, Days Remaining, Execution Progress %

---

### Vendors Table Views

1. **All Vendors** (Default)
   - Group by: Type
   - Sort: Vendor Name (A-Z)

2. **Grant Vendors**
   - Filter: Related Grants ≠ empty
   - Group by: 계약 상태
   - Sort: Vendor Name (A-Z)
   - Fields: Has Active Tasks, Tasks, Tasks Completed

---

### People Table Views

1. **All People** (Default)
   - Group by: Type
   - Sort: Name (A-Z)
   - Fields: Name, Role, Type, Total Tasks, Active Tasks, Tasks Completed, Workload

2. **Core Team**
   - Filter: Type = Core
   - Sort: Name (A-Z)

---

## INTERFACES SETUP

### Interface 1: CEO Dashboard (황효상)

**Access**: Assign to 황효상 only

**Layout** (4 sections):

#### Section 1: Project Overview (Kanban)
- Source: Projects table
- Group by: Status
- Card fields: Project Name, Goal, Owner, Progress %
- Sort: Progress % (ascending)

#### Section 2: Grant Pipeline (Summary)
- Source: Grants table
- Group by: Status
- Filter: Status ≠ Closed
- Fields: Grant Name, 기관명, Execution Progress %

#### Section 3: Critical Tasks (Grid)
- Source: Tasks table
- Filter Formula: `OR({Priority} = "High", {Due Date} < TODAY() + 7)`
- Sort: Priority (High → Low), Due Date
- Fields: Task Name, Owner, Priority, Due Date, Is Overdue

#### Section 4: Decision Needed (List)
- Source: Tasks table
- Filter: Priority = High, Status = Review
- Sort: Due Date
- Fields: Task Name, Owner, Domain, Notes

---

### Interface 2: Dev Dashboard (최태석)

**Access**: Assign to 최태석 only

**Layout** (4 sections):

#### Section 1: My Tasks (Kanban)
- Source: Tasks table
- Filter: Owner = 최태석, Domain = Dev
- Group by: Status
- Sort: Priority (High → Low), Due Date

#### Section 2: This Week (Calendar)
- Source: Tasks table
- Filter: Owner = 최태석, Domain = Dev, Due Date >= TODAY() - 3, Due Date <= TODAY() + 7
- Date field: Due Date

#### Section 3: Blocked / Overdue (Grid)
- Source: Tasks table
- Filter Formula: `AND({Owner} = "최태석", OR({Is Overdue} = "⚠️ Overdue", {Due Date} <= TODAY() + 3))`
- Sort: Due Date, Priority

#### Section 4: My Projects (Summary)
- Source: Projects table
- Filter: Owner = 최태석 OR Status = Active
- Sort: Status, Progress %

---

### Interface 3: Marketing / Grants Dashboard (이재영)

**Access**: Assign to 이재영 only

**Layout** (4 sections):

#### Section 1: My Marketing Tasks (Kanban)
- Source: Tasks table
- Filter Formula: `AND({Owner} = "이재영", OR({Domain} = "Marketing", {Domain} = "Biz", {Domain} = "Grant"))`
- Group by: Status
- Sort: Priority, Due Date

#### Section 2: Grant Pipeline (Summary)
- Source: Grants table
- Group by: Status
- Filter: Status ≠ Closed
- Sort: Start Date
- Fields: Grant Name, 기관명, Execution Progress %

#### Section 3: Grant Details (Expanded Cards)
- Source: Grants table
- Filter: Status = Accepted OR Status = Running
- Sort: End Date
- Fields: All grant fields + Tasks, Projects, Vendors rollups

#### Section 4: Vendor Connection Status (Grid)
- Source: Vendors table
- Filter: Related Grants ≠ empty
- Group by: 계약 상태
- Fields: Has Active Tasks, Tasks, Tasks Completed

---

### Interface 4: Grant Detail View

**Access**: All team members (read-only for non-owners)

**Layout** (6 sections):

#### Section 1: Grant Overview (Record Details)
- Source: Grants table
- Show: All grant basic fields

#### Section 2: Execution Summary (Charts)
- Progress Gauge: Execution Progress %
- Pie Chart: Task Status distribution

#### Section 3: Related Projects (Kanban)
- Source: Projects table
- Linked to current grant
- Group by: Status
- Fields: Project Name, Progress %

#### Section 4: Vendors (Grid)
- Source: Vendors table
- Linked to current grant
- Group by: 계약 상태
- Fields: Vendor Name, Type, Has Active Tasks

#### Section 5: All Tasks (Timeline)
- Source: Tasks table
- Related Grant = current grant
- Date field: Due Date
- Group by: Status or Owner

#### Section 6: Task List (Grid)
- Source: Tasks table
- Related Grant = current grant
- Sort: Due Date
- Fields: Task Name, Owner, Status, Priority, Related Project

---

### Interface 5: Weekly Review

**Access**: All team members

**Layout** (6 sections):

#### Section 1: Completed This Week (Grid)
- Source: Tasks table
- Filter Formula: `AND({Status} = "Done", {Completed Date} >= DATEADD(TODAY(), -WEEKDAY(TODAY()) + 1, "days"), {Completed Date} <= DATEADD(TODAY(), 7-WEEKDAY(TODAY()), "days"))`
- Sort: Completed Date (descending)
- Fields: Task Name, Owner, Priority, Completed Date

#### Section 2: Next Week Planning (Grid)
- Filter Formula: `AND({Due Date} >= DATEADD(TODAY(), 7-WEEKDAY(TODAY())+1, "days"), {Due Date} <= DATEADD(TODAY(), 14-WEEKDAY(TODAY()), "days"), {Status} != "Done")`
- Sort: Due Date, Priority

#### Section 3: Blocked Issues (List)
- Filter: Priority = High, Due Date < TODAY() + 3
- Sort: Due Date
- Color code: Red for Overdue, Orange for urgent

#### Section 4: Team Workload (Summary)
- Source: People table
- Fields: Name, Active Tasks, Tasks Completed, Workload

#### Section 5: Grant Status (Grid)
- Source: Grants table
- Filter: Status ≠ Closed
- Sort: Status, Execution Progress %

#### Section 6: Decisions Made This Week (List)
- Source: Decisions table
- Filter Formula: `AND({Decision Date} >= DATEADD(TODAY(), -WEEKDAY(TODAY()) + 1, "days"), {Decision Date} <= TODAY())`
- Sort: Decision Date (descending)

---

## AUTOMATION SETUP

### Automation 1: Grant Accepted → Auto-create Project and Tasks

**Trigger**:
- When record matches conditions
- Table: Grants
- Condition: Status changes to "Accepted"

**Actions**:

1. **Create Project**
   - Table: Projects
   - Fields:
     - Project Name: `{Grant Name} - Execution`
     - Goal: `Execute {Grant Name}`
     - Category: "Grant"
     - Status: "Active"
     - Start Date: `{Start Date}`
     - End Date: `{End Date}`
     - Owner: `{담당자}`

2. **Create Task 1** (Kick-off)
   - Table: Tasks
   - Fields:
     - Task Name: "Grant Kick-off & Planning"
     - Domain: "Grant"
     - Owner: `{담당자}`
     - Status: "Planned"
     - Priority: "High"
     - Due Date: `DATEADD({Start Date}, 7, "days")`
     - Related Project: [Project from step 1]
     - Related Grant: [Current Grant]

3. **Create Task 2** (Budget)
   - Table: Tasks
   - Fields:
     - Task Name: "Initial Budget Planning"
     - Domain: "Biz"
     - Owner: `{담당자}`
     - Status: "Planned"
     - Priority: "High"
     - Due Date: `DATEADD({Start Date}, 14, "days")`
     - Related Project: [Project from step 1]
     - Related Grant: [Current Grant]

---

### Automation 2: Due Date Reminder

**Trigger**:
- At a scheduled time
- Frequency: Daily at 8:00 AM

**Actions**:

1. **Send 2-day reminder**
   - Find records: Tasks
   - Filter: Due Date = TODAY() + 2, Status ≠ Done
   - Send email to Owner

2. **Send overdue alert**
   - Find records: Tasks
   - Filter: Is Overdue = "⚠️ Overdue"
   - Send email to Owner AND Project Owner

---

### Automation 3: Prevent Task Without Owner

**Trigger**:
- When record matches conditions
- Table: Tasks
- Condition: Status changes to Planned AND Owner is empty

**Action**:
- Send notification: "Task must have an owner before moving to Planned"
- Revert status to Backlog

---

### Automation 4: Vendor Without Tasks Warning

**Trigger**:
- When record matches conditions
- Table: Vendors
- Condition: 계약 상태 = "Signed" AND Has Active Tasks = "❌ No"

**Action**:
- Send notification to 담당자: "Vendor marked Signed but has no tasks"
- Prevent status change

---

### Automation 5: Weekly Status Report

**Trigger**:
- At a scheduled time
- Frequency: Every Friday at 5:00 PM

**Action**:
- Generate summary report
- Send to all Core Team members
- Include:
  - Tasks completed this week
  - Tasks due next week
  - Blocked/Overdue tasks
  - Grant status summary

---

## VALIDATION RULES

### Task Validations
- Owner required when Status ≠ Backlog
- Related Project always required
- Vendor must have tasks if contract = Signed

### Grant Validations
- 지원금 cannot exceed 총 사업비
- End Date must be after Start Date
- Default 담당자 = 이재영

### Vendor Validations
- 계약 상태 = "Signed" requires at least one Task
- Warning if not linked to any Grant or Project

---

## INITIAL DATA SETUP

### 1. Create People Records
Already listed above. Create these 3 people first.

### 2. Test Relationships
- Create a test Project
- Create a test Task linked to that Project
- Create a test Grant
- Link Grant to Project
- Verify all rollups and formulas calculate correctly

---

## TESTING CHECKLIST

- [ ] All 6 tables created
- [ ] All relationships work (links go both ways)
- [ ] All formula fields calculate correctly
- [ ] All views display and filter correctly
- [ ] All 5 interfaces created and configured
- [ ] Automation 1-5 set up and tested
- [ ] Validation rules tested
- [ ] Initial people records created
- [ ] Test data flows correctly through all tables

---

## ACCESS CONTROL

### Interface Permissions

| Interface | Can View | Can Edit |
|-----------|---------|----------|
| CEO Dashboard | 황효상 only | 황효상 only |
| Dev Dashboard | 최태석 only | 최태석 only |
| Marketing/Grants Dashboard | 이재영 only | 이재영 only |
| Grant Detail View | All | All |
| Weekly Review | All | All |

### Table Permissions

| Table | Core Team | External |
|-------|----------|----------|
| Tasks | Read/Write | Read only |
| Projects | Read/Write | Read only |
| Grants | Read/Write | Read only |
| Vendors | Read/Write | Read only |
| People | Read | Read only |
| Decisions | Read/Write | Read only |

---

## TROUBLESHOOTING

### Common Issues

**Issue**: Rollup fields showing 0
- **Fix**: Make sure linked records actually have the referenced field

**Issue**: Automations not triggering
- **Fix**: Check automation permissions, make sure you're the owner or have admin access

**Issue**: Formula errors
- **Fix**: Check field names match exactly (case-sensitive), check for missing fields

**Issue**: Interface not showing all sections
- **Fix**: Check view permissions on source tables

---

## SUPPORT

For issues or questions:
- Airtable Support: https://support.airtable.com
- Airtable Automation Guide: https://support.airtable.com/hc/en-us/articles/221032717
- Airtable Formula Reference: https://support.airtable.com/hc/en-us/articles/203255215-Formula-field-reference

---

## NEXT STEPS

1. ✅ Create all tables and fields
2. ✅ Set up all views
3. ✅ Create all interfaces
4. ✅ Set up automations
5. ✅ Create initial people records
6. ✅ Test with sample data
7. ✅ Train team on usage
8. ✅ Go live!

---

**Last Updated**: 2026-01-13
**Version**: 1.0
