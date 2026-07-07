# FILE 2 — FEATURES LIST

> **AI Instruction**: यह Wenddors User App की complete features list है। MVP features app का core हैं; Future features roadmap में हैं। हर नया code change इस list के अनुसार होना चाहिए।

---

## ✅ MVP FEATURES (Core — App इनके बिना incomplete है)

### A. Authentication & User Management

1. **Email/Password Sign Up & Login** — Firebase Auth से email-password registration और login; password strength indicator के साथ।
2. **Google Sign-In** — one-tap Google login (google_sign_in package); पहली बार login पर Firestore में user profile auto-create होती है।
3. **Phone OTP Login** — Firebase Phone Auth से mobile number + OTP verification (OTP verification screen मौजूद है)।
4. **Forgot / Reset Password** — email link से password reset का पूरा flow।
5. **Terms & Conditions Screen** — signup से पहले terms acceptance।
6. **User Profile Management** — name, phone, profile photo, couple photo, wedding date, estimated budget, guest count edit करना (Personal Info screen)।
7. **Profile Photo Upload** — camera/gallery से photo pick कर compress करके Cloudinary पर upload।
8. **Session Management** — auth state persist रहती है; splash screen पर auth check होकर auto-redirect।
9. **Account Security** — password change, email update, account delete (Security screen)।

### B. Vendor Discovery & Marketplace

10. **Home Screen Discovery** — personalized greeting, search bar, category grid, featured vendors, banners/ads।
11. **Vendor Categories** — 6 main categories (Services, Fashion, Food, Decoration, Music, Shops) और 40+ sub-categories (Photographer, Caterer, Mehndi, Pandit, DJ, Jewellery आदि)।
12. **Category-wise Vendor Listing** — किसी category के सभी vendors की list, rating/price के साथ।
13. **Vendor Search** — नाम, category, location से vendors search करना (dedicated Search screen)।
14. **Vendor Detail Page** — hero header, tabs (Info, Services, Gallery, Reviews), base price, rating, follower count, verified badge।
15. **Vendor Gallery & Lightbox** — vendor की photos/videos full-screen lightbox में देखना।
16. **Reviews & Ratings** — vendor पर 1–5 star review + title + content + photos; helpful votes; Add Review dialog।
17. **Favorite / Save Vendors** — vendors को save-list में add करना (Profile → Saved Vendors)।
18. **Follow Vendors** — vendor को follow करना ताकि उनकी नई reels/updates दिखें (Profile → Followed Vendors)।
19. **Block Vendors** — unwanted vendors को block करना (Profile → Blocked Vendors)।
20. **Category Ads/Banners** — categories में sponsored vendor banners (category_ads collection)।

### C. Booking & Payments

21. **Vendor Booking** — vendor detail से booking form sheet: event date, service details, quoted amount।
22. **Checkout & Razorpay Payment** — advance payment Razorpay gateway से (UPI, cards, netbanking, wallets)।
23. **Payment Success Screen** — payment confirm होने पर receipt-style success screen।
24. **My Bookings** — user की सभी bookings status-wise (pending/confirmed/completed/cancelled) list।
25. **Booking Detail** — booking की पूरी जानकारी: total, advance, paid, remaining, installments, next due date।
26. **Booking Cancellation** — rules के अनुसार booking cancel करना (देखें business_logic_and_rules.md)।
27. **Vendor Confirmation Link** — booking confirm कराने के लिए vendor को WhatsApp पर tokenized confirmation link भेजना (VendorConfirmationService)।

### D. Event Management (शादी के functions)

28. **Create Event** — शादी का event बनाना: title, wedding date, location, region, cover image।
29. **Event List & Detail** — सभी events (active/upcoming/completed) और हर event का dashboard।
30. **Ceremonies / Schedule** — event के अंदर ceremonies (Haldi, Mehndi, Sangeet, Phera आदि) date-time, venue, dress code के साथ।
31. **Event Guest List** — event-wise guests: RSVP status (yes/no/pending), category, relation, VIP flag, meal preference।
32. **Food Menu Management** — ceremony-wise meal planning: breakfast/lunch/high-tea/dinner, caterer, dish categories।
33. **Transport Management** — guests के आने-जाने और hotel rooms (check-in/check-out, rate) का प्रबंधन।
34. **Event Vendor Management** — event/ceremony से vendors जोड़ना, negotiation history, quoted vs paid amount।
35. **Manage Members** — परिवार के सदस्यों को event में invite करना (member invite sheet) ताकि वे planning में collaborate करें।
36. **Event Budget Tracker** — event-level budget: category-wise allocation vs spend।

