# 🇮🇹 P2P Chat Application

Questa applicazione implementa una chat **peer-to-peer reale** con interfaccia grafica Tkinter e crittografia simmetrica tramite `cryptography.Fernet`.

Ogni peer è autonomo: si comporta sia da **server** che da **client**, con connessione diretta tra i due peer.

Le emoji vengono visualizzate come immagini PNG.

---

## 📁 Struttura delle directory

```text
p2p_chat/
├── emoji_png/         # immagini emoji
├── avatar1.png        # avatar utente 1
├── avatar2.png        # avatar utente 2
├── chat_history.txt   # cronologia chat
├── chat_ui.py         # interfaccia grafica
├── p2p_core.py        # logica P2P e crittografia
├── peer_app.py        # modulo principale dell'app peer
├── requirements.txt   # dipendenze Python
└── venv/              # ambiente virtuale Python
```

---

## 🧰 Requisiti di sistema

* Python 3.8+
* `python3-tk` (necessario per Tkinter)
* `git`
* `pip`
* `venv`

---

## 📦 Installazione su Ubuntu/Debian

Aggiornare i pacchetti:

```bash
sudo apt-get update
```

Installare Tkinter e il supporto agli ambienti virtuali:

```bash
sudo apt-get install python3-tk python3-venv
```

---

## ⚙️ Configurazione dell'ambiente virtuale

Entrare nella directory del progetto:

```bash
cd ~/progetto/p2p_chat
```

Creare l'ambiente virtuale:

```bash
python3 -m venv venv
```

Attivarlo:

```bash
source venv/bin/activate
```

Aggiornare `pip`:

```bash
pip install --upgrade pip
```

Installare le dipendenze:

```bash
pip install -r requirements.txt
```

---

## 🗂️ Contenuto di `requirements.txt`

Il file `requirements.txt` deve contenere:

```text
Pillow
cryptography
```

---

## 😀 Supporto alle emoji

Le emoji vengono caricate come immagini PNG dalla directory:

```text
emoji_png/
```

I file devono essere denominati utilizzando il relativo **Unicode codepoint**.

Per le emoji predefinite:

```text
😀  😂  😍  😎  😢  👍  🙏  🎉  💡  🔥
```

sono necessari i seguenti file:

```text
1f600.png
1f602.png
1f60d.png
1f60e.png
1f622.png
1f44d.png
1f64f.png
1f389.png
1f4a1.png
1f525.png
```

La struttura sarà quindi simile a:

```text
emoji_png/
├── 1f600.png
├── 1f602.png
├── 1f60d.png
├── 1f60e.png
├── 1f622.png
├── 1f44d.png
├── 1f64f.png
├── 1f389.png
├── 1f4a1.png
└── 1f525.png
```

---

## 🔐 Chiave di crittografia

Nel file `p2p_core.py` è presente una chiave Fernet, ad esempio:

```python
key = b'WnXYeB4X_QJDIDktc9x0YgZz8zAkAO8ODHdxHZMROj8='
```

> **Nota:** per un progetto reale è consigliabile non inserire la chiave direttamente nel codice sorgente.

### Generare una nuova chiave

È possibile generare una nuova chiave con:

```bash
python3 - << 'EOF'
from cryptography.fernet import Fernet
print(Fernet.generate_key().decode())
EOF
```

Il comando produrrà una chiave simile a:

```text
WnXYeB4X_QJDIDktc9x0YgZz8zAkAO8ODHdxHZMROj8=
```

Sostituire quindi la chiave presente in `p2p_core.py`.

**Importante:** entrambi i peer devono utilizzare la **stessa chiave di crittografia**, altrimenti non saranno in grado di decifrare i messaggi ricevuti.

---

## 🚀 Avvio dell'applicazione

Per avviare la chat sono necessari **due terminali distinti**, uno per ciascun peer.

### Terminale 1 — Peer 1

Attivare l'ambiente virtuale:

```bash
source venv/bin/activate
```

Avviare il peer:

```bash
python3 peer_app.py 1
```

### Terminale 2 — Peer 2

Attivare l'ambiente virtuale:

```bash
source venv/bin/activate
```

Avviare il secondo peer:

```bash
python3 peer_app.py 2
```

Ogni comando aprirà una finestra Tkinter corrispondente a un peer.

La comunicazione avviene direttamente tra i due peer.

---

## 🔄 Funzionamento

Ogni peer può svolgere contemporaneamente il ruolo di:

* **Server**, per accettare connessioni;
* **Client**, per connettersi all'altro peer.

La comunicazione segue quindi un modello:

```text
┌──────────────┐             ┌──────────────┐
│    Peer 1    │◄───────────►│    Peer 2    │
│              │   P2P       │              │
│ Server/Client│             │ Server/Client│
└──────────────┘             └──────────────┘
```

I messaggi vengono cifrati prima di essere trasmessi attraverso la rete e decifrati dal peer destinatario.

---

## ✅ Funzionalità

### ✉️ Messaggi

I messaggi vengono:

1. inseriti dall'utente;
2. cifrati prima della trasmissione;
3. inviati direttamente all'altro peer;
4. decifrati dal destinatario;
5. visualizzati nella chat.

### 🔐 Crittografia

La comunicazione utilizza:

```text
cryptography.Fernet
```

Fernet fornisce una cifratura simmetrica autenticata.

### 😀 Emoji

Le emoji sono integrate nell'interfaccia grafica e vengono visualizzate utilizzando immagini PNG.

### 🕓 Cronologia

La cronologia della chat viene salvata localmente nel file:

```text
chat_history.txt
```

La cronologia conserva fino a **200 messaggi**.

