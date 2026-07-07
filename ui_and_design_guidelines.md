# FILE 8 — UI & DESIGN GUIDELINES

> **AI Instruction**: यह Wenddors का official design system है। सभी tokens `lib/src/config/theme/` में defined हैं। **कभी भी hard-coded hex color, font size, या spacing UI code में मत लिखो** — हमेशा `AppColors`, `AppTextStyles`, `AppSpacing` use करो।

---

## 🎨 Design Tone

**Modern • Elegant • Premium • Warm** — शादी जैसे भावनात्मक अवसर के लिए sophisticated लेकिन friendly। Teal-based fresh palette + serif display headings (Playfair Display) luxury feel देती हैं; clean sans-serif body (Lato) readability देती है। Minimal clutter, generous whitespace, soft rounded corners।

---

## 🌈 Color Scheme (Source: `app_colors.dart`)

### ☀️ Light Mode
| Token | Hex | Use |
|-------|-----|-----|
| Primary | `#09887F` | Teal — buttons, active states, links, focus borders |
| Secondary | `#0B1115` | Near-black — headings, emphasis |
| Background | `#F4F6F8` | Screen background (soft grey) |
| Surface | `#FFFFFF` | Cards, sheets, app bars |
| Text Primary | `#0B1115` | Main text |
| Text Secondary | `#8C8C8C` | Subtitles, hints |
| Text Inverse | `#FFFFFF` | Text on primary |
| Border / Divider | `#E2E6EA` | Borders, dividers |
| Border Focused | `#09887F` | Focused inputs |
| Success | `#10B981` | Confirmed, paid |
| Warning | `#F59E0B` | Pending, due soon |
| Error | `#D94F3D` | Errors, cancelled |
| Info | `#0EA5E9` | Info banners |
| Gold | `#F2B90D` | Premium/VIP badges, ratings ⭐ |
| Scrim | `#80000000` | Modal overlays |

### 🌙 Dark Mode
| Token | Hex | Use |
|-------|-----|-----|
| Background | `#0B0B0D` | Screen background |
| Card/Surface | `#121418` | Cards |
| Elevated | `#15171B` | Raised surfaces |
| Input | `#17191E` | Text fields |
| Muted | `#101216` | Subtle sections |
| Primary | `#09887F` | Same teal (brand consistency) |
| Foreground | `#F6F6F7` | Main text |
| Muted Foreground | `#A1A8B3` | Secondary text |
| Border | `#22242A` | Borders |
| Success | `#22C55E` | Positive |
| Destructive | `#E85D75` | Errors |
| Accent | `#00C2FF` | Highlights |

**Rule**: Light और Dark दोनों में हर screen test करो — theme toggle Settings में है (`theme_provider`), preference persist होती है।

---

## ✍️ Typography (Source: `app_text_styles.dart` — Google Fonts)

| Role | Font | Size / Weight | Use |
|------|------|---------------|-----|
| Display | **Playfair Display** | 32px bold | Hero headings, splash |
| Headline | **Playfair Display** | 24–28px bold | Screen titles |
| Title | **Playfair Display** | 20–22px bold | Section headers, card titles |
| Body Large | **Lato** | 16px regular | Main content |
| Body | **Lato** | 14px regular | Standard text |
| Body Small | **Lato** | 12px regular | Captions, timestamps |
| Label / Button | **Lato** | 14–16px semibold | Buttons, CTAs |
| UI Label | **Inter** | 13px medium | Form labels, tabs |
| Badge | **Manrope** | 11px semibold | Chips, badges, counters |

**Rules**:
- Headings = Playfair Display (serif, elegance); Body/UI = Lato (readability)
- ₹ amounts के लिए tabular figures — `intl` से `NumberFormat.currency(locale: 'en_IN', symbol: '₹')` (e.g. ₹1,45,000)
- Line height ~1.4 body के लिए

---

## 📐 Spacing & Shape (Source: `app_spacing.dart`)

