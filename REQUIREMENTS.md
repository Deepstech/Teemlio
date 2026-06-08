# Teemlio - Event Management Platform Requirements

## Overview
Teemlio is an event management platform with web and mobile applications enabling users to create, manage, and track events with schedules, checklists, inventory, real-time location tracking, messaging, subscription management, and team collaboration capabilities.

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

- **User Reactivation**:
  - Inactive user reactivated 3 days before renewal:
    - If auto-renewal ON: Prorated charge added
    - At renewal: $39.99 + ($9.99 × 2) = $59.97

### Invoice Management

#### Invoice Creation
- **Auto-Renewal ON**: Invoice auto-paid on renewal date
- **Auto-Renewal OFF**: Invoice created upon subscription expiry

#### Invoice Actions
- **Pay Button**: Visible for unpaid invoices, hidden after payment
- **Download**: Download payment details

---

## 3. Admin Panel

### Admin Access
- **Credentials**: Single admin credential for entire system
- **Login**: Admin logs in with company listing
- **Company List**: Shows all companies with filtering by name
- **Subscription Access**: Cannot manage subscription (Owner only)
- **Company Management**:
  - **List View Columns**: Company Name, Owner Name, Email, Phone, Action
  - **Action Options**: "Enter as Owner"
  - **Owner Access**: Full system access upon "Enter as Owner" selection
  - **Chat Access**: Admin can view chats (read-only, cannot send messages)

---

## 4. Notification Settings (March 2026)

### Notification Settings Page

#### Structure
- **3 Main Sections**: Email, Web Notifications, Mobile Notifications
- **Default State**: All notifications ON
- **Toggle Controls**: Enable/Disable per notification type
- **Bulk Controls**: "Enable All" and "Disable All" buttons per section

#### Access Control
- **Owner**: Can see and configure all notification settings
- **Admin**: Cannot see Email section
- **Independent Settings**: Owner disabled ≠ Admin disabled (per user settings)

### Email Notifications (Owner Only)
1. **Payment Successful for Newly Added User** - Confirmation when payment for new user succeeds
2. **Membership Renewed Successfully** - Confirmation when membership renews
3. **Payment Failed** - Notification when payment attempt fails
4. **Manual Payment Successful** - Confirmation when manual payment succeeds
5. **Membership Expiration Reminder** - Reminder before membership expires

### Web Notifications
1. **Payment Successful** (Owner only) - Notification when payment completes
2. **Membership Renewed Successfully** (Admin & Owner) - Confirmation when membership renews
3. **Membership Expiring** (Admin & Owner) - Reminder before membership expires
4. **Membership Expired** (Admin & Owner) - Alert when membership expires

### Mobile Notifications
1. **New Message** (Admin & Owner) - Notification when new message received

---

## 5. Mobile App Updates

### Update Popup System

#### Force Update
- **Trigger**: App version < minimum supported version
- **Behavior**: 
  - Blocks all pages (non-dismissible)
  - Cannot be skipped
  - Must update to continue
  - Opens App Store/Play Store on tap
  - Reappears after app relaunch

#### Optional Update
- **Trigger**: Minimum supported version ≤ app version < latest version
- **Behavior**:
  - Can be skipped once per user
  - User continues app after skip
  - Other users still see update
  - Opens App Store/Play Store on tap

#### No Update Required
- **Trigger**: App version = latest version
- **Behavior**: No popup shown

#### Dynamic Update Detection
- **Server Version Change**: If server version changes while app open:
  - Shows appropriate popup immediately
  - Force update persists even if optional available
  - Correct popup shown after app relaunch

#### Internet Requirement
- **Update Button**: Only functional with internet connection

---

## 6. Team Members Management

### Team Members List

#### Page Overview
- **Page Name**: Team Members
- **Content**: All team members for company
- **Search**: By name

#### List Display
- **Columns**: Picture, Name, Teemlio Handle, Email, Phone, Address, Action
- **Phone**: Proper format display
- **Address**: Comma-separated (no comma if missing)
- **Actions**: Edit, Delete

### Add Team Member

