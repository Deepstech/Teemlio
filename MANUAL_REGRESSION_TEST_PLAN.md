# Teemlio - Manual Regression Test Plan

## Overview
This document contains a focused manual regression test plan for critical features of the Teemlio platform. These are the essential tests that QA should perform manually after each update to catch critical issues before production release.

---

## Test Execution Guidelines

### When to Run
- **After every code update/deployment**
- **Before releasing to production**
- **When critical features are modified**

### Time Required
- **Quick Test (30 min)**: Critical path only
- **Standard Test (2-3 hours)**: All sections
- **Full Test (4-5 hours)**: All sections with edge cases

### Prerequisites
- Browser (Chrome, Firefox, Safari)
- 2-3 test user accounts (Owner, Admin, Staff)
- Mobile device/emulator (iOS/Android)
- Test data with sample events, contacts, templates

---

## CRITICAL PATH TESTS (Must Run Every Update - 30 mins)

| Test Case ID | Section | Steps | Expected Result | Priority |
|---|---|---|---|---|
| **CP-001** | **Login** | 1. Enter valid email, password, company ID | User redirected to calendar page (active subscription) or membership page (inactive) | CRITICAL |
| **CP-002** | **Login Error** | 1. Enter invalid email format (without @) | Error: "Please include @" displayed | CRITICAL |
| **CP-003** | **Subscription Check** | 1. Login as owner<br>2. Navigate to Membership | Subscription details display correctly with amount, dates | CRITICAL |
| **CP-004** | **Billing History** | 1. In Membership page<br>2. View Billing History section | Invoice list shows: Invoice ID, Date, Description, Amount | CRITICAL |
| **CP-005** | **Add Event** | 1. Click "Add New Event"<br>2. Select "Blank"<br>3. Fill Title, Client, Dates, Owner, Location<br>4. Click Save | Event created successfully, appears in Event List | CRITICAL |
| **CP-006** | **Event Detail** | 1. Click on event from list | Event detail page loads with all 5 tabs (Detail, Schedule, Checklist, Files, Location) | CRITICAL |
| **CP-007** | **Chat Send** | 1. Navigate to Chat<br>2. Open direct message<br>3. Type message, send | Message appears on right side in pink with timestamp | CRITICAL |
| **CP-008** | **Chat Delete** | 1. In Chat, open direct message<br>2. Click delete<br>3. Confirm | Chat removed from list | CRITICAL |
| **CP-009** | **Team Member Add** | 1. Go to Team Members<br>2. Click "Add New Team Member"<br>3. Fill Name, Email, Phone, Teemlio Handle<br>4. Click Save | Success message: "User added successfully" | CRITICAL |
| **CP-010** | **Contact Add** | 1. Go to Contacts<br>2. Click "Add New Contact"<br>3. Fill Name, Email, Phone, Tags<br>4. Click Save | Success message: "Contact added successfully" | CRITICAL |

---

## STANDARD TEST SUITE (Run for Most Updates - 2-3 hours)

### 1. AUTHENTICATION TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **AUTH-M-001** | Valid Login | Enter correct email, password, company ID | Redirected to calendar/membership page |
| **AUTH-M-002** | Invalid Email Format | Enter email without @ symbol | Error: "Please include @" |
| **AUTH-M-003** | Wrong Password | Enter correct email/company but wrong password | Error: "Incorrect email or Incorrect password" |
| **AUTH-M-004** | Missing Fields | Leave any required field empty, click login | Error: "Required" shown for each missing field |
| **AUTH-M-005** | Logout | Click logout button | Redirected to login page, session ended |

---

### 2. SUBSCRIPTION & BILLING TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **BILL-M-001** | Membership Page Display | Navigate to Membership page as owner | Shows: Teemlio Workspace, rate, amount, started date, next invoice date |
| **BILL-M-002** | Auto-Renewal ON | Check auto-renewal status when ON | Message shows: "Auto-Renewal is Currently Turned On. Your Plan Will Renew on [date]" |
| **BILL-M-003** | Auto-Renewal OFF | Check auto-renewal status when OFF | Message shows: "Auto-Renewal is Currently Turned Off. Your Plan Will Expire on [date]" |
| **BILL-M-004** | Billing History List | View billing history section | Columns: Invoice ID, Date, Description, Amount, Action visible |
| **BILL-M-005** | Download Invoice | Click action button on invoice | Invoice/payment details downloaded successfully |
| **BILL-M-006** | Admin No Subscription | Login as admin, go to Membership | Page shows "No Subscription" message |

---

