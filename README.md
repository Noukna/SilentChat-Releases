# Silent Chat 🤫🛡️

> Ultra-secure, anonymous, end-to-end encrypted Android messenger operating over the Tor network.

---

## 📖 Level 1 — Getting Started Guide (Walkthrough)

Welcome to **Silent Chat**!  
Silent Chat is a privacy-first messaging application. It operates over the Tor network, requiring no user account, no central server, and performing zero data collection. Everything is encrypted end-to-end.

---

### 1. Creating Your Identity
On first launch, you need to configure two parameters:
* **Your Pseudonym:** This is the display name visible to your contacts. You can enter one or tap the **"Random"** button to generate an anonymous handle (*e.g., SilentFox42*).
* **Your Password:** This is the cornerstone of your security. It encrypts your local data on the device.  
  ⚠️ *Warning: It cannot be recovered if forgotten. Choose it with care.*

---

### 2. Adding Contacts
Silent Chat relies on physical pairing without requiring a phone number:
1. Go to the **"My QR"** tab to display your unique code.
2. Ask your contact to scan this code using their application.
3. Reverse roles so that they add you to their contact list as well.
4. Once added, you can start chatting. A **green dot** indicates that your contact is online.

---

### 3. Secure Messaging
The interface is dark and minimalistic to preserve battery life and maintain discretion.
* **Text Messages:** Send standard text messages.
* **Voice Messages:** Tap the microphone icon to record a voice note (up to 1 minute).
* **Ephemeral Mode (1-on-1):** By default, messages self-destruct after a set delay (5 minutes by default). You can adjust this duration (1 to 10 minutes) via the chat menu. Received messages follow the sender's duration setting.

---

### 4. Ephemeral Rooms (Temporary Group Chats)
You can create temporary group chat rooms with multiple participants (up to 100 members max):
* **Creation:** Long-press (1 second) on a contact in your list, then select **"Create a room with this contact"**.
* **Invitation:** Once inside the room, use the **"+"** button to invite additional people from your own contact list.
* **Banners & Invitations:** A banner is displayed on the home screen to alert you of an active room or a pending invitation.
* **Member Identity:**
  * For a direct contact, you will see their local custom name.
  * For a non-contact member (friend of a friend), their name appears in the format `~Name`.
* **Zero Footprint:** The room exists purely in temporary memory (RAM). As soon as the room is closed or you exit the app, the room history is permanently erased.

---

### 5. Advanced Features
Access the main menu (**⋮** icon) to explore the application's powerful features:
* **Voice Anonymizer:** Enable this option to morph your voice into a neutral tone (neither male nor female) before sending. Your original voice is never transmitted.
* **Self-Destruction:** Set a maximum threshold for failed password attempts (*e.g., 4*). If someone attempts to brute-force access to your device and fails this many times, the app permanently wipes all data and uninstalls itself.
* **Updates:** Check for new releases directly within the app. Updating preserves your contacts and messages.
* **Sharing:** Send the app download link to your friends via SMS, email, or private messaging platforms (Telegram, Signal, etc.).
* **Donations:** If you appreciate the project, you can support its development using a Monero (XMR) address available in the menu.

---

### 6. Security, Privacy, and Notifications
* **Screenshot Blocking:** The app enforces system-level protection against screenshots and screen recording to prevent data leaks.
* **Local Data Erasure:** Your data is stored locally and encrypted. Deleting a contact permanently wipes the entire chat history from your device.
* **Confidential Notifications:** To preserve your privacy, notifications display only a generic message (*"N new messages"*). No message preview or contact name is ever exposed on the lock screen.
* **Battery Optimization Exemption & Background Execution:** To receive incoming messages while the screen is off, the app requests a battery optimization exemption. In the background, the connection remains active without excessive resource consumption.
* **Multi-language Support:** Available in 5 languages (French, English, Chinese, Spanish, Portuguese). You can switch languages at any time from the login menu.
* 💡 *Reminder: Security also depends on your habits. Use a strong password and never share your QR code publicly.*

---

## 🛡️ Level 2 — Key Features & Functional Model

### Privacy & Networking
* **Total Anonymity:** All communications route through the Tor network. Zero metadata exposed.
* **Exclusive Physical Pairing:** Contacts can only be added by directly scanning a QR Code in person (no central contact directory, no discovery by phone number or email).
* **System Isolation:** Hardware/OS-level blocking of screenshots and screen recording.

### Multi-member Ephemeral Rooms (Mesh/Relays)
* **P2P/Relay Architecture:** Rooms support up to 100 members with a maximum of 8 direct children per node.
* **Inter-member Anonymity:** No direct connection is established between non-friend members. No exchange of Ed25519 keys or Onion addresses occurs between non-contacts.
* **Zero Disk Persistence:** Room keys, topology, roster, and chat history (capped at 500 messages) are stored 100% in RAM.
* **Blind Invitation Handling:** Invitation expiration (120 s) reveals no information regarding whether a contact was unavailable or declined.

### Background Service & Notifications
* **Foreground Service Management:** Maintains Tor connectivity through a responsive background service (`specialUse`/`dataSync`) compliant with Android 12 through 15 restrictions.
* **Confidential Notifications:** High-priority notification channel displaying intentionally generic content without metadata leaks.
* **Message Catch-up (`RetryWorker`):** Periodic task (every 15 minutes) executing targeted pings exclusively to contacts with pending undelivered messages.

