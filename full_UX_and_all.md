पूरे app का नेविगेशन ढांचा


नीचे 5 tab वाला bottom navigation है जो हमेशा दिखता रहेगा (Home, Events, Chat, Reels, Profile)। हर tab की अपनी अलग history रहेगी, यानी tab बदलकर वापस आओ तो वहीं मिलेगा जहां छोड़ा था। कुछ screens (Chat की बातचीत, Checkout, Payment) full-screen खुलेंगी ताकि नीचे की tab bar छुप जाए और ध्यान न भटके। Login न होने पर हर protected screen अपने-आप Login पर भेज देगी।



1. शुरुआत और Login वाली screens


Splash (सबसे पहली screen)
बीच में app का logo (हल्का animation के साथ)।
Background में चुपचाप check होता है कि user पहले से logged in है या नहीं।
कोई button नहीं। 2 सेकंड बाद अपने-आप या तो Home खुलेगी (logged in), या Login (नया/logged out user)।


Login
ऊपर स्वागत वाला छोटा heading।
Email का field, नीचे Password का field (आंख वाला icon जिससे password दिख/छुप सके)।
नीचे बड़ा Login button।
उसके नीचे "Password भूल गए?" link (दाईं तरफ)।
बीच में एक divider "या" के साथ।
Google से Login button और Phone number से Login button।
सबसे नीचे "नया account? Sign Up करें" link।


Sign Up
First name, Last name, Email, Phone, Password के fields क्रम में।
Password field के ठीक नीचे live strength बताने वाली पट्टी (कमज़ोर/ठीक/मज़बूत)।
नीचे Terms & Conditions पढ़ने का checkbox (जब तक tick न हो, Sign Up button दबेगा नहीं)।
सबसे नीचे बड़ा Account बनाएं button।
नीचे "पहले से account है? Login करें" link।


Forgot Password
सिर्फ एक Email field और नीचे Reset link भेजें button।
ऊपर छोटा सा समझाने वाला text कि email पर link आएगा।


OTP Verification
ऊपर बताया जाए किस number पर OTP गया।
6 अलग-अलग digit box (टाइप करते ही अगले box पर cursor जाए)।
नीचे Resend OTP link जो 30 सेकंड के timer के बाद ही active हो।
OTP पूरा भरते ही अपने-आप verify होने की कोशिश हो।


Reset Password
नया Password और Confirm Password के दो field (दोनों में strength/match दिखे)।
नीचे Password बदलें button।


Terms & Conditions
पूरा scroll होने वाला text।
ऊपर back का chevron।


Onboarding (नया user पहली बार)
3-4 slide का flow: पहली slide में शादी की date चुनना (calendar), दूसरी में अनुमानित budget, तीसरी में पहला Event बनाना।
हर slide पर आगे/पीछे और "छोड़ें (Skip)" का option ऊपर।
आखिरी slide पर शुरू करें button जो सीधे Home पर ले जाए।


2. Home Tab (पहला tab)


Home
सबसे ऊपर personalized greeting ("नमस्ते Nilesh") बाईं तरफ, दाईं तरफ notification की घंटी (unread होने पर लाल dot)।
नीचे search bar (tap करते ही Search screen खुले)।
उसके नीचे promotional banners का slider।
फिर 6 मुख्य category का grid (Services, Fashion, Food, Decoration, Music, Shops) - हर icon tap पर उस category के vendors।
नीचे "Featured/Verified Vendors" का horizontal scroll carousel, हर card के कोने पर verified badge।
फिर "आपके पास के vendors" (location से) की list।
सबसे नीचे tips/blog section।
पूरी screen pull-to-refresh वाली।


Vendor Detail
ऊपर बड़ी hero image (photos हों तो swipe/tap पर full-screen lightbox खुले)।
नीचे shop का नाम + verified badge, rating के stars + reviews की गिनती, follower count।
ठीक नीचे 4 tab: Info (description, contact, location/map), Services (price list), Gallery (photos/videos), Reviews।
ऊपर की तरफ (image पर) Save/favorite का दिल, Follow button, और तीन-dot menu में Block/Share।
सबसे नीचे हमेशा दिखने वाली sticky पट्टी: बाईं तरफ Chat करें, दाईं तरफ Book Now।


Photo Lightbox
Full-screen image, बाएं-दाएं swipe, pinch से zoom, ऊपर बंद करने का cross।


Category Vendors (किसी एक category के vendors)
ऊपर category का नाम + back।
ऊपर filter (price range, rating) और sort (rating/price/nearby) के chip।
नीचे vendor cards की list (photo, नाम, rating, base price, verified badge)।


Vendor List (सारे vendors)
ऊपर search bar + category के chips।
नीचे सभी vendors की list।


