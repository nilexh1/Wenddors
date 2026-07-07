# FILE 1 — PROJECT OVERVIEW

> **AI Instruction**: यह file सबसे पहले पढ़ो। यह Wenddors User App का official overview है — पूरा project इसी vision के अनुसार बनाया गया है और आगे भी इसी के अनुसार develop होगा।

---

## 📱 Project Name

**Wenddors** — User App (Wedding Planner & Vendor Booking Platform)

- **Package Name**: `wenddors_user`
- **App Type**: B2C Mobile Application
- **Version**: 1.0.0+1

---

## 🎯 उद्देश्य (Purpose)

Wenddors एक complete wedding planning ecosystem है जो भारतीय शादियों की पूरी planning को एक ही app में manage करता है। यह app जोड़ों (couples) और उनके परिवारों को शादी की तैयारी के हर पहलू — vendor खोजना, book करना, budget manage करना, guest list बनाना, checklist track करना — के लिए एक single unified platform देता है।

Wenddors ecosystem में 2 apps हैं:
1. **User App** (यह project) — couples/families के लिए
2. **Vendor/Shopkeeper App** (अलग project) — vendors के लिए

दोनों apps एक ही Firebase project (`wenddors-72733`) share करते हैं।

---

## 🧩 Core Problem जो App Solve करती है

भारतीय शादियों में planning बिखरी हुई होती है — vendors WhatsApp पर, budget Excel में, guest list कागज़ पर, और bookings phone calls पर। इससे:

1. **Vendor Discovery कठिन है** — भरोसेमंद local vendors (photographer, caterer, decorator, पंडित, मेहंदी आदि) ढूंढना मुश्किल है
2. **Budget Control नहीं रहता** — कौन सी category में कितना खर्च हुआ, कितना advance दिया, कितना बाकी है — track नहीं होता
3. **Coordination Chaos** — परिवार के सदस्यों, vendors और ceremonies के बीच तालमेल नहीं बैठता
4. **Trust की कमी** — vendor की quality, rating, previous work देखने का कोई verified तरीका नहीं

**Wenddors का Solution**: Vendor marketplace (categories + search + reviews + reels) + Booking & Payment (Razorpay) + Event Management (ceremonies, guests, food menu, transport) + Budget Tracker + Checklist + In-app Chat — सब एक जगह।

---

## 👥 Target Audience

| Segment | Description |
|---------|-------------|
| **Primary** | 22–35 वर्ष के engaged couples (भारत, Tier 1–3 cities), जो अपनी शादी खुद plan कर रहे हैं |
| **Secondary** | Couples के परिवार के सदस्य (माता-पिता, भाई-बहन) जो event में members के रूप में जुड़ते हैं |
| **Tertiary** | Event planners जो multiple weddings manage करते हैं |
| **Language** | Hindi + English (bilingual UI support) |
| **Region** | India-first (₹ currency, Razorpay, Indian wedding ceremonies — Haldi, Mehndi, Sangeet, Phera आदि) |

---

## 📲 Platform

- **Primary**: Android (minSdk 23+) और iOS — दोनों
- **Framework**: Flutter (single codebase)
- **Secondary targets**: Web, Windows, macOS, Linux (Flutter scaffolding मौजूद है, लेकिन production focus Android/iOS पर है)

---

## 📝 App Summary (2 lines)

Wenddors एक Flutter-based wedding planning app है जिसमें users verified vendors खोजकर book कर सकते हैं, Razorpay से payment कर सकते हैं, और अपनी शादी के events, budget, guests, checklist व vendor chats — सब कुछ एक ही जगह manage कर सकते हैं। Firebase backend + Riverpod state management + GoRouter navigation पर बनी यह app Instagram-style Reels के ज़रिए vendors का काम showcase भी करती है।

---

## 🏆 Success Metrics (App किन KPIs पर मापी जाती है)

1. Vendor booking conversion rate (browse → book)
2. Budget tracker adoption (active events with budget set)
3. Chat response engagement (user ↔ vendor)
4. Reels watch-time और vendor follow rate
5. Checklist completion rate (planning engagement)

---

## 📁 Related Audit Files (पढ़ने का क्रम)

1. `project_overview.md` ← (यह file)
2. `current_progress_and_tasks.md` ← हर session की शुरुआत में
3. `features_list.md`
4. `screens_and_navigation.md`
5. `data_models.md`
6. `api_and_backend.md`
7. `tech_stack.md`
8. `folder_structure.md`
9. `ui_and_design_guidelines.md`
10. `business_logic_and_rules.md`
