# FILE 7 — FOLDER STRUCTURE

> **AI Instruction**: यह project की official directory structure है। नई file हमेशा नीचे बताए गए सही folder में बनाओ। `lib/src/` के बाहर app code नहीं जाता (सिर्फ entrypoint और firebase_options)।

---

## 🌳 Root Structure

```
wenddors_user/
├── android/                 # Android native (Gradle, google-services.json)
├── ios/                     # iOS native (GoogleService-Info.plist)
├── web/ linux/ macos/ windows/   # अन्य platform scaffolds
├── assets/                  # Images, icons, fonts, animations, data
├── lib/                     # ← सारा Dart code यहाँ
├── test/                    # Unit + widget tests (lib structure mirror करता है)
├── Plane/                   # 📋 Project documentation & audit files (यह folder)
├── pubspec.yaml             # Dependencies + assets registration
├── analysis_options.yaml    # Lint rules
└── README.md
```

> **Repo hygiene rule**: root में analysis dumps (`analyze_*.txt`), duplicate configs, service-account JSON जैसी loose files नहीं रहनी चाहिए — service-account keys `.gitignore` में हों और seeding scripts `tools/` में। नई ऐसी files root पर मत बनाओ।

---

## 📂 lib/ — Detailed Structure

```
lib/
├── main.dart                          # Entry: Firebase init, Hive init, ProviderScope, runApp
├── firebase_options.dart              # FlutterFire CLI generated (edit मत करो)
└── src/
    ├── app.dart                       # Root MaterialApp.router (theme + router wiring)
    │
    ├── config/                        # ⚙️ App-wide configuration
    │   ├── constants/
    │   │   ├── app_constants.dart     # Global constants (limits, durations, keys)
    │   │   └── env.dart               # Environment config (Cloudinary cloud names/presets)
    │   ├── di/
    │   │   └── service_locator.dart   # Dependency injection / service wiring
    │   ├── error/
    │   │   └── error_handler.dart     # Centralized error → user message mapping
    │   ├── routing/
    │   │   ├── app_router.dart        # GoRouter config (shell + सभी routes)
    │   │   └── route_names.dart       # AppRoutes constants (magic strings मना)
    │   ├── strings/
    │   │   └── app_strings.dart       # UI strings (Hindi/English)
    │   └── theme/
    │       ├── app_theme.dart         # ThemeData (light + dark)
    │       ├── app_colors.dart        # Color tokens
    │       ├── app_text_styles.dart   # Typography tokens
    │       ├── app_spacing.dart       # Spacing scale
    │       ├── app_shadows.dart       # Elevation/shadow tokens
    │       ├── app_animations.dart    # Durations/curves
    │       └── app_assets.dart        # Asset path constants
    │
    ├── models/                        # 📦 Data models (17) — pure Dart, no Flutter UI imports
    │   ├── index.dart                 # Barrel export — models यहीं से import करो
    │   ├── user_model.dart, vendor_model.dart, booking_model.dart,
    │   ├── event_model.dart, ceremony_model.dart, guest_model.dart,
    │   ├── event_vendor_booking_model.dart, food_menu_model.dart,
    │   ├── menu_item_model.dart, hotel_room_model.dart,
    │   ├── budget_model.dart, checklist_model.dart, chat_model.dart,
    │   └── reel_model.dart, review_model.dart, notification_model.dart, category_model.dart
    │
    ├── providers/                     # 🧠 Riverpod StateNotifiers (21) — UI ↔ Service bridge
    │   ├── index.dart                 # Barrel export
    │   ├── auth_provider.dart, user_provider.dart
    │   ├── vendor_provider.dart, booking_provider.dart, review_provider.dart
    │   ├── event_provider.dart, ceremony_provider.dart, guest_provider.dart
    │   ├── event_vendor_booking_provider.dart, menu_provider.dart
    │   ├── budget_provider.dart, checklist_provider.dart
    │   ├── chat_provider.dart, message_provider.dart
    │   ├── reel_provider.dart, notification_provider.dart
    │   ├── theme_provider.dart, connectivity_provider.dart, location_provider.dart
    │   ├── category_ad_provider.dart
    │   └── user_profile_upload_provider.dart, vendor_image_upload_provider.dart
    │
    ├── services/                      # 🔧 Business/IO services (15) — Firestore/HTTP/device APIs
    │   ├── index.dart                 # Barrel export
    │   ├── auth_service.dart          # Firebase Auth wrapper
    │   ├── firestore_service.dart     # Generic CRUD/batch/transaction layer
    │   ├── storage_service.dart       # Firebase Storage
    │   ├── cloudinary_service.dart    # Media uploads (Dio multipart)
    │   ├── payment_service.dart       # Razorpay flow
    │   ├── notification_service.dart  # FCM + local notifications
    │   ├── cache_service.dart         # Hive cache (TTL)
    │   ├── preferences_service.dart   # SharedPreferences wrapper
    │   ├── offline_sync_service.dart  # Offline write queue
    │   ├── background_sync_service.dart # Connectivity-triggered sync
    │   ├── vendor_confirmation_service.dart # Tokenized confirmation links
    │   ├── whatsapp_share_service.dart # WhatsApp deep links
    │   ├── market_rate_service.dart   # Category price benchmarks
    │   └── seed_service.dart          # Dev-only data seeding
    │
    ├── screens/                       # 📺 Feature-wise screens
    │   ├── splash_screen.dart, home_screen.dart, error_screen.dart, no_internet_screen.dart
    │   ├── auth/                      # login, signup, forgot/reset password, otp, terms
    │   ├── vendors/                   # list, search, category, detail (+vendor_detail/widgets/), reviews
    │   ├── my_bookings/               # bookings list + detail
    │   ├── checkout/                  # checkout + payment success
    │   ├── events/                    # events CRUD + schedule/guests/menu/transport/vendors/members/budget (+widgets/)
    │   ├── budget/                    # overview + category detail (+widgets/ sheets)
    │   ├── checklist/                 # list + detail (+widgets/ dialogs)
    │   ├── guests/                    # global guest list + detail (+widgets/)
    │   ├── chat/                      # chat list + chat screen (+widgets/ — bubbles, input bar, आदि)
    │   ├── reels/                     # feed (+widgets/ — page, actions, comment sheet, आदि)
    │   ├── profile/                   # profile + settings + sub-screens (+widgets/)
    │   ├── finalized/                 # finalized vendors
    │   ├── notifications/             # notifications list
    │   ├── invite/                    # e-invite (future)
    │   ├── language/                  # language selection
    │   └── security/                  # account security
    │
    ├── widgets/                       # 🧱 Shared/reusable widgets
    │   ├── main_shell.dart            # Bottom-nav shell
    │   ├── vendor_profile_image_picker.dart
    │   └── common/                    # Design-system widgets:
    │       ├── custom_button.dart, custom_text_field.dart
    │       ├── app_badge.dart, app_dialog.dart, app_bottom_sheet.dart
    │       ├── app_section_header.dart, app_skeleton.dart
    │       ├── optimized_image.dart   # cached_network_image wrapper (हर network image इसी से)
    │       ├── state_widgets.dart     # Loading/Empty/Error states
    │       ├── password_strength_indicator.dart
    │       └── back_confirmation_dialog.dart
    │
    └── utils/                         # 🛠️ Pure helpers
        ├── logger.dart                # AppLogger (print मना)
        ├── validators.dart            # Form validation (email, phone, password)
        ├── extensions.dart            # DateTime/String/BuildContext extensions
        ├── category_seeder.dart, event_seeder.dart, mock_event_data.dart  # dev-only
```

