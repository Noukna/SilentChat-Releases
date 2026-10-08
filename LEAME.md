# Silent Chat 🤫🛡️

> Mensajería para Android ultra segura, anónima y cifrada de extremo a extremo a través de la red Tor.

---

## 📖 Nivel 1 — Guía de inicio rápido (Walkthrough)

¡Bienvenido a **Silent Chat**!  
Silent Chat es una aplicación de mensajería centrada en la privacidad. Funciona a través de la red Tor, sin cuenta de usuario, sin servidor central y sin recolección de datos. Todo está cifrado de extremo a extremo.

---

### 1. Crear tu identidad
En el primer inicio, debes configurar dos elementos:
* **Tu seudónimo:** Es el nombre que verán tus contactos. Puedes elegir uno o usar el botón **« Aleatorio »** para generar un nombre anónimo (*ej: SilentFox42*).
* **Tu contraseña:** Es la piedra angular de tu seguridad. Cifra tus datos locales en el dispositivo.  
  ⚠️ *Atención: Es imposible de recuperar si la olvidas. Elígela con cuidado.*

---

### 2. Añadir contactos
Silent Chat funciona mediante emparejamiento físico, sin número de teléfono:
1. Ve a la pestaña **« Mi QR »** para mostrar tu código único.
2. Pide a tu contacto que escanee este código con su aplicación.
3. Invierte los papeles para que también te añada a su libreta de contactos.
4. Una vez añadidos, podéis chatear. Un **punto verde** indica que tu contacto está en línea.

---

### 3. Chatear de forma segura
La interfaz es sencilla y oscura para preservar la batería y mantener la discreción.
* **Mensajes de texto:** Envía mensajes estándar.
* **Mensajes de voz:** Mantén presionado el ícono del micrófono para grabar una nota de voz (1 minuto máximo).
* **Modo Efímero (1 a 1):** Por defecto, los mensajes se autodestruyen tras un plazo (5 min por defecto). Puedes ajustar esta duración (1 a 10 min) a través del menú del chat. Los mensajes recibidos siguen la configuración del remitente.

---

### 4. Salas efímeras (Chats grupales temporales)
Puedes crear salas de chat temporales con varias personas (hasta 100 miembros máximo):
* **Creación:** Mantén presionado (1 segundo) un contacto en tu lista y elige **« Crear una sala con este contacto »**.
* **Invitación:** Una vez dentro de la sala, usa el botón **« + »** para invitar a otras personas de tus propios contactos.
* **Banners e Invitaciones:** Aparece un banner en la pantalla de inicio para avisarte de una sala en curso o de una invitación pendiente.
* **Identidad de los miembros:**
  * Para un contacto directo, verás su nombre local.
  * Para un miembro no amigo (amigo de un amigo), su nombre aparece como `~Nombre`.
* **Cero huella:** La sala existe únicamente en la memoria temporal (RAM). Tan pronto como se cierra la sala o sales de la app, el historial se borra definitivamente.

---

### 5. Funciones avanzadas
Accede al menú principal (ícono **⋮**) para descubrir las potentes opciones de la aplicación:
* **Anonimizador de voz:** Activa esta opción para transformar tu voz en una voz neutra (ni masculina ni femenina) antes del envío. Tu voz original nunca se transmite.
* **Autodestrucción:** Configura un límite de intentos fallidos de contraseña (*ej: 4*). Si alguien intenta forzar el acceso a tu teléfono y falla esa cantidad de veces, la aplicación borra definitivamente todos tus datos y se desinstala.
* **Actualización:** Comprueba si hay nuevas versiones directamente en la app. Las actualizaciones conservan tus contactos y mensajes.
* **Compartir:** Envía el enlace de descarga de la aplicación a tus amigos por SMS, correo electrónico o mensajería privada (Telegram, Signal, etc.).
* **Donación:** Si aprecias el proyecto, puedes apoyar el desarrollo mediante una dirección Monero (XMR) disponible en el menú.

---

