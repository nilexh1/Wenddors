# FILE 4 — DATA MODELS

> **AI Instruction**: यह app के सभी data models का official reference है। सभी models `lib/src/models/` में हैं और `index.dart` से export होते हैं। हर model में `fromFirestore()` / `toFirestore()` (या `fromMap`/`toMap`) serialization methods हैं। नया field जोड़ते समय दोनों methods + `copyWith()` update करना अनिवार्य है।

---

## 📦 Models Overview (17 models)

| Model | File | Firestore Collection |
|-------|------|---------------------|
| UserModel | user_model.dart | `users` |
| VendorModel | vendor_model.dart | `vendors` |
| BookingModel | booking_model.dart | `bookings` |
| EventModel | event_model.dart | `events` |
| CeremonyModel | ceremony_model.dart | `events/{id}/ceremonies` |
| GuestModel | guest_model.dart | `events/{id}/guests` |
| EventVendorBookingModel | event_vendor_booking_model.dart | `events/{id}/vendorBookings` |
| FoodMenuModel | food_menu_model.dart | `events/{id}/foodMenus` |
| MenuItemModel | menu_item_model.dart | `events/{id}/menuItems` |
| HotelRoomModel | hotel_room_model.dart | `events/{id}/hotelRooms` |
| BudgetModel + BudgetCategory | budget_model.dart | `budgets` |
| ChecklistModel + ChecklistTask | checklist_model.dart | `checklists` |
| ChatConversation + ChatMessage | chat_model.dart | `chats`, `chats/{id}/messages` |
| ReelModel + ReelCommentModel | reel_model.dart | `reels`, `reels/{id}/comments` |
| ReviewModel | review_model.dart | `reviews` |
| NotificationModel | notification_model.dart | `users/{id}/notifications` |
| CategoryModel | category_model.dart | `categories` |

---

## 1. UserModel

```
uid: String                       // Firebase Auth UID (document ID)
email: String
phone: String?
firstName: String
lastName: String
profileImageUrl: String?          // Cloudinary URL
couplePhotoUrl: String?           // couple की photo
weddingDate: DateTime?
estimatedBudget: double?
guestCount: int?
serviceCategories: List<String>?  // interested categories
weddingDetails: Map<String,dynamic>?  // flexible extra details
createdAt: DateTime
updatedAt: DateTime
```

## 2. VendorModel

```
vendorId: String                  // document ID
userId: String                    // vendor app के auth user का UID
shopName: String
category: String                  // sub-category जैसे 'Photographer'
mainCategory: String?             // 'services' | 'fashion' | 'food' | 'decoration' | 'music' | 'shops'
description: String
imageUrl: String                  // profile/logo
bannerImageUrl: String
galleryImageUrls: List<String>
videoUrls: List<String>
basePrice: double
rating: double                    // aggregate (0–5)
reviewCount: int
followerCount: int
isVerified: bool                  // admin-verified badge
minimumBudget: double?
maximumBudget: double?
serviceDetails: Map<String,dynamic>?   // services + prices
bankDetails: Map<String,dynamic>?      // payouts के लिए (vendor app manages)
createdAt / updatedAt: DateTime
```

## 3. BookingModel (Marketplace booking — payment वाली)

```
bookingId: String
userId: String                    → UserModel.uid
eventId: String                   // Which event this booking is for
vendorId: String                  → VendorModel.vendorId
vendorName: String                // denormalized (fast list rendering)
eventDate: DateTime
totalAmount: double
advanceAmount: double
remainingAmount: double
paidAmount: double
paymentStatus: String             // 'pending' | 'partial' | 'paid' | 'refunded'
bookingStatus: String             // 'pending' | 'confirmed' | 'completed' | 'cancelled'
nextPaymentDueDate: DateTime?
installmentCount: int
createdAt / updatedAt: DateTime
```

## 4. EventModel (शादी का event)

