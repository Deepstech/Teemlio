# Teemlio - Event Management Platform Requirements

## Overview
Teemlio is an event management platform with web and mobile applications enabling users to create, manage, and track events with schedules, checklists, inventory, real-time location tracking, messaging, and subscription management capabilities.

---

## 1. Authentication & Account Management

### Login & Access Control
- **Required Fields**: Email ID, Password, Company ID
- **Validation Rules**:
  - Show "Required" error if any mandatory field is missing
  - Show "Please include @" if invalid email format
  - Show "Incorrect email or Incorrect password" for failed authentication
  - Prevent login if company is suspended
- **Redirect**: Upon successful login:
  - If subscription active: Redirect to calendar page (dashboard)
  - If subscription inactive: Redirect to membership/subscription page

### Reset Password
- **Required Fields**: Email ID, Company ID
- **Rules**:
  - Redirect to reset password page when clicked
  - Support multiple company IDs for same email (reset only for specified company)
  - Send reset email upon submission
  - Show "Required" error if fields missing
  
### Change Password
- **Process**:
  - User clicks email link from reset password email
  - Redirects to reset page with password input fields
  - Password must be changed to both password and confirm password fields
  - Old password cannot be used for login after change

### Forgot Company Code
- **Functionality**: Email company code to user
- **Rules**:
  - If user has multiple companies, show all company IDs and names in email
  - Support same email across multiple companies

---

## 2. Subscription & Billing

### Membership Page

#### Subscription Information
- **Page Title**: "Membership"
- **Display Section**: "Teemlio Workspace"
- **Show Rate**: Subscription plan rate

- **Amount Display**:
  - Subscription amount per month or year (based on database)
  - Format: "$XX.XX/month" or "$XX.XX/year"
  - Started at: MM/DD/YYYY
  - Next invoice date: MM/DD/YYYY

- **Auto-Renewal Status**:
  - **If OFF**: "Auto-Renewal is Currently Turned Off. Your Plan Will Expire on MM/DD/YYYY. Enable Auto-Renewal for Seamless Billing."
  - **If ON**: "Auto-Renewal is Currently Turned On. Your Plan Will Renew on MM/DD/YYYY. Manage your settings anytime."

- **User Management Section**:
  - Message: "Just a Gentle Reminder Easily Manage Users Whenever You Need. You Can Add or Remove Users at Any Time."
  - Add User button
  - Current user count display

#### Billing History
- **Section Name**: Billing History
- **List Columns**: Invoice ID, Date, Description, Amount, Action
- **Actions**: 
  - Download payment details
  - Pay (for unpaid invoices)

#### User Access Control
- **Owner/Admin Login**: Show full membership page
- **Admin Login**: Show message "No Subscription" (Admin cannot manage subscription)
- **Add User Option**:
  - Owner can add and pay
  - Admin cannot pay (Owner only)

#### Mobile App Subscription Validation
- **Subscription Expired**: Show modal popup - "Subscription Expired!!"
- **Non-dismissible**: No close button on popup
- **On Renewal**: Hide popup once subscription renewed

### Free Trial System
- **Trial Duration**: 14 days (configurable in system settings)
- **Plan Selection**: Users choose between Free Trial and Paid Workspace Plan
- **Features During Trial**:
  - Unlimited user additions without additional cost
  - Welcome email with trial-specific template
  - Trial expiry notification 2 days before expiration
  - Auto-redirect to subscription page upon expiration
  - Hide auto-payment option during active trial

- **Pricing**:
  - Base subscription: $39.99
  - Per user: $9.99
  - Formula: $39.99 + ($9.99 × number of users)

- **Membership Page Display**:
  - Show trial subscription status
  - Amount shows as 0 during trial
  - Subscription shows as 0 during trial

### Discount & Coupon Management

#### Admin Coupon Management
- **Access**: Admin portal
- **List View Columns**: Discount Code, Applicable On, Discount(%), Expiry Date, Account Limit, Applicable To, Action
- **Operations**: Add, Update, Delete (with confirmation)

#### Coupon Fields
- **Code**: User applies before payment
- **Applicable On**:
  - Signup: Only for new signups
  - New User Add-On: Only for adding new users
  - Invoices: For invoice cycle charges

