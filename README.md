# Silent Chat
**Private, anonymous, and end-to-end encrypted messaging.**

Silent Chat is an Android application that enables secure communication over the Tor network. It features a security core written in Rust (exposed via UniFFI) and a native Kotlin user interface.

---

## 🛡️ Key Features

- **Total Anonymity**: All communications pass through the Tor network (Rust Arti implementation). No metadata is exposed.
- **End-to-End Encryption (E2EE)**:
  - **Authentication**: Ed25519 signatures.
  - **Key Exchange**: X25519 (Ephemeral Diffie-Hellman).
  - **Symmetric Encryption**: ChaCha20-Poly1305 (AEAD).
  - **Key Derivation**: HKDF (SHA-256).
- **Local Security**: Encrypted database using SQLCipher. Encrypted identity using Argon2id (brute-force resistant KDF). In-memory wiping of sensitive secrets (Zeroize).
- **Ephemeral Messages**: Ability to define a lifespan (1 to 10 minutes) for sent messages. Deletion is triggered locally once delivery is confirmed.
- **Physical Pairing**: Contact addition via QR code scanning (no central contact server).
- **Data-Preserving Updates**: Full support for app updates while preserving your account, cryptographic keys, contacts, and settings intact.

---

## 🏗️ Technical Architecture

The application is structured into two distinct layers to maximize security and performance:

### 1. Rust Core (`native_core`)
Handles critical logic, cryptography, and networking.
- **Tor Client**: Based on the `arti-client` crate (v0.47). Manages bootstrapping, circuits, and Onion Services.
- **Crypto**:
  - `ed25519-dalek`: Signatures.
  - `x25519-dalek`: Key exchange.
  - `chacha20poly1305`: Message encryption.
  - `argon2`: Master key derivation from password.
- **Interfacing**: Uses UniFFI to automatically generate Kotlin bindings from Rust code.

### 2. Android Application (Kotlin)
Handles UI, local storage, and coordination.
- **UI**: Native Kotlin interface (no WebView), dark theme, optimized for readability.
- **Storage**:
  - **SQLCipher**: Encrypted database (contacts, messages, settings).
  - **Encrypted Files**: Cryptographic identity is stored in an encrypted file using a key derived from the password.
- **Network**: No direct Internet connection for messages. All traffic routes through the Tor tunnel managed by the Rust core.

---

## 🔐 Security Model

- **Identity**:
  Upon first use, the user chooses a pseudonym and a password. The password derives a master key via Argon2id. This master key derives two keys: one to encrypt identity secrets (Ed25519/X25519) and one to encrypt the SQLCipher database.  
  *Warning: If you forget your password, your data is permanently lost.*

- **Message Exchange**:
  Each message is encrypted with an ephemeral X25519 key. The sender signs the message with their Ed25519 private key. The receiver verifies authenticity and decrypts confidentiality. Only "paired" contacts (scanned QR code) can exchange messages.

- **Ephemeral Messages**:
  The sender can mark a message as ephemeral (chosen lifespan: 1–10 min). The timer starts only after the recipient acknowledges receipt (ACK). Deletion occurs locally on both devices.

---

## 🚀 Usage

1. **Create an identity**:
   - Open the application.
   - Choose a pseudonym.
   - Set a strong password.
   - Wait for the Tor connection (may take a few minutes on first launch).

2. **Add a contact**:
   - Go to the **"My QR"** tab to view your code.
   - Ask your contact to scan your QR code.
   - Scan your contact's QR code to complete pairing.
   > ⚠️ **Security Note:** Pairing must strictly take place in person. Screenshots are intentionally blocked at the OS level to prevent exchanging QR codes over the Internet, eliminating the risk of accidental exposure or key compromise.

3. **Update the application**:
   - You can update the app seamlessly. The process automatically preserves your account identity, cryptographic keys, contacts, and personal settings.

4. **Send a message**:
   - Select a contact.
   - Type your message.
   - *(Optional)* Enable ephemeral mode via the **"⋮"** menu to set a message lifespan.
   - Send.

5. **Presence status**:
   - The application sends periodic pings to verify if the contact is online.
   - A green indicator shows that the contact is reachable.
