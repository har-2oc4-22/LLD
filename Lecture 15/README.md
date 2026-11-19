# Lecture 15: Command Design Pattern

---

## 🎯 What is Command Pattern?

> **Definition**: *"Encapsulate a request as an object, thereby letting you parameterize other objects with different requests, queue or log requests, and support undoable operations."*

Command pattern sender (jo command invoke karta hai) aur receiver (jo actual kaam karta hai) ko **completely decouple** kar deta hai.

---

## 💡 Real-World Analogy: Restaurant Order Ticket 🧾 & Universal Remote 📱

### 1. Restaurant Order
- **Customer (Client)**: Menu se dish choose karta hai.
- **Waiter (Invoker)**: Order ko ek Order Slip (Command object) par likh leta hai. Waiter khud khana nahi banata!
- **Order Slip (Command)**: Iske paas details hain ki kya banana hai aur kis table ka hai.
- **Chef (Receiver)**: Order slip padh kar actual khana cook karta hai.
- *Faayda*: Customer aur Chef ko direct ek doosre se baat karne ki zaroorat nahi. Order slips ko queue kiya ja sakta hai, cancel (undo) kiya ja sakta hai, ya log kiya ja sakta hai.

### 2. Universal Remote Control
Remote controller ke paas buttons hain (Button 0, 1, 2, 3). Remote ko ye nahi pata ki button 0 dabane par Philips ki bulb jali ya Crompton ka fan chala. Remote sirf `button[0].execute()` call karta hai!

---

## 🏗️ The 4 Core Actors in Command Pattern

```text
[Client] ── creates ──> [ConcreteCommand] ── binds ──> [Receiver]
   │                           │                          (Light / Fan)
   └── loads into ──> [Invoker] (RemoteController)
                         │ calls
                         └──> execute() / undo()
```

1. **Command Interface**: `execute()` aur `undo()` declare karta hai.
2. **Concrete Command (`LightCommand`)**: Receiver class ka reference rakhta hai aur `execute()` ke andar receiver ka specific method (`light.on()`) call karta hai.
3. **Receiver (`Light`, `Fan`)**: Actual business logic perform karta hai.
4. **Invoker (`RemoteController`)**: Buttons ko commands assign karta hai aur click hone par execute trigger karta hai.

---

## 💻 Code Walkthrough

```java
// 1. Command Interface
interface Command {
    void execute();
    void undo();
}

// 2. Receiver (Does actual work)
class Light {
    public void on()  { System.out.println("Light is ON"); }
    public void off() { System.out.println("Light is OFF"); }
}

// 3. Concrete Command
class LightCommand implements Command {
    private Light light;
    public LightCommand(Light l) { this.light = l; }

    public void execute() { light.on(); }
    public void undo()    { light.off(); }
}

// 4. Invoker (Remote)
class RemoteController {
    private Command[] buttons = new Command[4];

    public void setCommand(int slot, Command cmd) { buttons[slot] = cmd; }
    public void pressButton(int slot) {
        if (buttons[slot] != null) buttons[slot].execute();
    }
}
```

---

## 🚀 Why is Command Pattern so Powerful?

1. **Undo / Redo Functionality**: Har command ke paas `undo()` hota hai, isliye Text Editors (Ctrl+Z) aur Graphics tools mein commands ka stack maintain karke easily unlimited Undo/Redo support kiya ja sakta hai.
2. **Queuing & Scheduling**: Commands objects hote hain, toh unhe Message Queue (RabbitMQ / Kafka) mein daal kar background workers se execute karwaya ja sakta hai.
3. **Macro Commands (Batch Execution)**: Ek single command ke andar multiple commands store karke ek click par poora home automation sequence ("Movie Night" mode: Dim lights, Close curtains, Turn on TV) run ho sakta hai.

---

## 📝 Lecture 15 Summary (Quick Revision)

- **Decoupling Action from Actor**: Invoker ko receiver ke internal details se koi matlab nahi hota.
- **Request as First-Class Object**: Request ko pass, store, log aur undo kiya ja sakta hai.
- **Open for Extension**: Naya electronic appliance (e.g. AC) aane par bas `AcCommand` banao, remote ka existing code change nahi hoga.