- **Discount Percentage**: Supports decimal values
- **Expiry Date**: Date after which coupon expires
- **Max Per Account Limit**: Maximum uses per account (e.g., limit of 2 = max 2 uses)
- **Applicable To**:
  - All: Available to all users
  - Email: Specific email addresses only

#### Coupon Examples
1. **3-Month Free Trial**:
   - Code: ABCXYZ
   - Applicable On: Invoices
   - Discount %: 100
   - Expiry Date: 04-30-2026
   - Max Per Account Limit: 3
   - Applicable To: Email (abc@gmail.com)

2. **Holiday Discount**:
   - Code: CHRIS2025
   - Applicable On: Signup
   - Discount %: 20
   - Expiry Date: 25-12-2025
   - Applicable To: All

### Auto-Payment System

#### Auto-Renewal Behavior
- **On**: Automatically deduct subscription amount on renewal date
- **Off**: Manual payment required before subscription expires
- **Bank Rejection**: If payment fails at bank, toggle auto-renewal off
- **Enable**: Clicking toggle enables auto-payment and starts deductions

#### User Addition Billing Logic
- **Prorated Calculation**: 
  - Example: Subscribed Nov 20 (1 month), added user Dec 30
  - User amount: $9.99/30 × 20 remaining days = $6.66
  - Total: Base ($39.99) + User ($6.66) = $46.65

- **Multiple Users**:
  - 2 users: $39.99 + ($9.99 × 2) = $59.97
  - 3 users: $39.99 + ($9.99 × 3) = $69.96

- **User Removal Before Renewal**:
  - Remove 1 user 5 days before renewal: $39.99 + ($9.99 × 1) = $49.98
  - 5 active users, remove 1, inactivate 1: $39.99 + ($9.99 × 2) = $59.97

- **User Reactivation**:
  - Inactive user reactivated 3 days before renewal:
    - If auto-renewal ON: Prorated charge added ($9.99/30 × 3 = $0.99)
    - At renewal: $39.99 + ($9.99 × 2) = $59.97
  - Manual payment: Recalculate and charge accordingly

#### Manual Payment Scenario
- **Subscription Ends**: Nov 20, 2025
- **Manual Payment**: Dec 1, 2025
- **Next Invoice Date**: Jan 1, 2026
- **Scenario**: 2 active users, inactivate 1 on Nov 24, pay on same date
  - Invoice generated for 2 users: $39.99 + $19.98 = $59.97
  - After inactivation: Recalculate to $39.99 + $9.99 = $49.98
  - Payment amount adjusted automatically

### Invoice Management

#### Invoice Creation
- **Auto-Renewal ON**: Invoice auto-paid on renewal date
- **Auto-Renewal OFF**: Invoice created upon subscription expiry
- **Invoice Display**: List with amounts, dates, descriptions

#### Invoice Actions
- **Pay Button**: 
  - Visible for unpaid invoices
  - Hidden after successful payment
- **Download**: Download payment details

#### Invoice Recalculation
- Users added/removed/inactivated/reactivated
- Prorated amounts calculated
- Invoice updated before payment

---

## 3. Admin Panel

### Admin Access
- **Credentials**: Single admin credential for entire system
- **Login**: Admin logs in with company listing
- **Company List**: Shows all companies with filtering by name
- **Subscription Access**: Cannot manage subscription (Owner only)

### Company Management
- **List View Columns**: Company Name, Owner Name, Email, Phone, Action
- **Action Options**: "Enter as Owner"
- **Owner Access**: Full system access upon "Enter as Owner" selection
- **Chat Access**: Admin can view chats (read-only, cannot send messages)
- **Redirect**: After login, redirect to company page

---

## 4. Contact Management

### Add Contact Options
- Add New Contact
- Import Contacts

### Import Contacts
- **Page Name**: Import Contacts
- **File Format**: Excel only (show "Invalid File Format" error for other types)
- **Required Columns**: Name, Email, Phone Number, Address, City, State, Country, Zip Code
- **Template**: Available for download
- **Validation Rules**:
  - All columns required
  - Show "Required" error if any missing
  - No duplicate emails
  - Check if email exists in users or contacts (show "Email already exist" error)
  - No duplicate contact values

- **Process**:
  1. Upload Excel file
  2. Preview step showing all contacts
  3. Display valid/invalid count
  4. Show errors if any
  5. Confirm import
  6. Display: Total contacts, Added contacts, Skipped contacts
  7. Option to edit imported contacts