### 6. Seguridad, privacidad y notificaciones
* **Sin capturas de pantalla:** La aplicación bloquea las capturas de pantalla y la grabación de pantalla a nivel de sistema para evitar filtraciones.
* **Borrado local:** Tus datos se almacenan localmente de forma cifrada. Eliminar un contacto borra permanentemente toda la conversación de tu dispositivo.
* **Notificaciones confidenciales:** Para preservar tu privacidad, las notificaciones muestran solo un mensaje genérico (*« N nuevos mensajes »*). Ningún texto ni nombre de contacto se expone en la pantalla de bloqueo.
* **Exención de batería y segundo plano:** Para recibir mensajes con la pantalla apagada, la app solicita una exención de optimización de batería. En segundo plano, la conexión se mantiene abierta sin consumir recursos innecesariamente.
* **Multilingüe:** Disponible en 5 idiomas (Francés, Inglés, Chino, Español, Portugués). Puedes cambiar de idioma en cualquier momento desde el menú de inicio de sesión.
* 💡 *Recordatorio: La seguridad también depende de tus hábitos. Utiliza una contraseña robusta y nunca compartas tu código QR públicamente.*

---

## 🛡️ Nivel 2 — Características Principales y Modelo Funcional

### Privacidad y Red
* **Anonimato total:** Todas las comunicaciones transitan por la red Tor. Ningún metadato queda expuesto.
* **Emparejamiento físico exclusivo:** Adición de contactos únicamente mediante escaneo directo de código QR en persona (sin servidor central de contactos, sin búsqueda por número o correo).
* **Aislamiento del sistema:** Bloqueo a nivel de hardware y sistema operativo de capturas y grabaciones de pantalla.

### Salas Efímeras Multimiembro (Malla/Relés)
* **Arquitectura P2P/Relé:** Salas de hasta 100 miembros con un máximo de 8 nodos hijos directos por nodo.
* **Anonimato entre miembros:** No se establece ninguna conexión directa entre miembros no amigos. Sin intercambio de claves Ed25519 ni direcciones Onion entre no contactos.
* **Cero Persistencia en Disco:** Las claves de la sala, topología, lista de miembros e historial (limitado a 500 mensajes) se almacenan al 100% en memoria RAM.
* **Gestión ciega de invitaciones:** La expiración de una invitación (120 s) no revela si un contacto no estaba disponible o si rechazó la solicitud.

### Servicio en Segundo Plano y Notificaciones
* **Gestión del Foreground Service:** Mantenimiento de la conexión Tor mediante un servicio de segundo plano reactivo (`specialUse`/`dataSync`) adaptado a las restricciones de Android 12 a 15.
* **Notificaciones confidenciales:** Canal de alta prioridad que muestra contenido deliberadamente genérico sin filtración de metadatos.
* **Recuperación de mensajes (`RetryWorker`):** Tarea periódica (cada 15 min) que realiza un ping dirigido únicamente a los contactos que tienen mensajes pendientes de entrega.

---

## 🏗️ Nivel 3 — Arquitectura Técnica y Especificaciones Criptográficas

La aplicación se divide en dos capas distintas para separar la lógica crítica nativa de la capa de presentación.

```
┌────────────────────────────────────────────────────────┐
│               Android UI (Kotlin Native)               │
│   (UI oscura, Foreground Service, SQLCipher, RAM Store)│
└───────────────────────────┬────────────────────────────┘
                            │ UniFFI Bindings (MessageListener)
┌───────────────────────────▼────────────────────────────┐
│                  Núcleo Rust (native_core)             │
│  (Arti Tor Client, Motor Sala/Relé, Cripto, RAM)       │
└────────────────────────────────────────────────────────┘
```

### 1. Núcleo Rust (`native_core`)
Gestiona la lógica crítica, la criptografía de bajo nivel, la red Onion Services y las salas:
* **Tor Client:** Basado en la caja (*crate*) `arti-client` (v0.47). Gestiona el bootstrap, los circuitos anónimos y los Onion Services v3.
* **Motor de Salas Efímeras (`on_flood`):** Retransmite los bloques (*blobs*) cifrados sin descifrado intermedio.
* **Primitivas Criptográficas:**
  * `ed25519-dalek` : Firmas digitales y autenticación de identidad.
  * `x25519-dalek` : Intercambio de claves Diffie-Hellman efímero.
  * `chacha20poly1305` : Cifrado simétrico autenticado (AEAD).
  * `argon2` : Función de derivación de clave (KDF) de alta seguridad.
  * `zeroize` : Limpieza explícita de la memoria RAM de secretos sensibles al ejecutar `wipeRuntime` o al cerrar la app.
