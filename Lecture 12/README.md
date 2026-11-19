# Lecture 12: Observer Design Pattern (Pub-Sub)

---

## 🎯 What is Observer Pattern?

> **Definition**: *"Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically."*

Also known as **Publisher-Subscriber (Pub-Sub)** pattern or **Listener** pattern.

---

## 💡 Real-World Analogy: YouTube Notification Bell 🔔

Socho aapka favorite YouTube channel hai:
- **Bina Observer Pattern ke (Polling approach)**: Har subscriber ko har 5 minute mein channel kholkar refresh karna padega dekhne ke liye ki nayi video aayi ya nahi (Huge CPU, Network, and Time waste!).
- **Observer Pattern ke saath (Event-driven approach)**: Subscribers bas **Subscribe & Bell Icon** click kar dete hain. Channel jab bhi nayi video upload karega, YouTube automatically sabhi registered subscribers ke phone par push notification bhej dega!

---

## 🏗️ Architecture: Subject & Observers

```text
       [Observable / Subject] (e.g., Channel)
           ├── subscribe(Observer)
           ├── unsubscribe(Observer)
           └── notifyObservers()
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
  [Observer 1]  [Observer 2]  [Observer 3] (e.g., Subscribers)
   update()      update()      update()
```

---

## 💻 Code Walkthrough

### 1. The Observer Interface
```java
interface ISubscriber {
    void update(); // Called whenever subject state changes
}
```

### 2. The Subject / Observable Interface & Class
```java
interface IChannel {
    void subscribe(ISubscriber s);
    void unsubscribe(ISubscriber s);
    void notifySubscribers();
}

class Channel implements IChannel {
    private List<ISubscriber> subscribers = new ArrayList<>();
    private String latestVideo;

    public void subscribe(ISubscriber s) { subscribers.add(s); }
    public void unsubscribe(ISubscriber s) { subscribers.remove(s); }

    public void notifySubscribers() {
        for (ISubscriber sub : subscribers) {
            sub.update(); // BroadCast to all!
        }
    }

    public void uploadVideo(String title) {
        this.latestVideo = title;
        System.out.println("Channel uploaded: " + title);
        notifySubscribers(); // Triggers event!
    }
}
```

### 3. Concrete Observer
```java
class Subscriber implements ISubscriber {
    private String name;
    private Channel channel;

    public Subscriber(String name, Channel channel) {
        this.name = name;
        this.channel = channel;
    }

    @Override
    public void update() {
        System.out.println("Notification for " + name + ": New Video Available!");
    }
}
```

---

## ⚖️ Push vs Pull Model

1. **Push Model**: Subject `update(data)` method mein poora updated data khud bhej deta hai.
2. **Pull Model**: Subject sirf notify karta hai (`update()`), aur observer khud apni zaroorat ka data `subject.getData()` se fetch karta hai (More flexible and loosely coupled).

---

## 📝 Lecture 12 Summary (Quick Revision)

- **One-to-Many Decoupling**: Subject ko ye janne ki zaroorat nahi hoti ki subscribers kaun hain, sirf unka common interface `update()` call karna hota hai.
- **Dynamic Subscription**: Observers runtime par kabhi bhi jud (subscribe) ya alag (unsubscribe) ho sakte hain.
- **Where to Use**: Event handling systems, UI Button click listeners, Stock market tickers, Notification systems, Webhooks.