#### Add Popup
- **Title**: "Add New Team Member"
- **Fields**: Profile Picture, Name, Email, Phone Number, Teemlio Handle, Address
- **Helper Text**: "The Unique Username is Used to Identify Team Member on Teemlio Making it Easier for App-to-App Messaging and Live Location Sharing."
- **Required Fields**: Name, Email, Phone No, Teemlio Handle

#### Validation
- **Error Display**: "Required" error with field highlight
- **Email Format**: Must be valid
- **Teemlio Handle**: Unique per company
- **Combination**: User ID + Teemlio Handle must be unique

#### Billing on User Addition
- **Prorated Calculation**: Days remaining × daily rate
  - Example: Oct 1 subscription, added Oct 10, $10/user/month
  - Amount: $10/30 × 20 days = $6.67
- **No Refund**: Deleted users don't get refunded
- **Success Message**: "User added successfully"

#### Payment Processing
- **Automatic Deduction**: Based on remaining subscription days
- **No Payment, No Add**: User not added if payment fails
- **Payment Success**: User added if payment succeeds

### Edit Team Member

#### Edit Popup
- **Fields**: Name, Email, Phone No, Teemlio Handle, Address
- **Pre-filled**: All current values shown
- **Profile Photo**: Compressed image display
- **Success Message**: "User updated successfully"

#### Payment on Edit
- **Payment Option**: Available after save
- **Payment Failure**: User not updated if payment fails

### Delete Team Member

#### Delete Process
- **Confirmation Dialog**: Required before deletion
- **Soft Delete**: Marked as deleted in database
- **Login Prevention**: Cannot login with deleted user
- **Email Reuse**: Can add new user with deleted user's email

---

## 7. Contact Management

### Contacts List

#### Page Overview
- **Page Name**: Contacts
- **Left Panel**: All added contacts
- **Search**: By name, email, phone
- **Add Button**: "Add New Contact"

#### Contact List Display
- **Columns**: Profile image, Name, Phone no, Contact tags
- **Contact Tags**: Colored (from global settings), can be multiple

### Contact Details

#### Right Panel
- **Top Information**: Profile image, Name, Contact tags, Teemlio Handle, Phone no, Email id, Address

#### Tabs (2 tabs)
1. **Events Tab** (Default - selected in pink)
   - All events for this contact (latest on top)
   - Display: Event name, Address, Date (from-to), Description

2. **Notes Tab**
   - All notes for this contact
   - Default: "No Data" if no notes
   - **Columns**: Note, Created by, Timestamp, Action
   - **Sorting**: Works on all columns except Action
   - **Add Notes**: "Add New Notes" button (adds blank line on top)
   - **Actions**: Edit, Delete
   - **Messages**: 
     - Add: "Notes Saved"
     - Edit: "Updated Notes"
     - Delete: "Notes Deleted"

#### Contact Deletion
- **Impact**: Everything linked with contact deleted except messages
- **Teemlio Handle**: Cannot have same handle in both user and contact in same company

### Add Contact

#### Add Popup
- **Fields**: Profile Picture, Name, Email, Phone no, Teemlio Handle, Address
- **Required Fields**: Name, Email, Phone no, Tags
- **Phone Validation**: Numeric, +, (), -, space accepted
- **Success Message**: "Contact added successfully"

#### Email & Handle Validation
- **Duplicate Email**: Cannot create in same company
- **Multi-Company**: Can create with same email in different companies
- **User Email Conflict**: Cannot use same email in user and contact in same company
- **Handle Uniqueness**: Cannot use same Teemlio handle twice per company (including users)

---

## 8. Event Templates

### Event Template List

#### Page Overview
- **Page Name**: Templates
- **Search**: On top left (by title and description)
- **Add Button**: On top right
- **Source**: Templates from both template page and events saved as templates

#### Template List Display
- **Columns**: Event Title, Event description, Date modified, Action
- **Date Format**: DD/MM/YYYY
- **Clickable**: Event title (redirects to detail page)
- **Sorting**: Working
- **Pagination**: Working
- **Delete**: Shows confirmation message

### Add Template

