# Lecture 27: Machine Coding Case Study - Tinder Clone (Dating & Matchmaking System)

---

## 🎯 System Overview

Is case study mein **Tinder / Bumble** jaise location-based dating aur matchmaking platform ka complete Low-Level Design cover kiya gaya hai.
System handle karta hai:
- User profiling with Age, Gender, Interests, and Geo-coordinates (`Location`).
- Dating preferences (Target gender, min/max age range, max distance radius).
- Location distance calculation using the **Haversine Formula**.
- Pluggable Matchmaking algorithms via **Strategy Pattern**.
- Swipe mechanics (LIKE, PASS, SUPER_LIKE) with mutual match detection.
- Real-time match notifications via **Observer Pattern**.
- In-app 1-on-1 messaging (`ChatRoom`).

---

## 📋 Key Functional Workflows

### 1. The Swipe & Mutual Match Engine
```text
User A swipes RIGHT (LIKE) on User B
   │
   └── Stored in User A's liked list
   │
User B swipes RIGHT (LIKE) on User A
   │
   ├── System detects: "It's a Match!" (Mutual Like)
   ├── Observer triggers push notification to both users
   └── Creates a 1-to-1 ChatRoom for messaging
```

### 2. Matchmaking Algorithms (Strategy Pattern)
- **Basic Matcher**: Filters users based on gender preference and age range.
- **Location-Based Matcher**: Filters users within maximum geographic distance radius ($d \le \text{maxDistanceKm}$) using the Haversine formula.
- **Interests-Based Matcher**: Scores profiles based on common shared hobbies/interests (e.g., Coding, Hiking, Music).

---

## 🏗️ Design Patterns Applied

| Pattern | Component | Why Chosen |
| :--- | :--- | :--- |
| **Strategy Pattern** | `Matcher` (`BasicMatcher`, `LocationBasedMatcher`, `InterestsBasedMatcher`) | Recommendation algorithms ko independent banata hai taaki A/B testing easily ki ja sake. |
| **Observer Pattern** | `NotificationService`, `UserNotificationObserver` | Jab do users aapas mein match hote hain, dono ko instant real-time alert push karta hai. |
| **Factory Pattern** | `MatcherFactory` | Client preferences ke according suitable matcher algorithm return karta hai. |
| **Facade Pattern** | `DatingApp` | User registration, recommendation fetching, swiping, aur chatting ke liye unified interface provide karta hai. |

---

## 💻 Code Walkthrough

```java
// 1. Initialize Dating App Facade
DatingApp app = new DatingApp();

// 2. Register Users with Location & Preferences
User user1 = new User("1", "Rohan", 25, Gender.MALE, new Location(28.7041, 77.1025)); // Delhi
User user2 = new User("2", "Pooja", 24, Gender.FEMALE, new Location(28.7045, 77.1030));

user1.getPreferences().setPreferredGender(Gender.FEMALE);
user1.getPreferences().setMaxDistanceKm(10.0);

// 3. Find Matches using Strategy
List<User> recommendations = app.getRecommendations("1", MatcherType.LOCATION_BASED);

// 4. Swipe Action & Match Detection
app.swipe("1", "2", SwipeAction.LIKE);
app.swipe("2", "1", SwipeAction.LIKE); 
// Triggers: "It's a Match! Chat room unlocked."
```

---

## 📝 Lecture 27 Summary (Quick Revision)

- **Haversine Distance**: Realistic spherical earth distance calculation between GPS points.
- **Decoupled Matchers**: Algorithm switching (Location vs Interests vs AI embeddings) is frictionless with Strategy Pattern.
- **Event-driven Matches**: Observer pattern cleanly decouples swiping logic from notification and messaging triggers.
