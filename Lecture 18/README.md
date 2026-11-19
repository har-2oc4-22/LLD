# Lecture 18: Machine Coding Case Study - Music Player Application (Spotify Core)

---

## 🎯 System Overview

Is case study mein ek complete **Music Player System (Spotify / Apple Music core)** ka Low-Level Design model kiya gaya hai.
Yeh project 4 core design patterns aur clean layered packaging ko demonstrate karta hai:
- Multiple playback strategies (Sequential, Shuffle/Random, Custom Queue).
- Multiple audio output hardware devices via Adapter Pattern (Bluetooth, Wired Speakers, Headphones).
- Single unified control interface via Facade Pattern.

---

## 📋 Functional Requirements

1. **Playlist & Song Management**:
   - Songs create karna (`title`, `artist`, `duration`).
   - Multiple playlists create aur manage karna (`PlaylistManager`).
2. **Playback Strategies**:
   - **Sequential Play**: Songs in original order.
   - **Random / Shuffle Play**: Shuffled order without repeat until all played.
   - **Custom Queue**: User-defined priority queue order.
3. **Hardware Device Integration**:
   - External vendors ke incompatible APIs (`BluetoothSpeakerAPI`, `WiredSpeakerAPI`, `HeadphonesAPI`) ko universal interface (`IAudioOutputDevice`) se connect karna.
4. **Playback Engine**:
   - `AudioEngine` song buffer ko currently connected active output device par stream karta hai.

---

## 🏗️ Design Patterns Applied

| Pattern | Component | Why Chosen |
| :--- | :--- | :--- |
| **Facade Pattern** | `MusicPlayerFacade` | Client ke liye `play()`, `pause()`, `next()`, `switchDevice()` ko simple one-line APIs provide karta hai. |
| **Adapter Pattern** | `BluetoothSpeakerAdapter`, `HeadphonesAdapter` | External vendor SDKs ke mismatched method names aur payloads ko hamare `IAudioOutputDevice` interface ke sath bridge karta hai. |
| **Strategy Pattern** | `SequentialPlayStrategy`, `RandomPlayStrategy`, `CustomQueueStrategy` | Playback order logic ko independent aur runtime-switchable banata hai. |
| **Factory Pattern** | `DeviceFactory` | Given `DeviceType` enum ke hisaab se correct adapter instantiate karta hai. |

---

## 🗂️ Package Architecture

```text
MusicPlayerApplication/
├── MusicPlayerFacade.java       # Facade (Main Controller)
├── core/
│   └── AudioEngine.java         # Sends raw audio stream to output device
├── device/
│   ├── IAudioOutputDevice.java  # Target output interface
│   ├── BluetoothSpeakerAdapter.java
│   ├── HeadphonesAdapter.java
│   └── WiredSpeakerAdapter.java
├── external/                    # Simulated 3rd-party vendor SDKs
│   ├── BluetoothSpeakerAPI.java
│   ├── HeadphonesAPI.java
│   └── WiredSpeakerAPI.java
├── strategies/
│   ├── PlayStrategy.java        # Strategy interface
│   ├── SequentialPlayStrategy.java
│   ├── RandomPlayStrategy.java
│   └── CustomQueueStrategy.java
├── managers/
│   ├── PlaylistManager.java
│   ├── StrategyManager.java
│   └── DeviceManager.java
└── models/
    ├── Song.java
    └── Playlist.java
```

---

## 💻 Code Walkthrough (Client Interaction)

```java
// 1. Create player facade
MusicPlayerFacade player = new MusicPlayerFacade();

// 2. Add songs and create playlist
Song s1 = new Song("Kesariya", "Arijit Singh", 240);
Song s2 = new Song("Husn", "Anuv Jain", 210);
player.createPlaylist("Favorites");
player.addSongToPlaylist("Favorites", s1);
player.addSongToPlaylist("Favorites", s2);

// 3. Select playback strategy & output device
player.setPlayStrategy(PlayStrategyType.RANDOM);
player.connectDevice(DeviceType.BLUETOOTH);

// 4. Play
player.play();
player.next();
```

---

## 📝 Lecture 18 Summary (Quick Revision)

- **Adapter for Hardware Isolation**: Core application logic kabhi bhi 3rd-party device SDKs par directly depend nahi karti.
- **Strategy for Playback Mode**: Changing from sequential to shuffle is a zero-downtime runtime pointer swap.
- **Facade for Ergonomics**: `MusicPlayerFacade` shields UI / CLI from coordinating 5 separate subsystems.
