# FILE 10 — CURRENT PROGRESS & TASKS

> **AI Instruction**: हर नए session में **सबसे पहले यही file पढ़ो** और session खत्म होने पर इसे **update करो**। यही file बताती है कि पिछली बार कहाँ छोड़ा था और अब क्या करना है।

**Last Updated**: 2026-07-07
**Current Branch**: `genspark_ai_developer` (base: `main`)
**Overall Completion**: ~70%

---

## ✅ क्या बन चुका है (DONE)

### Infrastructure (100%)
- [x] Flutter project setup (Dart ^3.11.0), सभी platforms scaffolded
- [x] Firebase integration — Auth, Firestore, Storage, Messaging (`firebase_options.dart`)
- [x] Riverpod state management — 21 providers wired
- [x] GoRouter — StatefulShellRoute + 5-tab bottom nav + auth redirect
- [x] Theme system — Light/Dark दोनों, complete design tokens (colors, text styles, spacing, shadows, animations)
- [x] Centralized ErrorHandler, AppLogger, validators, extensions
- [x] Offline stack — Hive cache, OfflineSyncService, BackgroundSyncService, ConnectivityProvider, No-Internet screen
- [x] Cloudinary media pipeline (reels/vendor/user/chat presets) + image compression

### Features (बने हुए)
- [x] **Auth** — Email/password signup+login, Google Sign-In, Phone OTP screens, forgot/reset password, terms, password strength indicator
- [x] **Home** — greeting, search, categories grid, banners, featured vendors
- [x] **Vendors** — category listing, search screen, vendor detail (4 tabs: info/services/gallery/reviews), photo lightbox, reviews + add-review dialog
- [x] **Vendor lists** — saved/followed/blocked vendors screens
- [x] **Bookings** — booking form sheet, my bookings list, booking detail, vendor confirmation link service (WhatsApp)
- [x] **Checkout & Payment** — Razorpay integration (PaymentService), checkout screen, payment success screen
- [x] **Events** — create event, event list/detail, schedule (ceremonies), event guest list, food menu, transport (hotel rooms), event vendors (negotiation history), manage members, event budget tracker
- [x] **Budget** — overview, category budget, add expense / create budget sheets
- [x] **Checklist** — list + detail, add/edit task dialogs, progress ring
- [x] **Guests (global)** — list, detail, add/edit dialogs
- [x] **Chat** — chat list, chat screen with rich messages (text/image/video/pdf/location/voice/quotation/booking-link), reply, reactions, pin, edit/delete, typing indicator, media viewer
- [x] **Reels** — vertical feed, like/comment/share/save, comment sheet, share sheet (WhatsApp), saved reels
- [x] **Profile** — profile screen, personal info, settings (theme/language), notification preferences, payment methods, security, help center, language screen
- [x] **Notifications** — FCM setup, in-app notifications screen
- [x] 17 data models + generic FirestoreService (CRUD/batch/transaction)
- [x] Seeding tooling (Node scripts + in-app seeders)

### Documentation (यह session)
- [x] `Plane/` folder में 10 audit files (project overview → progress) — 2026-07-07

---

## 🚧 क्या बाकी है (TODO — priority order)

### 🔴 High Priority
1. [ ] **README.md merge conflict resolve करो** — root README में अभी भी `<<<<<<< HEAD` conflict markers हैं
2. [ ] **Repo cleanup** — root से ~25 `analyze_*.txt` / analysis dump files हटाओ; `wenddors-72733-firebase-adminsdk-*.json` (service account key!) को git से हटाकर `.gitignore` में डालो — **security risk**
3. [ ] **Cloud Functions deploy** — `createRazorpayOrder` + `verifyRazorpaySignature` (अभी PaymentService client इन्हें expect करता है); बिना इनके production payment unsafe है
4. [ ] **Firestore Security Rules production-ready करके deploy** — baseline `Plane/api_and_backend.md` में लिखी है
5. [ ] **Booking cancellation rules (B6) पूरी तरह implement** — 7-day window + refund status flow
6. [ ] **flutter analyze zero-error बनाओ** — बचे हुए warnings/infos clean करो

### 🟡 Medium Priority
7. [ ] **Onboarding Flow** — 3-4 screen flow (wedding date → estimated budget → first event create) new user experience
8. [ ] **Duplicate Booking Prevention (B7)** — Booking create से पहले check कि user ने vendor को same event date पर book तो नहीं किया
9. [ ] **paymentStatus Auto-Compute (B4)** — Verify that paymentStatus is correctly recomputed on every payment update, never set manually
10. [ ] **Review gating (B10)** — completed booking check के बिना review न जा सके
11. [ ] **Reels view-count rule (H2)** — 3-second watch threshold
12. [ ] **Notification deep-linking (I2)** — हर type से सही screen खुले, terminated-state tap included
13. [ ] **Payment due reminders (I3)** — local notification scheduling
14. [ ] **Event aggregate counters (C6)** — sub-collection writes पर transaction-based sync
15. [ ] **Localization पूरा करो** — सभी hard-coded strings को app_strings.dart/ARB में लाओ; Hindi translations complete
16. [ ] **Tests लिखो** — services (auth, firestore, payment) + core providers के unit tests; अभी coverage बहुत कम है

### 🟢 Low Priority / Polish
14. [ ] Empty/error states का audit — हर list screen पर EmptyState/ErrorState consistent हो
15. [ ] Skeleton loaders सभी main lists पर
16. [ ] Dark mode visual QA — हर screen
17. [ ] App icon + splash branding final करो
18. [ ] Performance pass — reels memory, image sizes, Firestore query limits
19. [ ] E-invite feature (invite screen scaffold से आगे) — future feature #59

---

## 🐛 Known Bugs / Issues (Pending)

| # | Issue | Severity | Notes |
|---|-------|----------|-------|
| 1 | README.md में unresolved merge conflict markers | 🔴 | Repo hygiene — तुरंत fix |
| 2 | Service-account JSON repo में committed | 🔴 | Key rotate करो + git history से purge + .gitignore |
| 3 | Duplicate `google-services.json` root पर (`google-services (3).json`) | 🟡 | सही file `android/app/` में हो, बाकी delete |
| 4 | Home screen file बहुत बड़ी (~37KB) | 🟡 | Widgets में refactor करो (folder rules के अनुसार) |
| 5 | Legacy `status` field GuestModel में rsvpStatus के साथ duplicate | 🟢 | Migration के बाद हटाना |
| 6 | Events tab का icon `save_outlined` है | 🟢 | Fixed in screens_and_navigation.md |

---

## ➡️ अगला Task (NEXT — यहीं से शुरू करो)

**Task**: High-priority items #1–#3 —
1. README merge conflict resolve
2. Root cleanup + service-account key git से हटाना
3. Razorpay Cloud Functions का implementation/deploy plan

**क्यों**: ये security और repo integrity के blockers हैं — बाकी feature work इनके बाद।

---

## 📝 Session Log

| Date | Session Summary |
|------|-----------------|
| 2026-05-08 | APP_ANALYSIS.md Phase 1 — architecture mapping |
| 2026-07-07 | पूरी project audit — `Plane/` में 10 audit files बनाई (overview, features, screens, models, backend, tech stack, folders, design, rules, progress) |

> **Update Protocol**: हर session के अंत में — (1) DONE में completed items ✔ करो, (2) नए bugs table में जोड़ो, (3) "अगला Task" section rewrite करो, (4) Session Log में एक line जोड़ो, (5) Last Updated date बदलो।
