# FILE 6 — TECH STACK

> **AI Instruction**: यह project का official tech stack है। नया package जोड़ने से पहले check करो कि उसी काम का package पहले से मौजूद तो नहीं। Versions `pubspec.yaml` से sync रहने चाहिए।

---

## 🎯 Core

| Item | Version / Choice |
|------|------------------|
| **Framework** | Flutter (stable channel) |
| **Dart SDK** | `^3.11.0` |
| **App version** | 1.0.0+1 |
| **Platforms** | Android + iOS (primary); web/windows/macos/linux scaffolds मौजूद |
| **Localization** | flutter_localizations (Hindi + English), `generate: true` |

---

## 🧠 State Management — Riverpod

- **Package**: `flutter_riverpod: ^2.4.0`
- **Approach**: `StateNotifier` + `StateNotifierProvider` per-feature (auth, booking, budget, chat, checklist, event, guest, reel, review, vendor, notification, theme, connectivity, location, menu, message, ceremony, category_ad, user, upload providers — कुल 21 providers)
- **Pattern**: हर provider अपनी immutable state class रखता है (`isLoading`, `error`, data) और services को call करता है
- **DI**: `service_locator.dart` (lib/src/config/di/) — services का central wiring; providers services को यहीं से लेते हैं
- **Rule**: UI में direct Firestore call **कभी नहीं** — हमेशा Provider → Service → Firestore

---

## 🧭 Routing

- **Package**: `go_router: ^11.1.0`
- `StatefulShellRoute.indexedStack` — 5-tab bottom navigation, per-tab stacks
- Route constants: `route_names.dart` (`AppRoutes` class) — magic strings मना हैं
- Auth redirect logic router-level पर

---

## 🔥 Firebase Stack

| Package | Version | Use |
|---------|---------|-----|
| firebase_core | ^4.5.0 | init (`firebase_options.dart` — FlutterFire CLI generated) |
| firebase_auth | ^6.2.0 | Email/Google/Phone auth |
| cloud_firestore | ^6.1.0 | Primary DB |
| firebase_storage | ^13.1.0 | Documents/receipts backup store |
| firebase_messaging | ^16.1.2 | Push notifications |
| google_sign_in | ^6.2.0 | Google OAuth |

---

## 📦 Complete Package Map

### UI & Design
| Package | Version | काम |
|---------|---------|-----|
| google_fonts | ^6.1.0 | Playfair Display, Lato, Manrope, Inter |
| phosphor_flutter | ^2.1.0 | Icon set (modern icons) |
| cupertino_icons | ^1.0.8 | iOS-style icons |
| flutter_svg | ^2.3.0 | SVG assets |
| cached_network_image | ^3.4.1 | Network images + disk cache (सभी vendor/user images के लिए अनिवार्य) |
| table_calendar | ^3.0.9 | Wedding date / ceremony date pickers |

### Media
| Package | Version | काम |
|---------|---------|-----|
| image_picker | ^1.1.0 | Camera/gallery picking |
| flutter_image_compress | ^2.2.0 | Upload से पहले compression (अनिवार्य step) |
| video_player | ^2.9.1 | Reels + chat videos |
| video_player_win | ^3.2.2 | Windows video support |
| gal | ^2.3.2 | Gallery में media save |

### Networking & Backend
| Package | Version | काम |
|---------|---------|-----|
| dio | ^5.3.0 | Cloudinary uploads (multipart), external HTTP |
| connectivity_plus | ^6.1.1 | Network state detection |

### Storage & Caching
| Package | Version | काम |
|---------|---------|-----|
| hive + hive_flutter | ^2.2.3 / ^1.1.0 | Structured offline cache (vendors, bookings, events) |
| shared_preferences | ^2.2.3 | Simple prefs (theme, language, onboarding flags) — `PreferencesService` wrap करती है |
| path_provider, path | ^2.1.5 / ^1.9.0 | File paths |

### Payments
| Package | Version | काम |
|---------|---------|-----|
| razorpay_flutter | ^1.3.7 | Payment gateway (India — UPI/cards/netbanking) |

### Notifications
| Package | Version | काम |
|---------|---------|-----|
| flutter_local_notifications | ^17.0.0 | Foreground FCM display + reminders |

### Location & Permissions
| Package | Version | काम |
|---------|---------|-----|
| geolocator | ^13.0.0 | Nearby vendors (location-based) |
| permission_handler | ^11.3.0 | Runtime permissions (camera, location, notifications, mic) |

### Utilities
| Package | Version | काम |
|---------|---------|-----|
| intl | ^0.20.2 | Date/currency formatting (₹, dd MMM yyyy) |
| logger | ^2.1.0 | `AppLogger` wrapper (lib/src/utils/logger.dart) — `print()` मना है |
| uuid | ^4.0.0 | Client-side unique IDs |
| share_plus | ^12.0.1 | Native share sheet |
| url_launcher | ^6.3.2 | Phone/WhatsApp/web links (WhatsAppShareService इसे use करती है) |

### Dev & Testing
| Package | Version | काम |
|---------|---------|-----|
| flutter_test / integration_test | SDK | Unit + integration tests |
| mockito | ^5.4.4 | Mocks (codegen) |
| mocktail | ^1.0.3 | Mocks (no codegen — नए tests में preferred) |
| build_runner | ^2.4.8 | Codegen runner |
| flutter_lints | ^6.0.0 | Lint rules (`analysis_options.yaml`) |

---

## 🧩 Cross-cutting Choices (Decision Record)

| Concern | Decision | क्यों |
|---------|----------|------|
| State management | Riverpod (StateNotifier) | Testable, compile-safe, no BuildContext dependency |
| Media hosting | Cloudinary (primary) + Firebase Storage (docs) | Free tier + transformations + CDN |
| Image loading | cached_network_image + `OptimizedImage` wrapper widget | Consistent placeholders/errors |
| Offline | Firestore persistence + Hive + sync queue | Tier-2/3 India में weak network common |
| Payment | Razorpay | India-first (UPI) |
| Logging | logger via AppLogger | Filterable, production-safe |
| Error handling | `ErrorHandler` (config/error/) — centralized Firebase error → user-friendly Hindi/English messages | Consistent UX |
| Fonts | google_fonts runtime fetch + assets/fonts fallback | Offline safety |

---

## 🛠️ Assets

```
assets/
├── images/            → static images
│   ├── categories/    → category icons/photos
│   └── banners/       → home promotional banners
├── icons/             → app icons/SVGs
├── animations/        → Lottie/animated assets
├── fonts/             → bundled font files
└── data/              → seed/static JSON
```

---

## ⚙️ Build & Tooling

- **Android**: Gradle (android/), `google-services.json` app-level पर; minSdk 23; multidex enabled
- **iOS**: ios/ — `GoogleService-Info.plist` आवश्यक
- **Lints**: flutter_lints v6 — नई code warnings-free होनी चाहिए
- **Node tooling** (repo root): `package.json` + firebase-admin — seeding scripts के लिए (app का हिस्सा नहीं)
- **Commands**:
  - `flutter pub get`
  - `flutter analyze` (हर PR से पहले zero-error)
  - `flutter test`
  - `flutter build apk --release` / `flutter build ios`
