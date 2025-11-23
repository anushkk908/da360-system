# Digital Academy 360 - Google Sheets Database Structure

## Complete Sheet Structure with Sample Data

### Sheet 1: STUDENTS

| StudentID | Name | Phone | Email | BatchID | DeviceID | Status | RegisteredOn |
|-----------|------|-------|-------|---------|----------|--------|--------------|
| STD001 | Rahul Kumar | 9876543210 | rahul@example.com | BATCH001 | abc123def456 | Active | 2025-01-15 |
| STD002 | Priya Sharma | 9876543211 | priya@example.com | BATCH001 | def456ghi789 | Active | 2025-01-15 |
| STD003 | Amit Patel | 9876543212 | amit@example.com | BATCH002 | ghi789jkl012 | Active | 2025-02-01 |
| STD004 | Sneha Singh | 9876543213 | sneha@example.com | BATCH002 | jkl012mno345 | Active | 2025-02-01 |
| STD005 | Vikram Reddy | 9876543214 | vikram@example.com | BATCH003 | mno345pqr678 | Inactive | 2025-01-20 |

**Columns:**
- StudentID: Unique identifier (Primary Key)
- Name: Full name
- Phone: Contact number
- Email: Email address
- BatchID: Current batch assignment (Foreign Key)
- DeviceID: SHA-256 hash of device fingerprint
- Status: Active/Inactive
- RegisteredOn: Registration date

---

### Sheet 2: TRAINERS

| TrainerID | Name | Email | Phone | Status |
|-----------|------|-------|-------|--------|
| TR001 | Suresh Mehta | suresh@da360.com | 9876543220 | Active |
| TR002 | Anjali Desai | anjali@da360.com | 9876543221 | Active |
| TR003 | Rajesh Kumar | rajesh@da360.com | 9876543222 | Active |
| TR004 | Kavita Nair | kavita@da360.com | 9876543223 | On Leave |

**Columns:**
- TrainerID: Unique identifier (Primary Key)
- Name: Trainer name
- Email: Email address
- Phone: Contact number
- Status: Active/On Leave/Inactive

---

### Sheet 3: BATCHES

| BatchID | Name | TrainerID | ClassroomID | Mode | StartDate | EndDate | Status | Notes |
|---------|------|-----------|-------------|------|-----------|---------|--------|-------|
| BATCH001 | Web Dev Morning | TR001 | CR001 | Offline | 2025-01-15 | 2025-04-15 | Active | Regular batch |
| BATCH002 | Data Science Eve | TR002 | CR002 | Offline | 2025-02-01 | 2025-05-01 | Active | Weekend batch |
| BATCH003 | Digital Marketing | TR001 | ONLINE | Online | 2025-01-20 | 2025-03-20 | Active | Online only |
| BATCH004 | Python Basics | TR003 | CR001 | Offline | 2025-02-10 | 2025-05-10 | Active | New batch |

**Columns:**
- BatchID: Unique identifier (Primary Key)
- Name: Batch name
- TrainerID: Assigned trainer (Foreign Key)
- ClassroomID: Assigned classroom or "ONLINE" (Foreign Key)
- Mode: Online/Offline
- StartDate: Batch start date
- EndDate: Batch end date
- Status: Active/Completed/Paused
- Notes: Additional information

---

### Sheet 4: CLASSROOMS

| ClassroomID | LocationName | Latitude | Longitude | RadiusMeters | Active |
|-------------|--------------|----------|-----------|--------------|--------|
| CR001 | Main Building - Room 101 | 19.2403 | 72.8530 | 50 | Yes |
| CR002 | Main Building - Room 102 | 19.2405 | 72.8532 | 50 | Yes |
| CR003 | Annex Building - Hall A | 19.2410 | 72.8535 | 100 | Yes |
| ONLINE | Online Classroom | 0 | 0 | 0 | Yes |

**Columns:**
- ClassroomID: Unique identifier (Primary Key)
- LocationName: Readable location name
- Latitude: GPS latitude coordinate
- Longitude: GPS longitude coordinate
- RadiusMeters: Geofence radius
- Active: Yes/No

---

### Sheet 5: MODULES

| ModuleID | BatchID | ModuleName | Status | TentativeCompletionDate | ActualCompletionDate | TrainerID | Notes |
|----------|---------|------------|--------|------------------------|---------------------|-----------|-------|
| MOD001 | BATCH001 | HTML Basics | COMPLETED | 2025-01-20 | 2025-01-20 | TR001 | Completed on time |
| MOD002 | BATCH001 | CSS Fundamentals | COMPLETED | 2025-01-27 | 2025-01-28 | TR001 | 1 day delay |
| MOD003 | BATCH001 | JavaScript Intro | IN_PROGRESS | 2025-02-05 | | TR001 | Currently teaching |
| MOD004 | BATCH001 | React Basics | PENDING | 2025-02-15 | | TR001 | Not started |
| MOD005 | BATCH002 | Python Basics | COMPLETED | 2025-02-08 | 2025-02-08 | TR002 | On schedule |
| MOD006 | BATCH002 | Data Analysis | IN_PROGRESS | 2025-02-18 | | TR002 | Week 3 |

