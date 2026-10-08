# Silent Chat 🤫🛡️

> Mensageiro para Android ultra-seguro, anônimo e criptografado de ponta a ponta através da rede Tor.

---

## 📖 Nível 1 — Guia do Usuário (Walkthrough)

Bem-vindo ao **Silent Chat**!  
O Silent Chat é um aplicativo de mensagens focado em privacidade. Ele funciona através da rede Tor, sem conta de usuário, sem servidor central e sem coleta de dados. Tudo é criptografado de ponta a ponta.

---

### 1. Criar sua identidade
No primeiro acesso, você deve definir dois elementos:
* **Seu pseudônimo:** É o nome que seus contatos verão. Você pode escolher um ou usar o botão **« Aleatório »** para gerar um nome anônimo (*ex: SilentFox42*).
* **Sua senha:** É a chave mestra da sua segurança. Ela criptografa seus dados locais no dispositivo.  
  ⚠️ *Atenção: É impossível recuperá-la caso você a esqueça. Escolha com cuidado.*

---

### 2. Adicionar contatos
O Silent Chat funciona por pareamento físico, sem necessidade de número de telefone:
1. Vá até a aba **« Meu QR »** para exibir seu código exclusivo.
2. Peça ao seu contato para escanear este código com o aplicativo dele.
3. Invertam os papéis para que ele também adicione você à lista dele.
4. Uma vez adicionados, vocês podem conversar. Um **ponto verde** indica que seu contato está online.

---

### 3. Conversar com segurança
A interface é simples e escura para economizar bateria e manter a discrição.
* **Mensagens de texto:** Envie mensagens de texto padrão.
* **Mensagens de voz:** Mantenha pressionado o ícone de microfone para gravar um áudio (máximo de 1 minuto).
* **Modo Efêmero (1 para 1):** Por padrão, as mensagens se autodestroem após um tempo determinado (5 min por padrão). Você pode ajustar essa duração (1 a 10 min) através do menu da conversa. As mensagens recebidas seguem a configuração do remetente.

---

### 4. Salas efêmeras (Conversas em grupo temporárias)
Você pode criar salas de conversa temporárias reunindo várias pessoas (até 100 membros no máximo):
* **Criação:** Pressione e segure (1 segundo) sobre um contato na sua lista e escolha **« Criar uma sala com este contato »**.
* **Convite:** Dentro da sala, use o botão **« + »** para convidar outras pessoas da sua própria lista de contatos.
* **Banners e Convites:** Um banner é exibido na tela inicial para notificar sobre uma sala em andamento ou um convite pendente.
* **Identidade dos membros:**
  * Para um contato direto, você verá o nome local definido por você.
  * Para um membro não amigo (amigo de um amigo), o nome aparecerá no formato `~Nome`.
* **Zero rastro:** A sala existe apenas na memória temporária (RAM). Assim que a sala é fechada ou você sai do app, o histórico é apagado permanentemente.

---

### 5. Recursos avançados
Acesse o menu principal (ícone **⋮**) para descobrir opções avançadas do aplicativo:
* **Anonimizador de voz:** Ative esta opção para transformar sua voz em um tom neutro (nem masculino, nem feminino) antes do envio. Sua voz original nunca é transmitida.
* **Autodestruição:** Configure um número máximo de tentativas incorretas de senha (*ex: 4*). Se alguém tentar forçar o acesso ao seu celular e errar essa quantidade de vezes, o app apaga permanentemente todos os dados e se desinstala.
* **Atualização:** Verifique se há novas versões diretamente no aplicativo. As atualizações preservam seus contatos e mensagens.
* **Compartilhar:** Envie o link de download do aplicativo para seus amigos por SMS, e-mail ou mensagens privadas (Telegram, Signal, etc.).
* **Doação:** Se você gosta do projeto, pode apoiar o desenvolvimento através de um endereço Monero (XMR) disponível no menu.

---

