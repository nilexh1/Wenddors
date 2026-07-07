# FILE 9 — BUSINESS LOGIC & RULES

> **AI Instruction**: यह app के सभी business rules की official rulebook है। कोई भी feature code करते समय ये rules **अनिवार्य रूप से** enforce होने चाहिए — UI validation + provider logic + (जहाँ संभव हो) Firestore security rules, तीनों layers पर। यह file AI को गलत logic लिखने से बचाती है।

---

## 🔐 A. Authentication Rules

1. **A1**: Password minimum 8 characters — कम-से-कम 1 uppercase, 1 number (validators.dart enforce करता है; strength indicator दिखेगा)।
2. **A2**: Signup तभी complete जब Terms checkbox ✔ हो।
3. **A3**: Google sign-in से नया user आए तो Firestore `users/{uid}` doc **auto-create** होगा — बिना user doc के app के अंदर कोई operation नहीं।
4. **A4**: OTP resend में 30-second cooldown timer; OTP 6-digit।
5. **A5**: Logout पर local caches (Hive user-scoped boxes) clear होंगे — दूसरे user का data leak न हो।
6. **A6**: Account delete = Firebase Auth delete + `users/{uid}` doc delete; delete से पहले re-authentication और double confirmation dialog अनिवार्य।
7. **A7**: Unauthenticated user कोई भी protected route नहीं खोल सकता — GoRouter redirect हमेशा `/login` पर।

## 💰 B. Booking & Payment Rules

8. **B1**: Booking create करने के लिए **event date आज से future** में होनी चाहिए — past date पर booking form submit ही नहीं होगा।
9. **B2**: Advance amount = total का **minimum 20%, maximum 100%** — इस range के बाहर checkout नहीं।
10. **B3**: Payment amount **कभी client पर trust नहीं** — Razorpay order Cloud Function बनाता है और signature verification server-side होता है; verification fail = booking में paidAmount update नहीं।
11. **B4**: `paymentStatus` auto-computed है, manually set नहीं होता:
    - `paidAmount == 0` → `pending`
    - `0 < paidAmount < totalAmount` → `partial`
    - `paidAmount >= totalAmount` → `paid`
12. **B5**: `remainingAmount = totalAmount - paidAmount` — हर payment के बाद transaction में recompute; negative कभी नहीं।
13. **B6**: Booking cancel करने के rules:
    - `pending` booking → user कभी भी cancel कर सकता है
    - `confirmed` booking → event date से **7+ दिन पहले** ही cancel हो सकती है
    - `completed` booking → कभी cancel नहीं
    - Advance refund policy: 7+ दिन पहले cancel = full advance refund eligible; उसके बाद = no refund (refund Cloud Function से process होता है, status → `refunded`)
14. **B7**: एक user एक vendor को **एक ही event date** पर duplicate booking नहीं कर सकता — create से पहले existing pending/confirmed booking check।
15. **B8**: Payment in-progress के दौरान checkout screen से back = confirmation dialog; payment result आए बिना screen dispose न हो।
16. **B9**: Payment success के बाद ही Payment Success screen दिखेगा; failure पर checkout पर ही error + retry।
17. **B10**: Review सिर्फ वही user दे सकता है जिसकी उस vendor के साथ `completed` (या `confirmed`) booking हो — एक booking पर **एक ही review**।

## 📅 C. Event & Ceremony Rules

18. **C1**: Event का `weddingDate` create के समय future में होनी चाहिए।
19. **C2**: Event `status` date से auto-derive होगा: weddingDate future = `upcoming/active`; बीत जाने पर = `completed`।
20. **C3**: Ceremony की dateTime event की weddingDate window (±30 दिन) के अंदर होनी चाहिए।
21. **C4**: Event delete सिर्फ **owner** कर सकता है — members नहीं; delete से पहले confirmation + सभी sub-collections (guests, ceremonies, bookings…) cascade delete।
22. **C5**: Members event को **view + edit** कर सकते हैं लेकिन delete नहीं; member invite सिर्फ owner करता है।
23. **C6**: Event के aggregate counters (`membersCount`, `vendorsCount`, `confirmedGuestCount`, `totalCeremonies`) sub-collection writes के साथ transaction/increment से sync रहेंगे — कभी manually set नहीं।

## 🧾 D. Budget Rules

24. **D1**: `spentAmount` कभी manually edit नहीं — हमेशा expenses के sum से compute।
25. **D2**: Category में expense add करते समय अगर spend allocated budget से ऊपर जाए → warning दिखाओ (लेकिन block मत करो — real weddings में overspend होता है); category card पर red/over-budget indicator।
26. **D3**: Expense amount > 0 अनिवार्य; negative amounts मना।
27. **D4**: Budget currency हमेशा ₹ INR, Indian format (`₹1,45,000`)।
28. **D5**: Booking payment होने पर संबंधित budget category का spentBudget auto-update होगा (double-entry मत करो — payment एक ही जगह count हो)।

