# Silent Chat 🤫🛡️

> Messagerie Android ultra-sécurisée, anonyme et chiffrée de bout en bout via le réseau Tor.

---

## 📖 Niveau 1 — Guide de prise en main (Walkthrough)

Bienvenue dans **Silent Chat** !  
Silent Chat est une messagerie prioritaire sur la vie privée. Elle fonctionne via le réseau Tor, sans compte, sans serveur central et sans collecte de données. Tout est chiffré de bout en bout.

---

### 1. Créer votre identité
Au premier lancement, vous devez définir deux éléments :
* **Votre pseudonyme :** C'est le nom que vos contacts verront. Vous pouvez en choisir un ou utiliser le bouton **« Aléatoire »** pour générer un nom anonyme (*ex: SilentFox42*).
* **Votre mot de passe :** C'est la clé de voûte de votre sécurité. Il chiffre vos données sur l'appareil.  
  ⚠️ *Attention : Il est impossible à récupérer si vous l'oubliez. Choisissez-le avec soin.*

---

### 2. Ajouter des contacts
Silent Chat fonctionne par appairage physique, sans numéro de téléphone :
1. Allez dans l'onglet **« Mon QR »** pour afficher votre code unique.
2. Demandez à votre contact de scanner ce code avec son application.
3. Inversez les rôles pour qu'il vous ajoute également à son carnet.
4. Une fois ajoutés, vous pouvez discuter. Un **point vert** indique que votre contact est en ligne.

---

### 3. Discuter en toute sécurité
L'interface est épurée et sombre pour préserver la batterie et la discrétion.
* **Messages texte :** Envoyez des messages classiques.
* **Messages vocaux :** Appuyez sur l'icône micro pour enregistrer un vocal (1 minute max).
* **Mode Éphémère (1 à 1) :** Par défaut, les messages s'auto-détruisent après un délai (5 min par défaut). Vous pouvez ajuster cette durée (1 à 10 min) via le menu de la discussion. Les messages reçus suivent le réglage de l'émetteur.

---