### 6. Segurança, privacidade e notificações
* **Sem captura de tela:** O aplicativo bloqueia capturas e gravações de tela em nível de sistema para evitar vazamentos de dados.
* **Apagamento local:** Seus dados são armazenados localmente e criptografados. Apagar um contato exclui permanentemente toda a conversa do seu dispositivo.
* **Notificações confidenciais:** Para preservar sua privacidade, as notificações exibem apenas uma mensagem genérica (*« N novas mensagens »*). Nenhum texto ou nome de contato é exibido na tela de bloqueio.
* **Isenção de otimização de bateria & Segundo plano:** Para receber mensagens com a tela desligada, o app solicita isenção da otimização de bateria. Em segundo plano, a conexão permanece ativa sem consumo desnecessário.
* **Multilíngue:** Disponível em 5 idiomas (Francês, Inglês, Chinês, Espanhol, Português). Você pode alterar o idioma a qualquer momento no menu de login.
* 💡 *Lembrete: A segurança também depende dos seus hábitos. Use uma senha forte e nunca compartilhe seu QR code publicamente.*

---

## 🛡️ Nível 2 — Principais Recursos & Modelo Funcional

### Privacidade & Rede
* **Anonimato total:** Todas as comunicações trafegam pela rede Tor. Nenhuma metadado é exposta.
* **Pareamento físico exclusivo:** Adição de contatos feita exclusivamente por escaneamento presencial de QR Code (sem servidor central, sem busca por telefone ou e-mail).
* **Isolamento de sistema:** Bloqueio via hardware/SO de capturas e gravações de tela.

### Salas Efêmeras Multimembros (Malha/Relés)
* **Arquitetura P2P/Relé:** Salas de até 100 membros com no máximo 8 nós filhos diretos por nó.
* **Anonimato entre membros:** Nenhuma conexão direta é estabelecida entre membros não amigos. Nenhuma chave Ed25519 ou endereço Onion é trocado entre não contatos.
* **Zero Persistência em Disco:** Chaves da sala, topologia, lista de membros e histórico (limitado a 500 mensagens) são mantidos 100% em memória RAM.
* **Gerenciamento cego de convites:** A expiração de um convite (120 s) não revela se o contato estava indisponível ou se recusou a solicitação.

### Serviço em Segundo Plano & Notificações
* **Gerenciamento do Foreground Service:** Mantém a conexão Tor ativa via serviço de segundo plano reativo (`specialUse`/`dataSync`) adequado às restrições do Android 12 ao 15.
* **Notificações confidenciais:** Canal de alta prioridade exibindo conteúdo propositalmente genérico sem vazamento de metadados.
* **Recuperação de mensagens (`RetryWorker`):** Tarefa periódica (a cada 15 min) que envia pings direcionados apenas aos contatos com mensagens pendentes de entrega.

---

## 🏗️ Nível 3 — Arquitetura Técnica & Especificações Criptográficas

O aplicativo é dividido em duas camadas distintas para separar a lógica crítica nativa da camada de apresentação.

```
┌────────────────────────────────────────────────────────┐
│               Android UI (Kotlin Native)               │
│   (UI escura, Foreground Service, SQLCipher, RAM Store)│
└───────────────────────────┬────────────────────────────┘
                            │ UniFFI Bindings (MessageListener)
┌───────────────────────────▼────────────────────────────┐
│                 Núcleo Rust (native_core)              │
│  (Arti Tor Client, Motor de Sala/Relé, Cripto, RAM)    │
└────────────────────────────────────────────────────────┘
```

### 1. Núcleo Rust (`native_core`)
Gerencia a lógica crítica, criptografia de baixo nível, rede Onion Services e salas efêmeras:
* **Tor Client:** Baseado na crate `arti-client` (v0.47). Gerencia o bootstrap, circuitos anônimos e Onion Services v3.
* **Motor de Salas Efêmeras (`on_flood`):** Retransmite os blocos (*blobs*) criptografados sem descriptografia intermediária.
* **Primitivas Criptográficas:**
  * `ed25519-dalek` : Assinaturas digitais e autenticação de identidade.
  * `x25519-dalek` : Troca de chaves Diffie-Hellman efêmera.
  * `chacha20poly1305` : Criptografia simétrica autenticada (AEAD).
  * `argon2` : Função de derivação de chave (KDF) de alta segurança.
  * `zeroize` : Limpeza explícita de memória RAM para segredos sensíveis ao executar `wipeRuntime` ou ao fechar o app.
* **Interface UniFFI:** Gera os bindings para Kotlin e expõe o receptor estendido: `MessageListener` (`on_room_invite`, `on_room_message`, `on_room_event`).

