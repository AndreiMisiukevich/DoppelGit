## DoppelGit Usage Guide

Use this guide for day-to-day DoppelGit workflows:

- Adding and managing identities  
- Choosing GitHub Personal Access Token (PAT) scopes  
- Creating pull requests, issues, and comments as different identities  
- Using encryption and lock/unlock  
- Troubleshooting common GitHub API failures  
- Example workflows for teams, agencies, and maintainers  

> This guide assumes the extension is already installed. If not, see the main `README.md`.

---

## 1. Identities

### 1.1 What is an identity?

An **identity** in DoppelGit is:

- A **display name** (e.g. `Work – Org A`, `OSS – Maintainer`, `Client X Bot`)  
- A **GitHub Personal Access Token** (PAT)  
- Optional metadata such as color, avatar, or tags (implementation-dependent)  

DoppelGit uses the **active identity’s PAT** for all GitHub API calls it makes.

---

### 1.2 Adding a new identity

1. Open the DoppelGit popup or options page.  
2. Click **“Add identity”**.  
3. Fill in:
   - **Name** – human-friendly label you’ll recognize later.
   - **Token (PAT)** – paste a GitHub Personal Access Token.
   - Optional: color/icon/notes if supported by your UI.
4. Click **Save**.

There is no hard-coded limit on how many identities you can configure; the subscription (and 7-day free trial) are intended to support **unlimited identities** within reasonable, non-abusive use.

---

### 1.3 Recommended PAT scopes

The exact scopes depend on how GitHub PATs are set up (classic vs fine-grained). DoppelGit works with both but **fine-grained PATs are recommended** for tighter security.

#### 1.3.1 Minimal scopes for typical workflows

For **creating PRs, issues, and comments** in classic tokens:

- `repo` – access to repositories (including pull requests).  
- `issues` – for creating and commenting on issues (often implied by `repo`).  

For **fine-grained** PATs:

- Grant access only to the **specific repositories** where this identity should act.  
- Allow:
  - **Issues**: read/write  
  - **Pull requests**: read/write  
  - **Contents/metadata**: as needed for your workflow  

> Principle of least privilege: create a new PAT per identity with the narrowest scopes and repo access required.

---

### 1.4 Switching identities

1. Click the DoppelGit icon.  
2. In the popup, select an identity from the list (radio/select).  
3. DoppelGit marks it as **active**.

Effects:

- All supported actions (PRs, issues, comments) now use that **active identity’s PAT**.  
- The visible GitHub UI (avatar, name) may still reflect your browser’s logged-in user, but **actions sent via DoppelGit belong to the PAT owner**.

---

## 2. Core actions on GitHub

DoppelGit integrates directly into your GitHub workflow. Below are the supported actions and how to use them.

---

### 2.1 Creating pull requests

#### 2.1.1 From a branch you already pushed

1. Push your branch to GitHub.  
2. On GitHub, navigate to the repo and branch you want to open a PR from.  
3. Set the **active identity** in DoppelGit.  
4. Click the DoppelGit UI for **“Create PR”** (exact UI depends on implementation, e.g.:
   - A button in the popup,  
   - A button injected near GitHub’s “New pull request”, or  
   - A shortcut in the options page with a link).
5. Fill in:
   - **Title**  
   - **Description / body**  
   - Optional: labels, assignees (if supported in your version).
6. Confirm the action.

DoppelGit calls GitHub’s API using the **active identity’s PAT**, so the PR is **created by that identity**, not by the user you’re logged in as in the browser.

---

### 2.2 Creating issues

1. Go to the repository’s **Issues** tab on GitHub.  
2. Select your **active identity** in DoppelGit.  
3. Use the DoppelGit UI to create an issue:
   - Provide title and body.  
   - Optional: labels, assignees, milestones (depending on feature support).  
4. Submit.

The issue is authored by the **GitHub account that owns the PAT**.  

This is useful for:

- Bot issues (e.g. dependency updates or reminders).  
- Filing customer-facing issues under a support or organization account.  

---

### 2.3 Commenting on issues and PRs

1. Navigate to a specific **issue or pull request** on GitHub.  
2. Choose an **active identity** in DoppelGit.  
3. Use DoppelGit’s UI to add a comment:
   - Write the content in DoppelGit’s comment box, or  
   - Have DoppelGit send what you’ve typed into the GitHub comment box (implementation-dependent).  
4. Submit via DoppelGit.

The comment appears from the **PAT owner identity**.  

Common use cases:

- Post automated “status” comments or CI summaries as a bot identity.  
- Reply as an organization account rather than a personal account.  

---

## 3. Encryption & lock behavior

> Encryption features are included with the **subscription ($5/month or $49/year)** and are available during the **7-day free trial**.

### 3.1 Enabling encryption

1. Start a **subscription** (or free trial) via the extension’s purchase / licensing flow.  
2. Open the DoppelGit **settings / options page**.  
3. Look for **“Encryption”** or **“Protect tokens with passphrase”**.  
4. Choose a strong **passphrase** you can remember.  
5. Enable encryption and confirm.

When encryption is on:

- DoppelGit encrypts sensitive fields (such as PATs) using **AES-GCM**.  
- The encryption key is derived from your passphrase and **never leaves your device**.  