---

## 5. Inventory Management

### Import Inventory Items
- **Page Name**: Import Inventory Items
- **File Format**: Excel only
- **Required Columns**: Inventory Items, Unit
- **Template**: Available for download
- **Auto-Create Units**: If unit not in system, create it automatically
- **Sync**: Web and Mobile sync support
- **Validation Rules**:
  - All columns required
  - No duplicate values
  - Display valid/invalid count

- **Process**:
  1. Upload Excel file
  2. Preview step with all items
  3. Show valid/invalid count and errors
  4. Confirm import
  5. Display: Total, Added, Skipped
  6. Options: "Back to Inventory Items List" or "Add More Inventory Items"
  7. Edit capability (web and mobile)

### Import Inventory (Event-level)
- **Add Options**:
  1. Add New Inventory Item
  2. Add from Inventory List
  3. Import Inventory

- **File Format**: Excel only
- **Required Columns**: Inventory and Asset, Qty, Unit
- **Template**: Available for download
- **Validation**: Same as Import Inventory Items
- **Template Inheritance**: 
  - Inventory added to template gets included in new events
  - New inventory added to template after event creation doesn't appear in existing events

---

## 6. Schedule Management

### Schedule List Page
- **Required Field**: Description
- **Operations**: Add, Search, Update, Delete (with confirmation)
- **Validation**:
  - Description accepts alphanumeric and special characters
  - Soft delete: Don't remove from events/templates if deleted from list
  - Show pagination

### Schedule Page (All Events View)

#### Event List (Left Panel)
- **List Name**: Events
- **Event Scope**: Current and future events only (recent on top)
- **Content Display**:
  - Show all events for company
  - Remove deleted events from list
- **Search**: By event name
- **Filter**: By date range
- **Filter Logic**: 
  - Date range selection
  - If event overlaps with selected date, show it
  - Example: Event 1 (Oct 10-15) and Event 2 (Oct 13-Nov 2), selecting Oct 14 shows both
  - After filter applied, search operates on filtered results only
- **Default Selection**: Top event auto-selected, right panel shows its data

#### Schedule List (Right Panel)
- **Default State**: 
  - Blank events show "No Data"
  - Template without schedules shows "No Data"
  - Template with schedules shows them by default

- **List View**:
  - Columns: Date & Time, Description, Status, Action
  - Add button opens blank line on top

- **Date & Time**:
  - Calendar picker
  - Format: MM/DD/YYYY HH:MM AM/PM

- **Description**:
  - Alphanumeric and special characters accepted
  - Rich text formatting supported

- **Status**:
  - Default: Not completed
  - Checkbox to mark completed

- **Actions**:
  - Save (with validation)
  - Cancel (without saving)
  - Rearrange (drag-and-drop)
  - Template sequence maintained by default

- **Sync Across Views**:
  - Add from this page → appears in event detail
  - Add from event detail → appears here
  - Edit reflects on both
  - Delete reflects on both

### Import Schedule
- **Page Name**: Import Schedule
- **File Format**: Excel only
- **Required Columns**: Date and Time, Description
- **Template**: Available for download
- **Preview Columns**: Date and Time, Description, Status
- **Validation**:
  - Both columns required
  - No duplicate values
  - Display valid/invalid count

- **Process**:
  1. Upload Excel file
  2. Preview step
  3. Show valid/invalid count
  4. Confirm import
  5. Display: Total, Added, Skipped
  6. Edit capability
  7. Available on: Schedule page, Event schedule tab, Event template schedule tab

- **Template Inheritance**:
  - Schedules added to template appear in new events
  - New schedules added to template after event creation don't appear in existing events

---

## 7. Task Management

### Task List Page
- **Required Field**: Description
- **Operations**: Add, Search, Update, Delete (with confirmation)
- **Validation**:
  - Description accepts alphanumeric and special characters
  - Soft delete: Don't remove from events/templates if deleted from list
  - Show pagination

### Import Task
- **Page Name**: Import Task
- **File Format**: Excel only
- **Required Columns**: Date and Time, Description
- **Template**: Available for download
- **Preview Columns**: Date and Time, Description, Status
- **Validation**:
  - Both columns required
  - No duplicate values
  - Display valid/invalid count