Search
ऊपर active search bar (cursor तैयार)।
नीचे "हाल की searches" की list।
टाइप करते ही नीचे results (vendors) आते जाएं।


Vendor Reviews (पूरी list)
सारे reviews (stars, title, text, photos, "helpful" का count)।
ऊपर या नीचे Review जोड़ें का button/dialog - सिर्फ उस user को दिखे जिसकी उस vendor के साथ पूरी हो चुकी booking हो।


3. Events Tab (दूसरा tab) - शादी के functions


Events List
User के सभी events के बड़े cards (cover image, date, location, तैयारी का progress bar)।
नीचे दाईं तरफ round + (Create Event) button।
कोई event न हो तो खाली-state में समझाने वाला message + "पहला event बनाएं" button।


Create Event
Form: Title, Wedding date (calendar picker), Location, Region, Cover image upload।
सबसे नीचे Event बनाएं button।
Wedding date आज से आगे की ही चुनने दे।


Event Detail (event का dashboard)
ऊपर cover image + शादी का countdown (कितने दिन बचे)।
नीचे quick stats के छोटे cards: Guests, Vendors, Budget, Ceremonies।
फिर section की tiles जो अंदर ले जाएं: Guests, Schedule, Menu, Transport, Vendors, Members, Budget।
ऊपर तीन-dot menu में Edit/Delete (Delete सिर्फ owner को)।


Event Schedule (ceremonies)
Haldi, Mehndi, Sangeet, Phera आदि की timeline (क्रम में date-time के साथ)।
हर ceremony पर venue, dress code, expected guests।
नीचे + Ceremony जोड़ें, और हर ceremony पर edit।


Event Guest List
ऊपर RSVP filter के chip (सभी/हां/नहीं/pending)।
नीचे guests की list (नाम, relation, VIP badge, कितने लोग आएंगे, meal preference)।
नीचे + Guest जोड़ें button, हर guest tap पर edit dialog।
एक ही phone दुबारा जोड़ने पर warning।


Food Menu
ऊपर meal-type के tab: Breakfast / Lunch / High-Tea / Dinner।
हर tab में dish की categories (Beverages, Mains आदि) और caterer की जानकारी।
नीचे dish/caterer जोड़ने का option।


Transport
दो हिस्से: Hotel rooms (hotel नाम, room, check-in/out, rate, कौन रुकेगा) और transport arrangements।
नीचे room/transport जोड़ने का button।


Event Vendors
Ceremony के हिसाब से booked vendors की list।
हर vendor पर status (inquiry/negotiating/booked/confirmed आदि), quoted vs paid amount, negotiation history।
नीचे + Vendor जोड़ें।


Manage Members
परिवार के जुड़े members की list।
नीचे Member invite करें (phone/link से) - सिर्फ owner को दिखे।


Event Budget
इस event का category-wise allocated vs spent tracker (progress के साथ)।
नीचे expense जोड़ने का option।


4. Chat Tab (तीसरा tab)


Chat List
सारी बातचीत की list: vendor की photo, last message का preview, unread की गिनती वाला badge, समय।
ऊपर छोटा search।
कोई chat न हो तो खाली-state message।


Chat Detail (full-screen, नीचे की tab bar छुपी रहे)
ऊपर vendor का नाम + photo + tap पर उसकी profile।
बीच में message bubbles (text, image, video, PDF, location, voice note, quotation, booking-link)।
किसी message पर reply, reaction (❤️👍), pin, edit/delete का option (edit/delete सिर्फ अपने message पर)।
ऊपर pinned message की पट्टी, नीचे लिखते वक्त typing indicator।
सबसे नीचे input bar: बाईं तरफ attachment (photo/video/PDF/location/voice), बीच में text box, दाईं तरफ camera और mic, भरने पर send।
कई message एक साथ चुनने पर ऊपर selection वाला bar (delete/forward)।
Quotation message पर "Book Now" दबाने से उसी price के साथ booking form pre-fill हो।


Chat Media Viewer
Chat की photo/video full-screen।


5. Reels Tab (चौथा tab)


Reels Feed
Full-screen vertical video, ऊपर-नीचे swipe से अगली reel।
ऊपर vendor का avatar + नाम + Follow button।
नीचे caption और audio pill।
दाईं तरफ लंबवत action column: Like, Comment, Share, Save (हर पर count)।
ऊपर पतली progress पट्टी।
Vendor avatar tap पर सीधे Vendor Detail।


Comment Sheet (नीचे से खुलने वाली sheet)
Reel के सारे comments, हर पर like।
सबसे नीचे comment लिखने का box।


Share Sheet (नीचे से)
WhatsApp और बाकी apps में share करने के option।


6. Profile Tab (पांचवां tab)


