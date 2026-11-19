# Lecture 19: Composite Design Pattern

---

## 🎯 What is Composite Pattern?

> **Definition**: *"Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly."*

Jab aapke data mein **Tree-like structure (Hierarchy)** hoti hai, jahan ek element ya toh ek individual item (**Leaf**) ho sakta hai ya fir doosre items ka container (**Composite**), tab Composite pattern client ko dono ke sath **ek jaisa (uniformly)** behave karne deta hai bina unka type check kiye.

---

## 💡 Real-World Analogy: File System (Files & Folders) 📁

Aapke computer ka File System:
- **File (Leaf)**: Ek single file (e.g. `resume.pdf`). Iske andar koi aur file nahi ho sakti. Iska apna size hota hai (e.g., 2 MB).
- **Folder (Composite)**: Ek directory jisme files bhi ho sakti hain aur nested sub-folders bhi ho sakte hain.
- Jab aap root folder par right-click karke **"Get Size"** ya **"Delete"** karte ho, Windows ko ye nahi sochna padta ki andar file hai ya folder. Wo sab par uniformly `getSize()` ya `delete()` call karta hai, aur folder internally recursive way mein apne children par call propagate kar deta hai!

---

## 🏗️ Architecture: Component, Leaf & Composite

```text
                  [FileSystemItem] (Component Interface)
                     ├── getSize()
                     └── ls()
                     /          \
                    /            \
          [File] (Leaf)         [Folder] (Composite)
       returns own size         List<FileSystemItem> children
                                recurses on children
```

---

## 💻 Code Walkthrough

```java
// 1. Component Interface
interface FileSystemItem {
    int getSize();
    void ls(int indent);
}

// 2. Leaf (Individual Item)
class File implements FileSystemItem {
    private String name;
    private int size;

    public File(String name, int size) {
        this.name = name;
        this.size = size;
    }

    public int getSize() { return size; } // Simple return
    public void ls(int indent) { System.out.println(" ".repeat(indent) + name); }
}

// 3. Composite (Container holding Component references)
class Folder implements FileSystemItem {
    private String name;
    private List<FileSystemItem> children = new ArrayList<>();

    public void add(FileSystemItem item) { children.add(item); }

    // Recursive calculation: Treats files and nested folders uniformly!
    public int getSize() {
        int total = 0;
        for (FileSystemItem child : children) {
            total += child.getSize(); // Polymorphic recursion!
        }
        return total;
    }

    public void ls(int indent) {
        System.out.println(" ".repeat(indent) + "+ " + name);
        for (FileSystemItem child : children) {
            child.ls(indent + 4);
        }
    }
}
```

---

## 🌟 Benefits of Composite Pattern

1. **Uniform Client Code**: Client ko `if (item instanceof Folder)` check karne ki zaroorat nahi padti.
2. **Easy Extensibility**: Naya component type (e.g. `Symlink` ya `CompressedArchive`) add karna ho toh bas `FileSystemItem` implement karo; existing recursive traversal logic unmodified rahegi.

---

## 📝 Lecture 19 Summary (Quick Revision)

- **Tree Hierarchy**: Part-whole relationships aur nested structures ke liye best pattern.
- **Leaf vs Composite**: Leaf holds no children; Composite holds a collection of Component references.
- **Polymorphism in Recursion**: Client top-level root component par method trigger karta hai, aur tree automatically self-traverse ho jaata hai.
