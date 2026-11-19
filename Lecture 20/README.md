# Lecture 20: Template Method Design Pattern

---

## 🎯 What is Template Method Pattern?

> **Definition**: *"Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure."*

Jab kisi workflow ke steps ka order bilkul fixed hota hai (e.g., Step 1 -> Step 2 -> Step 3), lekin kuch specific steps ka implementation alag-alag subclasses ke hisaab se vary karta hai, tab Template Method use hota hai.

---

## 💡 Real-World Analogy: Tea / Coffee Recipe ☕ & Machine Learning Pipeline 🤖

### 1. Beverage Recipe
1. Boil Water
2. Brew (Tea leaves vs Coffee beans) -> **Custom step!**
3. Pour into Cup
4. Add Condiments (Lemon vs Milk & Sugar) -> **Custom step!**
Step sequence hamesha wahi rahegi; koi bhi step 4 ko step 1 se pehle execute nahi kar sakta.

### 2. Machine Learning Pipeline (Lecture Code Example)
Har ML model ka standard pipeline hota hai:
1. `loadData()` (Common)
2. `preprocessData()` (Common)
3. `trainModel()` (Specific to Neural Network vs Decision Tree)
4. `evaluateModel()` (Specific metrics)
5. `saveModel()` (Default or specific format)

---

## 🎬 The "Hollywood Principle"

> *"Don't call us, we'll call you!"*

Traditional programming mein subclass parent class ke methods ko call karti hai.
Template Method mein **Parent class control karti hai execution order ko**, aur specific moments par subclass ke overridden methods ko call karti hai.

---

## 💻 Code Walkthrough

```java
abstract class ModelTrainer {
    // 1. Template Method: Marked FINAL so subclasses CANNOT tamper with sequence
    public final void trainPipeline(String dataPath) {
        loadData(dataPath);
        preprocessData();
        trainModel();      // Abstract hook
        evaluateModel();   // Abstract hook
        saveModel();       // Hook with default implementation
    }

    protected void loadData(String path) {
        System.out.println("Common: Loading dataset from " + path);
    }

    protected void preprocessData() {
        System.out.println("Common: Normalizing and splitting data");
    }

    // Subclasses must implement their specific logic
    protected abstract void trainModel();
    protected abstract void evaluateModel();

    // Subclasses can optionally override
    protected void saveModel() {
        System.out.println("Default: Saving model as binary");
    }
}

// Concrete Implementation
class NeuralNetworkTrainer extends ModelTrainer {
    @Override
    protected void trainModel() {
        System.out.println("NeuralNet: Running Backpropagation & Gradient Descent");
    }

    @Override
    protected void evaluateModel() {
        System.out.println("NeuralNet: Computing Cross-Entropy Loss & Accuracy");
    }
}
```

---

## ⚖️ Template Method vs Strategy Pattern

| Parameter | Template Method Pattern | Strategy Pattern |
| :--- | :--- | :--- |
| **Mechanism** | **Inheritance** (Subclassing base abstract class) | **Composition** (Injecting interface strategy) |
| **Granularity** | Algorithm ke kuch **parts/steps** change karta hai | **Poora algorithm** replace kar deta hai |
| **Runtime Switching**| Fixed at compile time by chosen subclass | Dynamic (Runtime par change ho sakta hai) |

---

## 📝 Lecture 20 Summary (Quick Revision)

- **Fixed Skeleton**: Algorithm ka control-flow base class mein `final` method ke roop mein locked rehta hai.
- **Code Reuse**: Common steps base class mein ek hi baar likhe jaate hain (DRY principle).
- **Hooks & Inversion**: Subclasses sirf un specific steps ko implement karti hain jahan custom behavior ki zaroorat hoti hai.
