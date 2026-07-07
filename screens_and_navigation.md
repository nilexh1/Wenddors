# FILE 3 — SCREENS & NAVIGATION

> **AI Instruction**: यह app के सभी screens और navigation flow का official map है। Routing GoRouter से है — सभी routes `lib/src/config/routing/app_router.dart` और route names `route_names.dart` में defined हैं। कोई भी नई screen इसी pattern में जुड़ेगी।

---

## 🧭 Navigation Architecture

- **Router**: GoRouter (v11.x) — declarative, deep-link ready
- **Shell**: `StatefulShellRoute` + `MainShell` widget — 5-tab **Bottom Navigation Bar**
- **Nested Stacks**: हर tab की अपनी navigation stack (state preserve रहती है)
- **Root-level Routes**: Chat detail, Checkout, Notifications, Search आदि bottom-nav के ऊपर full-screen खुलते हैं
- **Auth Guard**: GoRouter redirect — unauthenticated user हमेशा `/login` पर redirect होता है
- **Drawer**: नहीं है — navigation पूरी तरह bottom nav + stack push से

### Bottom Navigation (5 Tabs)

| # | Tab | Icon | Route |
|---|-----|------|-------|
| 1 | Home | home_outlined | `/home` |
| 2 | Events | calendar_month | `/events` |
| 3 | Chat | chat_bubble_outline | `/chat` |
| 4 | Reels | play_circle_outline | `/reels` |
| 5 | Profile | person_outline | `/profile` |

---

## 📺 Screens की पूरी List (~55 screens)

### 1. Launch & Auth Flow (8 screens)

| Screen | Route | क्या दिखता है |
|--------|-------|----------------|
| **Splash** | `/splash` | Animated Wenddors logo; background में auth check; 2 sec बाद login या home पर redirect |
| **Login** | `/login` | Email + password fields, "Sign in with Google" button, Phone login option, Forgot Password link, Sign Up link |
| **Sign Up** | `/signup` | First/last name, email, phone, password + strength indicator, terms checkbox |
| **Forgot Password** | `/forgot-password` | Email field → reset link भेजना |
| **OTP Verification** | `/otp-verify` | 6-digit OTP input boxes, resend timer, auto-verify |
| **Reset Password** | `/reset-password` | नया password set करना |
| **Terms** | `/terms` | Terms & Conditions का scrollable text |
| **Onboarding** | `/onboarding` | 3-4 screen flow (wedding date → estimated budget → create first event) नए user के लिए |

### 2. Home Tab (Tab 1)

| Screen | Route | क्या दिखता है |
|--------|-------|----------------|
| **Home** | `/home` | Greeting header + notification bell, search bar, promotional banners, category grid (6 main categories), featured/verified vendors carousel, nearby vendors (location-based), blog/tips section |
| **Vendor Detail** | `/vendors/:vendorId` | Hero image header, shop name + verified badge, rating + reviews count, follower count, 4 tabs: **Info** (description, contact, location), **Services** (price list), **Gallery** (photos/videos), **Reviews**; sticky bottom bar: Chat + Book Now buttons |
| **Photo Lightbox** | (push) | Gallery image full-screen zoom/swipe viewer |
| **Category Vendors** | `/category-vendors` | Selected category के vendors की list — filter (price, rating), sort options |
| **Vendor List** | `/vendors` | सभी vendors browse — search + category chips |
| **Search** | `/search` | Search bar + recent searches + results (vendors) |
| **Vendor Reviews** | (push) | Vendor के सभी reviews की full list + Add Review dialog |

### 3. Events Tab (Tab 2)

| Screen | Route | क्या दिखता है |
|--------|-------|----------------|
| **Events List** | `/events` | User के सभी events के cards (cover image, date, location, progress); Create Event FAB |
| **Create Event** | `/events/create` | Event form: title, wedding date, location, region, cover image upload |
| **Event Detail** | `/events/:eventId` | Event dashboard: countdown, quick stats (guests, vendors, budget, ceremonies), section tiles → Guests, Schedule, Menu, Transport, Vendors, Members, Budget |
| **Event Schedule** | `/events/:eventId/schedule` | Ceremonies की timeline (Haldi, Mehndi, Sangeet…) — date-time, venue, dress code; add/edit ceremony |
| **Event Guest List** | `/events/:eventId/guests` | Event के guests — RSVP filter (yes/no/pending), VIP badge, add/edit guest dialogs |
| **Food Menu** | `/events/:eventId/menu` | Meal-type tabs (Breakfast/Lunch/High-Tea/Dinner) — categories में dishes, caterer info |
| **Transport** | `/events/:eventId/transport` | Hotel rooms (check-in/out, guests, rate) + transport arrangements |
| **Event Vendors** | `/events/:eventId/vendors` | Ceremony-wise booked vendors — status, quoted/paid amounts, negotiation history |
| **Manage Members** | `/events/:eventId/members` | Family members list + invite sheet (phone/link से invite) |
| **Event Budget** | `/events/:eventId/budget` | Event का budget tracker — category-wise allocated vs spent |

### 4. Chat Tab (Tab 3)

| Screen | Route | क्या दिखता है |
|--------|-------|----------------|
| **Chat List** | `/chat` | सभी conversations — vendor photo, last message preview, unread badge, timestamp; empty state |
| **Chat Detail** | `/chat/:chatId` (root-level) | Message bubbles (text/image/video/PDF/location/voice/quotation/booking-link), reply preview, reactions, pinned header, typing indicator, input bar (attachment + camera + mic), selection app bar (multi-select) |
| **Chat Media Viewer** | (push) | Chat की media full-screen |