---

## 🏗️ Level 3 — Technical Architecture & Cryptographic Specifications

The application is structured into two distinct layers to isolate core native logic from the presentation layer.

```
┌────────────────────────────────────────────────────────┐
│               Android UI (Kotlin Native)               │
│   (Dark UI, Foreground Service, SQLCipher, RAM Store)  │
└───────────────────────────┬────────────────────────────┘
                            │ UniFFI Bindings (MessageListener)
┌───────────────────────────▼────────────────────────────┐
│                  Rust Core (native_core)               │
│  (Arti Tor Client, Room/Relay Engine, Crypto, RAM)     │
└────────────────────────────────────────────────────────┘
```

### 1. Rust Core (`native_core`)
Handles critical business logic, low-level cryptography, Onion Services networking, and ephemeral rooms:
* **Tor Client:** Built upon the `arti-client` crate (v0.47). Manages bootstrapping, anonymous circuits, and v3 Onion Services.
* **Ephemeral Room Engine (`on_flood`):** Re-transmits encrypted blobs without intermediate decryption.
* **Cryptographic Primitives:**
  * `ed25519-dalek`: Digital signatures and identity authentication.
  * `x25519-dalek`: Ephemeral Diffie-Hellman key exchange.
  * `chacha20poly1305`: Authenticated symmetric encryption (AEAD).
  * `argon2`: High-security Key Derivation Function (KDF).
  * `zeroize`: Explicit RAM memory wiping for sensitive secrets during `wipeRuntime` or application shutdown.
* **UniFFI Interface:** Generates Kotlin bindings and exposes the extended listener interface: `MessageListener` (`on_room_invite`, `on_room_message`, `on_room_event`).

### 2. Android Application (Kotlin)
* **UI & Session:** Native Android interface (no WebView). Room chat history is preserved in RAM (up to 500 messages).
* **Foreground Service:**
  * Utilizes `specialUse` (API 34+) and `dataSync` (API 29-33) types to bypass the 6-hour execution timeout on Android 15.
  * Active/Passive mode handling: halts background `tick()` invocations while keeping the socket open for incoming pings.
  * Handles `POST_NOTIFICATIONS` permission (Android 13+).
* **Storage:**
  * `SQLCipher`: Encrypted relational database (contacts, 1:1 direct messages, metadata).
  * `Encrypted Files`: The cryptographic identity is saved in an encrypted container protected by the key derived from Argon2id.
* **Scheduler:** `RetryWorker` triggers pings every 15 minutes for contacts with pending messages.

---

## 🔐 Level 4 — Cryptographic Model & Security Flows

### Identity Generation & Isolation
1. Upon initial setup, the user password is processed through **Argon2id** to derive a *Master Key*.
2. The *Master Key* derives two distinct sub-keys:
   * $K_{ID}$: Encryption key for the cryptographic identity key store (Ed25519/X25519 keypair).
   * $K_{DB}$: Decryption key for the `SQLCipher` database.

### 1:1 Direct Message Encryption Protocol (E2EE)
For each direct message transmitted:
1. **Ephemeral Exchange:** An ephemeral X25519 keypair is generated to guarantee *Perfect Forward Secrecy* (PFS).
2. **Derivation:** A session key is derived via **HKDF-SHA256**.
3. **Symmetric Encryption:** Payload content (text or audio note) is encrypted using **ChaCha20-Poly1305**.
4. **Signature & Authentication:** The sender signs the packet with their private **Ed25519** key.
5. **Verification:** The recipient verifies the signature using the paired public key and decrypts the payload.

### Ephemeral Room Protocol (Two-Layer Encryption)
Rooms leverage a dual-layer cryptographic wrapper:

1. **Hop-by-Hop Transport Layer (Point-to-Point):**
   * Each hop between direct contacts reuses the $X25519 + ChaCha20\text{-}Poly1305$ exchange layer with $Ed25519$ signatures.
   * Utilizes a room-specific HKDF salt: `schat-v1-room` to strictly isolate room frames from 1:1 direct messages.
2. **Payload Layer (Room Level):**
   * Generated by the room host and transmitted hop-by-hop inside the invitation blob.
   * Payload content is end-to-end encrypted with the room key.
3. **Traffic Masking (Padding):**
   * All room messages are padded to uniform block sizes of $512 \text{ bytes} \times 2^n$ to prevent traffic analysis based on packet size.
4. **Topology Anonymity & Relay Tree:**
   * Each member receives a temporary random ID unique to the room.
   * Real Onion addresses and public Ed25519 keys of members are never revealed to non-friends.
   * Tree topology is known **exclusively by the host**. Upstream topology status reports are encrypted specifically for the host key.
   * Control commands `GONE` (kick) and `CLOSE` (room teardown) must be signed by the host key.
5. **Heartbeat Management (Cascading):**
   * Heartbeat ping every $10\text{ s}$. After $3$ consecutive failures or $75\text{ s}$ of inactivity, the connection is considered dead.
   * If the parent node is lost, the node purges its memory state and propagates a `BYE` command to its children.