# Digital Academy 360 - System Architecture

## Overview
Complete Training Management System with 3 PWA Portals + Google Apps Script Backend + Single Spreadsheet Database

## System Components

```
┌─────────────────────────────────────────────────────────────────┐
│                    GITHUB PAGES (Hosting)                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                   │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │   Student    │  │   Trainer    │  │    Admin     │          │
│  │   Portal     │  │   Portal     │  │   Portal     │          │
│  │    (PWA)     │  │    (PWA)     │  │    (PWA)     │          │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘          │
│         │                  │                  │                   │
└─────────┼──────────────────┼──────────────────┼──────────────────┘
          │                  │                  │
          └──────────────────┼──────────────────┘
                             │
                    ┌────────▼────────┐
                    │  Google Apps    │
                    │     Script      │
                    │   (Backend API) │
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │ Google Sheets   │
                    │   (Database)    │
                    │  12 Sheets      │
                    └─────────────────┘
```

## Data Flow Architecture

### Student Portal Flow
```
StudentID → Students Sheet → BatchID → Batches Sheet
    ↓
TrainerID + ClassroomID + Modules
    ↓
Attendance Validation (Device + Geofence + Code)
    ↓
AttendanceLog + Notifications
```

### Trainer Portal Flow
```
TrainerID → Batches (where TrainerID matches)
    ↓
Students in those Batches
    ↓
Today's Attendance + Module Progress
    ↓
Mark Attendance / Complete Modules
```

### Admin Portal Flow
```
Admin Access → All Sheets
    ↓
Create/Update/Delete: Students, Trainers, Batches, Classrooms, Modules
    ↓
Trigger: Notifications + Audit Logs + Email
```

## Attendance Flow Sequence

```
┌─────────┐                 ┌──────────┐                ┌──────────┐
│ Student │                 │  System  │                │  Admin   │
└────┬────┘                 └────┬─────┘                └────┬─────┘
     │                           │                           │
     │ 1. Login                  │                           │
     ├─────────────────────────> │                           │
     │                           │                           │
     │ 2. Verify Device Hash     │                           │
     │ <─────────────────────────┤                           │
     │                           │                           │
     │ 3. Request Attendance Code│                           │
     ├─────────────────────────> │                           │
     │                           │                           │
     │ 4. Check Geolocation      │                           │
     │ <─────────────────────────┤                           │
     │                           │                           │
     │ 5. Generate Unique Code   │                           │
     │ <─────────────────────────┤                           │
     │                           │                           │
     │ 6. Punch In               │                           │
     ├─────────────────────────> │                           │
     │                           │                           │
     │                           │ 7. Log to AttendanceLog   │
     │                           ├─────────────────────────> │
     │                           │                           │
     │ 8. Session Active         │                           │
     │ <─────────────────────────┤                           │
     │                           │                           │
     │ ... Class Duration ...    │                           │
     │                           │                           │
     │ 9. Punch Out              │                           │
     ├─────────────────────────> │                           │
     │                           │                           │
     │                           │ 10. Calculate Duration    │
     │                           ├───────┐                   │
     │                           │       │                   │
     │                           │ <─────┘                   │
     │                           │                           │
     │                           │ 11. Mark P/H/A Status     │
     │                           ├─────────────────────────> │
     │                           │                           │
     │ 12. Attendance Recorded   │                           │
     │ <─────────────────────────┤                           │
```

## Batch Management Flow