- **Process**:
  1. Upload Excel file
  2. Preview step
  3. Show valid/invalid count
  4. Confirm import
  5. Display: Total, Added, Skipped
  6. Edit capability
  7. Available on: Task page, Event task tab, Event template task tab

- **Template Inheritance**:
  - Tasks added to template appear in new events
  - New tasks added to template after event creation don't appear in existing events

---

## 8. Event Management

### Event List
- **Page Name**: Event List
- **Search**: By event title (top of page)
- **Filter**: By date column
- **Search & Filter Logic**: AND logic (both conditions must match)
- **Add Button**: "Add New Event" at top of page

- **List View Columns**: Event Title, Customer, Owner, Start Date & Time, End Date & Time, Location, Action
- **Clickable Fields**:
  - Event Title: Redirects to event detail page
  - Location: Shows event location

- **Actions**: Delete with confirmation message
- **Delete Message**: "Are You Sure You Want to Delete This Event? Deleting This Event Will Remove All Associated Data and Cannot be Undone."
- **Delete Confirmation**: Shows "Event Deleted" success message

### Event Detail Page

#### Left Panel
- **Display Information**:
  - Title
  - Location (clickable)
  - Start Date & Time
  - End Date & Time
  - Client (with profile icon, name, phone, email)
  - Owner (with profile icon, name, phone, email)
  - Event Description

- **Date Format**: MM/DD/YYYY HH:MM AM/PM
- **Updates**: Real-time reflection of changes

#### Right Panel - Tabs (5 tabs)
1. **Event Detail** (default selected)
   - Multiple contact selection
   - Blank detail for new contact entries
   - Contact detail editor with formatting options
   - Event Specialist dropdown (from users and contacts)
   - Event Details dropdown (from global settings, searchable)
   - Create new detail option if searched value not in list
   - Manual detail entry capability
   - Save details as string (not ID)
   - Option to save event as template

2. **Schedule**
   - Default: "No Data" if blank
   - Columns: Date & Time, Description, Status, Action
   - Add new schedule option
   - Date/Time picker with calendar
   - Format: MM/DD/YYYY HH:MM AM/PM
   - Status: Default pending (not completed)
   - Completed checkbox
   - Reorder capability (drag-and-drop)
   - Template inheritance: Maintain template sequence by default

3. **Checklist** (Inventory Asset List)
   - Columns: Event Inventory and Assets, Qty, Status, Action
   - Default: Pending status (not selected)
   - Edit capability
   - Delete with confirmation
   - Drag-and-drop reorder
   - Success message after add/update
   - Add inventory items options:
     - Add from Inventory Item List Template
     - Add from Checklist Template
     - Add Custom Inventory Asset
   - Inventory popup: Search, checkbox, qty, name, unit
   - Checklist template popup: Select checklist, show assets
   - Custom entry: Blank editable name and qty
   - Status buttons: Loaded, Installed, Picked Up (all clickable, turn green when active)
   - Pickup: Strikethrough text when picked up from site
   - Qty validation: No negative values
   - Sync Across Views:
     - Add from this page → appears in Inventory page
     - Add from Inventory page → appears here
     - Edit reflects on both
     - Delete reflects on both

4. **Files and Media**
   - Default: "No Folder Added"
   - Add folder option
   - Folder name unique per event (can repeat across different events)
   - Edit and delete options per folder
   - Unique validation on folder rename
   - Template inheritance: Copy all folders and documents to new events
   - **File Upload**:
     - Support: Images, Videos, Documents
     - Batch upload: Up to 10 files simultaneously
     - File storage: Original and compressed versions
     - Display: Compressed files with thumbnails
     - Video display: Thumbnail with video icon
     - Document support: PDF, DOC, DOCX only
     - Drag-and-drop: Supported
     - Progress indicator: 0-100% upload progress
     - Preview mode: Original size for images
     - Video preview: Thumbnail with play icon, full playback in popup
     - Document preview: PDF/DOC preview mode

5. **Live Location**
   - **Left Panel**: List of event specialists (with checkboxes)
   - **Right Panel**: Location map
   - **Validation**: Error if no specialist selected - "No Event Specialist Selected. Please Select at Least One Event Specialists to Send a Live Location Request."
   - **Request Feature**: Send location request popup with message option
   - **Multi-select**: Send same message to multiple selected specialists
   - **Message Delivery**: Through messaging system

### Calendar Page (Dashboard)