### E. Planning Tools

37. **Budget Overview** — total budget, spent, remaining, progress bar; category-wise budget breakdown।
38. **Category Budget Detail** — किसी एक category (जैसे Photography) का detailed खर्च और expenses list।
39. **Add Expense** — bottom sheet से नया expense entry (amount, category, note)।
40. **Wedding Checklist** — timeline-based tasks (6+ months, 3–6 months, 1–3 months pहले), priority, due date, completion tracking।
41. **Add/Edit Task** — checklist में custom task जोड़ना/edit करना, reminder के साथ।

### F. Communication

42. **In-App Chat (User ↔ Vendor)** — real-time messaging Firestore streams से; chat list + chat detail।
43. **Rich Chat Messages** — text, image, video, PDF, location, voice note, quotation (price + description), booking link।
44. **Chat Features** — reply, reactions (❤️👍), pin messages, edit, delete-for-me/everyone, typing indicator, read receipts (sent/delivered/read)।
45. **Chat Media Viewer** — chat की photos/videos full-screen देखना।
46. **Push Notifications** — FCM से booking updates, payment confirmations, chat messages, reminders।
47. **In-App Notifications Screen** — सभी notifications की list, read/unread, priority, type-wise icons।
48. **Notification Preferences** — user किस type की notifications चाहता है, toggle से control।

### G. Reels (Vendor Showcase)

49. **Reels Feed** — Instagram-style vertical video feed जिसमें vendors अपना काम दिखाते हैं; auto-play, progress bar, audio pill।
50. **Reel Interactions** — like, comment (comment sheet), share (share sheet + WhatsApp), save, view count।
51. **Saved Reels** — user के saved reels की grid (Profile → Saved Reels)।
52. **Reel → Vendor Navigation** — reel से सीधे vendor profile पर जाना।

### H. App Infrastructure

53. **Light/Dark Theme** — दोनों themes, user toggle (theme_provider), preference persist होती है।
54. **Offline Support** — Hive caching + connectivity detection; No Internet screen; background sync service जो online आते ही pending writes sync करता है।
55. **Language Selection** — Hindi/English language screen (flutter_localizations)।
56. **Help Center** — FAQs और support contact।
57. **Splash Screen** — animated logo + auth check + auto-routing।
58. **Error Screen** — global error handling के लिए dedicated error screen।

---

## 🚀 FUTURE FEATURES (Roadmap — अभी build नहीं करने, planning में हैं)

59. **E-Invites / Digital Invitation Cards** — customizable wedding invitation cards बनाकर WhatsApp पर share करना (invite screen scaffold मौजूद है)।
60. **Guest RSVP Links** — guests को link भेजकर वे खुद RSVP कर सकें।
61. **AI Budget Suggestions** — market rates के आधार पर category-wise budget recommendation (MarketRateService की foundation मौजूद है)।
62. **Vendor Comparison** — 2–3 vendors को side-by-side compare करना (price, rating, services)।
63. **EMI / Installment Payments** — booking amount installments में देना (installmentCount field model में मौजूद है)।
64. **Post-Event Features** — event के बाद photos share, vendor reviews prompt, memories album।
65. **Live Order/Booking Tracking** — vendor की service delivery का live status।
66. **Seating Arrangement Planner** — reception की table-wise seating chart।
67. **Referral & Rewards** — दोस्तों को invite करने पर rewards।
68. **Multi-language Expansion** — मराठी, गुजराती, तमिल आदि।
69. **In-App Video Call with Vendor** — chat से video consultation।
70. **Web Dashboard** — बड़ी screen पर planning के लिए responsive web version।
71. **Guest Side (Bride/Groom) Tagging** — Guests को लड़के या लड़की वालों के रूप में categorize करना।
72. **PDF Exports** — Budget, Guest List, और Itinerary को PDF में export/share करना।
73. **Bulk RSVP Management** — एक साथ कई guests का RSVP status update करना या WhatsApp broadcast भेजना।

---

## 🔑 Feature Priority (अगर conflict हो तो यह क्रम)

`Auth > Booking/Payment > Vendor Discovery > Chat > Events > Budget > Checklist > Guests > Reels > बाकी`