**Columns:**
- ModuleID: Unique identifier (Primary Key)
- BatchID: Associated batch (Foreign Key)
- ModuleName: Module name
- Status: PENDING/IN_PROGRESS/COMPLETED
- TentativeCompletionDate: Planned date
- ActualCompletionDate: Actual completion date
- TrainerID: Assigned trainer (Foreign Key)
- Notes: Additional information

---

### Sheet 6: ATTENDANCE_LOG

| Timestamp | Date | StudentID | BatchID | TrainerID | PunchInTime | PunchOutTime | DurationMinutes | Status | DeviceVerified | GeoVerified | Remarks |
|-----------|------|-----------|---------|-----------|-------------|--------------|-----------------|--------|----------------|-------------|---------|
| 2025-11-23 11:05:00 | 2025-11-23 | STD001 | BATCH001 | TR001 | 11:05:00 | 13:28:00 | 143 | PRESENT | Yes | Yes | Regular attendance |
| 2025-11-23 11:07:00 | 2025-11-23 | STD002 | BATCH001 | TR001 | 11:07:00 | 13:30:00 | 143 | PRESENT | Yes | Yes | Regular attendance |
| 2025-11-23 17:05:00 | 2025-11-23 | STD003 | BATCH002 | TR002 | 17:05:00 | 18:50:00 | 105 | PRESENT | Yes | Yes | Regular attendance |
| 2025-11-22 11:10:00 | 2025-11-22 | STD001 | BATCH001 | TR001 | 11:10:00 | 12:30:00 | 80 | HALF_DAY | Yes | Yes | Left early |
| 2025-11-22 11:00:00 | 2025-11-22 | STD002 | BATCH001 | TR001 | | | 0 | ABSENT | No | No | No punch in |

**Columns:**
- Timestamp: Record creation timestamp
- Date: Attendance date
- StudentID: Student identifier (Foreign Key)
- BatchID: Batch identifier (Foreign Key)
- TrainerID: Trainer identifier (Foreign Key)
- PunchInTime: Login time
- PunchOutTime: Logout time
- DurationMinutes: Calculated duration
- Status: PRESENT/HALF_DAY/ABSENT
- DeviceVerified: Yes/No
- GeoVerified: Yes/No (N/A for online)
- Remarks: Additional notes

---

### Sheet 7: TRAINER_ATTENDANCE

| Date | TrainerID | Status | SubstituteTrainerID | Remarks |
|------|-----------|--------|---------------------|---------|
| 2025-11-23 | TR001 | PRESENT | | Regular day |
| 2025-11-23 | TR002 | PRESENT | | Regular day |
| 2025-11-22 | TR001 | PRESENT | | Regular day |
| 2025-11-22 | TR002 | ABSENT | TR003 | Medical leave |
| 2025-11-21 | TR003 | PRESENT | | Regular day |

**Columns:**
- Date: Attendance date
- TrainerID: Trainer identifier (Foreign Key)
- Status: PRESENT/ABSENT/ON_LEAVE
- SubstituteTrainerID: Replacement trainer if absent
- Remarks: Reason for absence

---

### Sheet 8: AUDIT_LOG

| Timestamp | Action | OldValue | NewValue | BatchID | TrainerID | ClassroomID | AdminID | Entity |
|-----------|--------|----------|----------|---------|-----------|-------------|---------|--------|
| 2025-11-23 10:30:00 | BATCH_MERGE | BATCH001,BATCH002 | BATCH005 | BATCH005 | TR001 | CR001 | admin@da360.com | BATCH |
| 2025-11-23 09:15:00 | TRAINER_CHANGE | TR001 | TR002 | BATCH003 | TR002 | ONLINE | admin@da360.com | BATCH |
| 2025-11-22 14:20:00 | CLASSROOM_CHANGE | CR001 | CR002 | BATCH001 | TR001 | CR002 | admin@da360.com | BATCH |
| 2025-11-22 11:00:00 | MODULE_COMPLETED | IN_PROGRESS | COMPLETED | BATCH001 | TR001 | | TR001 | MODULE |
| 2025-11-21 16:45:00 | STUDENT_MOVED | BATCH001 | BATCH002 | BATCH002 | TR002 | | admin@da360.com | STUDENT |

**Columns:**
- Timestamp: Action timestamp
- Action: Type of change
- OldValue: Previous value
- NewValue: New value
- BatchID: Related batch
- TrainerID: Related trainer
- ClassroomID: Related classroom
- AdminID: Actor email
- Entity: STUDENT/TRAINER/BATCH/MODULE/CLASSROOM