* **Interfaz UniFFI:** Genera las vinculaciones (*bindings*) Kotlin y expone el receptor extendido: `MessageListener` (`on_room_invite`, `on_room_message`, `on_room_event`).

### 2. Aplicación Android (Kotlin)
* **UI y Sesión:** Interfaz nativa de Android (sin WebView). El historial de las salas se conserva en RAM (máximo 500 mensajes).
* **Servicio en Segundo Plano (`Foreground Service`):**
  * Utiliza los tipos `specialUse` (API 34+) y `dataSync` (API 29-33) para eludir el límite de tiempo de 6 horas en Android 15.
  * Gestión del modo Activo/Pasivo: detiene la emisión de `tick()` en segundo plano manteniendo el socket abierto para pings entrantes.
  * Gestión del permiso `POST_NOTIFICATIONS` (Android 13+).
* **Almacenamiento:**
  * `SQLCipher` : Base de datos relacional cifrada (contactos, mensajes directos 1:1, metadatos).
  * `Archivos cifrados` : La identidad criptográfica se guarda en un contenedor cifrado por la clave derivada de Argon2id.
* **Programador:** `RetryWorker` reanuda los pings cada 15 min para contactos con mensajes pendientes.

---

## 🔐 Nivel 4 — Modelo Criptográfico y Flujos de Seguridad

### Generación y Aislamiento de la Identidad
1. En la primera configuración, la contraseña del usuario pasa por **Argon2id** para derivar una *Clave Maestra*.
2. La *Clave Maestra* deriva dos subclaves distintas:
   * $K_{ID}$ : Clave de cifrado del llavero de identidad criptográfica (par de claves Ed25519/X25519).
   * $K_{DB}$ : Clave de descifrado de la base de datos `SQLCipher`.

### Protocolo de Cifrado de Mensajes 1:1 (E2EE)
Para cada mensaje directo transmitido:
1. **Intercambio Efímero:** Se genera un par de claves efímeras X25519 para garantizar *Perfect Forward Secrecy* (PFS).
2. **Derivación:** Se deriva una clave de sesión mediante **HKDF-SHA256**.
3. **Cifrado Simétrico:** El contenido (texto o nota de voz) se cifra con **ChaCha20-Poly1305**.
4. **Firma y Autenticación:** El remitente firma el paquete con su clave privada **Ed25519**.
5. **Verificación:** El destinatario verifica la firma con la clave pública emparejada y descifra el contenido.

### Protocolo de Salas Efímeras (Cifrado de Doble Capa)
Las salas utilizan una superposición de dos capas criptográficas:

1. **Capa de Transporte Salto a Salto (Punto a Punto):**
   * Cada salto entre dos contactos directos reutiliza la capa de intercambio $X25519 + ChaCha20\text{-}Poly1305$ con firma $Ed25519$.
   * Uso de una sal HKDF específica: `schat-v1-room` para aislar herméticamente las tramas de la sala de los mensajes 1:1.
2. **Capa de Carga Útil (Sala):**
   * Generada por el anfitrión de la sala y transmitida de contacto en contacto dentro del blob de invitación.
   * El contenido se cifra de extremo a extremo con la clave de la sala.
3. **Enmascaramiento de Tamaño (Padding):**
   * Todos los mensajes de la sala se alinean con rellenos (*padding*) de tamaño $512 \text{ bytes} \times 2^n$ para evitar el análisis de tráfico basado en el tamaño de los paquetes.
4. **Anonimato de la Topología y Árbol de Relés:**
   * Cada miembro dispone de un identificador temporal aleatorio único para la sala.
   * Las pares de direcciones Onion y claves públicas Ed25519 reales de los miembros nunca se transmiten a no amigos.
   * La topología del árbol es conocida **únicamente por el anfitrión**. Los informes de topología ascendentes se cifran para la clave específica del anfitrión.
   * Las órdenes de control `GONE` (expulsión) y `CLOSE` (cierre de la sala) están firmadas obligatoriamente por la clave del anfitrión.
5. **Gestión del Latido (Cascada):**
   * Latido de control cada $10\text{ s}$. Tras $3$ fallos consecutivos o $75\text{ s}$ de silencio, el enlace se considera roto.
   * Si se pierde el nodo padre, el nodo se purga a sí mismo y transmite una orden `BYE` a sus nodos hijos.