```
┌──────────────────────────────────────────────────────────┐
│                   BATCH OPERATIONS                        │
├──────────────────────────────────────────────────────────┤
│                                                            │
│  CREATE BATCH                                             │
│  ├─ Assign BatchID (unique)                              │
│  ├─ Assign TrainerID                                     │
│  ├─ Assign ClassroomID                                   │
│  ├─ Set Timings (from timing_slots)                     │
│  ├─ Set Mode (Online/Offline)                           │
│  └─ Trigger: Email to Trainer + Create Notification     │
│                                                            │
│  MERGE BATCHES                                            │
│  ├─ Select Source BatchIDs                               │
│  ├─ Create New BatchID                                   │
│  ├─ Move all Students to New BatchID                    │
│  ├─ Merge Module Lists                                   │
│  ├─ Update AttendanceLog references                     │
│  ├─ Deactivate Old Batches                              │
│  └─ Trigger: Email All + Notifications + Audit          │
│                                                            │
│  SPLIT BATCH                                              │
│  ├─ Select Source BatchID                                │
│  ├─ Create Multiple New BatchIDs                        │
│  ├─ Distribute Students                                  │
│  ├─ Duplicate Modules with different dates              │
│  ├─ Update References                                    │
│  └─ Trigger: Notifications + Audit                      │
│                                                            │
│  CHANGE TRAINER                                           │
│  ├─ Update Batches.TrainerID                            │
│  ├─ Update Modules.TrainerID                            │
│  └─ Trigger: Email Old + New Trainer + Students         │
│                                                            │
│  CHANGE CLASSROOM                                         │
│  ├─ Update Batches.ClassroomID                          │
│  └─ Trigger: Email Students + Trainer                   │
│                                                            │
└──────────────────────────────────────────────────────────┘
```

## Module Tracking Flow

```
┌────────────┐
│   Admin    │
│  Creates   │
│   Module   │
└─────┬──────┘
      │
      ▼
┌────────────────────────────────────┐
│ Module Entry in MODULES Sheet      │
│ - ModuleID (unique)                │
│ - BatchID                          │
│ - ModuleName                       │
│ - Status: PENDING                  │
│ - TentativeCompletionDate          │
│ - TrainerID                        │
└─────┬──────────────────────────────┘
      │
      ▼
┌────────────────────────────────────┐
│  Visible in Trainer Portal         │
│  Trainer marks: IN_PROGRESS        │
└─────┬──────────────────────────────┘
      │
      ▼
┌────────────────────────────────────┐
│  Trainer marks: COMPLETED          │
│  - Sets ActualCompletionDate       │
│  - Adds Notes                      │
└─────┬──────────────────────────────┘
      │
      ▼
┌────────────────────────────────────┐
│  Notifications Triggered           │
│  - Email to Admin                  │
│  - Email to Students               │
│  - In-app notification             │
└────────────────────────────────────┘
```

## Google Sheets Structure

### Sheet List
1. **STUDENTS** - Student master data with device binding
2. **TRAINERS** - Trainer master data
3. **BATCHES** - Batch configuration with trainer/classroom assignment
4. **CLASSROOMS** - Physical locations with geofence coordinates
5. **MODULES** - Module tracking per batch
6. **ATTENDANCE_LOG** - Punch in/out records with validation
7. **TRAINER_ATTENDANCE** - Trainer attendance and substitutes
8. **AUDIT_LOG** - Complete change tracking
9. **CHANGE_HISTORY** - JSON-based history
10. **NOTIFICATIONS** - In-app notification queue
11. **CONFIG** - System configuration
12. **ATTENDANCE_MATRIX** - Pivot view (auto-generated)
13. **TIMING_SLOTS** - Time slot definitions
14. **BATCH_TIMINGS** - Batch-specific timing assignments

## Security Layers

```
┌─────────────────────────────────────────────┐
│         Authentication Layer                 │
│  - StudentID/TrainerID + OTP                │
│  - Device Hash Verification                 │
│  - Admin Secret Key                         │
└─────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────┐
│         Validation Layer                     │
│  - Geofence (Haversine)                     │
│  - Unique Attendance Code                   │
│  - Time Window Validation                   │
│  - Duplicate Prevention                     │
└─────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────┐
│         Authorization Layer                  │
│  - Role-based Access (Student/Trainer/Admin)│
│  - Sheet-level Permissions                  │
│  - API Endpoint Restrictions                │
└─────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────┐
│         Audit Layer                          │
│  - All Changes Logged                       │
│  - Before/After Values                      │
│  - Timestamp + Actor                        │
└─────────────────────────────────────────────┘
```