---

## 📏 File Placement Rules

| क्या बना रहे हो | कहाँ रखो |
|-----------------|----------|
| नई screen | `screens/<feature>/<name>_screen.dart` |
| Screen-specific widget (सिर्फ उसी screen में use) | `screens/<feature>/widgets/` |
| 2+ screens में reusable widget | `widgets/common/` |
| नया data model | `models/` + `index.dart` में export |
| Firestore/HTTP logic | `services/` — कभी screen में नहीं |
| State logic | `providers/` — StateNotifier pattern |
| नया route | `route_names.dart` में constant + `app_router.dart` में entry |
| Colors/fonts/spacing | सिर्फ `config/theme/` tokens — hard-coded hex/size मना |
| UI strings | `config/strings/app_strings.dart` |
| Validation/formatting helper | `utils/` |
| Tests | `test/` में mirror path — `test/src/services/auth_service_test.dart` |

---

## 🔄 Layer Flow (Architecture)

```
Screen (UI)
   ↓ ref.watch / ref.read
Provider (StateNotifier — state + orchestration)
   ↓ calls
Service (Firestore / Cloudinary / Razorpay / Device APIs)
   ↓ serializes via
Model (fromFirestore / toFirestore)
```

**कड़ा नियम**: Screen कभी Service या Firestore को directly touch नहीं करती। Provider कभी BuildContext नहीं रखता। Service कभी UI import नहीं करती।
