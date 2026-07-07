# FILE 5 — API & BACKEND

> **AI Instruction**: Wenddors का backend **Firebase (Serverless)** है — कोई custom REST server नहीं। सारी data access Firestore SDK से होती है, media Cloudinary पर, payments Razorpay से। नीचे दिया गया structure ही single source of truth है।

---

## 🏗️ Backend Architecture

```
Firebase Project: wenddors-72733
├── Firebase Auth          → Email/Password, Google, Phone OTP
├── Cloud Firestore        → Primary database (सभी app data)
├── Firebase Storage       → Backup media store (documents/receipts)
├── Firebase Messaging     → Push notifications (FCM)
└── Cloud Functions        → Razorpay order creation, signature verify,
                              notification triggers, aggregate counters

External Services:
├── Cloudinary             → Images/Videos (unsigned upload presets)
│     ├── dzaqvrpmw  → reels + vendor gallery
│     ├── djhqqdsib  → user profile photos
│     ├── dvnfni7cz  → chat media
│     └── dzbt5nnsr  → vendor profiles (business app)
└── Razorpay               → Payment gateway (UPI/cards/netbanking)
```

> **Note**: User App और Vendor App एक ही Firebase project share करते हैं। Custom claims से role separation: `{ "role": "user" | "shopkeeper" }`.

---

## 🔐 Authentication

| Method | Provider | Flow |
|--------|----------|------|
| Email/Password | Firebase Auth | signUp → email verify → Firestore `users/{uid}` doc create |
| Google Sign-In | google_sign_in + Firebase | one-tap → अगर नया user तो profile auto-create |
| Phone OTP | Firebase Phone Auth | verifyPhoneNumber → SMS OTP → signInWithCredential |

- **Session**: Firebase ID token (auto-refresh); `authStateChanges()` stream से GoRouter redirect
- **Authorization**: Firestore Security Rules + custom claims (`role: user`)
- **`AuthService`** (lib/src/services/auth_service.dart) सारे auth operations wrap करता है

---

## 🗄️ Firestore Collections (Data API)

App में generic **`FirestoreService`** है (create, read, readWhere, watch, watchCollection, update, delete, batch ops, transaction, increment, arrayUnion/Remove) — सभी providers इसी से DB access करते हैं।

### Top-level Collections

| Collection | Doc ID | Owner-Write | पढ़ता कौन है |
|-----------|--------|-------------|---------------|
| `users` | auth UID | खुद user | खुद user |
| `vendors` | vendorId | vendor app | सभी authenticated (public read) |
| `bookings` | auto | user create; vendor status update | user + related vendor |
| `events` | auto | event owner + members | owner + members |
| `budgets` | auto | user | user |
| `checklists` | auto | user | user |
| `chats` | `{userId}_{vendorId}` | दोनों participants | दोनों participants |
| `reels` | auto | vendor app | public read |
| `reviews` | auto | user (verified booking) | public read |
| `categories` | slug | admin only | public read |
| `category_ads` | auto | admin only | public read |
| `favorites` | `{userId}_{vendorId}` | user | user |
| `following` | `{userId}_{vendorId}` | user | user + vendor (count) |
| `blockedVendors` | `{userId}_{vendorId}` | user | user |
| `finalizedVendors` | auto | user | user |
| `reelInteractions` | `{userId}_{reelId}` | user | user |
| `orders` | auto | Cloud Function | user + vendor |

### Sub-collections

```
users/{uid}/notifications/{notificationId}
events/{eventId}/ceremonies/{ceremonyId}
events/{eventId}/guests/{guestId}
events/{eventId}/vendorBookings/{bookingId}
events/{eventId}/foodMenus/{menuId}
events/{eventId}/menuItems/{itemId}
events/{eventId}/hotelRooms/{roomId}
events/{eventId}/members/{memberId}
chats/{chatId}/messages/{messageId}
reels/{reelId}/comments/{commentId}
```

---

## 📡 Data Operations (Endpoint-equivalent Map)

Firestore में REST endpoints नहीं होते — नीचे हर feature का "logical endpoint" दिया है:

### Auth
| Operation | Method-equivalent | Input → Output |
|-----------|------------------|----------------|
| Sign up | `AuthService.signUpWithEmail` | {email, password, name} → UserCredential + users doc |
| Login | `AuthService.signInWithEmail` | {email, password} → UserCredential |
| Google login | `AuthService.signInWithGoogle` | — → UserCredential |
| Phone OTP | `verifyPhoneNumber` → `signInWithPhoneOtp` | {phone} → verificationId → {otp} → UserCredential |
| Reset password | `sendPasswordResetEmail` | {email} → reset mail |

### Vendors (READ)
| Operation | Query |
|-----------|-------|
| List by category | `vendors.where('category'==X).orderBy('rating', desc)` |
| Featured | `vendors.where('isVerified'==true).limit(10)` |
| Search | `vendors.where('shopName' >= q).where('shopName' <= q+'\uf8ff')` |
| Detail | `vendors/{vendorId}` (doc read) |
| Reviews | `reviews.where('vendorId'==X).orderBy('createdAt', desc)` |

### Bookings (WRITE + READ)
| Operation | Action |
|-----------|--------|
| Create booking | `bookings.add({...})` — status='pending', paymentStatus='pending' |
| My bookings | `bookings.where('userId'==uid).orderBy('createdAt', desc)` + optional status filter |
| Update payment | transaction: paidAmount += X; paymentStatus recompute |
| Cancel | `bookings/{id}.update({bookingStatus:'cancelled'})` (rules के अधीन) |