### 2. Aplicação Android (Kotlin)
* **Interface & Sessão:** Interface nativa do Android (sem WebView). O histórico das salas é mantido em RAM (máximo de 500 mensagens).
* **Serviço em Segundo Plano (`Foreground Service`):**
  * Utiliza os tipos `specialUse` (API 34+) e `dataSync` (API 29-33) para contornar o limite de 6 horas do Android 15.
  * Gerenciamento do modo Ativo/Passivo: interrompe invocações do `tick()` em segundo plano mantendo o socket aberto para pings recebidos.
  * Gerenciamento de permissão `POST_NOTIFICATIONS` (Android 13+).
* **Armazenamento:**
  * `SQLCipher` : Banco de dados relacional criptografado (contatos, mensagens diretas 1:1, metadados).
  * `Arquivos Criptografados` : A identidade criptográfica é salva em um contêiner protegido pela chave derivada do Argon2id.
* **Agendador:** `RetryWorker` executa pings a cada 15 min para contatos com mensagens pendentes.

---

## 🔐 Nível 4 — Modelo Criptográfico & Fluxos de Segurança

### Geração & Isolamento de Identidade
1. Na configuração inicial, a senha do usuário passa pelo **Argon2id** para derivar uma *Chave Mestra*.
2. A *Chave Mestra* deriva duas subchaves distintas:
   * $K_{ID}$ : Chave de criptografia do chaveiro de identidade criptográfica (par de chaves Ed25519/X25519).
   * $K_{DB}$ : Chave de descriptografia do banco de dados `SQLCipher`.

### Protocolo de Criptografia de Mensagens 1:1 (E2EE)
Para cada mensagem direta transmitida:
1. **Troca Efêmera:** Um par de chaves efêmeras X25519 é gerado para garantir *Perfect Forward Secrecy* (PFS).
2. **Derivação:** Uma chave de sessão é derivada via **HKDF-SHA256**.
3. **Criptografia Simétrica:** O conteúdo (texto ou nota de voz) é criptografado com **ChaCha20-Poly1305**.
4. **Assinatura & Autenticação:** O remetente assina o pacote com sua chave privada **Ed25519**.
5. **Verificação:** O destinatário verifica a assinatura usando a chave pública correspondente e descriptografa o conteúdo.

### Protocolo de Salas Efêmeras (Criptografia em Duas Camadas)
As salas usam uma sobreposição de duas camadas criptográficas:

1. **Camada de Transporte Salto a Salto (Ponto a Ponto):**
   * Cada salto entre contatos diretos reutiliza a camada de troca $X25519 + ChaCha20\text{-}Poly1305$ com assinatura $Ed25519$.
   * Utilização de um sal HKDF específico: `schat-v1-room` para isolar estritamente os quadros da sala das mensagens 1:1.
2. **Camada de Carga Útil (Sala):**
   * Gerada pelo anfitrião da sala e transmitida salto a salto dentro do blob de convite.
   * O conteúdo é criptografado de ponta a ponta com a chave da sala.
3. **Ocultação de Tamanho (Padding):**
   * Todas as mensagens da sala são ajustadas com preenchimentos (*padding*) de tamanho $512 \text{ bytes} \times 2^n$ para evitar análise de tráfego baseada no tamanho dos pacotes.
4. **Anonimato da Topologia & Árvore de Relés:**
   * Cada membro recebe um identificador temporário aleatório exclusivo para a sala.
   * Endereços Onion e chaves públicas Ed25519 reais dos membros nunca são revelados a não amigos.
   * A topologia da árvore é conhecida **exclusivamente pelo anfitrião**. Os relatórios de topologia ascendentes são criptografados para a chave do anfitrião.
   * Comandos de controle `GONE` (expulsão) e `CLOSE` (encerramento da sala) devem ser assinados pela chave do anfitrião.
5. **Gerenciamento de Batimento Cardíaco (Cascata):**
   * Sinal de controle a cada $10\text{ s}$. Após $3$ falhas consecutivas ou $75\text{ s}$ de silêncio, a conexão é considerada morta.
   * Se o nó pai for perdido, o nó limpa seu estado de memória e propaga um comando `BYE` aos seus nós filhos.