---

### 3.2 Locking and unlocking

#### 3.2.1 Unlock flow

1. When the extension is locked, DoppelGit will require a **passphrase** before it can read PATs.  
2. Enter your passphrase in the prompt.  
3. Upon successful decryption:
   - Identities become available.  
   - Actions (PRs, issues, comments) work as normal.  

#### 3.2.2 Lock behavior

Depending on your settings, DoppelGit may:

- Auto-lock after a period of inactivity.  
- Lock when the browser closes.  
- Allow manual lock from the popup or options page.

When locked:

- PATs remain encrypted at rest.  
- DoppelGit cannot perform actions using identities until you unlock.  

> **Important:** If you forget your passphrase, DoppelGit cannot recover your PATs. You would need to reset the extension and reconfigure identities.

---

## 4. Troubleshooting GitHub API calls

Even with a correct setup, you may run into errors coming from GitHub’s API. DoppelGit aims to surface clear error messages, but here are typical cases and how to fix them.

### 4.1 401 Unauthorized

Symptoms:

- DoppelGit shows an error like `401 Unauthorized` or “Bad credentials”.  

Causes:

- PAT is invalid or has been revoked.  
- PAT has expired (for tokens with an expiration).  
- Token string was pasted incorrectly (e.g. truncated or extra whitespace).

Fix:

1. Regenerate a PAT in GitHub with appropriate scopes.  
2. Update the identity in DoppelGit with the new token.  
3. Try the action again.

---

### 4.2 403 Forbidden

Symptoms:

- DoppelGit reports `403 Forbidden` or “insufficient permissions”.

Causes:

- PAT lacks the required **scopes** (e.g. missing `repo` or `issues`).  
- Fine-grained PAT does not include the **target repository**.  
- Repository access is restricted (e.g. you’re not a collaborator or member).

Fix:

1. Re-check PAT scopes and repository access in GitHub.  
2. For fine-grained tokens, ensure:
   - The repo is listed under **Repository access**, and  
   - The necessary resource types (issues, pull requests, contents) have **read/write**.
3. Update the identity’s token if new scopes are needed.

---

### 4.3 404 Not found

Symptoms:

- DoppelGit shows `404 Not Found` when accessing a repo or issue.

Causes:

- PAT doesn’t have access to the repository (private repo you can’t see).  
- The resource really doesn’t exist (wrong URL, deleted branch, etc.).  

Fix:

- Confirm the identity (PAT owner) can see the repo in GitHub’s web UI.  
- Double-check the URL and resource IDs being used.

---

### 4.4 Rate limiting

Symptoms:

- Error mentioning “API rate limit exceeded”.

Causes:

- The identity has made too many requests in a short period.  
- Using the same PAT across many automated scripts and tools.

Fix:

- Wait for the rate limit window to reset.  
- Spread out automated jobs or reduce frequency.  
- Use multiple identities/PATs responsibly when appropriate.

---

## 5. Example workflows

### 5.1 Work vs personal accounts

**Goal:** Keep company contributions and personal OSS contributions separated while using one browser profile.

Setup:

- Identity 1: `Work – Company` with a work PAT scoped to company repos.  
- Identity 2: `OSS – Personal` with a PAT scoped to your public/OSS repos.  

Workflow:

1. When working on company projects, activate `Work – Company`.  
2. Create PRs and issues; they appear as your work account.  
3. Switch to `OSS – Personal` for open-source repositories.  
4. Any DoppelGit-initiated PRs/issues/comments reflect that identity instead.

---

### 5.2 Agency or consulting work

**Goal:** Contribute to multiple client repos with clear separation and potentially different bot-like behavior.

Setup:

- Identity 1: `Client A – Dev`  
- Identity 2: `Client B – Dev`  
- Identity 3: `Agency Bot` for templated comments and status updates.  

Workflow:

1. Switch to each client’s identity when working in their repos.  
2. Use `Agency Bot` to post automated comments about deployments, release notes, or checks.  
3. All audit logs in GitHub clearly show which identity acted.

---

### 5.3 OSS maintainer with automation

**Goal:** Use a bot-like identity to manage common maintainer tasks.

Setup:

- Identity 1: `OSS – Maintainer` (your main maintainer account).  
- Identity 2: `OSS – Bot` with PAT limited to the specific repo.  

Workflow:

1. Use `OSS – Maintainer` for reviews and important decisions.  
2. Use `OSS – Bot` identity for:
   - Auto-reply templates.  
   - Labelling issues.  
   - Posting CI-related status.  

This keeps noisy automation separate from your main account.

---

## 6. Screenshot reference / placeholders

In your GitHub repo, you can add screenshots under `docs/images/` and reference them here:

- Identities list:

  `![DoppelGit identities list](./images/identities-list.png)`

- Identity creation form:

  `![Create identity form](./images/create-identity.png)`

- Encryption/passphrase prompt:

  `![Encryption unlock screen](./images/encryption-unlock.png)`

- PR flow example:

  `![Creating a PR with DoppelGit](./images/pr-flow.png)`

Replace these placeholders with real screenshots for your public documentation.