#### Default Redirect
- **On Login**: User redirected to calendar page (if subscription active)

#### Calendar View
- **Top Display**:
  - Total events for today
  - Total upcoming events

- **Calendar Display**:
  - Event title shown on dates
  - Default view: Current month
  - View options: Week, Day, List
  
- **Week View**: Sunday to Saturday (e.g., Oct 26 - Nov 1, 2025)
- **Day View**: Current day only
- **List View**: All events for current week with date range header (e.g., "26th Oct to 1st Nov 2025")
- **No Events**: Show "No Events to Display" when empty

#### Events Today (Right Panel)
- **Display Information**:
  - Title (clickable, color changes on hover)
  - Start date
  - Address
  - Client name with profile icon
  - Chat icon

- **Event Count**: Show in bracket next to "Events Today"
- **Chat**: Clicking chat icon redirects to chat page with person open
- **Default View**: All contacts/users except logged-in user

### Add/Update Event

#### Initial Selection Popup
- **Message**: "Select From the Options Below to Create a New Event"
- **Options**:
  1. **Blank**: Create new event from scratch
  2. **Template**: Use pre-defined template

- **Descriptions**:
  - Blank: "Create a Completely New Event from a Blank Slate with No Pre-filled Details. Perfect for Unique or Custom Events."
  - Template: "Quickly Set Up an Event Using a Pre-defined Template with Default Settings and Structure, fully customizable to Your Needs."

- **Interaction**: Hover shows hand icon, clickable

#### Blank Event Form
- **Fields**: Title, Client, Start Date & Time, End Date & Time, Owner, Event Location, Event Description/Event Details
- **Mandatory Fields**: All of the above
- **Validation**:
  - Show "Required" error for missing fields
  - Highlight missing fields
  - Title: Alphanumeric only
  - Event Description: Alphanumeric and special characters, with formatting options (header, bold, italic, bullet list, numbered list, links)
  - Event Location: Alphanumeric only

- **Date & Time**:
  - Calendar picker for both start and end
  - Format: MM/DD/YYYY HH:MM AM/PM
  - Validation: End date/time cannot be greater than start date/time

- **Client Dropdown**:
  - Shows all company contacts
  - Search functionality
  - Create new contact option if search result not in list
  - New contact added shows immediately in form

- **Owner Dropdown**:
  - Shows all company users

- **Back Button**: Return to selection popup

#### Template-Based Event Form
- **Process**: Select template, then show all fields auto-filled
- **Auto-fill Fields**: Title, Client, Start Date & Time, End Date & Time, Owner, Event Location, Event Description
- **Editable**: All auto-filled fields can be edited
- **Same Validation**: As blank form

### Mobile Calendar

#### Default View
- **On Login**: User redirected to this page
- **Page Name**: Calendar
- **Default Display**: Current week
- **Month Navigation**: Name of month with left/right arrows

#### Week View
- **Display**: Sunday to Saturday (e.g., Oct 26 - Nov 1, 2025)
- **Current Date**: Selected by default
- **Events**: Show below calendar for selected date
- **No Events**: Show "No Events to Display"
- **Navigation**: Left/right arrows show previous/next week

#### View Options
- Week, Day, List views available

#### Event Details Display
- **Information Shown**:
  - Title (clickable → redirects to event detail page)
  - Start date to End date
  - Event Address (hyperlink → map with address)
  - Client name with profile icon
  - Owner name with profile icon
  - Phone icon (→ calls contact)
  - Email icon (→ opens email to contact)
  - Chat icon (→ redirects to chat page with contact open)

- **Accuracy**: All values display correctly
- **Scrolling**: Works smoothly
- **Design**: Matches Figma specifications

---

## 9. Inventory Asset List Page

### Event List (Left Panel)
- **List Name**: Events
- **Event Scope**: Current and future events only (recent on top)
- **Content Display**:
  - Show all events for company
  - Remove deleted events from list
- **Search**: By event name
- **Filter**: By date range
- **Filter Logic**: 
  - Date range selection
  - If event overlaps with selected date, show it
  - After filter applied, search operates on filtered results only
- **Default Selection**: Top event auto-selected, right panel shows its data

### Inventory Asset List (Right Panel)
- **Default State**: 
  - Blank events show "No Data"
  - Template without inventory shows "No Data"
  - Template with inventory shows them by default