```
eventId: String
userId: String?                   // owner
title: String
weddingDate: DateTime
location: String
region: String?
imageUrl / coverImageUrl: String?
status: String                    // 'active' | 'upcoming' | 'completed'
membersCount: int                 // collaborators
vendorsCount: int
activeChats: int
totalBudget: double
paidAmount: double
totalCeremonies: int
confirmedGuestCount: int
budgetByCategory: Map<String,double>?
ownerName / ownerPhone: String?
createdAt / updatedAt: DateTime?
```

## 5. CeremonyModel (event की sub-collection)

```
ceremonyId, eventId: String
name: String                      // 'Haldi', 'Mehndi', 'Sangeet', 'Phera'...
dateTime: DateTime
venueName, venueAddress: String
dresscode: String?
themeColor: String?
expectedGuestCount: int
vendorBookingIds: List<String>    → EventVendorBooking IDs
notes: String?
```

## 6. GuestModel

```
guestId, eventId: String
name: String
phone / email: String?
rsvpStatus: String                // 'yes' | 'no' | 'pending'
status: String                    // legacy — rsvpStatus use करो
category: String?                 // 'Family', 'Friends', 'Colleagues'
relation: String?                 // 'मामा', 'दोस्त' आदि
guestCount: int                   // साथ आने वाले लोग (खुद सहित)
additionalGuests: int
confirmedGuests: int?
isVip: bool
mealPreference: String?           // 'veg' | 'non-veg' | 'jain'
dietaryRestrictions: List<String>?
userId: String?
createdAt / updatedAt: DateTime?
```

## 7. EventVendorBookingModel (event-level vendor tracking — payment gateway के बिना)

```
bookingId, eventId, ceremonyId, vendorId: String
vendorName, vendorCategory, vendorPhone: String   // denormalized
status: VendorBookingStatus       // enum: inquiry, negotiating, booked, confirmed, completed, cancelled
quotedAmount, advanceAmount, paidAmount: double
confirmationStatus: String?       // vendor confirmation link का state
negotiationHistory: List<NegotiationEntry>?   // {date, note, amount}
nextPaymentDueDate: DateTime?
createdAt / updatedAt: DateTime?
```

## 8. FoodMenuModel & MenuItemModel

```
FoodMenuModel:
  menuId, eventId: String
  mealType: String                // 'breakfast' | 'lunch' | 'high_tea' | 'dinner'
  servingTime: String
  catererName / catererImageUrl: String?
  categories: Map<String, List<String>>   // {'Beverages': ['Chai'], 'Mains': ['Paneer']}
  notes: String?

MenuItemModel:
  itemId, eventId, ceremonyId: String
  name, category: String
  dietaryType: String?            // 'veg' | 'non-veg' | 'jain'
  isSelected: bool
  estimatedQuantity: int?
```

## 9. HotelRoomModel

```
roomId, eventId: String
hotelName: String
roomNumber / roomType: String?
guestNames: List<String>?
checkIn / checkOut: DateTime?
ratePerNight: double
```

## 10. BudgetModel + BudgetCategory

```
BudgetModel:
  budgetId, userId, eventId: String
  totalBudget, spentAmount: double
  categories: List<BudgetCategory>
  createdAt / updatedAt: DateTime

BudgetCategory:
  categoryId, categoryName, icon: String
  allocatedBudget, spentBudget: double
  color: Color
```

## 11. ChecklistModel + ChecklistTask

```
ChecklistModel:
  checklistId, userId, eventId, title: String
  tasks: List<ChecklistTask>
  createdAt / updatedAt: DateTime

ChecklistTask:
  taskId, title, description: String
  priority: String                // 'high' | 'medium' | 'low'
  dueDate: DateTime
  isCompleted: bool
  assignedTo: String?
  completedAt: DateTime?
```

## 12. ChatConversation + ChatMessage

