# Lecture 28: Builder Design Pattern (Simple, Step Builder, Director)

---

## 🎯 What is Builder Pattern?

> **Definition**: *"Separate the construction of a complex object from its representation so that the same construction process can create different representations."*

Jab kisi class ke paas bahut saare fields (attributes) hote hain — jinme se kuch mandatory hote hain aur bahut saare optional hote hain — tab Builder pattern ek clean, readable, aur fluent API provide karta hai object construct karne ke liye.

---

## 💡 The Problem: The "Telescoping Constructor" Nightmare 😱

Maan lo aapko ek `HttpRequest` class banani hai:
- Mandatory: `url`, `method`
- Optional: `headers`, `queryParams`, `body`, `timeout`, `retries`, `followRedirects`, `sslEnabled`

### ❌ Without Builder (Bad Practice):
```java
// Telescoping constructor with endless nulls!
HttpRequest req = new HttpRequest("https://api.com", "GET", null, null, null, 30, 3, true, true);
```
- Kaunsa parameter kis index par hai yaad rakhna namumkin hai.
- Ek chhota sa type mismatch (e.g. timeout ki jagah retries) huge production bug ban sakta hai.

---

## 🏗️ The 3 Flavors of Builder in this Lecture

### 1. 🧱 Simple Builder (Fluent Method Chaining)
Nested static builder class with `return this` for chaining:

```java
HttpRequest request = new HttpRequest.HttpRequestBuilder()
    .withUrl("https://api.example.com/users")
    .withMethod("POST")
    .withHeader("Content-Type", "application/json")
    .withBody("{\"name\": \"Harsh\"}")
    .withTimeout(5000)
    .build(); // Validates required fields and returns HttpRequest
```

---

### 2. 🚶 Step Builder (Enforced Step-by-Step Construction)
- **Concept**: Interfaces ke through user ko force kiya jaata hai ki pehle mandatory steps complete kare, uske baad hi optional steps aur `build()` access ho sake!
- Example:
  `UrlStep` -> `MethodStep` -> `OptionalSteps` -> `build()`
- Compiler ensure karta hai ki aap bina URL diye kabhi bhi `build()` call hi na kar pao!

---

### 3. 🎬 Builder with Director (Predefined Blueprints)
Jab application mein standard requests bar-bar create hoti hain, toh ek **Director** class common configurations ko encapsulate kar leti hai:

```java
class HttpRequestDirector {
    public HttpRequest makeSimpleGetRequest(String url) {
        return new HttpRequest.HttpRequestBuilder()
            .withUrl(url)
            .withMethod("GET")
            .withTimeout(3000)
            .build();
    }

    public HttpRequest makePostJsonRequest(String url, String jsonBody) {
        return new HttpRequest.HttpRequestBuilder()
            .withUrl(url)
            .withMethod("POST")
            .withHeader("Content-Type", "application/json")
            .withBody(jsonBody)
            .build();
    }
}
```

---

## 🌟 Benefits of Builder Pattern

1. **Immutability**: Object ek baar `build()` hone ke baad immutable banaya ja sakta hai (no setters needed).
2. **Readability & Fluency**: Method chaining code ko English sentence ki tarah readable banata hai.
3. **Centralized Validation**: `build()` method ke andar check kiya ja sakta hai ki kya state mathematically aur logically valid hai.

---

## 📝 Lecture 28 Summary (Quick Revision)

- **Eliminates Telescoping Constructors**: Null values pass karne ki zaroorat khatam.
- **Fluent API (`return this`)**: Chaining allow karta hai.
- **Step Builder**: Compile-time safety for mandatory ordering.
- **Director**: Standard configurations ke liye quick factories ka kaam karta hai.
