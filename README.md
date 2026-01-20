# AutoLoginIP

A Terraria TShock server plugin that securely auto-logs players by pairing their IP and UUID with AES-256 encrypted passwords. Configurable delays and retry logic help login complete reliably after join.

## English

### Features
- Auto-login based on the combination of IP and UUID
- AES-256 (CBC + PKCS7) encryption for stored passwords
- Configurable delay before first login attempt, retry interval, and max attempts
- Works with MySQL/MariaDB or SQLite through TShock DB abstractions
- Automatic config regeneration when crypto keys are invalid or missing
- Automatic cleanup of stale entries when IP/UUID no longer match

### Requirements
- Terraria server with TShock (API v2.1, .NET 6)
- Database configured in TShock (`sqlite` or `mysql`)

### Installation
1. Build or obtain `AutoLoginIP.dll`.
2. Place the DLL in your TShock `ServerPlugins` folder.
3. Start the server once to generate `AutoLoginConfig.json` in `tshock/AutoLoginConfig.json`.

### Configuration
`AutoLoginConfig.json` is generated automatically. Invalid or missing keys are regenerated at startup. Key fields:
- `AESKey`, `AESIV`: Base64 keys (32-byte key, 16-byte IV). Regenerated if missing/invalid.
- `AutoLoginDelayMs`: Delay before the first login attempt (default 2000 ms).
- `LoginRetryIntervalMs`: Delay between attempts (default 700 ms).
- `MaxLoginAttempts`: Maximum attempts (default 5).
- `MsgActivated`, `MsgDeactivated`, `MsgRefused`, `MsgExecuted`, `MsgNoAccount`, `MsgUsage`, `MsgConfigRegenerated`, `MsgCryptoError`: Localized messages.

### Usage
- `/autologin <password>`: Enable secure auto-login for your account (stores encrypted password + IP + UUID).
- `/autologin off`: Disable auto-login and delete the stored entry.

Behavior:
- On join, if IP and UUID match the stored entry, the plugin issues `/login <password>` up to the configured attempts until the player is logged in, then confirms success.
- If IP or UUID differ, the auto-login is refused, a warning is sent, and the stored entry is removed.

### Database schema
Table `AutoLogin`:
- `Username` (PK)
- `EncryptedPassword`
- `IP`
- `UUID`
- `LastUpdated` (datetime/text depending on DB)

### Security notes
- Keys live in `AutoLoginConfig.json`; protect server files and backups.
- Rotate keys only after clearing existing entries (old passwords become unreadable).
- Auto-login is tied to IP and UUID; if either changes, the entry is rejected and removed.
- Avoid sharing the DLL or config with untrusted parties.

### Troubleshooting
- **Auto-login refused**: IP/UUID changed; run `/autologin <password>` again from the new context.
- **Crypto errors**: Delete/rename `AutoLoginConfig.json` to let the plugin regenerate it, then re-enable auto-login.
- **DB errors or missing table**: Ensure TShock DB connectivity; the plugin creates `AutoLogin` on startup.

---

## Français

### Fonctionnalités
- Connexion automatique basée sur le duo IP + UUID
- Mots de passe stockés chiffrés en AES-256 (CBC + PKCS7)
- Délais configurables avant la première tentative, intervalle entre essais et nombre max d’essais
- Compatible MySQL/MariaDB ou SQLite via l’abstraction DB de TShock
- Régénération automatique de la config quand les clés sont invalides ou manquantes
- Suppression automatique des entrées obsolètes quand l’IP ou l’UUID ne correspondent plus

### Prérequis
- Serveur Terraria avec TShock (API v2.1, .NET 6)
- Base de données configurée dans TShock (`sqlite` ou `mysql`)

### Installation
1. Compilez ou récupérez `AutoLoginIP.dll`.
2. Placez le DLL dans le dossier `ServerPlugins` de TShock.
3. Démarrez le serveur une fois pour générer `AutoLoginConfig.json` dans `tshock/AutoLoginConfig.json`.

### Configuration
`AutoLoginConfig.json` est généré automatiquement. Les clés invalides ou manquantes sont régénérées au démarrage. Champs principaux :
- `AESKey`, `AESIV` : clés Base64 (clé 32 octets, IV 16 octets). Régénérées si manquantes/invalides.
- `AutoLoginDelayMs` : délai avant la première tentative (2000 ms par défaut).
- `LoginRetryIntervalMs` : délai entre tentatives (700 ms par défaut).
- `MaxLoginAttempts` : nombre maximal d’essais (5 par défaut).
- `MsgActivated`, `MsgDeactivated`, `MsgRefused`, `MsgExecuted`, `MsgNoAccount`, `MsgUsage`, `MsgConfigRegenerated`, `MsgCryptoError` : messages localisés.

### Utilisation
- `/autologin <motdepasse>` : active l’auto-login sécurisé pour votre compte (stocke mot de passe chiffré + IP + UUID).
- `/autologin off` : désactive l’auto-login et supprime l’entrée.

Comportement :
- À la connexion, si l’IP et l’UUID correspondent à l’entrée stockée, le plugin exécute `/login <motdepasse>` jusqu’à réussite, puis confirme.
- Si l’IP ou l’UUID diffèrent, l’auto-login est refusé, un avertissement est envoyé et l’entrée est supprimée.

### Schéma de base de données
Table `AutoLogin` :
- `Username` (PK)
- `EncryptedPassword`
- `IP`
- `UUID`
- `LastUpdated` (datetime/texte selon la base)

### Notes de sécurité
- Les clés sont dans `AutoLoginConfig.json` : protégez les fichiers serveur et les sauvegardes.
- Rotations de clés : purgez les entrées existantes avant de changer les clés (anciens mots de passe illisibles).
- L’auto-login est lié à l’IP et l’UUID ; si l’un change, l’entrée est rejetée et supprimée.
- Ne partagez pas le DLL ou la config avec des tiers non fiables.

### Dépannage
- **Auto-login refusé** : IP/UUID changé ; relancez `/autologin <motdepasse>` depuis le nouveau contexte.
- **Erreurs de chiffrement** : supprimez/renommez `AutoLoginConfig.json` pour le régénérer, puis réactivez l’auto-login.
- **Erreurs BD ou table absente** : vérifiez la connectivité DB TShock ; la table `AutoLogin` est créée au démarrage.