### 3. TEAM MEMBERS TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **TEAM-M-001** | List Display | Go to Team Members page | All team members displayed with columns: Picture, Name, Handle, Email, Phone, Address, Action |
| **TEAM-M-002** | Add New Member | Click "Add Team Member", fill Name/Email/Phone/Handle, save | Success: "User added successfully" |
| **TEAM-M-003** | Add Duplicate Email | Try to add team member with existing email in same company | Error: "Email already exists" |
| **TEAM-M-004** | Edit Member | Click edit on team member row, change values, save | Success: "User updated successfully" |
| **TEAM-M-005** | Delete Member | Click delete, confirm | Team member removed from list |
| **TEAM-M-006** | Search Member | Enter name in search field | Matching team members displayed |

---

### 4. CONTACTS TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **CONTACT-M-001** | List Display | Go to Contacts page | All contacts displayed with: Picture, Name, Phone, Tags |
| **CONTACT-M-002** | Add Contact | Click "Add New Contact", fill Name/Email/Phone/Tags, save | Success: "Contact added successfully" |
| **CONTACT-M-003** | View Contact Details | Click on contact name | Right panel shows: Name, Tags, Teemlio Handle, Phone, Email, Address |
| **CONTACT-M-004** | Events Tab | Click Events tab on contact detail | Shows all events for contact with: Event name, Address, Date, Description |
| **CONTACT-M-005** | Add Note | Click "Add New Notes", enter note, save | Success: "Notes Saved" |
| **CONTACT-M-006** | Edit Note | Click edit on note, change text, save | Success: "Updated Notes" |
| **CONTACT-M-007** | Delete Note | Click delete on note, confirm | Note removed, shows: "Notes Deleted" |
| **CONTACT-M-008** | Delete Contact | Click delete on contact, confirm | Contact removed from list |
| **CONTACT-M-009** | Search Contact | Enter name in search | Matching contacts displayed |

---

### 5. EVENT TEMPLATES TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **TEMPLATE-M-001** | Template List | Go to Templates page | All templates displayed with: Event Title, Description, Date Modified, Action |
| **TEMPLATE-M-002** | Add Template | Click "Add Template", enter Title & Description, save | Redirected to template detail page |
| **TEMPLATE-M-003** | Edit Template | Click edit icon, modify Title/Description, save | Success: "Updated successfully" |
| **TEMPLATE-M-004** | View Template Tabs | Open template detail | 4 tabs visible: Event Details, Schedule, Checklist, File and Media |
| **TEMPLATE-M-005** | Add Schedule to Template | Go to Schedule tab, add new schedule with date/description | Schedule added to template |
| **TEMPLATE-M-006** | Add Checklist to Template | Go to Checklist tab, add inventory item | Inventory item added |
| **TEMPLATE-M-007** | Add Folder to Template | Go to File & Media tab, add folder | Folder created and appears in list |
| **TEMPLATE-M-008** | Delete Template | Click delete, confirm | Template removed from list, shows: "Are You Sure You Want to Delete?" |
| **TEMPLATE-M-009** | Search Template | Enter title in search | Matching templates displayed |

---

### 6. EVENT MANAGEMENT TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **EVENT-M-001** | Event List Display | Go to Events page | All events displayed with: Title, Customer, Owner, Start Date, End Date, Location |
| **EVENT-M-002** | Add Blank Event | Click "Add Event" → "Blank", fill all required fields, save | Event created and appears in list |
| **EVENT-M-003** | Add Template Event | Click "Add Event" → "Template", select template, save | Event created with template data pre-filled |
| **EVENT-M-004** | View Event Detail | Click event title | Event detail page loads with title, location, dates, client info |
| **EVENT-M-005** | View Event Tabs | On event detail page, check all tabs | All 5 tabs present: Event Detail, Schedule, Checklist, Files, Live Location |
| **EVENT-M-006** | Add Schedule to Event | Go to Schedule tab, add schedule | Schedule appears in list |
| **EVENT-M-007** | Add Inventory to Event | Go to Checklist tab, add inventory | Inventory appears with Qty, Status columns |
| **EVENT-M-008** | Mark Inventory Installed | Click "Installed" button on inventory | Button turns green, status updated |
| **EVENT-M-009** | Upload File | Go to Files tab, create folder, upload image | File uploaded and displays with thumbnail |
| **EVENT-M-010** | Search Events | Enter event title in search | Matching events displayed |
| **EVENT-M-011** | Delete Event | Click delete on event, confirm | Shows: "Are You Sure You Want to Delete This Event?", event removed after confirmation |

---

