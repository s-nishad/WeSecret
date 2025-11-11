# Messenger Clone - Features & Backend Requirements

## ✨ New Features Implemented

### 1. 📱 Fully Responsive Design
- **Desktop**: Side-by-side layout with sidebar and chat area
- **Tablet** (≤768px): Optimized spacing and font sizes
- **Mobile** (≤768px): 
  - Full-screen chat list
  - Chat opens in overlay with back button
  - Touch-friendly buttons and spacing
  - Full-width modal dialogs

### 2. 👤 Profile Settings Modal
Click on your profile in the header to access:
- **Profile Picture Upload** - Click to upload/change
- **Name Edit** - Update your display name
- **Phone Number** - Add/update phone number
- **Email** - Display only (cannot be changed)
- **Password Change**:
  - Requires current password
  - Enter new password
  - Confirm new password
  - Minimum 6 characters validation

### 3. ⏰ Message Timestamps
Every message shows when it was sent:
- "Just now" - Less than 1 minute
- "5m ago" - Minutes ago
- "2h ago" - Hours ago
- "3d ago" - Days ago
- "Jan 15" - Older than 7 days

### 4. 💬 Message Previews in Chat List
- Shows last message in each conversation
- "You: " prefix for messages you sent
- Truncated to 40 characters with "..."
- "Loading..." while fetching
- "No messages yet" for new chats

### 5. 📝 User Registration
- **Sign Up Form** - Tab-based Login/Sign Up interface
- **Profile Picture Upload** - Optional during registration (max 5MB)
- **Form Validation** - Email format, password matching (min 6 chars), required fields
- **Live Preview** - See profile picture before submitting
- **Auto-login redirect** - After successful registration, switches to login tab
- **Complete User Info** - Name, email, phone (optional), password, profile picture (optional)

### 6. 📍 Existing Features
- ✅ Session persistence (no logout on reload)
- ✅ Beautiful gradient UI design
- ✅ Two tabs: Chats & Users
- ✅ Real-time messaging via WebSocket
- ✅ Previous message history loading
- ✅ User list to start new conversations
- ✅ Auto room creation

---

## 🔧 Backend Requirements

### Required Endpoints

#### 1. ✅ Already Implemented (Assumed)
```
POST   /api/v1/auth/login/                    - Login
POST   /api/v1/auth/registration/             - Register new user
GET    /api/v1/auth/user/                     - Get current user
GET    /api/v1/chat/rooms/                    - List user's rooms
POST   /api/v1/chat/rooms/                    - Create/get room
GET    /api/v1/chat/rooms/{id}/messages/      - Get room messages
POST   /api/v1/chat/rooms/{id}/messages/      - Send message (text + file)
WS     ws://localhost:8000/ws/chat/{id}/      - WebSocket chat
```

**Registration Endpoint Details:**
- Endpoint: `POST /api/v1/auth/registration/`
- Content-Type: `multipart/form-data`
- Required fields: `email`, `password1`, `password2`, `name`
- Optional fields: `phone_number`, `profile_picture` (image file)
- Returns: User data with authentication token

#### 2. ⚠️ NEW Endpoints Needed

##### A. User List Endpoint
```
GET /api/v1/auth/users/
```
**Response:**
```json
[
  {
    "id": "user_id_123",
    "email": "user@example.com",
    "name": "User Name",
    "phone_number": "1234567890",
    "profile_picture": "http://example.com/pic.jpg"
  }
]
```

##### B. Profile Update Endpoint
```
PATCH /api/v1/auth/user/update/
Content-Type: multipart/form-data
```

**Request Body (FormData):**
- `name` (optional) - String
- `phone_number` (optional) - String
- `profile_picture` (optional) - File upload
- `current_password` (optional) - String (required if changing password)
- `new_password` (optional) - String

**Response:**
```json
{
  "id": "user_id",
  "email": "user@example.com",
  "name": "Updated Name",
  "phone_number": "1234567890",
  "profile_picture": "http://example.com/new_pic.jpg"
}
```

**Error Responses:**
- 400 - Invalid data or passwords don't match
- 401 - Current password incorrect
- 403 - Unauthorized

---

## 📊 Expected Data Formats

### Message Object
```json
{
  "id": 1,
  "content": "Message text",
  "message": "Message text",  // Alternative field name
  "sender_id": "user_id",
  "sender": "user_id",  // Alternative field name
  "timestamp": "2025-01-15T10:30:00Z",
  "created_at": "2025-01-15T10:30:00Z"  // Alternative field name
}
```

### Room Object
```json
{
  "id": 1,
  "other_user_name": "John Doe",
  "user1_name": "Current User",
  "user2_name": "Other User",
  "user1_id": "user_id_1",
  "user2_id": "user_id_2"
}
```

### WebSocket Message Format
**Sent:**
```json
{
  "message": "Hello",
  "sender_id": "user_id"
}
```

**Received:**
```json
{
  "message": "Hello",
  "sender_id": "user_id",
  "timestamp": "2025-01-15T10:30:00Z"
}
```

---

## 🎨 Mobile Features

### Navigation
- Chat list is the main screen
- Clicking a chat opens it in full screen
- Back button (←) returns to chat list
- Smooth transitions

### Responsive Breakpoints
- **Desktop**: > 768px - Side-by-side layout
- **Mobile**: ≤ 768px - Stacked layout with navigation

---

## 🚀 How to Use

1. **Sign Up** (New Users) - Click "Sign Up" tab, fill form with name, email, password, optional profile picture
2. **Login** - Enter email and password in "Login" tab
3. **View Chats** - See all your conversations with last message preview
4. **Start New Chat** - Go to Users tab, click any user
5. **Send Messages** - Type and press Enter or click Send
6. **Send Files** - Click 📎 button, select file, add optional caption
7. **Edit Profile** - Click your profile picture in header
8. **Mobile** - Use back button to return to chat list

---

## 📝 Notes

- Session persists in localStorage
- Profile pictures show initials if not uploaded
- Timestamps update relative to current time
- Mobile-optimized touch targets
- All modals close when clicking outside
- Password must be 6+ characters

---

**Frontend File**: `chat.html` (Complete & Ready)
**Backend**: Add 2 new endpoints listed above