### 5. Reels Tab (Tab 4)

| Screen | Route | क्या दिखता है |
|--------|-------|----------------|
| **Reels Feed** | `/reels` | Full-screen vertical video pager — vendor avatar + follow, caption, audio pill, action column (like/comment/share/save), progress bar |
| **Comment Sheet** | (bottom sheet) | Reel के comments — add comment, like comment |
| **Share Sheet** | (bottom sheet) | WhatsApp/other apps में share |

### 6. Profile Tab (Tab 5)

| Screen | Route | क्या दिखता है |
|--------|-------|----------------|
| **Profile** | `/profile` | Profile photo + couple photo, name, wedding countdown, stats (bookings/saved/following), menu items list |
| **Personal Info** | `/profile/personal-info` | Name, phone, wedding date, budget, guest count edit; photo picker sheet |
| **Settings** | `/profile/settings` | Theme toggle (light/dark), language, notifications, logout, delete account |
| **Notification Preferences** | `/profile/notification-preferences` | Type-wise notification toggles |
| **Payment Methods** | `/profile/payment-methods` | Saved payment methods / transaction history |
| **Saved Vendors** | `/profile/saved-vendors` | Favorite vendors की list |
| **Followed Vendors** | `/profile/followed-vendors` | Follow किए vendors |
| **Blocked Vendors** | `/profile/blocked-vendors` | Block किए vendors + unblock |
| **Saved Reels** | `/profile/saved-reels` | Saved reels grid |
| **Security** | `/profile/security` | Password change, email update, delete account |
| **Help Center** | `/profile/help-center` | FAQs + contact support |
| **Language** | `/profile/language` | Hindi/English selection |
| **Vendor Lists** | (push) | Saved/Followed/Blocked का combined hub |

### 7. Bookings & Payment (root-level)

| Screen | Route | क्या दिखता है |
|--------|-------|----------------|
| **My Bookings** | `/bookings` | Status tabs (All/Pending/Confirmed/Completed/Cancelled) — booking cards |
| **Booking Detail** | `/bookings/:bookingId` | Vendor info, event date, amounts (total/advance/paid/remaining), payment status, installments, next due date, actions (Pay/Cancel/Chat) |
| **Checkout** | `/checkout` | Booking summary + amount + Razorpay pay button |
| **Payment Success** | `/payment-success` | ✅ animation, payment details, "View Booking" CTA |
| **Finalized Vendors** | (push) | Confirm हो चुके vendors की summary list |

### 8. Planning Tools (root/tab-level)

| Screen | Route | क्या दिखता है |
|--------|-------|----------------|
| **Budget Overview** | `/budget` | Total/spent/remaining cards + progress, category list, Add Expense FAB (Event Context Indicator required) |
| **Category Budget** | `/budget/category` | एक category का detail — expenses list, add expense sheet |
| **Checklist** | `/checklist` | Circular progress ring, filter chips (All/This Week/Overdue/Done), timeline sections, task cards, Add Task FAB (Event Context Indicator required) |
| **Checklist Detail** | `/checklist/:taskId` | Task का पूरा detail — edit dialog, notes, complete toggle |
| **Guest List (Global)** | `/guests` | सभी events के combined guests (Event Context Indicator required) |
| **Guest Detail** | (push) | Guest की details + edit dialog |

### 9. Global / Utility

| Screen | Route | क्या दिखता है |
|--------|-------|----------------|
| **Notifications** | `/notifications` | Notification list — type icons, read/unread, tap → related screen |
| **No Internet** | (overlay) | Offline state — retry button; cached data दिखता रहता है |
| **Error** | `/error` | Friendly error message + Go Home button |
| **Invite** | (push) | E-invite (future feature scaffold) |

---

## 🔀 Navigation Flow (User Journeys)

```
SPLASH ──auth check──┬─→ LOGIN ⇄ SIGNUP → TERMS
                     │      ↓ forgot → FORGOT PW → OTP → RESET PW
                     └─→ MAIN SHELL (Bottom Nav)

MAIN JOURNEY (Booking):
HOME → Category tap → CATEGORY VENDORS → VENDOR DETAIL
     → [Book Now] → BOOKING FORM SHEET → CHECKOUT → Razorpay
     → PAYMENT SUCCESS → BOOKING DETAIL
     → [Chat] → CHAT DETAIL

EVENT JOURNEY:
EVENTS → CREATE EVENT → EVENT DETAIL
       → Schedule / Guests / Menu / Transport / Vendors / Members / Budget

DISCOVERY JOURNEY:
REELS → vendor avatar tap → VENDOR DETAIL → Book/Chat/Follow
HOME → search icon → SEARCH → VENDOR DETAIL

PLANNING JOURNEY:
PROFILE / HOME quick-links → BUDGET → CATEGORY BUDGET → ADD EXPENSE
                           → CHECKLIST → TASK DETAIL
                           → GUESTS → GUEST DETAIL
```

---

## 📐 Navigation Rules

1. **Back Behavior**: Tab के अंदर back = stack pop; root पर back = exit confirmation dialog (`back_confirmation_dialog.dart`)
2. **Deep Links**: `/vendors/:vendorId`, `/bookings/:bookingId`, `/chat/:chatId` — notification tap से सीधे खुलते हैं
3. **Auth Redirect**: कोई भी protected route unauthenticated state में `/login` पर redirect
4. **Chat Detail root-level है** ताकि bottom nav hide रहे (full-screen typing experience)
5. **Checkout & Payment root-level हैं** — payment के दौरान user tab switch न कर सके