#### Add Popup
- **Title**: "Add Event Template"
- **Fields**: Title, Event Description / Event Details
- **Required**: Both fields
- **Editor**: All formatting options working
- **After Add**: Redirects to template detail page

### Template Detail Page

#### Left Panel
- **Display**: Title, Description
- **Edit Icon**: Opens edit popup with prefilled values
- **Success Message**: "Updated successfully"

#### Right Panel - Tabs (4 tabs)
1. **Event Details Tab** (Default)
   - **Default**: "No event detail added" if none
   - **Add Button**: "Add event detail"
   - **Fields**: Event specialist (users/contacts), Event Details (searchable dropdown)
   - **Manual Entry**: Saved as string (not ID)

2. **Schedule Tab**
   - **Columns**: Date & Time, Description, Action
   - **Date Format**: MM/DD/YYYY
   - **Actions**: Edit, Delete

3. **Checklist Tab**
   - **Add Options**: 
     1. Add from Inventory Item List Template
     2. Add from Checklist Template
     3. Add custom inventory asset
   - **Columns**: Event inventory and Assets, Qty, Action

4. **File and Media Tab**
   - **Default**: "No folder added"
   - **Add Folder**: Create new folder
   - **Folder Name**: Unique per template
   - **Actions**: Edit, Delete

---

## 9. Schedule List Management

### Schedule List Page
- **Required Field**: Description
- **Operations**: Add, Search, Update, Delete (with confirmation)
- **Validation**: Alphanumeric and special characters accepted
- **Soft Delete**: Don't remove from events/templates if deleted from list
- **Pagination**: Working

---

## 10. Task List Management

### Task List Page
- **Required Field**: Description
- **Operations**: Add, Search, Update, Delete (with confirmation)
- **Validation**: Alphanumeric and special characters accepted
- **Soft Delete**: Don't remove from events/templates if deleted from list
- **Pagination**: Working

---

## 11. Contact & User Constraints

### Email & Handle Uniqueness
- **Same Teemlio Handle**: Cannot exist in both user and contact in same company
- **One Record Per Handle**: Per company only
- **Handle Updates**: Must propagate to all linked records

### Contact Addition
- **Same Email Different Company**: Allowed
- **Same Email Same Company**: Not allowed
- **Email vs User Conflict**: Cannot use same email in user and contact in same company
- **Teemlio Handle**: Unique across users and contacts per company

---

## 12. Event Management (Summary)

### Event List
- **Search**: By event title
- **Filter**: By date (AND logic with search)
- **Columns**: Event Title, Customer, Owner, Start Date & Time, End Date & Time, Location, Action
- **Delete**: With confirmation - "Event Deleted"

### Event Detail Page - 5 Tabs
1. **Event Detail** - Contact details, specialist, event details
2. **Schedule** - Date & Time, Description, Status
3. **Checklist** - Inventory items with status tracking
4. **Files and Media** - Folder management and file uploads
5. **Live Location** - Specialist location tracking

### Calendar Dashboard
- **Default**: Current month view
- **Views**: Month, Week, Day, List
- **Display**: Events for today, upcoming events
- **Mobile**: Week view default

### Add/Update Event
- **Options**: Blank or Template-based
- **Required**: Title, Client, Start Date & Time, End Date & Time, Owner, Location, Description
- **Date Validation**: End time cannot be greater than start time

---

## 13. General Architecture

### Data Relationships
- **Users**: Company users with roles
- **Contacts**: External/internal contacts
- **Events**: Company events with templates
- **Subscriptions**: Company-level with billing
- **Team Members**: Company staff with billing impact
- **Chat**: Messages and groups

### Security & Permissions
- **Role-Based**: Owner, Admin, Staff
- **Admin Limitations**: Cannot manage subscriptions, cannot send chat messages
- **Notification Settings**: Independent per user
- **Soft Deletes**: User and contact deletions
- **Data Preservation**: Messages preserved on contact delete

### Real-Time Features
- **Live Location**: Map tracking with ETA
- **Chat Sync**: Real-time message delivery
- **Notifications**: Cross-device sync
- **App Updates**: Dynamic version detection

