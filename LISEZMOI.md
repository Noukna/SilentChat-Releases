# Silent Chat
**Messagerie privée, anonyme et chiffrée de bout en bout.**

Silent Chat est une application Android qui permet de communiquer de manière sécurisée via le réseau Tor. Elle utilise un cœur de sécurité écrit en Rust (exposé via UniFFI) et une interface Kotlin native.

---

## 🛡️ Caractéristiques Principales

- **Anonymat total** : Toutes les communications transitent par le réseau Tor (implémentation Rust Arti). Aucune donnée de métadonnée n'est exposée.
- **Chiffrement de bout en bout (E2EE)** :
  - **Authentification** : Signatures Ed25519.
  - **Échange de clés** : X25519 (Diffie-Hellman éphémère).
  - **Chiffrement symétrique** : ChaCha20-Poly1305 (AEAD).
  - **Dérivation de clés** : HKDF (SHA-256).
- **Sécurité locale** : Base de données chiffrée avec SQLCipher. Identité chiffrée avec Argon2id (KDF résistant aux attaques par force brute). Effacement mémoire des secrets sensibles (Zeroize).
- **Messages Éphémères** : Possibilité de définir une durée de vie (1 à 10 minutes) pour les messages envoyés. La suppression est déclenchée localement après confirmation de la remise.
- **Appairage physique** : Ajout de contacts via scan de QR Code (pas de serveur central de contacts).
- **Mises à jour préservant les données** : Prise en charge des mises à jour de l'application tout en conservant l'intégralité de votre compte, de vos contacts et de vos réglages.

---

## 🏗️ Architecture Technique

L'application est divisée en deux couches distinctes pour maximiser la sécurité et la performance :

### 1. Cœur Rust (`native_core`)
Gère la logique critique, la cryptographie et le réseau.
- **Tor Client** : Basé sur la crate `arti-client` (v0.47). Gère le bootstrap, les circuits et les Onion Services.
- **Crypto** :
  - `ed25519-dalek` : Signatures.
  - `x25519-dalek` : Échange de clés.
  - `chacha20poly1305` : Chiffrement des messages.
  - `argon2` : Dérivation de la clé maître depuis le mot de passe.
- **Interfaçage** : Utilise UniFFI pour générer automatiquement les bindings Kotlin à partir du code Rust.

### 2. Application Android (Kotlin)
Gère l'UI, le stockage local et la coordination.
- **UI** : Interface native Kotlin (pas de WebView), thème sombre, optimisée pour la lisibilité.
- **Stockage** :
  - **SQLCipher** : Base de données chiffrée (contacts, messages, réglages).
  - **Fichiers chiffrés** : L'identité cryptographique est stockée dans un fichier chiffré avec la clé dérivée du mot de passe.
- **Réseau** : Aucune connexion directe à Internet pour les messages. Tout passe par le tunnel Tor géré par le cœur Rust.

---

## 🔐 Modèle de Sécurité

- **Identité** :
  À la première utilisation, l'utilisateur choisit un pseudonyme et un mot de passe. Le mot de passe est utilisé pour dériver une clé maître via Argon2id. Cette clé maître dérive deux clés : une pour chiffrer l'identité (Ed25519/X25519) et une pour chiffrer la base de données SQLCipher.  
  *Avertissement : Si vous oubliez votre mot de passe, vos données sont perdues définitivement.*

- **Échange de Messages** :
  Chaque message est chiffré avec une clé éphémère X25519. L'émetteur signe le message avec sa clé privée Ed25519. Le destinataire vérifie la signature (authenticité) et déchiffre le message (confidentialité). Seuls les contacts "appairés" (QR Code scanné) peuvent échanger des messages.

- **Messages Éphémères** :
  L'émetteur peut marquer un message comme éphémère (durée choisie : 1–10 min). Le minuteur démarre uniquement après que le destinataire a confirmé la réception (ACK). La suppression est effectuée localement sur les deux appareils.

---

## 🚀 Utilisation

1. **Créer une identité** :
   - Ouvrez l'application.
   - Choisissez un pseudonyme.
   - Définissez un mot de passe fort.
   - Attendez la connexion à Tor (peut prendre quelques minutes la première fois).

2. **Ajouter un contact** :
   - Allez dans l'onglet **"Mon QR"** pour voir votre code.
   - Demandez à votre contact de scanner votre QR Code.
   - Scannez le QR Code de votre contact pour l'ajouter.
   > ⚠️ **Remarque de sécurité :** Cet appairage doit obligatoirement s'effectuer en présentiel. Les captures d'écran sont volontairement bloquées au niveau du système afin d'empêcher l'échange ou la fuite de ces QR codes via Internet, garantissant ainsi qu'ils ne puissent pas être interceptés ou compromis.

3. **Mettre à jour l'application** :
   - Vous pouvez mettre à jour l'application en toute simplicité. La procédure conserve automatiquement votre compte, vos clés cryptographiques, vos contacts ainsi que vos préférences.

4. **Envoyer un message** :
   - Sélectionnez un contact.
   - Tapez votre message.
   - *(Optionnel)* Activez le mode éphémère via le menu **"⋮"** pour définir une durée de vie.
   - Envoyez.

5. **Statut de présence** :
   - L'application envoie des pings périodiques pour vérifier si le contact est en ligne.
   - Une pastille verte indique que le contact est joignable.