## Notification Architecture

```
┌──────────────────────────────────────────────┐
│            NOTIFICATION ENGINE                │
├──────────────────────────────────────────────┤
│                                               │
│  Trigger Events:                             │
│  ├─ Batch Merge                             │
│  ├─ Batch Split                             │
│  ├─ Trainer Change                          │
│  ├─ Classroom Change                        │
│  ├─ Module Completed                        │
│  ├─ Module Rescheduled                      │
│  ├─ Trainer Absent                          │
│  └─ Student Moved                           │
│                                               │
│  Notification Channels:                      │
│  ├─ Email (via MailApp)                     │
│  │   └─ HTML Templates                      │
│  │                                           │
│  └─ In-App (NOTIFICATIONS sheet)            │
│      └─ Polling API                         │
│                                               │
│  Recipients:                                 │
│  ├─ Students (filtered by BatchID)          │
│  ├─ Trainers (specific TrainerID)           │
│  └─ Admin (all notifications)               │
│                                               │
└──────────────────────────────────────────────┘
```

## Technology Stack

- **Frontend**: Vanilla HTML5 + CSS3 + JavaScript (PWA)
- **Hosting**: GitHub Pages (Static)
- **Backend**: Google Apps Script (JavaScript)
- **Database**: Google Sheets (Single Spreadsheet)
- **Email**: Google MailApp API
- **Geolocation**: Browser Geolocation API
- **Authentication**: Custom OTP + Device Fingerprinting
- **Offline Support**: Service Workers + Cache API

## Key Features

### Device Locking
- SHA-256 hash of browser fingerprint
- Stored on first login
- Validated on every subsequent login
- Admin override capability

### Geofencing
- Haversine formula for distance calculation
- Classroom coordinates in CLASSROOMS sheet
- Configurable radius per classroom
- Bypassed for ONLINE batches
- Failed attempts logged for fraud detection

### Unique Attendance Code
- Daily 4-digit code per student
- Formula: Hash(StudentID + Date + DailyCodeSeed)
- Prevents code sharing
- Device + Geofence must also pass

### Batch Timings Intelligence
- Predefined slots (offline & online)
- Custom time slot creation
- Validation against overlaps
- Instant sync across all portals
- Notification on timing changes

## Deployment Flow

```
1. Create Google Spreadsheet
   └─ Create all 14 sheets with headers

2. Deploy Apps Script
   └─ Copy code to Apps Script Editor
   └─ Deploy as Web App
   └─ Set permissions (Anyone with link)
   └─ Copy Web App URL

3. Configure Frontend
   └─ Update API_URL in all portals
   └─ Update logo URL
   └─ Configure SERVICE_WORKER

4. Deploy to GitHub Pages
   └─ Create repo: da360-system
   └─ Upload student/, trainer/, admin/ folders
   └─ Enable GitHub Pages
   └─ Set branch: main, folder: /root

5. Set Triggers
   └─ Daily trigger: Auto-mark absent
   └─ Hourly trigger: Sync attendance matrix

6. Test System
   └─ Create test student
   └─ Create test trainer
   └─ Create test batch
   └─ Test attendance flow
   └─ Test notifications
```

## Scalability Considerations

- **Caching**: Apps Script Cache Service for frequent queries
- **Indexing**: Efficient lookups using filter() instead of nested loops
- **Batch Operations**: Process multiple students in single API call
- **Pagination**: For attendance reports with 300+ students
- **Rate Limiting**: Prevent abuse with request throttling

## Maintenance & Monitoring

- **Audit Logs**: Track all system changes
- **Error Logging**: Catch and log all exceptions
- **Performance Metrics**: Measure API response times
- **Data Validation**: Prevent corrupt data entry
- **Backup Strategy**: Weekly export of entire spreadsheet

---

**System Version**: 1.0  
**Last Updated**: November 2025  
**Designed for**: Digital Academy 360  
**Max Capacity**: 300+ students per center