| Token | Value |
|-------|-------|
| xs / sm / md / lg / xl / xxl | 4 / 8 / 16 / 24 / 32 / 48 px |
| Screen horizontal padding | 16px |
| Card padding | 16px |
| Input padding | 16px horizontal, 14px vertical |
| Card border radius | 12–16px |
| Button border radius | 12px |
| Bottom sheet radius | 24px (top corners) |
| Chip/badge radius | full (pill) |
| Bottom nav elevation | 10 |
| Shadows | `app_shadows.dart` tokens — soft, low-opacity |

---

## 🧱 Common Widgets (widgets/common/ — हमेशा इन्हें ही use करो)

| Widget | File | Description |
|--------|------|-------------|
| **CustomButton** | custom_button.dart | Primary/secondary/outline/text variants; loading state built-in; full-width default |
| **CustomTextField** | custom_text_field.dart | Label + hint + validation error + prefix/suffix icons; focused border = primary teal |
| **OptimizedImage** | optimized_image.dart | cached_network_image wrapper — shimmer placeholder + error fallback; **हर network image इसी से** |
| **AppBadge** | app_badge.dart | Status chips (Confirmed=success, Pending=warning, Cancelled=error, VIP=gold) |
| **AppDialog** | app_dialog.dart | Standard confirm/alert dialogs |
| **AppBottomSheet** | app_bottom_sheet.dart | Rounded-top draggable sheets — forms इसी में खुलते हैं |
| **AppSectionHeader** | app_section_header.dart | Section title + "See All" action row |
| **AppSkeleton** | app_skeleton.dart | Shimmer loading placeholders (lists/cards) |
| **StateWidgets** | state_widgets.dart | LoadingView, EmptyState (illustration + message + CTA), ErrorState (retry button) |
| **PasswordStrengthIndicator** | password_strength_indicator.dart | Signup में live strength meter |
| **BackConfirmationDialog** | back_confirmation_dialog.dart | Unsaved changes / app exit confirmation |

---

## 🖼️ Iconography & Imagery

- **Icons**: Phosphor icons (primary set) + Material outlined icons (bottom nav)
- **Imagery**: Vendor photos rounded-corner cards में; profile photos circular; reels full-bleed
- **Placeholders**: photo न होने पर initials-avatar या category icon — कभी broken image नहीं

---

## 🎬 Motion (Source: `app_animations.dart`)

| Animation | Duration/Curve |
|-----------|----------------|
| Page transitions | 300ms, easeInOut |
| Bottom sheets | 250ms slide-up |
| Button press | scale 0.97, 100ms |
| Skeleton shimmer | 1.2s loop |
| Splash logo | fade+scale 800ms |
| Payment success | check-mark draw + confetti |

---

## 🧭 Component Patterns

1. **App Bars**: Surface color, no heavy elevation; title in Playfair Display; back = iOS-style chevron
2. **Cards**: Surface + 12–16px radius + soft shadow; tap ripple
3. **Lists**: Skeleton loading → content; empty → EmptyState widget; error → ErrorState + retry
4. **Forms**: CustomTextField + inline validation (validators.dart); submit = CustomButton with loading spinner; errors Hindi/English friendly messages (ErrorHandler से)
5. **Status colors हर जगह consistent**: pending=warning, confirmed/paid=success, cancelled/error=error, VIP/premium=gold
6. **Snackbars**: floating, rounded, status color accent
7. **Pull-to-refresh**: सभी main lists पर
8. **FABs**: primary teal, circular — Add Task, Add Expense, Create Event

---

## ♿ Accessibility & Responsiveness

- Touch targets ≥ 48×48
- Text contrast WCAG AA (दोनों themes)
- `MediaQuery.textScaler` respect करो — fixed-height text containers मना
- छोटी screens (360dp width) पर layout overflow न हो — scrollables use करो

---

## 🌐 Bilingual UI

- Strings `app_strings.dart` से; Hindi + English दोनों
- Devanagari के लिए भी Lato/Noto fallback ठीक rendering दे — long Hindi strings के लिए flexible widths रखो