---

### Sheet 9: CHANGE_HISTORY

| Timestamp | ActorEmail | ActionType | DetailsJSON |
|-----------|------------|------------|-------------|
| 2025-11-23 10:30:00 | admin@da360.com | BATCH_MERGE | {"sourceBatches":["BATCH001","BATCH002"],"targetBatch":"BATCH005","studentsM oved":25,"reason":"Low enrollment"} |
| 2025-11-23 09:15:00 | admin@da360.com | TRAINER_CHANGE | {"batchID":"BATCH003","oldTrainer":"TR001","newTrainer":"TR002","reason":"Trainer request"} |
| 2025-11-22 14:20:00 | admin@da360.com | CLASSROOM_CHANGE | {"batchID":"BATCH001","oldClassroom":"CR001","newClassroom":"CR002","reason":"Room maintenance"} |

**Columns:**
- Timestamp: Change timestamp
- ActorEmail: Who made the change
- ActionType: Type of action
- DetailsJSON: Complete change details in JSON format

---

### Sheet 10: NOTIFICATIONS

| NotificationID | Timestamp | RecipientEmail | RecipientType | Subject | Body | SentStatus | ReadStatus | BatchID |
|----------------|-----------|----------------|---------------|---------|------|------------|------------|---------|
| NOTIF001 | 2025-11-23 10:35:00 | rahul@example.com | STUDENT | Batch Merged | Your batch BATCH001 has been merged into BATCH005 | SENT | UNREAD | BATCH005 |
| NOTIF002 | 2025-11-23 10:35:00 | suresh@da360.com | TRAINER | Batch Merged | Batches BATCH001 and BATCH002 merged into BATCH005 | SENT | READ | BATCH005 |
| NOTIF003 | 2025-11-23 09:20:00 | amit@example.com | STUDENT | Trainer Changed | Your trainer has been changed from Suresh to Anjali | SENT | READ | BATCH003 |
| NOTIF004 | 2025-11-22 14:25:00 | rahul@example.com | STUDENT | Classroom Changed | Your classroom has been changed to Room 102 | SENT | READ | BATCH001 |

**Columns:**
- NotificationID: Unique identifier
- Timestamp: Creation timestamp
- RecipientEmail: Recipient email address
- RecipientType: STUDENT/TRAINER/ADMIN
- Subject: Notification subject
- Body: Notification message
- SentStatus: PENDING/SENT/FAILED
- ReadStatus: READ/UNREAD
- BatchID: Related batch (if applicable)

---

### Sheet 11: CONFIG

| ConfigKey | ConfigValue | Description |
|-----------|-------------|-------------|
| InstituteLatitude | 19.2403 | Main institute location |
| InstituteLongitude | 72.8530 | Main institute location |
| AllowedRadius | 50 | Default geofence radius (meters) |
| DailyCodeSeed | DA360SECRET2025 | Seed for daily code generation |
| AdminEmail | admin@da360.com | Primary admin email |
| NotificationEmailServer | noreply@da360.com | Email sender address |
| MinDurationMinutes | 90 | Minimum class duration for PRESENT |
| HalfDayThreshold | 60 | Minimum for HALF_DAY status |
| TimeZone | Asia/Kolkata | System timezone |
| MaxDeviceChanges | 2 | Max device changes per student |
| AttendanceCodeExpiry | 300 | Code validity in seconds (5 min) |
| GeofenceRequired | true | Enforce geofencing for offline |
| SystemVersion | 1.0 | Current system version |

**Columns:**
- ConfigKey: Configuration parameter name
- ConfigValue: Parameter value
- Description: What this config controls

---

### Sheet 12: ATTENDANCE_MATRIX

| StudentID | Name | 2025-11-18 | 2025-11-19 | 2025-11-20 | 2025-11-21 | 2025-11-22 | 2025-11-23 | Percentage |
|-----------|------|------------|------------|------------|------------|------------|------------|------------|
| STD001 | Rahul Kumar | P | P | P | P | H | P | 91.67% |
| STD002 | Priya Sharma | P | P | P | P | A | P | 83.33% |
| STD003 | Amit Patel | P | P | P | P | P | P | 100% |
| STD004 | Sneha Singh | P | A | P | P | P | P | 83.33% |

**This sheet is AUTO-GENERATED via trigger**

Columns:
- StudentID: Student identifier
- Name: Student name
- Date columns: P (Present), A (Absent), H (Half Day)
- Percentage: Monthly attendance percentage

---

### Sheet 13: TIMING_SLOTS