---

## 🧹 Pulizia dell'ambiente

Per disattivare l'ambiente virtuale:

```bash
deactivate
```

Per rimuovere l'ambiente virtuale, la cache Python e la cronologia:

```bash
rm -rf venv __pycache__ chat_history.txt
```

---

## ❓ FAQ

### Perché non posso usare `pip install tkinter`?

Tkinter non viene normalmente installato tramite `pip`.

Su Ubuntu/Debian viene fornito dal pacchetto di sistema:

```bash
sudo apt-get install python3-tk
```

---

### Come posso aggiungere nuove emoji?

1. Inserire il file PNG nella directory:

```text
emoji_png/
```

2. Utilizzare come nome del file il relativo Unicode codepoint.

3. Aggiungere il simbolo alla lista `emoji_list` presente in:

```text
chat_ui.py
```

Ad esempio:

```python
emoji_list = ['😀', '😂', '😍', '😎', '😢', '👍', '🙏', '🎉', '💡', '🔥']
```

---

# 🇬🇧 P2P Chat Application

This project implements a **real peer-to-peer chat application** using a Tkinter graphical interface and symmetric encryption through `cryptography.Fernet`.

Each peer is autonomous and acts as both a **server** and a **client**, allowing direct communication between the two peers.

Emojis are rendered as PNG images.

---

## 📁 Directory Structure

```text
p2p_chat/
├── emoji_png/         # emoji images
├── avatar1.png        # peer 1 avatar
├── avatar2.png        # peer 2 avatar
├── chat_history.txt   # chat history
├── chat_ui.py         # GUI interface
├── p2p_core.py        # P2P and encryption logic
├── peer_app.py        # main peer launcher
├── requirements.txt   # Python dependencies
└── venv/              # Python virtual environment
```

---

## 🧰 System Requirements

* Python 3.8+
* `python3-tk`
* `git`
* `pip`
* `venv`

---

## 📦 Installation on Ubuntu/Debian

Update the package list:

```bash
sudo apt-get update
```

Install Tkinter and Python virtual environment support:

```bash
sudo apt-get install python3-tk python3-venv
```

---

## ⚙️ Virtual Environment Setup

Go to the project directory:

```bash
cd ~/project/p2p_chat
```

Create the virtual environment:

```bash
python3 -m venv venv
```

Activate it:

```bash
source venv/bin/activate
```

Upgrade `pip`:

```bash
pip install --upgrade pip
```

Install the project dependencies:

```bash
pip install -r requirements.txt
```

---

## 🗂️ `requirements.txt`

The `requirements.txt` file should contain:

```text
Pillow
cryptography
```

---

## 😀 Emoji Support

Emoji images must be placed inside:

```text
emoji_png/
```

The PNG files must be named using their Unicode codepoint.

Default emoji set:

```text
😀  😂  😍  😎  😢  👍  🙏  🎉  💡  🔥
```

Required files:

```text
1f600.png
1f602.png
1f60d.png
1f60e.png
1f622.png
1f44d.png
1f64f.png
1f389.png
1f4a1.png
1f525.png
```

---

## 🔐 Encryption Key

The Fernet encryption key is defined in `p2p_core.py`:

```python
key = b'WnXYeB4X_QJDIDktc9x0YgZz8zAkAO8ODHdxHZMROj8='
```

> **Note:** for a real-world project, storing the encryption key directly in the source code is not recommended.

### Generate a new key

Run:

```bash
python3 - << 'EOF'
from cryptography.fernet import Fernet
print(Fernet.generate_key().decode())
EOF
```

Replace the existing key in `p2p_core.py` with the generated one.

**Important:** both peers must use the **same encryption key** in order to decrypt each other's messages.

---

## 🚀 Running the Application

Open **two separate terminals**, one for each peer.

### Terminal 1 — Peer 1

```bash
source venv/bin/activate
python3 peer_app.py 1
```

### Terminal 2 — Peer 2

```bash
source venv/bin/activate
python3 peer_app.py 2
```

Each command launches a Tkinter window representing one peer.

The two peers communicate directly with each other.

---

## 🔄 How It Works

Each peer can act simultaneously as:

* **Server**, accepting incoming connections;
* **Client**, connecting to the other peer.

Architecture:

```text
┌──────────────┐             ┌──────────────┐
│    Peer 1    │◄───────────►│    Peer 2    │
│              │    P2P       │              │
│ Server/Client│             │ Server/Client│
└──────────────┘             └──────────────┘
```

Messages are encrypted before being transmitted over the network and decrypted by the receiving peer.

---

## ✅ Features

* 🔒 Encrypted messaging
* 🔄 Direct peer-to-peer communication
* 🖥️ Tkinter graphical interface
* 😀 Emoji support
* 💾 Local chat history
* 🕓 Up to 200 stored messages

---

## 🧹 Cleanup

Deactivate the virtual environment:

```bash
deactivate
```

Remove the virtual environment, Python cache and chat history:

```bash
rm -rf venv __pycache__ chat_history.txt
```

---

## ❓ FAQ

### Why can't I install Tkinter with `pip`?

Tkinter is normally provided by the operating system rather than installed through `pip`.

On Ubuntu/Debian:

```bash
sudo apt-get install python3-tk
```

### How can I add more emojis?

Add the corresponding PNG file to:

```text
emoji_png/
```

Then add the emoji character to `emoji_list` in:

```text
chat_ui.py
```

For example:

```python
emoji_list = ['😀', '😂', '😍', '😎', '😢', '👍', '🙏', '🎉', '💡', '🔥']
```

---

## 📄 License

This project is intended for educational and demonstration purposes.
