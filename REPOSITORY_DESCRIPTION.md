# Instagram Clone - Repository Description

## Overview

This repository contains a full-stack Instagram clone application created by SimCoder as part of a YouTube tutorial series. The project demonstrates a complete social media platform with mobile apps, web admin panel, and cloud backend infrastructure.

**Version:** 1.0  
**License:** Apache License 2.0  
**Original Author:** SimCoder (https://github.com/simcoderYoutube)

## Project Structure

The repository is organized into three main components:

```
InstagramClone/
├── frontend/          # React Native mobile application
├── admin/            # ReactJS web admin panel
├── backend/          # Firebase Cloud Functions
├── firestore_rules.txt    # Firestore security rules
├── storage_rules.txt      # Firebase Storage security rules
└── images/           # Project assets and mockups
```

---

## 1. Frontend - Mobile Application

**Location:** `/frontend`  
**Technology Stack:**
- React Native (SDK 42)
- Expo Framework
- Redux for state management
- Firebase SDK (v8.2.3)
- React Navigation v5

### Key Features

#### Authentication
- Email/password login and registration
- Firebase Authentication integration
- Persistent authentication state

#### Main Navigation (Bottom Tabs)
1. **Feed** - View posts from followed users
2. **Search** - Find and discover users
3. **Camera** - Capture photos/videos
4. **Chat** - Messaging system with unread indicators
5. **Profile** - User profile management

#### Core Functionality

**Posts & Media**
- Camera integration for photo/video capture
- Gallery picker for existing media
- Video recording with thumbnails
- Image caching for performance
- Post creation with captions
- Like/unlike posts
- Comment on posts
- Delete own posts

**Social Features**
- Follow/unfollow users
- User search by username
- View user profiles (own and others)
- Profile editing
- Follower/following counts
- User blocking system

**Chat System**
- One-on-one messaging
- Real-time message updates
- Unread message indicators
- Chat list ordered by recent activity
- Push notifications for new messages

**Notifications**
- Expo push notifications
- Notification types:
  - Post interactions (type 0)
  - New messages (type 1)
  - Profile follows (type 2)
- Deep linking from notifications

### Component Structure

```
frontend/components/
├── auth/
│   ├── Login.js          # Login screen
│   └── Register.js       # Registration screen
├── main/
│   ├── add/
│   │   ├── Camera.js     # Camera capture interface
│   │   └── Save.js       # Post creation/editing
│   ├── chat/
│   │   ├── Chat.js       # Individual chat screen
│   │   └── List.js       # Chat list view
│   ├── post/
│   │   ├── Comment.js    # Comment view/creation
│   │   ├── Feed.js       # Main feed display
│   │   └── Post.js       # Individual post view
│   ├── profile/
│   │   ├── Edit.js       # Profile editing
│   │   ├── Profile.js    # Profile display
│   │   └── Search.js     # User search
│   └── random/
│       ├── Blocked.js    # Blocked user screen
│       └── CachedImage.js # Image caching utility
├── Main.js               # Main navigation container
├── styles.js             # Shared styles
└── utils.js              # Utility functions
```

### Redux State Management

**State Structure:**
- `userState`: Current user data, posts, following, chats
- `usersState`: Other users' data, posts, likes

**Key Actions:**
- `fetchUser()` - Load current user data
- `fetchUserPosts()` - Load user's posts
- `fetchUserFollowing()` - Load following list
- `fetchUserChats()` - Load chat conversations
- `fetchUsersData()` - Load other users' data
- `sendNotification()` - Send push notifications
- `deletePost()` - Remove a post
- `reload()` - Refresh all user data

### Dependencies Highlights
- `expo-camera` - Camera functionality
- `expo-image-picker` - Gallery access
- `expo-av` - Audio/video playback
- `expo-notifications` - Push notifications
- `react-native-paper` - UI components
- `redux-thunk` - Async Redux actions
- `firebase` - Backend services

---

## 2. Admin Panel - Web Application

**Location:** `/admin`  
**Technology Stack:**
- React 17
- Material-UI (v4)
- Firebase SDK
- React Router DOM
- Bootstrap 4

### Features

**User Management**
- View all registered users
- User details display
- Ban/unban users
- View user statistics (followers, following)
- Data grid with sorting and filtering

**Post Management**
- View all posts by user
- Post moderation capabilities
- Delete posts
- View post metadata

**Authentication**
- Admin login system
- Firebase authentication
- Protected routes

### Component Structure

```
admin/src/components/
├── Home.js       # Main dashboard with drawer navigation
├── Users.js      # User list with data grid
├── User.js       # Individual user details
├── Post.js       # Post details and moderation
├── login.js      # Admin login
└── Admin.js      # Admin utilities
```

### Admin Interface
- Material-UI drawer navigation
- Responsive data grids
- Real-time Firestore listeners
- Chip-based status indicators (banned/active)
- Route-based navigation

### Configuration
- Firebase config in `/admin/src/config/config.js`
- Environment-specific settings

---

## 3. Backend - Firebase Cloud Functions

**Location:** `/backend/functions`  
**Technology:** Node.js with Firebase Admin SDK

### Cloud Functions

**Like Management**
```javascript
addLike()    // Increment post like count
removeLike() // Decrement post like count
```

**Follow Management**
```javascript
addFollower()    // Update follower/following counts
removeFollower() // Decrement follower/following counts
```

**Comment Management**
```javascript
addComment() // Increment post comment count
```

### Function Triggers
- Firestore document onCreate/onDelete triggers
- Automatic counter updates
- Maintains data consistency across collections

---

## 4. Firebase Configuration

### Firestore Database Structure

```
/users/{userId}
  - name, username, email
  - followersCount, followingCount
  - notificationToken
  - banned (boolean)

/posts/{userId}/userPosts/{postId}
  - caption, downloadURL, type
  - creation (timestamp)
  - likesCount, commentsCount
  
  /likes/{userId}
  /comments/{commentId}

/following/{userId}/userFollowing/{followingId}

/chats/{chatId}
  - users (array)
  - lastMessage, lastMessageTimestamp
  - read status per user
  
  /messages/{messageId}

/admin/{adminId}
  - Admin user records

/feed/{userId}
  - Cached feed data
```

### Security Rules

**Firestore Rules** (`firestore_rules.txt`):
- Public read for users and posts
- Write restricted to resource owners
- Admin override capabilities
- Chat access restricted to participants
- Admin collection locked down

**Storage Rules** (`storage_rules.txt`):
- Public read for all media
- Write restricted to authenticated users
- Path-based permissions:
  - `/profile/{uid}` - User profile pictures
  - `/post/{uid}/{postId}` - User post media

---

## Technology Stack Summary

### Frontend (Mobile)
- **Framework:** React Native with Expo
- **State Management:** Redux with Redux Thunk
- **Navigation:** React Navigation v5
- **UI Components:** React Native Paper, React Native Elements
- **Media:** Expo Camera, Expo Image Picker, Expo AV
- **Notifications:** Expo Notifications
- **Backend:** Firebase (Auth, Firestore, Storage)

### Admin (Web)
- **Framework:** React 17
- **UI Library:** Material-UI v4
- **Routing:** React Router DOM v5
- **Styling:** Bootstrap 4
- **Backend:** Firebase (Auth, Firestore)

### Backend
- **Platform:** Firebase Cloud Functions
- **Runtime:** Node.js
- **Database:** Cloud Firestore
- **Storage:** Firebase Storage
- **Authentication:** Firebase Authentication

---

## Key Features Summary

### User Features
✅ User registration and authentication  
✅ Profile creation and editing  
✅ Photo and video posting  
✅ Like and comment on posts  
✅ Follow/unfollow users  
✅ Real-time feed from followed users  
✅ User search functionality  
✅ Direct messaging (chat)  
✅ Push notifications  
✅ Image caching for performance  
✅ Video playback with controls  

### Admin Features
✅ User management dashboard  
✅ Ban/unban users  
✅ View all users and posts  
✅ Post moderation  
✅ Real-time data updates  

### Technical Features
✅ Real-time database synchronization  
✅ Cloud-based media storage  
✅ Serverless backend functions  
✅ Security rules enforcement  
✅ Push notification system  
✅ Optimized image loading  
✅ Redux state management  
✅ Responsive admin interface  

---

## Development Setup

### Prerequisites
- Node.js and npm
- Expo CLI
- Firebase account
- React Native development environment

### Installation Steps
1. Clone the repository
2. Set up Firebase project
3. Configure Firebase credentials in:
   - `frontend/App.js`
   - `admin/src/config/config.js`
4. Install dependencies:
   ```bash
   cd frontend && npm install
   cd ../admin && npm install
   ```
5. Deploy Firebase functions and rules
6. Run the applications:
   ```bash
   # Frontend
   cd frontend && expo start
   
   # Admin
   cd admin && npm start
   ```

---

## Project Origins

This project was created as part of a YouTube tutorial series by SimCoder:
- **YouTube Channel:** [SimCoder](https://www.youtube.com/channel/UCQ5xY26cw5Noh6poIE-VBog)
- **Tutorial Series:** [Instagram Clone Playlist](https://www.youtube.com/watch?v=xE8UEX7vXVQ&list=PLxabZQCAe5fgatwOQny9wKJVs4YD6xkf1)
- **Branch:** Master branch contains the redesigned version
- **Alternative:** YouTube series original code in `youtube_series` branch

---

## Architecture Highlights

### Data Flow
1. **User Actions** → Redux Actions → Firebase API
2. **Firebase Changes** → Real-time Listeners → Redux State → UI Update
3. **Cloud Functions** → Automatic data aggregation and consistency

### Performance Optimizations
- Image caching with `react-native-expo-cached-image`
- Optimized FlatList rendering
- Real-time listeners with cleanup
- Lazy loading of user data
- Video thumbnail generation

### Security
- Firebase Authentication for user identity
- Firestore security rules for data access
- Storage rules for media access
- Admin-only functions and collections
- User blocking system

---

## Contributing

Contributions are welcome! See the project wiki for contribution guidelines:
- [How to Contribute](https://github.com/SimCoderYoutube/InstagramClone/wiki/How-to-Contribute)
- [Setup Guide](https://github.com/SimCoderYoutube/InstagramClone/wiki/Setup-your-project)

---

## Support

- **Issues:** [GitHub Issues](https://github.com/SimCoderYoutube/InstagramClone/issues)
- **Documentation:** [Project Wiki](https://github.com/SimCoderYoutube/InstagramClone/wiki)
- **YouTube:** [SimCoder Channel](https://www.youtube.com/c/SimpleCoder?sub_confirmation=1)
- **Twitter:** [@simcoder_here](https://twitter.com/simcoder_here)
- **Instagram:** [@simcoder_here](https://www.instagram.com/simcoder_here/)

---

## License

Copyright © 2021 SimCoder

This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.

Some dependencies may be licensed differently - check individual package licenses.
