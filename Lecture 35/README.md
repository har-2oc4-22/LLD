# Lecture 35: Mediator Design Pattern

---

## 🎯 What is Mediator Pattern?

> **Definition**: *"Define an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently."*

Jab bahut saari classes (Colleagues) ko aapas mein ek doosre se communicate karna hota hai, toh direct connections ($O(N^2)$ spaghetti network) banane ki bajaye hum ek central **Mediator (Hub)** laga dete hain. Sabhi components sirf Mediator se baat karte hain, aur Mediator message ko sahi destination tak deliver karta hai.

---

## 💡 Real-World Analogy: Air Traffic Control (ATC) Tower ✈️

Socho ek busy airport par 20 airplanes land hone wale hain:
- **Without Mediator**: Har flight ke pilot ko baaki sabhi 19 pilots se radio par baat karni padegi ki *"Bhai tu kahan hai? Main runway par aa raha hoon!"* Yeh $N \times (N-1)$ connections complete crash aur disaster karwa dega.
- **With Mediator (ATC Tower)**: Koi bhi pilot kisi doosre pilot se directly baat nahi karta. Sabhi pilots sirf **ATC Tower (Mediator)** se baat karte hain. ATC runway schedule aur spacing manage karta hai!

---

## 🏗️ Architecture: Mesh vs Star Topology

```text
[Without Mediator - Mesh (N^2)]        [With Mediator - Hub & Spoke (O(N))]
     [A] ─── [B]                                [A]       [B]
      │ ╲   ╱ │                                   ╲       ╱
      │   ╳   │                                  [Mediator]
      │ ╱   ╲ │                                   ╱       ╲
     [C] ─── [D]                                [C]       [D]
```

---

## 💻 Code Walkthrough (Chat Room with Mute Feature)

### 1. Mediator Interface & Concrete Mediator
```java
interface IMediator {
    void registerColleague(Colleague c);
    void send(String from, String msg);
    void sendPrivate(String from, String to, String msg);
}

class ChatMediator implements IMediator {
    private List<Colleague> colleagues = new ArrayList<>();
    private List<Pair<String, String>> mutes = new ArrayList<>();

    public void send(String from, String msg) {
        System.out.println("[" + from + " broadcasts]: " + msg);
        for (Colleague c : colleagues) {
            if (!c.getName().equals(from) && !isMuted(c.getName(), from)) {
                c.receive(from, msg);
            }
        }
    }

    public void sendPrivate(String from, String to, String msg) {
        for (Colleague c : colleagues) {
            if (c.getName().equals(to)) {
                c.receive(from, msg);
                return;
            }
        }
    }
}
```

### 2. Colleague Class (User)
```java
abstract class Colleague {
    protected IMediator mediator; // Only knows Mediator!
    public Colleague(IMediator m) { this.mediator = m; }
}

class User extends Colleague {
    private String name;

    public void send(String msg) {
        mediator.send(name, msg); // Delegates to mediator!
    }

    public void receive(String from, String msg) {
        System.out.println(name + " received from " + from + ": " + msg);
    }
}
```

---

## ⚖️ Mediator vs Observer (Key Differences)

| Parameter | Mediator Pattern | Observer Pattern |
| :--- | :--- | :--- |
| **Communication** | **Bidirectional / Multi-party**: Any colleague can send and receive via mediator | **Unidirectional**: One Subject broadcasts to Many Observers |
| **Intent** | Tight coupling between a group of peer objects ko simplify karna | One object ke state change par sabhi dependents ko notify karna |
| **Center of Logic** | Complex interaction logic central mediator mein concentrated hoti hai | Subject observers ke interaction logic se oblivious rehta hai |

---

## 📝 Lecture 35 Summary (Quick Revision)

- **Decoupled Peers**: Colleagues ek doosre ke direct references nahi rakhte.
- **Centralized Interaction**: Business rules (jaise message filtering, mute lists, priority dispatch) ek hi central mediator class mein handle hote hain.
- **Real-World Examples**: Chat applications (Slack / WhatsApp channels), UI Dialogs (Form fields enabling/disabling based on checkboxes).