## ✅ E. Checklist Rules

29. **E1**: Task complete करने पर `completedAt` timestamp set होगा; uncheck पर null।
30. **E2**: `dueDate < आज` और `!isCompleted` → task **Overdue** है (red badge); overdue tasks sort में सबसे ऊपर।
31. **E3**: Task priority: `high` | `medium` | `low` — high priority tasks पर reminder notification due date से 3 दिन पहले।
32. **E4**: Checklist progress % = completed/total tasks — circular ring यही दिखाएगी।

## 👥 F. Guest Rules

33. **F1**: Guest का `name` अनिवार्य; phone optional लेकिन दिया तो valid 10-digit Indian format।
34. **F2**: `rsvpStatus` सिर्फ `yes | no | pending` — कोई और value नहीं (legacy `status` field मत use करो)।
35. **F3**: `confirmedGuestCount` (event पर) = sum of guestCount जहाँ rsvpStatus == 'yes'।
36. **F4**: Duplicate guest (same phone, same event) add करने पर warning।

## 💬 G. Chat Rules

37. **G1**: `chatId` **deterministic** है: `{userId}_{vendorId}` — नई conversation से पहले existing check करो, duplicate chat docs कभी न बनें।
38. **G2**: Blocked vendor के साथ chat नहीं — block करने पर conversation hide + नए messages नहीं जा सकते।
39. **G3**: Message edit/delete सिर्फ **sender** कर सकता है; "delete for everyone" 1 घंटे के अंदर, उसके बाद सिर्फ "delete for me" (`deletedFor` array)।
40. **G4**: Unread counts: message भेजने पर receiver का unreadCount++; chat खोलने पर अपना count = 0 (batch write)।
41. **G5**: Media upload से पहले compression अनिवार्य; image max ~1MB, video max 50MB; upload progress दिखेगा।
42. **G6**: Quotation message से "Book Now" tap → उसी price के साथ booking form pre-fill।

## 🎬 H. Reels Rules

43. **H1**: Like/Save toggle idempotent है — `reelInteractions/{userId_reelId}` doc + counter increment transaction में साथ-साथ; double-tap से double-count कभी नहीं।
44. **H2**: View count तब बढ़ेगा जब reel **3+ seconds** देखी जाए — per user per session एक बार।
45. **H3**: एक समय पर सिर्फ **एक** video player play — page change पर previous dispose (memory leak मना)।
46. **H4**: Blocked vendors की reels feed में **नहीं** दिखेंगी।
47. **H5**: Reels feed paginated है (10 per page) — पूरा collection एक साथ कभी मत खींचो।

## 🔔 I. Notification Rules

48. **I1**: User की notification preferences respect करो — disabled type की in-app entry बनेगी लेकिन push नहीं जाएगा।
49. **I2**: Notification tap = `type` + `relatedId` से deep-link (booking → booking detail, chat → chat screen…)।
50. **I3**: Payment due reminders: `nextPaymentDueDate` से 3 दिन पहले और due date पर।

## 📶 J. Offline & Sync Rules

51. **J1**: Offline में reads cache (Firestore persistence + Hive) से serve होंगे — blank screen कभी नहीं, stale-data indicator ठीक है।
52. **J2**: Offline writes queue होंगे (OfflineSyncService); connectivity लौटने पर auto-sync।
53. **J3**: **Payments offline कभी नहीं** — checkout में network check अनिवार्य; offline पर Pay button disabled + message।
54. **J4**: Sync conflicts में **server data wins** (last-write-wins), सिवाय user के local unsent chat messages के।

## 🛡️ K. Security & Data Rules

55. **K1**: Razorpay secret, Cloudinary API secret, service-account keys — **कभी app code/repo में नहीं**; unsigned presets और Cloud Functions ही use होंगे।
56. **K2**: हर Firestore write में `userId` = `request.auth.uid` — user दूसरे का data कभी modify नहीं कर सकता (rules-level enforce)।
57. **K3**: सभी user inputs validate + trim; Firestore में raw HTML/script save नहीं।
58. **K4**: Images upload से पहले compress (bandwidth + storage cost)।
59. **K5**: PII (phone/email) सिर्फ ज़रूरी जगह दिखे; logs में PII कभी print नहीं (AppLogger policy)।
60. **K6**: Seeders (`seed_service`, `category_seeder`, `event_seeder`) सिर्फ debug builds में चलें — `kDebugMode` guard अनिवार्य।

---

## ⚖️ Conflict Resolution Priority

अगर दो rules टकराएँ: **Security (K) > Payment (B) > Data integrity (C/D/F) > UX**