### 7. CALENDAR/DASHBOARD TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **CALENDAR-M-001** | Calendar Default View | Navigate to Calendar page | Month view displayed with event titles on dates |
| **CALENDAR-M-002** | Week View | Click "Week" option | Shows Sunday-Saturday week with events |
| **CALENDAR-M-003** | Day View | Click "Day" option | Shows current day with all events |
| **CALENDAR-M-004** | List View | Click "List" option | Shows all events for current week with date range header |
| **CALENDAR-M-005** | Events Today Panel | View right panel | Shows all events for today with title, start date, address, client |
| **CALENDAR-M-006** | Event Count | Check event counter | Correct number shown for today's events |
| **CALENDAR-M-007** | Chat from Calendar | Click chat icon on event | Redirected to chat page with that contact |

---

### 8. CHAT & MESSAGING TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **CHAT-M-001** | Open Chat Page | Navigate to Chat | Messages list on left, chat area on right |
| **CHAT-M-002** | Direct Message List | View Direct Message tab | All direct chats listed with: Profile, Name, Last message, Time |
| **CHAT-M-003** | Send Message | Click contact, type message, send | Message appears on right side in pink |
| **CHAT-M-004** | Message Timestamp | Send message | Timestamp displayed correctly on message |
| **CHAT-M-005** | Message Seen Indicator | Send message, recipient reads | Double tick (pink) shown on message |
| **CHAT-M-006** | Unread Count | Receive messages without reading | Unread count badge shown on contact |
| **CHAT-M-007** | Delete Direct Chat | Click delete on chat, confirm | Chat removed from direct message list |
| **CHAT-M-008** | Group Chat Tab | Click Group Chat tab | All group chats displayed |
| **CHAT-M-009** | Archive Chat | Click archive on direct message | Chat moved to Archive tab |
| **CHAT-M-010** | Unarchive Chat | Click unarchive on archived chat | Chat moved back to Direct Message |

---

### 9. NOTIFICATION TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **NOTIF-M-001** | Notification Menu | Click bell icon in menu | Dropdown shows recent notifications (4-5 most recent) |
| **NOTIF-M-002** | Notification Count | View bell icon badge | Badge shows correct number of unread notifications |
| **NOTIF-M-003** | Mark as Read | Click on notification | Notification marked as read, count decreases |
| **NOTIF-M-004** | Mark All as Read | Click "Mark All as Read" button | All notifications marked as read, count resets to 0 |
| **NOTIF-M-005** | Notification Types | Check notification center | Payment, Subscription, Membership notifications display correctly |
| **NOTIF-M-006** | Read More | Click "Read More" in dropdown | Full notification page opens |

---

### 10. GENERAL FUNCTIONALITY TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **GEN-M-001** | Page Load | Navigate to each major page | All pages load without errors or broken layouts |
| **GEN-M-002** | Error Handling | Trigger error (e.g., invalid input) | Error message displayed clearly to user |
| **GEN-M-003** | Success Messages | Complete action (add/update/delete) | Success message displays briefly |
| **GEN-M-004** | Form Validation | Submit form with missing required fields | Error shown for each missing field with highlight |
| **GEN-M-005** | Responsive Design | View page on desktop and tablet | Layout adjusts correctly for different screen sizes |
| **GEN-M-006** | Data Persistence | Refresh page after making changes | Data remains intact, not lost |
| **GEN-M-007** | Navigation Menu | Click different menu items | Each item navigates to correct page |
| **GEN-M-008** | Pagination | View list with many items | Pagination controls present and functional |
| **GEN-M-009** | Sorting | Click column headers | List sorts in correct order (ascending/descending) |
| **GEN-M-010** | Cross-browser | Test on Chrome and Firefox | Features work consistently across browsers |

---

## MOBILE APP SPECIFIC TESTS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **MOBILE-M-001** | App Login | Enter credentials on mobile | User logged in successfully |
| **MOBILE-M-002** | Calendar Week View | Open Calendar on mobile | Default week view displayed |
| **MOBILE-M-003** | Event Detail Mobile | Tap on event | Event detail page opens with all tabs |
| **MOBILE-M-004** | Chat Mobile | Open Chat tab | Messages list and chat area functional |
| **MOBILE-M-005** | Send Message Mobile | Type and send message | Message sent and displays correctly |
| **MOBILE-M-006** | Notification Badge | Receive notification | Badge displays on menu icon |
| **MOBILE-M-007** | Tap Notification | Tap notification | Redirects to correct page (chat, notification center) |
| **MOBILE-M-008** | Subscription Expired Popup | Open app with expired subscription | Non-dismissible popup shows "Subscription Expired!!" |
| **MOBILE-M-009** | App Update Popup | App version outdated | Force update popup appears, cannot dismiss |
| **MOBILE-M-010** | Optional Update Popup | Minor app update available | Optional update popup shown, can skip |