- **List View**:
  - Columns: Event Inventory and Assets, Qty, Status, Action
  - Add button opens blank line on top

- **Status**:
  - Default: Pending (not selected)
  - Clickable buttons: Loaded, Installed, Picked Up (turn green when active)

- **Actions**:
  - Edit capability
  - Delete with confirmation
  - Rearrange (drag-and-drop)

- **Add Inventory Items Options**:
  1. Add from Inventory Item List Template
     - Popup with all items from global settings
     - Search, checkbox, qty column
     - Name and unit display
     - Multiple selection and add capability

  2. Add from Checklist Template
     - Popup with checklist dropdown
     - Asset list shows after checklist selection
     - Multiple selection and add capability

  3. Add Custom Inventory Asset
     - Blank editable name and qty
     - Add on top of list

- **Qty Validation**: No negative values
- **Pickup Status**: Strikethrough text when picked up
- **Sync Across Views**:
  - Add from this page → appears in event detail
  - Add from event detail → appears here
  - Edit reflects on both
  - Delete reflects on both

---

## 10. Files Management

### Files Page
- **Left Panel**: List of all events (with search and date filter)
- **Right Panel**: Folders for selected event
- **Default**: "No Folder Added" when empty

#### Search & Filter
- **Search**: By event name (top of page)
- **Date Filter**: Filter by date range
- **Search Result**: "No Data" if no matches
- **Filter Result**: Search operates on filtered results

#### File Operations
- **Upload Types**: Images, Videos, Documents
- **Batch Upload**: Up to 10 files simultaneously
- **File Storage**: Original and compressed versions saved
- **Display**: Compressed files with thumbnails
- **Document Support**: PDF, DOC, DOCX only
  - Error for unsupported types (.xls, .xlsx, .txt, etc.)
- **Drag-and-Drop**: Supported
- **Upload Progress**: 0-100% indicator

#### File Preview
- **Images**: Original size in preview mode
- **Videos**: 
  - Thumbnail with video icon in list
  - Thumbnail with play icon in preview
  - Full playback in popup with proper design
- **Documents**: Preview mode support

#### Folder Management
- **Add Folder**: Create new folder
- **Folder Rules**:
  - Name unique per event
  - Can repeat across different events
  - Edit capability with unique validation
  - Delete capability
- **Template Inheritance**: All folders and documents copied to new events created from template

---

## 11. Chat & Messaging

### Chat Page Layout

#### Left Section - Message List
- **List Name**: Messages
- **Content**: Users and contacts (except logged-in user)
- **Search**: By name (first name and last name)
- **Add New Group**: Button on top of search
- **Default Order**: 
  - Users/contacts with messages shown on top (most recent first)
  - Users/contacts without messages shown below
  - Can sort alphabetically
- **Tab Structure**:
  - Direct Message (default)
  - Group Chat
  - Archive

#### Direct Message List
- **Display Per Contact**:
  - Full name
  - Profile image
  - Last message preview
  - Time or date
  - Unread message count (if any)

- **Group Chat List**:
  - Group name
  - Group profile image
  - Last message
  - Message count
  - Unread count

#### Right Section - Chat Area

##### Direct Message Chat
- **Default State**: "How Messaging Works" (placeholder with reference text)
- **Message Display**:
  - Logged-in user messages: Right side, pink color
  - Other user messages: Left side
  - Each message shows: Name, time, message content

- **Header Info**:
  - Contact name
  - Profile image
  - "Click here for contact information" link

- **Message Features**:
  - Support: Alphanumeric, special characters, emojis
  - Long messages: 1000+ characters supported
  - File types: Images, videos, documents
  - Live location sharing
  - Message seen indicator: Pink double tick
  - Unread messages: Show date/time header above first unread
  - Pagination: Load more messages on scroll (prevents performance issues)

- **Location Sharing**:
  - Show preview, Stop sharing, Share ETA options
  - Forward capability

- **Message Reactions**: 
  - Tick: Single tick for sent
  - Double tick: Seen indicator (pink colored)

- **Scroll Behavior**:
  - Default scroll to bottom (latest messages)
  - Load older messages on scroll up

- **Message Updates**:
  - If new message arrives while on different page, show count on message menu
  - If on chat page with user A and message arrives from user B, user B moves to top but stays unselected (continue chatting with A)

##### Group Chat
- **List**: Shows group chats, with recent messages on top
- **Unread Count**: Shows per group

