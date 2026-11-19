# Lecture 38: Visitor Design Pattern

---

## 🎯 What is Visitor Pattern?

> **Definition**: *"Represent an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates."*

Jab aapke paas ek stable class structure (e.g., `TextFile`, `ImageFile`, `VideoFile`) hota hai, aur aapko unpar nayi-nayi operations (e.g. Compression, Virus Scanning, Size Calculation, Cloud Backup) add karni hoti hain bina un classes ke existing code ko modify kiye, tab Visitor pattern use hota hai.

---

## 💡 Real-World Analogy: Doctor Visiting Patients in a Hospital 🏥

Hospital mein alag-alag type ke patients hote hain (Cardio patient, Ortho patient, Neuro patient).
- Patients ko khud apne treatment ke instruments carry karne ki zaroorat nahi hoti.
- **Cardiologist (Visitor 1)** aata hai aur har patient ko uski problem ke hisaab se check karta hai.
- Kal ko naya **Eye Specialist (Visitor 2)** aata hai aur bina hospital ke beds ya patients ki classes modify kiye sabka eye test perform kar leta hai.

---

## 🔄 The Magic of "Double Dispatch"

Visitor pattern **Double Dispatch** mechanism use karta hai:
1. **First Dispatch**: Client element par `accept(visitor)` call karta hai.
2. **Second Dispatch**: Element apne type ke sath visitor ka specific method call karta hai: `visitor.visit(this)`.

```text
[Client] ──> element.accept(visitor)
                 │
                 └──> visitor.visit(this)  (Double Dispatch!)
```

---

## 💻 Code Walkthrough

### 1. The Visitor Interface
```java
interface FileSystemVisitor {
    void visit(TextFile file);
    void visit(ImageFile file);
    void visit(VideoFile file);
}
```

### 2. Elements with `accept()` Method
```java
abstract class FileSystemItem {
    protected String name;
    public abstract void accept(FileSystemVisitor visitor);
}

class TextFile extends FileSystemItem {
    @Override
    public void accept(FileSystemVisitor visitor) {
        visitor.visit(this); // Passes itself to visitor!
    }
}

class ImageFile extends FileSystemItem {
    @Override
    public void accept(FileSystemVisitor visitor) {
        visitor.visit(this);
    }
}
```

### 3. Adding New Operations as Visitors (OCP at its Best!)
```java
// Operation 1: Size Calculation
class SizeCalculationVisitor implements FileSystemVisitor {
    public void visit(TextFile file)  { System.out.println("Calc text size"); }
    public void visit(ImageFile file) { System.out.println("Calc image size"); }
    public void visit(VideoFile file) { System.out.println("Calc video size"); }
}

// Operation 2: Virus Scanning (Zero changes to file classes!)
class VirusScanningVisitor implements FileSystemVisitor {
    public void visit(TextFile file)  { System.out.println("Scanning text file for malicious scripts"); }
    public void visit(ImageFile file) { System.out.println("Scanning image steganography"); }
    public void visit(VideoFile file) { System.out.println("Scanning video metadata"); }
}
```

---

## ⚖️ When to Use Visitor Pattern

- **Use when**: Object structure (`TextFile`, `ImageFile`) stable ho, lekin unpar operations (`Calculate`, `Scan`, `Export`) frequently badal rahi hon ya badh rahi hon.
- **Avoid when**: Agar element types frequently add hote hain, kyunki naya element aane par saare Visitors mein ek nayi `visit(NewElement)` method add karni padegi.

---

## 📝 Lecture 38 Summary (Quick Revision)

- **Separate Data from Algorithms**: Element classes mein sirf data rehta hai, unpar hone wale algorithms visitor classes mein hote hain.
- **Double Dispatch**: Ensures correct method execution based on both the visitor type and the concrete element type at runtime.
- **Open/Closed**: Adding new operations is extremely easy; adding new element types requires updating the visitor interface.