### Chat (REALTIME)
| Operation | Action |
|-----------|--------|
| Conversation list | `chats.where('userId'==uid).orderBy('lastMessageAt', desc)` — stream |
| Messages | `chats/{chatId}/messages.orderBy('createdAt')` — stream |
| Send | batch: messages.add + chat doc update (lastMessage, unreadCountVendor++) |
| Mark read | batch update status='read' + unreadCountUser=0 |

### Reels (REALTIME + WRITE)
| Operation | Action |
|-----------|--------|
| Feed | `reels.orderBy('createdAt', desc).limit(10)` — paginated |
| Like/Save | `reelInteractions/{uid_reelId}` set + `reels/{id}` increment counter |
| Comment | `reels/{id}/comments.add` + commentCount increment |

### Events / Budget / Checklist / Guests
सभी CRUD `FirestoreService` के generic methods से — collection paths ऊपर की table अनुसार।

---

## ☁️ Cloud Functions (Server-side logic)

| Function | Trigger | काम |
|----------|---------|-----|
| `createRazorpayOrder` | HTTPS callable | {amount, bookingId} → Razorpay order_id (secret key server पर ही रहती है) |
| `verifyRazorpaySignature` | HTTPS callable | {orderId, paymentId, signature} → verified boolean; success पर booking update |
| `onBookingCreated` | Firestore trigger | vendor को FCM notification |
| `onChatMessage` | Firestore trigger | receiver को FCM push |
| `onReviewCreated` | Firestore trigger | vendor का rating aggregate recompute |
| `vendorConfirmation` | HTTPS | tokenized link verify → booking confirm (VendorConfirmationService इसका client है) |

> **Rule**: Razorpay **secret key कभी app code में नहीं** — order creation और signature verification हमेशा Cloud Function में।

---

## 🖼️ Cloudinary (Media API)

**Upload URL**: `POST https://api.cloudinary.com/v1_1/{cloudName}/{image|video}/upload`
(unsigned, multipart form-data via Dio) — config `lib/src/config/constants/env.dart` में:

| Use-case | Cloud Name | Preset | Folder |
|----------|-----------|--------|--------|
| Reels | dzaqvrpmw | wenddors_reels | wenddors/reels |
| Vendor gallery | dzaqvrpmw | wedding_reels | vendor_profiles |
| User profile | djhqqdsib | user_profile | wenddors/users |
| Chat media | dvnfni7cz | chat_preset | — |

Response से `secure_url` लेकर Firestore doc में save होता है। Upload से पहले `flutter_image_compress` से compression।

---

## 💳 Razorpay (Payment API)

Flow (`PaymentService`):
```
1. User "Pay" दबाता है (Checkout screen)
2. App → Cloud Function: createOrder({amount, bookingId}) → order_id
3. razorpay.open({key, order_id, amount, prefill: {name, email, phone}})
4. SUCCESS callback → Cloud Function: verifySignature(...)
5. Verified → booking.paidAmount update, paymentStatus recompute
6. → Payment Success screen + notification
```
Events: `EVENT_PAYMENT_SUCCESS`, `EVENT_PAYMENT_ERROR`, `EVENT_EXTERNAL_WALLET`

---

## 🔔 FCM (Notifications)

- `NotificationService` — FCM token registration → `users/{uid}.fcmToken`
- Foreground: `flutter_local_notifications` से display
- Background/terminated: FCM default + tap पर deep-link (`relatedId` से route)
- Notification doc भी `users/{uid}/notifications` में save होता है (in-app list के लिए)

---

## 🔒 Firestore Security Rules (Baseline)

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{db}/documents {
    function signedIn() { return request.auth != null; }
    function isOwner(uid) { return request.auth.uid == uid; }

    match /users/{uid} { allow read, write: if isOwner(uid); }
    match /users/{uid}/notifications/{n} { allow read, write: if isOwner(uid); }
    match /vendors/{v} { allow read: if signedIn(); allow write: if false; } // vendor app rules अलग
    match /categories/{c} { allow read: if signedIn(); }
    match /reels/{r} { allow read: if signedIn();
      match /comments/{c} { allow read: if signedIn();
        allow create: if signedIn() && request.resource.data.userId == request.auth.uid; } }
    match /bookings/{b} {
      allow create: if signedIn() && request.resource.data.userId == request.auth.uid;
      allow read, update: if signedIn() &&
        (resource.data.userId == request.auth.uid || resource.data.vendorId == request.auth.uid); }
    match /chats/{chatId} {
      allow read, write: if signedIn() && request.auth.uid in chatId.split('_');
      match /messages/{m} { allow read, write: if signedIn() && request.auth.uid in chatId.split('_'); } }
    match /events/{e} {
      allow read, write: if signedIn() && resource.data.userId == request.auth.uid;
      match /{sub=**} { allow read, write: if signedIn(); } // members check function से refine
    }
    match /reviews/{r} { allow read: if signedIn();
      allow create: if signedIn() && request.resource.data.userId == request.auth.uid; }
  }
}
```

---

## 📶 Offline & Caching Strategy

1. **Firestore offline persistence**: enabled — reads cache से serve होते हैं
2. **Hive cache** (`CacheService`): vendors list, bookings, events का structured cache TTL के साथ
3. **OfflineSyncService + BackgroundSyncService**: offline में किए गए writes queue होते हैं; connectivity लौटने पर sync
4. **ConnectivityProvider**: `connectivity_plus` से live network state → No Internet overlay

---

## 🌱 Seeding (Development)

- `seed_firebase.js`, `seed_vendors.js` (Node + firebase-admin) — categories/vendors का test data
- App के अंदर `SeedService`, `category_seeder.dart`, `event_seeder.dart` — dev-only seeding
- **Rule**: Seeders production build में कभी trigger न हों (debug-mode guard)
