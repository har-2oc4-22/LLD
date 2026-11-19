# Lecture 04: Object Modeling & Class Relationships (UML)

---

## 🎯 Concepts at a Glance

Jab hum LLD mein complex system design karte hain, toh classes akele exist nahi karti — wo ek doosre se communicate karti hain.
Lecture 04 mein hum cover karte hain **Class Relationships & UML Basics**:

1. **Dependency** (*"Uses-a"*)
2. **Association** (*"Knows-a"*)
3. **Aggregation** (*"Has-a" - Weak Relationship*)
4. **Composition** (*"Has-a" - Strong Relationship / Death Dependency*)
5. **Generalization** (*"Is-a" - Inheritance*)
6. **Realization** (*"Implements" - Interface*)

---

## 🔍 The 6 Class Relationships Explained with Analogies

```text
Dependency  ---> Association  ---> Aggregation (◇)  ---> Composition (◆)
[Weakest link]                                           [Strongest link]
```

### 1. 🔗 Dependency ("Uses-A")
- **Definition**: Ek class doosri class ko sirf temporarily kisi method ke parameter ya local variable ke form mein use karti hai.
- **Analogy**: Aap printer use karne ke liye cyber cafe gaye. Document print hua, connection khatam. Aap printer ko apne paas permanently store nahi karte.
- **Code**:
  ```java
  class Driver {
      void drive(Car car) { // Driver uses Car as a method parameter
          car.startEngine();
      }
  }
  ```
- **UML Symbol**: Dashed line with open arrow (`- - - ->`).

---

### 2. 🤝 Association ("Knows-A")
- **Definition**: Do classes ke beech structural link hota hai jisme dono independent hoti hain lekin ek doosre ke baare mein jaanti hain.
- **Analogy**: Ek Doctor aur Patient. Doctor ke paas patient ki list ho sakti hai, aur patient ke paas doctor ka reference ho sakta hai. Dono ek dusre ke bina bhi independently exist karte hain.
- **Code**:
  ```java
  class Teacher {
      private List<Student> students; // Knows about students
  }
  ```
- **UML Symbol**: Solid line (`──────`).

---

### 3. 🏢 Aggregation ("Has-A" - Weak Coupling)
- **Definition**: Ek Part-Whole relationship jisme Child object Parent ke bina bhi independently exist kar sakta hai.
- **Analogy**: Ek University aur Professor. University ke paas professors hote hain. Agar University band ho jaye, toh Professor ki existence khatam nahi hoti, wo kisi doosri university join kar sakte hain.
- **Code**:
  ```java
  class Department {
      private Professor professor; // Professor is passed from outside
      public Department(Professor prof) {
          this.professor = prof;
      }
  }
  ```
- **UML Symbol**: Hollow Diamond (`◇─────`).

---

### 4. 🫀 Composition ("Has-A" - Strong Coupling / Death Dependency)
- **Definition**: Part-Whole relationship jisme Child object ka lifecycle Parent object par completely depend karta hai. Agar Parent destroy hua, toh Child bhi destroy ho jayega.
- **Analogy**: Human Body aur Heart. Heart body ke andar hi create hota hai. Agar body destroy ho gayi, toh heart independently function nahi kar sakta.
- **Code**:
  ```java
  class House {
      private Room livingRoom;

      public House() {
          // LivingRoom is created inside House and cannot exist without House
          this.livingRoom = new Room("Living Room");
      }
  }
  ```
- **UML Symbol**: Filled / Solid Diamond (`◆─────`).

---

### 5. 🧬 Generalization & Realization
- **Generalization (Inheritance)**: Subclass extends Superclass (`class Dog extends Mammal`). UML: Solid line with hollow triangle (`─────▷`).
- **Realization (Interface Implementation)**: Concrete class implements Interface (`class Dog implements Animal`). UML: Dashed line with hollow triangle (`- - - ▷`).

---

## ⚖️ Aggregation vs Composition (Interview Favorite!)

| Parameter | Aggregation (Weak) | Composition (Strong) |
| :--- | :--- | :--- |
| **Lifecycle** | Child can exist independently of parent | Child cannot exist without parent |
| **Ownership** | Shared ownership | Exclusive / strict ownership |
| **Creation** | Child usually passed via constructor/setter | Child usually created inside parent constructor |
| **UML Symbol** | Hollow Diamond (`◇`) | Solid / Filled Diamond (`◆`) |
| **Real Example** | Car and Music Player (can remove player) | Car and Engine (core vital part) |

---

## 📝 Lecture 04 Summary (Quick Revision)

- **Dependency**: Temporary usage in a method (`car.drive(gps)`).
- **Association**: Peer relationship where both are independent (`doctor.check(patient)`).
- **Aggregation**: Parent has child, but child survives if parent dies (`team.members`).
- **Composition**: Child is part of parent; parent death kills child (`order.orderItems`).
- **UML Mastery**: System design interviews mein Class Diagrams draw karte waqt ye symbols interviewer ko dikhate hain ki aapko object relationships ki deep understanding hai.
