# 🔐 AutoLoginIP – Secure Auto Login for TShock

AutoLoginIP is a **secure auto-login plugin for Terraria TShock servers**, using **AES-256 encryption**, **IP address**, and **UUID verification** to protect player accounts.

---

## 🇫🇷 Français

### 📖 Description

**AutoLoginIP** permet aux joueurs de se connecter automatiquement à leur compte **TShock** sans devoir taper leur mot de passe à chaque reconnexion.

La sécurité est une priorité :  
le mot de passe est **chiffré en AES-256**, et l’auto-login est validé uniquement si **l’IP et l’UUID correspondent**.

---

### ✨ Fonctionnalités

- 🔐 Chiffrement AES-256 (mot de passe jamais en clair)
- 🌍 Vérification de l’adresse IP
- 🆔 Vérification de l’UUID TShock
- 📁 Génération automatique du fichier de configuration
- 💬 Messages entièrement configurables
- 🗄️ Compatible MySQL / MariaDB
- ❌ Désactivation de l’auto-login à tout moment

---

### 📦 Installation

1. Télécharge `AutoLogin.dll`
2. Place le fichier dans :
   ```
   /ServerPlugins/
   ```
3. Redémarre le serveur
4. Le fichier de configuration sera créé automatiquement :
   ```
   /tshock/AutoLoginConfig.json
   ```

---

### ⚙️ Configuration (`AutoLoginConfig.json`)

```json
{
  "AESKey": "BASE64_32_BYTES_KEY",
  "AESIV": "BASE64_16_BYTES_IV",
  "MsgActivated": "Auto-login sécurisé activé (IP + UUID).",
  "MsgDeactivated": "Auto-login désactivé !",
  "MsgRefused": "Auto-login refusé (IP ou UUID différent).",
  "MsgExecuted": "Auto-login exécuté !",
  "MsgNoAccount": "Vous n'avez pas de compte.",
  "MsgUsage": "Usage: /autologin <motdepasse> ou /autologin off"
}
```

⚠️ **Important**  
Ne modifie **jamais** `AESKey` ou `AESIV` après que des joueurs aient activé l’auto-login.

---

### 🧾 Commandes

- `/autologin <motdepasse>` → Activer
- `/autologin off` → Désactiver

---

## 🇬🇧 English

### 📖 Description

**AutoLoginIP** allows players to automatically log into their **TShock account** without typing their password every time they join the server.

Passwords are **encrypted using AES-256**, and auto-login only works if **IP and UUID match**.

---

### ✨ Features

- 🔐 AES-256 password encryption
- 🌍 IP address verification
- 🆔 UUID verification
- 📁 Automatic config generation
- 💬 Customizable messages
- 🗄️ MySQL / MariaDB support
- ❌ Disable auto-login anytime

---

### 🧾 Commands

- `/autologin <password>` → Enable
- `/autologin off` → Disable

---

## 🧑‍💻 Requirements


- TShock 5.2+


---

## 👤 Author

**C0sm0s**
