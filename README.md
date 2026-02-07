# KKU Emergency

A dual Android application system for emergency incident reporting and management at Khon Kaen University (KKU). The platform connects students, staff, and the general public with emergency responders through real-time incident reporting, live chat communication, and a coordinated dispatch workflow — all backed by Firebase services for authentication, real-time database, and push notifications.

---

## Table of Contents

- [Features](#features)
- [Screenshots](#screenshots)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Database Schema](#database-schema)
- [Firestore Operations](#firestore-operations)
- [Role-Based Access](#role-based-access)

---

## Features

### Client-Facing (KKUEmergencyClient)

- **Incident Reporting** — Submit emergency reports with incident type, location, relation to victim, and additional details.
- **Real-Time Status Tracking** — Monitor incident status updates live (Pending, Accepted, In Progress, Completed).
- **In-App Chat** — Communicate directly with the assigned emergency staff member.
- **Survival Guides** — Browse emergency survival guides filtered by incident type with images and detailed instructions.
- **Account Management** — Register with role selection (Student / Staff / General), update profile, and reset password.
- **Phone Call to Staff** — Call the assigned staff member directly from the incident detail screen.

### Staff / Responder (KKUEmergencyStaff)

- **Incident Dashboard** — View all unassigned and active incidents with search and type-based filtering.
- **Incident Assignment** — Accept and self-assign unassigned incidents.
- **Status Management** — Update incident status through the full lifecycle (Pending → Accepted → In Progress → Completed).
- **Chat with Reporter** — Real-time messaging with the person who reported the incident.
- **Push Notifications** — Receive FCM push alerts for new incidents, new messages, and status changes.
- **Profile & Availability** — Toggle availability status (Available / Working), edit profile, and manage password.

### System / Infrastructure

- **Firebase Authentication** — Secure email/password authentication for both apps with role validation.
- **Cloud Firestore** — Real-time NoSQL database with snapshot listeners for live data updates.
- **Firebase Cloud Messaging** — Push notification delivery to staff devices for new incidents and messages.
- **Koin Dependency Injection** — Clean DI architecture in the Staff app for repositories and view models.

---

## Screenshots

> **Note:** This project is a native Android application. Screenshots are available in the project documentation at [`doc/KKU Emergency.pdf`](doc/KKU%20Emergency.pdf).

| Client App | Staff App |
|:---:|:---:|
| Login & Registration | Staff Login |
| Home Dashboard | Incidents Dashboard |
| Report Incident Form | Incident Detail & Status Update |
| Incident Status Tracking | Chat with Reporter |
| Chat with Staff | Profile & Availability |
| Survival Guides | Push Notifications |

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Language** | Kotlin |
| **Client App SDK** | Android API 24–34 (compileSdk 34) |
| **Staff App SDK** | Android API 27–35 (compileSdk 35) |
| **Architecture** | MVVM (Model-View-ViewModel) |
| **UI Toolkit** | Android XML Layouts + Jetpack Compose (Staff app) |
| **View Binding** | ViewBinding & DataBinding |
| **Authentication** | Firebase Authentication (email/password) |
| **Database** | Cloud Firestore (real-time NoSQL) |
| **Push Notifications** | Firebase Cloud Messaging (FCM) |
| **Dependency Injection** | Koin 3.4.0 (Staff app) |
| **Image Loading** | Glide 4.16.0 (Client app) |
| **Image Slider** | ImageSlideshow 0.1.0 (Client app) |
| **Maps** | Google Play Services Maps 18.1.0 (Staff app) |
| **Material Design** | Material Components for Android |
| **Build System** | Gradle (Groovy for Client, Kotlin DSL for Staff) |
| **Java Compatibility** | Java 11 |

---

## Project Structure

```
KKUEmergency/
├── doc/
│   └── KKU Emergency.pdf              # Project documentation with screenshots
│
└── sourceCode/
    ├── KKUEmergencyClient/             # Client app (for public users)
    │   └── app/src/main/
    │       ├── java/com/example/sos/
    │       │   ├── models/             # Data classes (User, Incident, Message, etc.)
    │       │   ├── repository/         # Firestore data access layer
    │       │   ├── viewmodel/          # MVVM ViewModels
    │       │   ├── adapter/            # RecyclerView adapters
    │       │   ├── home/               # Home screen activity
    │       │   ├── login/              # Login fragment
    │       │   ├── register/           # Two-step registration fragments
    │       │   ├── report/             # Incident reporting activity
    │       │   ├── account/            # User profile management
    │       │   ├── message/            # Chat room list
    │       │   ├── chat/               # Chat messaging screen
    │       │   ├── pending/            # Active/completed incidents list
    │       │   ├── guides/             # Survival guides browser
    │       │   └── MainActivity.kt     # Entry point with login/register tabs
    │       └── res/                    # Layouts, drawables, strings, colors
    │
    └── KKUEmergencyStaff/              # Staff app (for emergency responders)
        └── app/src/main/
            ├── java/com/example/sosstaff/
            │   ├── models/             # Data classes (StaffUser, Incident, ChatRoom, etc.)
            │   ├── auth/               # Login activity, ViewModel, and AuthRepository
            │   ├── main/
            │   │   ├── MainContainer.kt      # Bottom navigation host
            │   │   ├── incidents/            # Incident list, detail, and status management
            │   │   ├── chat/                 # Chat rooms list and messaging
            │   │   └── profile/              # Staff profile and availability
            │   ├── common/
            │   │   ├── di/                   # Koin dependency injection modules
            │   │   └── utils/                # Notification helpers and utilities
            │   ├── services/                 # FCM messaging service
            │   ├── SosStaffApplication.kt    # Application class (Koin init)
            │   └── MainActivity.kt           # Splash + auth check entry point
            └── res/                          # Layouts, drawables, strings, colors
```

---

## Getting Started

### Prerequisites

- **Android Studio** Hedgehog (2023.1.1) or newer
- **JDK 11** or higher
- **Android SDK** with API level 35 installed
- A **Firebase project** with Authentication, Cloud Firestore, and Cloud Messaging enabled
- A physical device or emulator running **Android 7.0+ (API 24)** for the Client app, or **Android 8.1+ (API 27)** for the Staff app

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/<your-username>/KKUEmergency.git
   cd KKUEmergency
   ```

2. **Set up Firebase**

   - Create a Firebase project at [console.firebase.google.com](https://console.firebase.google.com)
   - Enable **Email/Password** authentication
   - Create a **Cloud Firestore** database
   - Enable **Cloud Messaging**
   - Download `google-services.json` for each app:
     - Register Android app with package `com.example.sos` → place in `sourceCode/KKUEmergencyClient/app/`
     - Register Android app with package `com.example.sosstaff` → place in `sourceCode/KKUEmergencyStaff/app/`

3. **Seed the Firestore database**

   Create the following collections in Firestore:
   - `users` — populated automatically on client registration
   - `staff` — manually add staff documents with fields: `name`, `email`, `phone`, `position`, `status`, `fcmToken`, `lastActiveAt`, `createdAt`
   - `incidents` — populated automatically on incident reports
   - `chats` — populated automatically with incidents
   - `messages` — populated automatically in chat
   - `survivalGuides` — manually add guide documents with fields: `title`, `content`, `incidentType`, `imageUrl`, `createdAt`, `updatedAt`

4. **Open in Android Studio**

   - Open `sourceCode/KKUEmergencyClient` as a project for the Client app
   - Open `sourceCode/KKUEmergencyStaff` as a project for the Staff app

5. **Build and run**

   ```bash
   # Client app
   cd sourceCode/KKUEmergencyClient
   ./gradlew assembleDebug

   # Staff app
   cd sourceCode/KKUEmergencyStaff
   ./gradlew assembleDebug
   ```

### Available Gradle Tasks

| Task | Description |
|---|---|
| `./gradlew assembleDebug` | Build debug APK |
| `./gradlew assembleRelease` | Build release APK |
| `./gradlew build` | Full build with checks |
| `./gradlew test` | Run unit tests |
| `./gradlew connectedAndroidTest` | Run instrumented tests on device/emulator |
| `./gradlew lint` | Run Android lint checks |
| `./gradlew clean` | Clean build outputs |

---

## Database Schema

This project uses **Cloud Firestore** (NoSQL document database). Each top-level collection maps to a data model in the application.

### Entity-Relationship Diagram

```
┌──────────────┐         ┌──────────────────┐         ┌──────────────┐
│    users     │         │    incidents     │         │    staff     │
├──────────────┤         ├──────────────────┤         ├──────────────┤
│ id (UID)     │───1:N──▶│ reporterId       │◀──N:1───│ id (UID)     │
│ email        │         │ assignedStaffId  │─────────│ name         │
│ firstName    │         │ incidentType     │         │ email        │
│ lastName     │         │ location         │         │ phone        │
│ phoneNumber  │         │ status           │         │ position     │
│ role         │         │ reportedAt       │         │ status       │
│ createdAt    │         │ completedAt      │         │ fcmToken     │
└──────────────┘         └────────┬─────────┘         └──────────────┘
                                  │
                              1:1 │
                                  ▼
                         ┌──────────────────┐
                         │     chats        │
                         ├──────────────────┤
                         │ id (=incidentId) │
                         │ incidentId       │
                         │ userId           │
                         │ staffId          │
                         │ lastMessage      │
                         │ active           │
                         └────────┬─────────┘
                                  │
                              1:N │
                                  ▼
                         ┌──────────────────┐
                         │    messages      │
                         ├──────────────────┤
                         │ id               │
                         │ chatId           │
                         │ senderId         │
                         │ senderType       │
                         │ message          │
                         │ timestamp        │
                         │ isRead           │
                         └──────────────────┘

┌────────────────────┐
│  survivalGuides    │
├────────────────────┤
│ id                 │
│ title              │
│ content            │
│ incidentType       │
│ imageUrl           │
│ createdAt          │
│ updatedAt          │
└────────────────────┘
```

### Key Models

| Model | Description |
|---|---|
| **User** | Client app users (students, staff, general public) who can report incidents |
| **StaffUser** | Emergency responders who handle and manage incidents |
| **Incident** | Emergency incident reports with type, location, status, and assignment |
| **ChatRoom** | One-to-one chat rooms linking a reporter to assigned staff, tied to an incident |
| **Message** | Individual chat messages within a chat room |
| **SurvivalGuide** | Emergency survival guide articles with instructions and images |

### Collection: `users`

| Field | Type | Constraints |
|---|---|---|
| `id` | `String` | Firebase UID (document ID) |
| `email` | `String` | Required, used for authentication |
| `firstName` | `String` | Required |
| `lastName` | `String` | Required |
| `phoneNumber` | `String` | Required |
| `role` | `String` | One of: `นักศึกษา` (Student), `บุคลากร` (Staff), `บุคคลทั่วไป` (General) |
| `createdAt` | `Long` | Epoch milliseconds, auto-set on creation |

### Collection: `staff`

| Field | Type | Constraints |
|---|---|---|
| `id` | `String` | Firebase UID (document ID, `@DocumentId`) |
| `name` | `String` | Full name |
| `email` | `String` | Required, used for authentication |
| `phone` | `String` | Contact phone number |
| `position` | `String` | Job title (e.g., Safety Officer, Nurse) |
| `status` | `String` | Default: `ว่าง` (Available); also `กำลังทำงาน` (Working) |
| `fcmToken` | `String` | Firebase Cloud Messaging token for push notifications |
| `lastActiveAt` | `Date` | Last active timestamp |
| `createdAt` | `Date` | Account creation timestamp |

### Collection: `incidents`

| Field | Type | Constraints |
|---|---|---|
| `id` | `String` | Auto-generated document ID (`@DocumentId`) |
| `reporterId` | `String` | UID of the reporting user |
| `reporterName` | `String` | Cached reporter full name |
| `reporterPhone` | `String` | Cached reporter phone number |
| `incidentType` | `String` | One of: `อุบัติเหตุบนถนน` (Traffic Accident), `จับสัตว์` (Animal Capture), `ทะเลาะวิวาท` (Dispute), `อื่นๆ` (Other) |
| `location` | `String` | Location description |
| `relationToVictim` | `String` | One of: `ผู้ประสบเหตุ` (Victim), `ผู้เห็นเหตุการณ์` (Witness), `เพื่อนผู้ประสบเหตุ` (Friend of Victim) |
| `additionalInfo` | `String` | Free-text additional details |
| `status` | `String` | One of: `รอรับเรื่อง` (Pending), `เจ้าหน้าที่รับเรื่องแล้ว` (Accepted), `กำลังดำเนินการ` (In Progress), `เสร็จสิ้น` (Completed) |
| `assignedStaffId` | `String` | UID of assigned staff; empty string if unassigned |
| `assignedStaffName` | `String` | Cached assigned staff name |
| `reportedAt` | `Long` | Epoch milliseconds, auto-set on creation |
| `lastUpdatedAt` | `Long` | Epoch milliseconds, updated on status change |
| `completedAt` | `Long?` | Epoch milliseconds; `null` until completed |

### Collection: `chats`

| Field | Type | Constraints |
|---|---|---|
| `id` | `String` | Document ID (same as `incidentId`, `@DocumentId`) |
| `incidentId` | `String` | Reference to the parent incident |
| `incidentType` | `String` | Cached incident type |
| `userId` | `String` | Reporter's UID |
| `userName` | `String` | Cached reporter name |
| `staffId` | `String` | Assigned staff UID; empty if unassigned |
| `staffName` | `String` | Cached staff name |
| `lastMessage` | `String` | Text of the most recent message |
| `lastMessageTime` | `Long` | Epoch milliseconds of last message |
| `staffUnreadCount` | `Int` | Unread message count for staff |
| `userUnreadCount` | `Int` | Unread message count for user |
| `active` | `Boolean` | `true` while incident is open; set to `false` on completion |

### Collection: `messages`

| Field | Type | Constraints |
|---|---|---|
| `id` | `String` | Auto-generated document ID (`@DocumentId`) |
| `chatId` | `String` | Reference to parent chat room |
| `senderId` | `String` | UID of the sender |
| `senderName` | `String` | Cached sender name |
| `senderType` | `String` | `"user"` or `"staff"` |
| `message` | `String` | Message text content |
| `timestamp` | `Date` / `Long` | Message timestamp (Staff app uses `Date`, Client app uses `Long`) |
| `isRead` | `Boolean` | `false` by default; set to `true` when recipient reads |

### Collection: `survivalGuides`

| Field | Type | Constraints |
|---|---|---|
| `id` | `String` | Document ID |
| `title` | `String` | Guide title |
| `content` | `String` | Full guide content / instructions |
| `incidentType` | `String` | Related incident type for filtering |
| `imageUrl` | `String` | URL to guide cover image |
| `createdAt` | `Long` | Epoch milliseconds |
| `updatedAt` | `Long` | Epoch milliseconds |

---

## Firestore Operations

Since this project uses Firebase Cloud Firestore instead of a REST API, the data access layer is organized as repository classes that perform Firestore queries and return `LiveData` streams.

### Authentication

| Operation | Repository | Description |
|---|---|---|
| `registerUser(email, password, user)` | `UserRepository` | Creates Firebase Auth account and `users` document |
| `loginUser(email, password)` | `UserRepository` | Signs in via Firebase Auth |
| `login(email, password)` | `AuthRepository` | Signs in staff + validates `staff` collection + updates FCM token |
| `logout()` | `UserRepository` / `AuthRepository` | Signs out; staff app also clears FCM token |
| `resetPassword(email)` | `UserRepository` / `AuthRepository` | Sends Firebase password reset email |
| `getCurrentUser()` | `UserRepository` | Fetches current user document from `users` |
| `getCurrentStaff()` | `AuthRepository` | Fetches current staff document from `staff` |

### Incidents

| Operation | Repository | Description |
|---|---|---|
| `reportIncident(type, location, relation, info)` | `ReportRepository` | Creates `incidents` document + corresponding `chats` document |
| `getActiveIncidentsForCurrentUser()` | `IncidentRepository` | LiveData: user's incidents where status != Completed |
| `getCompletedIncidentsForCurrentUser()` | `IncidentRepository` | LiveData: user's incidents where status == Completed |
| `getUnassignedIncidents()` | `IncidentsRepository` | LiveData: incidents with empty `assignedStaffId` |
| `getActiveIncidents()` | `IncidentsRepository` | LiveData: all non-completed incidents |
| `getCompletedIncidents()` | `IncidentsRepository` | LiveData: all completed incidents |
| `updateIncidentStatus(id, status)` | `IncidentsRepository` | Updates status; auto-assigns staff if accepting; closes chat if completing |
| `searchIncidents(query)` | `IncidentsRepository` | Client-side filtering across all incidents |

### Chat & Messaging

| Operation | Repository | Description |
|---|---|---|
| `getChatRoomsForCurrentUser()` | `ChatRepository` | LiveData: chat rooms ordered by `lastMessageTime` DESC |
| `getAssignedChatRooms()` | `ChatRepository` | LiveData: chats assigned to current staff |
| `getUnassignedChatRooms()` | `ChatRepository` | LiveData: chats with no assigned staff |
| `getMessagesForChatRoom(chatId)` | `ChatRepository` | LiveData: messages ordered by `timestamp` ASC |
| `sendMessage(chatId, text)` | `ChatRepository` | Creates message + updates `lastMessage` and unread count on chat |
| `updateReadStatus(chatId)` | `ChatRepository` | Marks all unread messages as read |
| `assignChatRoom(chatId)` | `ChatRepository` | Assigns chat to current staff; updates both `chats` and `incidents` |

### Survival Guides

| Operation | Repository | Description |
|---|---|---|
| `getAllGuides()` | `GuidesRepository` | All guides ordered by title |
| `getGuidesByIncidentType(type)` | `GuidesRepository` | Guides filtered by incident type |
| `getGuideById(id)` | `GuidesRepository` | Single guide document |

### Staff Profile

| Operation | Repository | Description |
|---|---|---|
| `getCurrentStaffProfile()` | `ProfileRepository` | Fetches current staff document |
| `updateStaffProfile(name, phone)` | `ProfileRepository` | Updates staff name and phone |
| `updateStaffStatus(status)` | `ProfileRepository` | Toggles availability (Available / Working) |
| `changePassword(newPassword)` | `ProfileRepository` | Updates Firebase Auth password |

---

## Role-Based Access

| Role | App | Access | Description |
|---|---|---|---|
| **Student** (`นักศึกษา`) | Client | Full client features | KKU students who can report incidents and chat with staff |
| **University Staff** (`บุคลากร`) | Client | Full client features | KKU personnel who can report incidents and chat with staff |
| **General Public** (`บุคคลทั่วไป`) | Client | Full client features | Non-KKU individuals who can report incidents and chat with staff |
| **Emergency Staff** | Staff | Full staff features | Emergency responders who manage incidents, chat, and receive notifications |

### How Authorization Works

1. **Separate apps, separate access** — The Client and Staff apps are completely separate Android applications. Each app authenticates against a different Firestore collection (`users` vs `staff`).

2. **Firebase Authentication** — Both apps use Firebase email/password authentication. On login, the Staff app performs an additional check to verify the user's UID exists in the `staff` collection; if not, the user is signed out and denied access.

3. **Data isolation via queries** — Client users can only see their own incidents (queries filter by `reporterId == currentUserId`). Staff members see all incidents but can only be assigned to ones they accept.

4. **FCM token management** — Staff FCM tokens are stored on login and cleared on logout, ensuring push notifications are only delivered to actively authenticated staff devices.

5. **Chat room binding** — Chat rooms are created alongside incidents and bound to the reporter's UID. Staff members access chats through assignment, ensuring only the assigned responder communicates with the reporter.

---

## License

This project was developed as part of a coursework project at Khon Kaen University.
