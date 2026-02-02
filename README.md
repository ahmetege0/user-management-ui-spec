# User Management Screen - UI Specification

## 1. Overview

This document describes the user interface requirements for the User Management screen. The screen allows administrators to view, create, and edit user accounts.

---

## 2. Page Structure

The screen is divided into two main sections:

- **Left Panel (50%)**: User list table with filter and action buttons
- **Right Panel (50%)**: User form for creating/editing users

---

## 3. UI Components

### 3.1 Top Action Bar (Left Panel)

#### New User Button
- **Type**: Button
- **Label**: "+ New User"
- **Style**: Blue background, white text
- **Position**: Top-left
- **Function**: Clears the form and prepares it for creating a new user

#### Hide Disabled User Checkbox
- **Type**: Checkbox
- **Label**: "Hide Disabled User"
- **Default State**: Unchecked
- **Position**: Next to New User button
- **Function**: Filters the user list to show only enabled users when checked

#### Save User Button
- **Type**: Button
- **Label**: "Save User"
- **Style**: Blue background, white text
- **Position**: Top-right
- **Function**: Saves the current form data (create new or update existing user)

### 3.2 User Table (Left Panel)

**Table Columns:**
1. **ID**: Numeric identifier (auto-generated)
2. **User Name**: Text
3. **Email**: Text
4. **Enabled**: Boolean (true/false)

**Table Features:**
- Each column header has sorting capability (ascending/descending)
- Rows alternate between white and light blue backgrounds
- Clicking a row selects it (highlights with blue background) and loads data into the form
- Selected row remains highlighted

**Initial Data Display:**
The table shows at least these users on load:
- ID: 1, User Name: AdminUser, Email: admin@piworks.net, Enabled: true
- ID: 2, User Name: Test User, Email: testuser@piworks.net, Enabled: true

### 3.3 User Form (Right Panel)

#### Form Header
- **Text**: "New User" (when creating) or "Edit User" (when editing)
- **Style**: Large gray text

#### Form Fields

**Username**
- **Label**: "Username :"
- **Type**: Text input
- **Required**: Yes
- **Validation**: Must contain only letters, numbers, underscores, and hyphens (no spaces)

**Display Name**
- **Label**: "Display Name :"
- **Type**: Text input
- **Required**: No

**Phone**
- **Label**: "Phone :"
- **Type**: Text input
- **Required**: No

**Email**
- **Label**: "Email :"
- **Type**: Text input
- **Required**: Yes
- **Validation**: Must be valid email format

**User Roles**
- **Label**: "User Roles :"
- **Type**: Dropdown (single select)
- **Options**: 
  - Guest
  - Admin
  - SuperAdmin
- **Default**: Shows placeholder "Select user roles..."
- **Required**: Yes

**Enabled**
- **Label**: "Enabled :"
- **Type**: Checkbox
- **Default State**: Unchecked for new users

---

## 4. Initial Page State

When the page first loads:

1. **User Table**:
   - Displays all users from the database
   - Sorted by ID in ascending order
   - No row is selected

2. **Filter Checkbox**:
   - "Hide Disabled User" is unchecked

3. **User Form**:
   - Header shows "New User"
   - All input fields are empty
   - User Roles dropdown shows placeholder text
   - Enabled checkbox is unchecked

---

## 5. User Interactions and Behaviors

### 5.1 Creating a New User

**User Action**: Click "+ New User" button

**System Behavior**:
1. Clear all form fields
2. Set form header to "New User"
3. Reset User Roles dropdown to placeholder
4. Uncheck Enabled checkbox
5. Deselect any selected table row

**User Action**: Fill in form fields and click "Save User"

**System Behavior**:
1. Validate required fields (Username, Email, User Roles)
2. If validation fails:
   - Show error message for each invalid field
   - Prevent saving
3. If validation passes:
   - Send data to backend API
   - On success:
     - Add new user to the table with new ID
     - Show success message
     - Clear the form
     - Return to "New User" state
   - On error:
     - Show error message
     - Keep form data

### 5.2 Editing an Existing User

**User Action**: Click on a user row in the table

**System Behavior**:
1. Highlight the selected row (blue background)
2. Change form header to "Edit User"
3. Fill form fields with selected user's data:
   - Username → user's username
   - Display Name → user's display name
   - Phone → user's phone
   - Email → user's email
   - User Roles → user's role (dropdown shows selected value)
   - Enabled → user's enabled status (checkbox checked/unchecked)

**User Action**: Modify form fields and click "Save User"

