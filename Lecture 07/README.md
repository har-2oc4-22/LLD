# Lecture 07: Practical Case Study - Document Editor (Applying SOLID)

---

## 🎯 Case Study Overview

Is lecture mein hum seekhte hain ki kaise **Real-World Problem** (Document Editor jaise MS Word ya Google Docs ka simplified core) par **SOLID Principles** apply karke **Bad Design (Spaghetti / Tight-coupling)** ko **Good Clean Design** mein transform karte hain.

---

## ❌ Problem with Bad Design (The Naive Approach)

Naive approach mein beginner ek single `DocumentEditor` class bana deta hai:
- Text, Images, Line Breaks sabko ek single list ya string mein manually handle karta hai.
- Har element ke render type ke liye `if-else` lagata hai (`if (type == "TEXT") ... else if (type == "IMAGE") ...`).
- File save karne aur DB save karne ka code bhi usi class ke andar likh deta hai.

### Why this Fails?
1. **SRP Violated**: Document editing, rendering aur file storage sab ek hi class me mix ho gaye.
2. **OCP Violated**: Kal ko agar naya element (jaise `TableElement` ya `VideoElement`) add karna ho, toh purane rendering logic ke andar ghuske `if-else` modify karna padega.
3. **DIP Violated**: Editor directly `FileWriter` (concrete file IO) par depend kar raha hai, test karna mushkil ho jata hai.

---

## ✅ The Clean Architecture (Good Design)

Humne problem ko 3 clean layers mein divide kiya:

```text
[DocumentEditor] (Facade / Coordinator)
    │
    ├── [Document] (Composite Container)
    │       └── List<DocumentElement>
    │               ├── TextElement (implements DocumentElement)
    │               ├── ImageElement (implements DocumentElement)
    │               ├── NewLineElement (implements DocumentElement)
    │               └── TabSpaceElement (implements DocumentElement)
    │
    └── [Persistence] (Abstraction)
            ├── FileStorage (implements Persistence)
            └── DBStorage (implements Persistence)
```

### 1. Element Layer (Polymorphism & OCP)
Har document element (Text, Image, NewLine, TabSpace) `DocumentElement` interface ko implement karta hai:
```java
interface DocumentElement {
    String render();
}

class TextElement implements DocumentElement { ... }
class ImageElement implements DocumentElement { ... }
class NewLineElement implements DocumentElement { ... }
```
*Faayda*: Naya element aane par bas nayi class implement karo, `Document` class ko pata bhi nahi chalega ki naya type aa gaya!

### 2. Document Container
Elements ki list maintain karta hai aur unhe sequentially compose karke render karta hai:
```java
class Document {
    private List<DocumentElement> elements = new ArrayList<>();
    
    public void addElement(DocumentElement el) { elements.add(el); }
    
    public String render() {
        StringBuilder sb = new StringBuilder();
        for (DocumentElement el : elements) {
            sb.append(el.render());
        }
        return sb.toString();
    }
}
```

### 3. Persistence Layer (DIP)
Save karne ke liye abstraction use karte hain:
```java
interface Persistence {
    void save(String data);
}

class FileStorage implements Persistence { ... }
class DBStorage implements Persistence { ... }
```

---

## 💡 How SOLID is Followed Here

| Principle | How It is Applied |
| :--- | :--- |
| **SRP** | `DocumentElement` renders itself, `Document` manages collection, `Persistence` handles saving, `DocumentEditor` coordinates client actions. |
| **OCP** | Naya element (`TableElement`, `EquationElement`) bina existing code ko modify kiye add ho sakta hai. |
| **LSP** | Har `DocumentElement` seamlessly `render()` kar sakta hai bina runtime error diye. |
| **ISP** | `DocumentElement` interface ke paas sirf ek single focused method `render()` hai. |
| **DIP** | `DocumentEditor` concrete `FileStorage` ke bajaye `Persistence` interface par depend karta hai. |

---

## 📝 Lecture 07 Summary (Quick Revision)

- **Separate Data from IO**: Content modeling (document elements) aur Data storage (File/DB) ko hamesha alag-alag layers mein rakho.
- **Polymorphic Rendering**: `if-else` based type checking ki jagah common interface (`DocumentElement`) aur dynamic dispatch use karo.
- **Testability**: Interfaces use karne se unit testing mein Mock Persistence inject karna bilkul easy ho jata hai.