| TimingSlotID | TimingLabel | StartTime | EndTime | Mode | CreatedBy | CreatedAt |
|--------------|-------------|-----------|---------|------|-----------|-----------|
| TS001 | Morning Offline | 11:00 AM | 01:30 PM | Offline | admin@da360.com | 2025-01-01 |
| TS002 | Afternoon Offline | 02:30 PM | 05:00 PM | Offline | admin@da360.com | 2025-01-01 |
| TS003 | Evening Offline | 05:00 PM | 07:00 PM | Offline | admin@da360.com | 2025-01-01 |
| TS004 | Early Morning Online | 07:30 AM | 09:30 AM | Online | admin@da360.com | 2025-01-01 |
| TS005 | Mid Morning Online | 10:30 AM | 12:30 PM | Online | admin@da360.com | 2025-01-01 |
| TS006 | Night Online | 09:00 PM | 10:30 PM | Online | admin@da360.com | 2025-01-01 |
| TS007 | Custom Weekend | 09:00 AM | 12:00 PM | Offline | admin@da360.com | 2025-02-01 |

**Columns:**
- TimingSlotID: Unique identifier (Primary Key)
- TimingLabel: Readable name
- StartTime: Class start time
- EndTime: Class end time
- Mode: Online/Offline
- CreatedBy: Admin who created this
- CreatedAt: Creation timestamp

---

### Sheet 14: BATCH_TIMINGS

| BatchTimingID | BatchID | TimingSlotID | EffectiveFrom | EffectiveTo | Status |
|---------------|---------|--------------|---------------|-------------|--------|
| BT001 | BATCH001 | TS001 | 2025-01-15 | 2025-04-15 | Active |
| BT002 | BATCH002 | TS003 | 2025-02-01 | 2025-05-01 | Active |
| BT003 | BATCH003 | TS004 | 2025-01-20 | 2025-03-20 | Active |
| BT004 | BATCH004 | TS001 | 2025-02-10 | 2025-05-10 | Active |
| BT005 | BATCH001 | TS002 | 2025-02-01 | 2025-02-28 | Historical |

**Columns:**
- BatchTimingID: Unique identifier (Primary Key)
- BatchID: Associated batch (Foreign Key)
- TimingSlotID: Assigned timing slot (Foreign Key)
- EffectiveFrom: Start date of this timing
- EffectiveTo: End date of this timing
- Status: Active/Historical

---

## Relationships

```
STUDENTS.BatchID → BATCHES.BatchID
STUDENTS.DeviceID → Unique per student

BATCHES.TrainerID → TRAINERS.TrainerID
BATCHES.ClassroomID → CLASSROOMS.ClassroomID

BATCH_TIMINGS.BatchID → BATCHES.BatchID
BATCH_TIMINGS.TimingSlotID → TIMING_SLOTS.TimingSlotID

MODULES.BatchID → BATCHES.BatchID
MODULES.TrainerID → TRAINERS.TrainerID

ATTENDANCE_LOG.StudentID → STUDENTS.StudentID
ATTENDANCE_LOG.BatchID → BATCHES.BatchID
ATTENDANCE_LOG.TrainerID → TRAINERS.TrainerID

TRAINER_ATTENDANCE.TrainerID → TRAINERS.TrainerID
TRAINER_ATTENDANCE.SubstituteTrainerID → TRAINERS.TrainerID

NOTIFICATIONS.RecipientEmail → STUDENTS.Email OR TRAINERS.Email
NOTIFICATIONS.BatchID → BATCHES.BatchID
```

## How to Create This Spreadsheet

1. Create new Google Spreadsheet named "DA360_Database"
2. Create 14 sheets with exact names above
3. Add column headers as first row in each sheet
4. Add sample data rows
5. Format headers (bold, background color)
6. Freeze header row (View → Freeze → 1 row)
7. Note the Spreadsheet ID from URL
8. Use this ID in Apps Script

## Data Validation Rules

**STUDENTS Sheet:**
- Status: Dropdown (Active, Inactive)
- Email: Must contain @

**TRAINERS Sheet:**
- Status: Dropdown (Active, On Leave, Inactive)

**BATCHES Sheet:**
- Mode: Dropdown (Online, Offline)
- Status: Dropdown (Active, Completed, Paused)
- TrainerID: Validate against TRAINERS sheet
- ClassroomID: Validate against CLASSROOMS sheet

**CLASSROOMS Sheet:**
- Active: Dropdown (Yes, No)
- RadiusMeters: Number between 10-500

**MODULES Sheet:**
- Status: Dropdown (PENDING, IN_PROGRESS, COMPLETED)

**ATTENDANCE_LOG Sheet:**
- Status: Dropdown (PRESENT, HALF_DAY, ABSENT)
- DeviceVerified: Dropdown (Yes, No)
- GeoVerified: Dropdown (Yes, No, N/A)

**TIMING_SLOTS Sheet:**
- Mode: Dropdown (Online, Offline)

This structure supports 300+ students with efficient lookups and scalability.