Profile
ऊपर profile photo + couple photo, नाम, शादी का countdown।
नीचे stats: कितनी bookings, saved, following।
फिर menu items की list जो अंदर ले जाए: Personal Info, Settings, Notification Preferences, Payment Methods, Saved Vendors, Followed Vendors, Blocked Vendors, Saved Reels, Security, Help Center, Language।


Personal Info
Name, phone, wedding date, budget, guest count edit करने के fields।
ऊपर profile photo पर tap → camera/gallery चुनने की sheet।
नीचे Save button।


Settings
Theme toggle (Light/Dark), Language, Notifications का shortcut।
नीचे Logout, और सबसे नीचे (हल्के में) Account Delete।


Notification Preferences
हर type (Booking, Payment, Chat, Reminder, Review, Promo) के आगे on/off toggle।


Payment Methods
Saved payment methods और पुराने transactions की history।


Saved Vendors / Followed Vendors / Blocked Vendors
तीनों अलग list; Blocked में हर के आगे Unblock; Saved/Followed में tap पर Vendor Detail।


Saved Reels
Save की हुई reels का grid, tap पर full-screen।


Security
Password बदलना, email update, और double-confirmation के साथ account delete।


Help Center
FAQs की list + support से संपर्क का option।


Language
Hindi / English का चुनाव (radio जैसा)।


7. Booking और Payment (full-screen, tab के ऊपर)


Booking Form (vendor detail से खुलने वाली sheet)
Event चुनना, event date, service details, quoted amount।
नीचे आगे बढ़ें / Checkout button।
Past date पर submit न हो; उसी vendor को उसी date पर दुबारा book करने से रोके।


My Bookings
ऊपर status के tab: All / Pending / Confirmed / Completed / Cancelled।
नीचे booking cards (vendor, date, amount, payment status)।


Booking Detail
Vendor info, event date, रकम का breakdown (total/advance/paid/remaining), payment status, installments, अगली due date।
नीचे action buttons: Pay, Cancel (rules के हिसाब से enable), Chat।


Checkout (full-screen)
Booking summary + amount + advance।
नीचे बड़ा Pay करें button (Razorpay)।
Payment चलते वक्त back दबाने पर confirmation।
Offline होने पर Pay button बंद + message।


Payment Success
ऊपर हरा check animation, payment की जानकारी।
नीचे Booking देखें button।


Finalized Vendors
Confirm हो चुके vendors की summary list।


8. Planning Tools


Budget Overview
ऊपर Total / Spent / Remaining के cards + progress।
नीचे category-wise list (over-budget होने पर लाल indicator)।
नीचे + Expense जोड़ें button।
ऊपर event चुनने का indicator (किस event का budget)।


Category Budget
एक category का detail: सारे expenses की list।
नीचे expense जोड़ने की sheet (amount, note - amount 0 से ज़्यादा ज़रूरी)।


Checklist
ऊपर circular progress ring (कितना पूरा हुआ)।
नीचे filter chips: All / This Week / Overdue / Done।
Timeline के हिसाब से task cards (Overdue वाले ऊपर, लाल badge)।
नीचे + Task जोड़ें button।


Checklist Detail
Task का पूरा detail: title, notes, priority, due date।
ऊपर edit, बीच में complete/uncheck toggle।


Guest List (Global - सभी events के)
सभी events के combined guests, ऊपर event चुनने का indicator।


Guest Detail
Guest की पूरी जानकारी + edit dialog।


9. Global / Utility screens


Notifications
सारी notifications की list, type के icon, read/unread अलग दिखे।
किसी पर tap → सीधे related screen (booking → booking detail, chat → chat आदि)।


No Internet
ऊपर से overlay में offline का message + Retry button।
पुराना cached data पीछे दिखता रहे, screen कभी खाली न हो।


Error
दोस्ताना error message + Home पर जाएं button।


💡 UX के कुछ ज़रूरी नियम (जो पूरे app को असली professional feel देंगे)


हर list में तीन हाल: loading पर shimmer/skeleton, खाली पर समझाने वाला empty-state + एक CTA button, error पर retry।
Status के रंग हर जगह एक जैसे मतलब रखें: pending = warning, confirmed/paid = success, cancelled = error, VIP/premium = gold।
सभी main lists में pull-to-refresh।
Add Task, Add Expense, Create Event जैसे मुख्य actions के लिए round floating (+) button नीचे दाईं तरफ।
Tab के अंदर back = एक step पीछे; tab की जड़ पर back = "app बंद करें?" confirmation।
Notification या link से tap करने पर सीधे उसी item की detail खुले (deep-link)।
कोई भी destructive action (delete, cancel, logout) से पहले confirmation dialog।
सारे touch targets उंगली-friendly, Hindi/English दोनों text ठीक फिट हों।