### 4. Salons éphémères (Discussions de groupe temporaires)
Vous pouvez créer des salons de discussion temporaires regroupant plusieurs personnes (jusqu'à 100 membres max) :
* **Création :** Maintenez un appui long (1 seconde) sur un contact dans votre liste, puis choisissez **« Créer un salon avec ce contact »**.
* **Invitation :** Une fois dans le salon, utilisez le bouton **« + »** pour inviter d'autres personnes parmi vos propres contacts.
* **Bannières & Invitations :** Une bannière s'affiche sur l'écran d'accueil pour vous signaler un salon en cours ou une invitation en attente.
* **Identité des membres :**
  * Pour un contact direct, vous verrez son nom local.
  * Pour un membre non-ami (ami d'un ami), son nom apparaît sous la forme `~Nom`.
* **Zéro trace :** Le salon existe uniquement en mémoire temporaire (RAM). Dès que le salon est fermé ou que vous quittez l'app, l'historique du salon est définitivement effacé.

---

### 5. Fonctionnalités avancées
Accédez au menu principal (icône **⋮**) pour découvrir les options puissantes de l'application :
* **Anonymisateur de voix :** Activez cette option pour transformer votre voix en une voix neutre (ni masculine, ni féminine) avant l'envoi. Votre voix d'origine n'est jamais transmise.
* **Autodestruction :** Configurez un nombre d'échecs de mot de passe (*ex: 4*). Si quelqu'un tente de forcer l'accès à votre téléphone et échoue ce nombre de fois, l'application efface définitivement toutes vos données et se désinstalle.
* **Mise à jour :** Vérifiez les nouvelles versions directement dans l'app. Les mises à jour conservent vos contacts et messages.
* **Partage :** Envoyez le lien de téléchargement de l'application à vos amis via SMS, e-mail ou messageries privées (Telegram, Signal, etc.).
* **Donation :** Si vous appréciez le projet, vous pouvez soutenir le développement via une adresse Monero (XMR) disponible dans le menu.

---

### 6. Sécurité, confidentialité et notifications
* **Pas de capture d'écran :** L'application bloque les captures d'écran et l'enregistrement d'écran au niveau système pour empêcher les fuites.
* **Effacement local :** Vos données sont stockées localement et chiffrées. Supprimer un contact efface définitivement toute la discussion de votre appareil.
* **Notifications confidentielles :** Pour préserver votre vie privée, les notifications affichent uniquement un message générique (*« N nouveaux messages »*). Aucun texte ni nom de contact n'est jamais exposé sur l'écran de verrouillage.
* **Exemption de batterie & Arrière-plan :** Pour recevoir les messages écran éteint, l'app demande une exemption d'optimisation batterie. En arrière-plan, la connexion reste ouverte sans consommer inutilement de ressource.
* **Multilingue :** Disponible en 5 langues (Français, Anglais, Chinois, Espagnol, Portugais). Vous pouvez changer de langue à tout moment dans le menu de connexion.
* 💡 *Rappel : La sécurité dépend aussi de vos habitudes. Utilisez un mot de passe fort et ne partagez jamais votre QR code publiquement.*

---

## 🛡️ Niveau 2 — Caractéristiques Principales & Modèle Fonctionnel

### Confidentialité & Réseau
* **Anonymat total :** Toutes les communications transitent par le réseau Tor. Aucune métadonnée n'est exposée.
* **Appairage physique exclusif :** Ajout de contacts uniquement par scan QR Code direct en présentiel (pas de serveur central de contacts, pas de découverte par numéro ou email).
* **Isolation système :** Blocage matériel/OS des captures et enregistrements d'écran.

### Salons Éphémères Multi-membres (Mesh/Relais)
* **Architecture P2P/Relais :** Salons jusqu'à 100 membres avec maximum 8 enfants directs par nœud.
* **Anonymat inter-membres :** Aucune connexion directe n'est tentée entre membres non-amis. Aucun échange de clé Ed25519 ou d'adresse Onion entre non-contacts.
* **Zéro Persistance Disque :** Clés de salon, topologie, roster et historique (limité à 500 messages) sont stockés à 100% en mémoire RAM.
* **Gestion d'invitation aveugle :** L'expiration d'une invitation (120 s) ne révèle pas si un contact est indisponible ou s'il a refusé.

### Service d'Arrière-plan & Notifications
* **Gestion du Foreground Service :** Maintien de la connexion Tor via un service d'arrière-plan réactif (`specialUse`/`dataSync`) adapté aux contraintes d'Android 12 à 15.
* **Notifications confidentielles :** Canal haute priorité affichant un contenu volontairement générique sans aucune fuite de métadonnées.
* **Rattrapage des messages (`RetryWorker`) :** Tâche périodique (toutes les 15 min) effectuant un ping ciblé uniquement vers les contacts ayant des messages en attente de livraison.

---

## 🏗️ Niveau 3 — Architecture Technique & Spécifications Cryptographiques

L'application est divisée en deux couches distinctes pour séparer la logique critique native de la couche de présentation.

```
┌────────────────────────────────────────────────────────┐
│               Android UI (Kotlin Native)               │
│   (UI sombre, Foreground Service, SQLCipher, RAM Store)│
└───────────────────────────┬────────────────────────────┘
                            │ UniFFI Bindings (MessageListener)
┌───────────────────────────▼────────────────────────────┐
│                  Cœur Rust (native_core)               │
│  (Arti Tor Client, Moteur Salon/Relais, Crypto, RAM)   │
└────────────────────────────────────────────────────────┘
```

### 1. Cœur Rust (`native_core`)
Gère la logique critique, la cryptographie bas niveau, le réseau Onion Services et les salons :
* **Tor Client :** Basé sur la crate `arti-client` (v0.47). Gère le bootstrap, les circuits anonymes et les Onion Services v3.
* **Moteur de Salons Éphémères (`on_flood`) :** Re-transmet les blobs chiffrés sans déchiffrement intermédiaire.
* **Primitives Cryptographiques :**
  * `ed25519-dalek` : Signatures numériques et authentification d'identité.
  * `x25519-dalek` : Échange de clés Diffie-Hellman éphémère.
  * `chacha20poly1305` : Chiffrement symétrique authentifié (AEAD).
  * `argon2` : Fonction de dérivation de clé (KDF) haute sécurité.
  * `zeroize` : Nettoyage explicite de la mémoire RAM des secrets sensibles lors du `wipeRuntime` ou de la fermeture.
* **Interfaçage UniFFI :** Génère les bindings Kotlin et expose le listener étendu : `MessageListener` (`on_room_invite`, `on_room_message`, `on_room_event`).

### 2. Application Android (Kotlin)
* **UI & Session :** Interface native Android (sans WebView). L'historique des salons est conservé en RAM (max 500 messages).
* **Service d'arrière-plan (`Foreground Service`) :**
  * Utilise les types `specialUse` (API 34+) et `dataSync` (API 29-33) pour contourner le timeout de 6h sur Android 15.
  * Gestion du mode Actif/Passif : stoppe l'émission de `tick()` en arrière-plan tout en conservant la socket ouverte pour les pings entrants.
  * Gestion de la permission `POST_NOTIFICATIONS` (Android 13+).
* **Stockage :**
  * `SQLCipher` : Base de données relationnelle chiffrée (contacts, messages direct 1:1, métadonnées).
  * `Fichiers chiffrés` : L'identité cryptographique est conservée dans un conteneur chiffré par la clé issue d'Argon2id.
* **Planificateur :** `RetryWorker` relance les pings toutes les 15 min pour les contacts ayant des messages en attente.

---

## 🔐 Niveau 4 — Modèle Cryptographique & Flux de Sécurité

### Génération & Isolation de l'Identité
1. Au premier démarrage, le mot de passe utilisateur passe dans **Argon2id** pour dériver une *Clé Maître*.
2. La *Clé Maître* dérive deux sous-clés distinctes :
   * $K_{ID}$ : Clé de chiffrement du trousseau d'identité cryptographique (paire Ed25519/X25519).
   * $K_{DB}$ : Clé de déchiffrement de la base de données `SQLCipher`.

### Protocole de Chiffrement des Messages 1:1 (E2EE)
Pour chaque message direct transmis :
1. **Échange Éphémère :** Un couple de clés éphémères X25519 est généré pour assurer le *Perfect Forward Secrecy* (PFS).
2. **Dérivation :** Une clé de session est dérivée via **HKDF-SHA256**.
3. **Chiffrement Symétrique :** Le contenu (texte ou vocal) est chiffré via **ChaCha20-Poly1305**.
4. **Signature & Authentification :** L'émetteur signe le paquet avec sa clé privée **Ed25519**.
5. **Vérification :** Le destinataire vérifie la signature avec la clé publique appairée et déchiffre le contenu.

### Protocole des Salons Éphémères (Chiffrement à Deux Couches)
Les salons utilisent une superposition de deux couches cryptographiques :

1. **Couche Transport Hop-by-Hop (Point à Point) :**
   * Chaque saut entre deux contacts directs réutilise la couche d'échange $X25519 + ChaCha20\text{-}Poly1305$ avec signature $Ed25519$.
   * Utilisation d'un sel HKDF spécifique : `schat-v1-room` afin d'isoler hermétiquement les trames de salon des messages 1:1.
2. **Couche Payload (Salon) :**
   * Générée par l'hôte du salon et transmise de contact à contact au sein du blob d'invitation.
   * Le contenu est chiffré de bout en bout avec la clé du salon.
3. **Masquage de Taille (Padding) :**
   * Tous les messages de salon sont alignés par des rembourrages (padding) de taille $512 \text{ octets} \times 2^n$ afin d'empêcher l'analyse de trafic basée sur la taille des paquets.
4. **Anonymat de la Topologie & Arbre de Relais :**
   * Chaque membre dispose d'un identifiant temporaire aléatoire unique au salon.
   * Les paires d'adresses Onion et clés publiques Ed25519 réelles des membres ne sont jamais transmises aux non-amis.
   * La topologie de l'arbre est connue **uniquement par l'hôte**. Les rapports de topologie remontants sont chiffrés pour la clé spécifique de l'hôte.
   * Les ordres de contrôle `GONE` (éjection) et `CLOSE` (fermeture du salon) sont obligatoirement signés par la clé de l'hôte.
5. **Gestion du Battement de Cœur (Cascade) :**
   * Battement toutes les $10\text{ s}$. Après $3$ échecs consécutifs ou $75\text{ s}$ de silence, le lien est considéré comme rompu.
   * Si le parent est perdu, le nœud se purge et transmet un ordre `BYE` à ses enfants.