**System Behavior**:
1. Validate required fields
2. If validation passes:
   - Send updated data to backend API
   - On success:
     - Update the user's row in the table
     - Show success message
     - Clear the form
     - Return to "New User" state
   - On error:
     - Show error message
     - Keep form data

### 5.3 Filtering Users

**User Action**: Check "Hide Disabled User" checkbox

**System Behavior**:
1. Filter table to show only users where Enabled = true
2. Hide all rows where Enabled = false
3. If currently selected user is disabled, clear the form

**User Action**: Uncheck "Hide Disabled User" checkbox

**System Behavior**:
1. Show all users in the table regardless of enabled status

### 5.4 Sorting Table

**User Action**: Click on a column header

**System Behavior**:
1. First click: Sort by that column in ascending order
2. Second click: Sort by that column in descending order
3. Update table rows based on sort order
4. Show sort indicator (arrow) on the active column

---

## 6. Requirements

### 6.1 Functional Requirements

1. The system must display all users in a sortable table
2. The system must allow creating new users with all required fields
3. The system must allow editing existing users
4. The system must validate Username (no spaces, alphanumeric + underscore/hyphen)
5. The system must validate Email (valid email format)
6. The system must require Username, Email, and User Roles fields
7. The system must filter users based on enabled status when checkbox is checked
8. The system must assign unique IDs to new users
9. The system must prevent saving with invalid data
10. The system must show appropriate success/error messages

### 6.2 Validation Rules

**Username**:
- Required field
- Cannot be empty
- Cannot contain spaces
- Can only contain: letters (a-z, A-Z), numbers (0-9), underscores (_), hyphens (-)

**Email**:
- Required field
- Must be valid email format (contains @ and domain)

**User Roles**:
- Required field
- Must select one option from dropdown

**Display Name** and **Phone**:
- Optional fields
- No validation required

### 6.3 Error Messages

Display these error messages when validation fails:

- Username empty: "Username is required"
- Username invalid characters: "Username can only contain letters, numbers, underscores, and hyphens"
- Email empty: "Email is required"
- Email invalid format: "Please enter a valid email address"
- User Role not selected: "Please select a user role"

### 6.4 Success Messages

- After creating new user: "User created successfully"
- After updating user: "User updated successfully"

---

## 7. Visual Design

### Colors
- Primary Button: Blue (#0088CC)
- Button Text: White
- Table Header: Blue background, white text
- Table Row Alternate: Light blue (#E3F2FD)
- Selected Row: Blue highlight
- Form Labels: Dark gray
- Error Text: Red

### Spacing
- Padding between form fields: 15-20px
- Table cell padding: 10px
- Button padding: 8px 16px

### Typography
- Form header: 24px
- Labels: 14px
- Input text: 14px
- Button text: 14px

---

## 8. Component Behavior Details

### Save User Button
- Enabled by default
- Shows loading indicator while saving
- Disabled during save operation to prevent double-click

### User Table Rows
- Hover effect: Slight background color change
- Click anywhere on row to select
- Only one row can be selected at a time

### Form Inputs
- Show red border when validation fails
- Clear error state when user starts typing
- All inputs have consistent height (40px)

### User Roles Dropdown
- Opens on click
- Shows all three options
- Selected option is highlighted
- Closes after selection

---

## 9. API Requirements

The frontend should make these API calls:

- **GET /users** - Fetch all users on page load
- **POST /users** - Create new user
- **PUT /users/{id}** - Update existing user

**User Data Model**:
```json
{
  "id": 1,
  "username": "string",
  "displayName": "string",
  "phone": "string",
  "email": "string",
  "userRole": "Guest|Admin|SuperAdmin",
  "enabled": true
}
```

---

## 10. Development Notes

### What to Show Initially
1. Load and display all users from the database in the table
2. Show empty form with "New User" header
3. Set Hide Disabled User checkbox to unchecked
4. No user row should be selected

### Key Behaviors to Implement
- Form clears when clicking New User button
- Form populates when clicking table row
- Table filters when Hide Disabled checkbox changes
- Validation happens before save
- Form clears after successful save
- Success/error messages appear after save attempt

### Testing Checklist
- [ ] Create new user with valid data
- [ ] Create new user with invalid data (should show errors)
- [ ] Edit existing user
- [ ] Filter disabled users
- [ ] Sort by each column
- [ ] Validate username format
- [ ] Validate email format
- [ ] Check required field validation
- [ ] Verify ID auto-increment for new users
- [ ] Test form clear on New User button
- [ ] Test form populate on row click

---

## Document Version
- Version: 1.0
- Date: February 2, 2026