---

## EDGE CASES & ERROR SCENARIOS

| TC ID | Test Case | Steps | Expected Result |
|---|---|---|---|
| **EDGE-M-001** | Empty List | Navigate to empty section (no events/contacts) | "No Data" message displayed |
| **EDGE-M-002** | Long Text Field | Enter very long text in description field | Text accepted and stored correctly |
| **EDGE-M-003** | Special Characters | Enter special chars (@, #, $, %, &) in form | Text accepted and saved correctly |
| **EDGE-M-004** | Rapid Clicks | Click submit button multiple times quickly | Only one request sent, no duplicates |
| **EDGE-M-005** | Slow Network | Test on slow internet | Loading indicators shown, operations eventually complete |
| **EDGE-M-006** | Session Expired | Stay logged in for extended period | Auto-logout occurs, redirected to login |
| **EDGE-M-007** | Delete Confirmation | Delete item, close popup before confirming | Item NOT deleted, remains in list |
| **EDGE-M-008** | Cancel Edit | Edit item, click cancel before saving | Changes NOT saved, original data remains |

---

## Test Execution Checklist

Use this checklist for each test run:

```
Date: ________________
Tester: ________________
Build Version: ________________
Environment: ☐ Web (Chrome/Firefox/Safari)  ☐ Mobile (iOS/Android)

CRITICAL PATH (30 mins)
☐ CP-001: Login
☐ CP-002: Login Error
☐ CP-003: Subscription Check
☐ CP-004: Billing History
☐ CP-005: Add Event
☐ CP-006: Event Detail
☐ CP-007: Chat Send
☐ CP-008: Chat Delete
☐ CP-009: Team Member Add
☐ CP-010: Contact Add

STANDARD SUITE (if time allows)
☐ Authentication Tests (5 tests)
☐ Subscription & Billing Tests (6 tests)
☐ Team Members Tests (6 tests)
☐ Contacts Tests (9 tests)
☐ Event Templates Tests (9 tests)
☐ Event Management Tests (11 tests)
☐ Calendar Tests (7 tests)
☐ Chat & Messaging Tests (10 tests)
☐ Notification Tests (6 tests)
☐ General Functionality Tests (10 tests)

MOBILE TESTS (if applicable)
☐ Mobile App Specific Tests (10 tests)

EDGE CASES (if time allows)
☐ Edge Cases Tests (8 tests)

SUMMARY
Total Tests Run: ____
Passed: ____
Failed: ____
Blocked: ____
Pass Rate: ____%

FAILURES (if any):
1. TC ID: ____________  Issue: ____________________________
2. TC ID: ____________  Issue: ____________________________
3. TC ID: ____________  Issue: ____________________________

Notes/Observations:
_____________________________________________________________
_____________________________________________________________

Tester Signature: ________________  Date: ________________
```

---

## When Tests Fail

1. **Document the failure**
   - Screenshot of error
   - Steps to reproduce
   - Expected vs Actual result
   - Browser/Device/OS details

2. **Severity Levels**
   - **CRITICAL**: Feature completely broken, blocks usage
   - **HIGH**: Feature broken but workaround exists
   - **MEDIUM**: Minor issue, feature still usable
   - **LOW**: Cosmetic issue, no functional impact

3. **Create Bug Ticket** with:
   - Test Case ID that failed
   - Reproduction steps
   - Screenshot/video
   - Expected result
   - Actual result

---

## Test Execution Frequency

| Update Type | Tests to Run | Time |
|---|---|---|
| **Critical Bug Fix** | CP (10) + Affected Section | 30-60 min |
| **Minor Feature Update** | CP (10) + Affected Section | 1-2 hours |
| **Major Feature Addition** | Full Standard Suite | 2-3 hours |
| **Multiple Updates** | Full Standard Suite | 2-3 hours |
| **Pre-Production** | CP (10) + Standard Suite | 2-3 hours |
| **Weekly Full** | CP (10) + Standard Suite + Mobile | 3-4 hours |

---

## Quick Reference

**Must Always Test After Update:**
1. Login with valid/invalid credentials
2. Subscription page displays correctly
3. Add/View/Delete event
4. Send/Receive chat message
5. Add contact and note
6. View error messages for invalid input
7. Check on mobile app
8. Verify no broken UI/layout

**Time-Saving Tips:**
- Use keyboard shortcuts (Tab to navigate, Enter to submit)
- Use test data generator for bulk operations
- Combine related tests (e.g., edit + delete together)
- Do mobile testing while desktop tests run
- Check browser console for JavaScript errors