- **Unread Message Logic**:
  - Example: 5 users in group, Person 1 (4 messages), Person 2 (6 messages), total 10 unread
  - For all 3 other users: 10 unread
  - For Person 1: 6 unread
  - For Person 2: 4 unread
  - When Person 3 reads: For others (1, 2, 4, 5) still unread; for Person 3 now read

- **Group Information**: 
  - Name of group
  - All member names
  - Accessible via "Contact Information" link

- **Live Location Forwarding**:
  - Forward chain maintains original duration
  - Example: Person 1 shares 2 hrs from 12pm
    - Person 1 → Person 2 (12pm)
    - Person 1 → Person 3 (1pm, only 1 hr remaining)
    - Person 2 → Person 4 (12:30pm)
    - All stop sharing at 2pm (original end time)
  - When Person 1 stops location: All forwarded locations stop
  - Non-Teemlio user: Show name and profile from contact details
  - Duplicate users: Show once on live location page

### Notifications

#### In-App Notifications
- **Trigger**: When user/contact sends message
- **Display**: 
  - Notification badge on message icon (count increases)
  - Unread count in message list
  - Unread count on specific contact/user

#### Push Notifications
- **Title**: User/Contact name
- **Description**: 
  - Text message: Message preview
  - Photo: "Photo" with icon
  - Video: "Video" with icon
  - File: "File" with icon
  - Multiple media: "[count] Photo" (e.g., "3 Photo") with icon
  - Long messages: Show with "…" truncation

- **Action**: Click redirects to message page with selected chat open
- **Count Display**: 
  - Message menu shows total count
  - Individual contact shows count per user

### Chat Management

#### Delete Chat
- **Direct Chat**:
  - Deleted by one user: Removed from their list only
  - Deleted by both users: Removed from database, not visible to either
  - If one user deletes then other messages again: Chat restarts for first user, shows all history
  - Deleting then recreating direct message: Adds to list again
  - Both delete then recreate: Adds to list but no old messages (deleted from db)

- **Group Chat**:
  - Delete for one user: Chat removed from their list, they're removed from group
  - Other members see user removed from group
  - After deleting: User can't message or add members
  - If all members delete: All chat and media deleted from db and Digital Ocean
  - If user deleted then re-added by another member: Group appears again
  - If user deleted: Don't receive notifications for that group

#### Archive Chat
- **Archive Options**:
  - Direct message → Archive
  - Group chat → Archive
  - Tab: "Archive" shows total (direct + group)

- **Archive Rules**:
  - One user archives: Doesn't archive for other user
  - One user unarchives: Doesn't unarchive for other user
  - If archived and message sent: Removed from archive for both users
  - Show appropriate message on archive/restore

- **Archive Message Examples**:
  - "Archived Direct Message Restored"
  - "Archived Direct Message Deleted"
  - "Archived Group Chat Restored"
  - "Archived Group Chat Deleted"

### Live Location Page

#### Event List (Left Panel)
- **List Name**: Events
- **Event Scope**: Current and future events only (recent on top)
- **Content Display**:
  - Show all events for company
  - Remove deleted events from list
- **Search**: By event name
- **Filter**: By date range
- **Default Selection**: Top event auto-selected, right panel shows its data

#### Live Location Tracking (Right Panel)
- **Display Mode**: Event-wise or per user/contact
- **Map Display**:
  - Click event → Show locations of all assigned specialists (if shared)
  - Left panel: All event specialists list (with checkboxes)

- **Specialist Management**:
  - Non-Teemlio user: Show name from contact table
  - Remove specialist from event: No longer shows on live location
  - Add specialist to event: Appears on live location
  - Duplicate specialists: Show once
  - Overlapping locations: Display both on map clearly

- **Location Request**:
  - Select specialist(s) → Click "Request Location"
  - Popup opens with message field
  - Max character: 160
  - Accept: Alphanumeric and special characters
  - Error if missing: "Required"
  - Send to selected specialist(s) only

- **Active Location Sharing**:
  - Show location icon
  - Display on map with icon
  - Show event location on map
  - Calculate time from event location

- **Location Details**:
  - Click on user location: Show name, phone, estimated time to reach location
  - Moving user: Show real-time movement on map
  - Update estimated time as user moves

---

## 12. Notification System

### Notification Center (Web)

