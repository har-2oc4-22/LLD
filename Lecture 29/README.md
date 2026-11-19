# Lecture 29: Iterator Design Pattern

---

## 🎯 What is Iterator Pattern?

> **Definition**: *"Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation."*

Iterator pattern ka main purpose hota hai kisi bhi collection ya data structure (Array, LinkedList, Binary Tree, Graph) ko **traverse (iterate)** karne ka ek standard universal tareeqa dena bina uske internal data structure ko bahar expose kiye.

---

## 💡 Real-World Analogy: TV Remote Next Button 📺

Aap TV remote par **"Channel Next (CH+)"** button press karte ho:
- TV ke andar channels kaise stored hain — array mein hain, database mein hain, ya satellite frequency map mein — aapko koi farak nahi padta.
- Remote aapko ek uniform interface deta hai: `next()` aur `hasNext()`.

---

## 🏗️ The Problem: Diverse Data Structures, One Loop

Maan lo aapke paas 3 alag-alag data structures hain:
1. `LinkedList` (Node pointers)
2. `BinaryTree` (Left & Right child nodes)
3. `Playlist` (Dynamic array)

Bina Iterator pattern ke, client code ko har ek ke liye alag-alag traversal code likhna padta:
- LinkedList ke liye: `while (node != null) node = node.next;`
- Binary Tree ke liye: Inorder traversal recursion ya stack.
- Playlist ke liye: `for (int i=0; i < size; i++)`.

**With Iterator Pattern**:
Teeno ke liye client ka code **bilkul identical** ho jata hai:
```java
Iterator<T> it = collection.getIterator();
while (it.hasNext()) {
    T item = it.next();
}
```

---

## 💻 Code Walkthrough

### 1. The Generic Interfaces
```java
interface Iterator<T> {
    boolean hasNext();
    T next();
}

interface Iterable<T> {
    Iterator<T> getIterator();
}
```

### 2. Binary Tree Inorder Iterator (Using Stack)
```java
class BinaryTreeInorderIterator implements Iterator<Integer> {
    private Deque<BinaryTree> stack = new ArrayDeque<>();

    private void pushLefts(BinaryTree node) {
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }

    public BinaryTreeInorderIterator(BinaryTree root) {
        pushLefts(root);
    }

    public boolean hasNext() {
        return !stack.isEmpty();
    }

    public Integer next() {
        BinaryTree node = stack.pop();
        int val = node.data;
        if (node.right != null) {
            pushLefts(node.right);
        }
        return val; // Returns next in-order element!
    }
}
```

---

## 🌟 Benefits of Iterator Pattern

1. **SRP (Single Responsibility)**: Collection ka kaam sirf elements ko hold karna hai, traversal algorithms (e.g. Inorder, Preorder, Reverse traversal) separate iterator classes me rehte hain.
2. **Multiple Simultaneous Iterations**: Ek hi collection par ek sath do independent iterators run ho sakte hain (e.g., comparing elements).
3. **Encapsulation**: Tree ke pointers ya nodes private rehte hain, bahar sirf data elements surface hote hain.

---

## 📝 Lecture 29 Summary (Quick Revision)

- **Universal Traversal API**: `hasNext()` aur `next()` provide karta hai.
- **Data Structure Agnostic**: Client ko collection ke internal representation ki information nahi hoti.
- **Java Standard Library**: Java ka `java.util.Iterator` aur `for (item : collection)` loop isi pattern par based hai.
