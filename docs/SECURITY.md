## DoppelGit Security Overview

Security is a core design goal for DoppelGit. This document explains:

- What data DoppelGit stores and where  
- How **chrome.storage.local** is used  
- How optional **passphrase-based encryption** with **AES-GCM** works (subscription feature)  
- Why Personal Access Tokens (PATs) remain on your device  
- Threat model and recommended safe practices  

> This document describes the intended security model of the extension. Always review the source code and your browser’s extension security model if you have strict security requirements.

---

## 1. Data storage model

### 1.1 chrome.storage.local

DoppelGit stores configuration and identity data using the browser’s **extension storage** APIs, typically:

- `chrome.storage.local` (or the equivalent in Chromium-based browsers).

Characteristics:

- Data is stored **locally** on your machine, per-browser-profile.  
- It is **not synced** by DoppelGit to any external server.  
- The browser may choose to sync some data depending on your settings, but DoppelGit does not implement its own sync or remote backup.

Stored data typically includes:

- Identities (names, colors, metadata)  
- GitHub Personal Access Tokens (PATs) – encrypted if encryption is enabled  
- Settings (e.g. active identity, preferences, lock timeout)  
- Licensing / plan metadata (minimal and implementation-dependent)  

---

### 1.2 Optional encryption (subscription)

With the **subscription ($5/month or $49/year)** – and during the **7-day free trial** – DoppelGit can encrypt sensitive information at rest using **AES-GCM** in the browser.

- A **passphrase** is provided by you.  
- An encryption key is derived from that passphrase using a suitable key-derivation function (e.g. PBKDF2, scrypt, or similar – see implementation).  
- PATs and other sensitive fields are encrypted using **AES-GCM** before being written to `chrome.storage.local`.

When DoppelGit is **locked**:

- PATs remain stored only in **encrypted form**.  
- DoppelGit does not hold the decryption key in memory.  
- GitHub API calls cannot be performed on behalf of identities until you **unlock** with your passphrase.

When you **unlock**:

- DoppelGit derives the key from your passphrase.  
- Decrypts the encrypted data into memory.  
- Allows actions (PRs, issues, comments) using the recovered PATs.

> If you forget your passphrase, DoppelGit cannot recover or reset your encryption key. You will need to reset the extension and re-enter PATs.

---

## 2. No external servers

### 2.1 Local-only architecture

DoppelGit is explicitly designed to **avoid any external backend**:

- No app-specific servers receive your PATs.  
- No proxy endpoints; requests go **directly from your browser to GitHub**.  
- No third-party analytics libraries are required for normal functioning.

### 2.2 GitHub API calls

All GitHub API traffic follows this flow:

1. DoppelGit builds a request in your browser (e.g. `https://api.github.com/...`).  
2. It attaches the **active identity’s PAT** in an `Authorization` header.  
3. The browser sends the request **directly to GitHub** over HTTPS.  

There is **no intermediate server** that could log or intercept tokens or payloads.

---

## 3. Threat model

### 3.1 In-scope threats

DoppelGit is designed to mitigate:

- **Accidental exfiltration** of tokens to a remote server owned by the extension author.  
- **Local disclosure** of plaintext tokens to casual inspection:
  - Pro users can enable encryption so PATs are not stored plaintext at rest.  
- **Misuse of tokens by the extension itself**:
  - DoppelGit aims to perform only the actions you invoke (PRs, issues, comments).  

### 3.2 Out-of-scope threats

DoppelGit cannot protect you from:

- **Compromised browser or OS**  
  - If your system is malware-infected or your browser is compromised, any extension or stored secret may be at risk.  
- **Malicious extensions**  
  - Other extensions with broad permissions could potentially read page content or interfere with GitHub pages.  
- **Physical device theft**  
  - If someone has physical access to your unlocked machine/browser, they may be able to act as you.
- **Phishing and social engineering**  
  - DoppelGit cannot prevent you from granting excessive scopes or sharing PATs in unsafe ways.

> Encryption at rest helps reduce damage in some situations (e.g. someone copying your profile directory), but it is not a full substitute for OS-level security.

---

## 4. Safe PAT usage

### 4.1 Principle of least privilege

When creating PATs for DoppelGit:

- Prefer **fine-grained PATs** when available.  
- Limit tokens to the **smallest set of repositories** needed.  
- Grant **read/write** only where necessary (e.g. issues, pull requests, contents).  
- Consider separate tokens for:
  - Work vs. personal use  
  - Different clients  
  - Bot vs. human identities  

### 4.2 Rotation and revocation

Best practices:

- **Rotate PATs regularly**, especially for important repos.  
- **Revoke** any token that:
  - Is no longer needed.  
  - May have been exposed or suspected compromised.  
- After revocation, update or delete the corresponding identity in DoppelGit.

---

## 5. User privacy protections

### 5.1 No token upload by design

By design, DoppelGit:

- Does **not** send PATs to any server other than GitHub’s official API endpoints.  
- Does **not** implement cloud backup of your identities.  
- Does **not** rely on a hosted “account” backend that could contain your tokens.

### 5.2 Optional analytics

If analytics are present at all, they should be:

- **Strictly opt-in**.  
- **Token-free** – no PATs, no repository contents, no raw requests.  
- Focused solely on high-level usage (e.g. feature adoption, error rates), if enabled.  

For detailed privacy policy and analytics behavior, see [`PRIVACY.md`](./PRIVACY.md).

---

## 6. Security checklist for users

Before relying on DoppelGit for sensitive work:

1. **Review the source code** of the extension (especially storage and network logic).  
2. Ensure:
   - No external endpoints receive PATs.  
   - Encryption is implemented as described, if you rely on it.  
3. Use **fine-grained PATs** with minimal repository permissions.  
4. Consider enabling **encryption (available with the subscription)** if:
   - You work with high-value repos, or  
   - You share your machine, or  
   - You want extra protection for at-rest data.  
5. Keep your OS and browser **up-to-date**.  
6. Revoke PATs promptly if:
   - You uninstall DoppelGit, or  
   - You suspect compromise, or  
   - You change roles/clients.

---

## 7. Security-related screenshots (placeholders)

You can add security-related illustrations under `docs/images/`:

- Storage & encryption overview:

  `![DoppelGit security overview](./images/security-overview.png)`

- Encryption settings:

  `![Encryption settings page](./images/encryption-settings.png)`

- Lock screen:

  `![Locked state / passphrase prompt](./images/locked-state.png)`

Replace these with real screenshots in your public repository.