#### Notification Menu
- **Icon**: Bell icon in menu
- **Badge**: Shows unread notification count
- **Dropdown**: Shows 4-5 recent unread notifications
- **Read More Button**: Opens full notification page

#### Notification List
- **Actions**:
  - Click notification: Marks as read
  - Mark All as Read button: Marks all as read
- **Unread Count**: Decreases when notifications read

#### Notification Types & Messages

1. **Subscription Renewal**:
   - Message: "Subscription Renew. Your subscription is about to expire. Renew now to avoid interruption."
   - Trigger: Payment completed

2. **Subscription Expiry Warning**:
   - Message: "Subscription About to End. Your subscription is ending soon. Don't miss out—renew before it expires."
   - Trigger: 7 days before subscription end

3. **Payment Successful**:
   - Message: "Payment Successful. Payment of $[amount] was received."
   - Trigger: After successful payment

4. **Subscription Ended**:
   - Message: "Subscription Ended. Your plan has expired. Please renew to continue enjoying our services."
   - Trigger: Upon subscription expiration

#### Notification Timing
- **Display Format**: Relative time
  - "2 minutes ago"
  - "2 days ago"
  - "2 weeks ago"

#### Message Notifications
- **Excluded**: Messages don't show in notification center
- **Separate**: Use unread message count on message icon instead

### Notification Center (Mobile)

#### Notification Panel
- **Bell Icon**: On top of every page
- **Badge**: Shows unread notification count
- **List**: Shows all notifications with pagination
- **Same Notifications**: As web version

#### Notification Actions
- **Click Notification**: Marks as read
- **Mark All as Read**: Button on top of list
- **Unread Count**: Decreases when notifications read

#### Push Notifications
- **Enabled**: For all notification types
- **Chat Notifications**: Redirect directly to chat with person open
- **Action on Click**: Redirect to notification page

#### Sync Behavior
- **Cross-Device**: Read in mobile → reflects as read in web (and vice versa)
- **Unread Count**: Syncs across devices

#### Empty State
- **No Notifications**: "No Notifications Right Now. New Notifications will Appear Here When Available."

---

## 13. General Rules Across All Features

### Pagination
- Implement consistent pagination across all list views
- Show valid page numbers and navigation options

### Formatting Support
- Support for: Headers, Bold, Italic, Bullet lists, Numbered lists, Links
- Apply across all rich text fields (descriptions, event details)

### Error Handling
- Consistent error messaging
- Field highlighting for validation errors
- Confirmation dialogs for destructive actions

### Data Sync
- Web and mobile synchronization for imported data
- Real-time updates when data is modified
- Chat messages, location tracking, event changes sync across devices
- Notification read status syncs across web and mobile

### User Experience
- Hover effects (hand icon for clickable elements)
- Responsive design for web and mobile
- Accessible UI components
- Smooth scrolling and transitions
- Loading indicators for async operations

---

## Architecture Notes

### Data Relationships
- **Users**: Company users with roles (Owner, Admin, Staff)
- **Contacts**: Company contacts (can be external, Teemlio and non-Teemlio)
- **Events**: Belong to company, have templates
- **Templates**: Reusable event configurations
- **Schedules/Tasks**: Can be template-level or event-level
- **Inventory**: Can be global list or event-specific
- **Chat Groups**: Can have multiple members
- **Live Location**: Associated with events and users
- **Subscriptions**: Company-level, with billing history
- **Invoices**: Generated automatically or on-demand

### Storage Considerations
- Original and compressed file versions
- Video thumbnails
- Chat history and media
- Deleted chat cleanup from Digital Ocean
- Archived chat management

### Notification System
- Real-time in-app notifications
- Push notifications with proper formatting
- Badge counts for unread messages and notifications
- Notification deduplication
- Cross-device notification sync

### Real-Time Features
- Live location tracking with ETA calculation
- Live chat updates
- Real-time event synchronization
- Specialist status updates
- Notification delivery

### Billing System
- Subscription lifecycle management
- Auto-renewal with payment processing
- Prorated billing for mid-cycle changes
- Invoice generation and management
- Payment failure handling
- User addition/removal billing logic

### Security & Permissions
- Role-based access control (Owner, Admin, Staff)
- Admin cannot manage subscriptions
- Chat permissions (members only, except admin read-only)
- Live location sharing permissions
- File access restrictions per event
