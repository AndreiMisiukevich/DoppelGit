## DoppelGit Privacy Policy (Developer-Focused Summary)

This document explains how DoppelGit handles your data, with a focus on:

- What data is stored and where  
- Whether data leaves your machine  
- How (and if) analytics are collected  
- How encryption and browser security interact  

> This is a technical overview intended for developers. For legal or compliance use, adapt and extend this into a formal policy as needed.

---

## 1. What DoppelGit stores

DoppelGit primarily stores:

- **Identities**
  - Display names (e.g. `Work – Org`, `OSS – Maintainer`).  
  - Associated GitHub Personal Access Tokens (PATs).  
  - Optional metadata (color, tags, etc.).  

- **Settings**
  - Active identity.  
  - Preferences (e.g. lock timeout, UI options).  
  - Plan / licensing state (minimal, implementation-dependent).  

All of this is stored in **browser extension storage** (e.g. `chrome.storage.local`) within your browser profile.

---

## 2. Where your data lives

### 2.1 Local-only storage

- DoppelGit stores its data **only in your browser** via APIs such as `chrome.storage.local`.  
- DoppelGit does **not**:
  - Upload PATs to any external servers controlled by the project.  
  - Maintain a hosted database of your identities.  
  - Sync your identity configuration across devices.

If your browser supports syncing some extension data, that behavior is controlled by the browser, not by DoppelGit.

### 2.2 Encryption (subscription)

With the **subscription ($5/month or $49/year)** – and during the **7-day free trial** – DoppelGit can encrypt sensitive data at rest:

- Uses **AES-GCM** in the browser.  
- Encryption key derived from your **passphrase**.  
- Encrypted data is still stored in `chrome.storage.local`, but only decrypted when you unlock the extension.

If you forget your passphrase:

- DoppelGit cannot recover your PATs.  
- You must reset and reconfigure identities.

---

## 3. Network communication

### 3.1 GitHub API

When DoppelGit performs actions (PRs, issues, comments):

- Requests are sent **directly from your browser to GitHub** (`api.github.com`) over HTTPS.  
- The **active identity’s PAT** is included in the `Authorization` header of those requests.  

No intermediate server:

- DoppelGit does **not** proxy GitHub API calls through a custom backend.  
- PATs are not sent to a third-party service as part of normal operation.

### 3.2 Licensing / payments (if applicable)

If the subscription is sold via a payment provider or extension monetization platform:

- A minimal set of data may be exchanged with that provider:
  - License identifiers,  
  - Payment status,  
  - Possibly anonymized or account-scoped identifiers.  

PATs and GitHub data are **never required** by the licensing/payment provider and are not shared.

---

## 4. Analytics and telemetry

### 4.1 Default behavior

By default, DoppelGit is intended to work **without any analytics**.

If analytics are ever present:

- They must be **opt-in**.  
- They must **never** include:
  - PAT values,  
  - Repository contents,  
  - Exact API request payloads,  
  - Any PII not strictly necessary.  

### 4.2 Possible opt-in metrics

If you opt in, examples of data that might be collected:

- Anonymous usage counts (e.g. number of identities configured).  
- Feature usage (e.g. which features are used frequently).  
- Error categories (e.g. rate of GitHub 401/403 responses).  

Purpose:

- Improve UX, stability, and prioritization of new features.  
- Measure adoption of features like encryption, team workflows, etc.

---

## 5. No usage tracking of GitHub content

DoppelGit does **not** need:

- Your actual code or repository contents.  
- The full text of issues, PRs, or comments you create.  

The extension interacts with GitHub on your behalf but:

- It does not maintain its own copy of your repositories.  
- It does not log your GitHub activity to a central server.  

Any logging that exists is:

- Local to your browser (e.g. console logs), or  
- Limited to error codes and high-level metadata (if analytics are opt-in).

---

## 6. Browser-based security model

### 6.1 Extension permissions

DoppelGit relies on:

- Access to `github.com` pages (for UI integration).  
- Access to `api.github.com` for API requests.  
- Extension storage APIs for saving identities and settings.

As with any extension:

- Review requested permissions during installation.  
- Keep your browser and OS up to date.  
- Only install extensions from sources you trust.

### 6.2 Your responsibilities

- Protect your machine and browser profile with:
  - Strong OS account password,  
  - Disk encryption where possible,  
  - Updated antivirus/anti-malware (if relevant).  
- Use **fine-grained PATs** where possible.  
- Revoke tokens in GitHub if:
  - You uninstall DoppelGit, or  
  - You suspect compromise.

---

## 7. Data deletion

### 7.1 Removing data manually

To delete DoppelGit data:

- You can:
  - Remove identities and tokens from within the extension UI, and/or  
  - Clear extension data via your browser’s extension settings, and/or  
  - Uninstall the extension, which typically removes its storage.

### 7.2 After uninstall

When you uninstall DoppelGit:

- The browser generally deletes the extension’s local storage.  
- DoppelGit no longer has any runtime access to your PATs or settings.

For maximum safety:

- **Revoke PATs in GitHub** you no longer wish to use.

---

## 8. Screenshot placeholders

Recommended screenshots for a public-facing privacy page (placeholders):

- Identity storage explanation:

  `![Where DoppelGit stores data](./images/privacy-storage.png)`

- Encryption and privacy settings:

  `![Privacy and encryption options](./images/privacy-encryption.png)`

Replace these placeholders with real screenshots before publishing.