```
ChatConversation:
  chatId: String                  // "{userId}_{vendorId}" (sorted) — deterministic
  userId, userName, userImageUrl: String
  vendorId, vendorName, vendorImageUrl: String
  lastMessage, lastMessageType: String
  lastMessageAt: DateTime
  unreadCountUser / unreadCountVendor: int
  pinnedMessageIds: List<String>?

ChatMessage:
  messageId, chatId, senderId: String
  text: String
  type: ChatMessageType           // enum: text, image, video, pdf, location, voice, quotation, bookingLink
  status: MessageStatus           // enum: sending, sent, delivered, read
  mediaUrl / thumbnailUrl / fileName / voiceUrl / linkUrl: String?
  latitude / longitude: double?   // location message
  duration: int?                  // voice seconds
  price / description: —          // quotation message
  replyToMessageId: String?
  reactions: Map<String,int>?     // {"❤️": 2}
  pinnedBy / deletedFor: List<String>?
  isEdited / isDeleted: bool
  createdAt: DateTime
```

## 13. ReelModel + ReelCommentModel

```
ReelModel:
  reelId, vendorId: String
  vendorName, vendorCategory, vendorImageUrl: String   // denormalized
  videoUrl, thumbnailUrl: String  // Cloudinary
  caption: String, tags: List<String>, audioTitle: String
  likeCount, commentCount, shareCount, saveCount, viewCount: int
  isVerified: bool
  createdAt: DateTime

ReelCommentModel:
  commentId, reelId, userId, userName, userImageUrl, text: String
  likeCount: int
  createdAt: DateTime
```

## 14. ReviewModel

```
reviewId, userId, vendorId: String
bookingId: String?                // verified purchase link
rating: double                    // 1–5
title, content: String
photoUrls: List<String>?
tags: List<String>?               // 'On Time', 'Great Quality'
helpfulCount: int
createdAt / updatedAt: DateTime
```

## 15. NotificationModel

```
notificationId, userId: String
title, body: String
type: String                      // 'booking' | 'payment' | 'chat' | 'reminder' | 'review' | 'promo'
relatedId: String?                // deep-link target ID
imageUrl: String?
isRead: bool
priority: String                  // 'high' | 'normal' | 'low'
data: Map<String,dynamic>?
createdAt: DateTime
```

## 16. CategoryModel

```
id, name: String
type: String                      // 'services' | 'fashion' | 'food' | 'decoration' | 'music' | 'shops'
imageUrl: String?
isActive: bool
```

---

## 🔗 Relationships (ER Summary)

```
User (1) ──── (N) Booking ──── (1) Vendor
User (1) ──── (N) Event
Event (1) ─── (N) Ceremony
Event (1) ─── (N) Guest
Event (1) ─── (N) EventVendorBooking ──── (1) Vendor
Event (1) ─── (N) FoodMenu / MenuItem / HotelRoom
Ceremony (1) ─ (N) EventVendorBooking
Event (1) ──── (1) Budget ─── (N) BudgetCategory
Event (1) ──── (1) Checklist ─ (N) ChecklistTask
User (1) ──── (N) ChatConversation ──── (1) Vendor
ChatConversation (1) ─ (N) ChatMessage
Vendor (1) ─── (N) Reel ─── (N) ReelComment
User (1) ──── (N) Review ──── (1) Vendor
User (1) ──── (N) Notification
User (N) ──── (N) Vendor   [favorites, following, blockedVendors — junction docs]
User (N) ──── (N) Reel     [reelInteractions — like/save junction]
```

---

## 📏 Model Conventions (नए models के लिए rules)

1. सभी fields `final` — models immutable हैं; बदलाव `copyWith()` से
2. Firestore Timestamps ↔ DateTime conversion `fromFirestore`/`toFirestore` में
3. **Denormalization allowed**: list rendering के लिए name/image जैसे fields duplicate रखो (जैसे `vendorName` booking में) — लेकिन source बदलने पर sync करना provider की जिम्मेदारी है
4. Status fields के लिए valid values comment में लिखो (या enum बनाओ — नए code में enum preferred)
5. Nullable fields defensive parsing के साथ पढ़ो (`data['x'] as String?`)
