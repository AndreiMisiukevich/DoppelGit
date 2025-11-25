## DoppelGit – Multiple GitHub identities, one browser

**DoppelGit** is a Chrome extension that lets you act as **multiple GitHub identities** in one browser using **Personal Access Tokens (PATs)** instead of juggling accounts, profiles, or browsers.

Pick an identity, perform GitHub actions as that identity, and switch in seconds:

- **Create pull requests**
- **Open issues**
- **Comment on issues**
- **Comment on pull requests**

All configuration (including PATs) is stored **locally in your browser**. Tokens are **never uploaded to any server**. You can optionally enable **local passphrase-based encryption** for additional protection.

> DoppelGit is a developer productivity tool designed for people who contribute to GitHub as multiple personas: employees, contractors, OSS maintainers, bots, or client accounts.

---

## Features

- **Multiple identities via PATs**
  - Represent different GitHub users (e.g. company account, OSS account, client account, bot).
  - Each identity is backed by a GitHub Personal Access Token.
- **GitHub-native actions**
  - Create pull requests, open issues, and comment on issues/PRs as the selected identity.
- **Local-only storage**
  - Identities and tokens live in `chrome.storage.local` only.
  - No remote database, no backend, no third-party service.
- **Optional encryption**
  - Protect sensitive PATs with a local passphrase.
  - Encrypted using **AES-GCM** in the browser (when enabled).
- **Per-site awareness**
  - Works directly on `github.com` in your existing workflow.
  - No extra dashboard; a configuration/options page is available when you need it.
- **Developer-first UX**
  - Keyboard-friendly where possible.
  - Transparent and debuggable (clear errors when GitHub says “no”).

---

## Pricing

DoppelGit has a **Free** tier for simple setups and a **Pro** tier for heavy users.

An **action** is a single GitHub operation triggered via DoppelGit (for example, creating a pull request, opening an issue, or posting a comment).

### Plans

| Plan     | Price                | Identities       | Actions/day    | Encryption | Support           | Extras                                             |
|---------|----------------------|------------------|----------------|-----------|-------------------|---------------------------------------------------|
| **Free** | **$0**               | 1 identity       | 50 actions/day | No        | Basic             | Core identity switching & GitHub actions          |
| **Pro** | **$5/month** or **$49/year** | Unlimited identities | Unlimited     | Yes       | Priority support | Early access to automation + more customization   |

- **Free**
  - Great for trying DoppelGit or running a single “alternate” identity.
  - No encryption features.
- **Pro**
  - For power users, consultants, agencies, and OSS maintainers.
  - Enables **local encryption**, removes action limits, and unlocks advanced features as they ship.

---

## Screenshots

> These are example screenshot paths for this repository.

- **Identities overview**

  ![DoppelGit identities list](./docs/images/identities-list.png)

- **GitHub PR with identity selector**

  ![Using DoppelGit on a GitHub PR](./docs/images/pr-identity-selector.png)

- **Encryption / passphrase screen**

  ![DoppelGit encryption settings](./docs/images/encryption-settings.png)

---

## Installation

DoppelGit is distributed as a Chrome extension and works on **Chrome**, **Brave**, and **Edge** (Chromium-based).

### Option 1 – From the Chrome Web Store (recommended)

1. Open the Chrome Web Store page for DoppelGit.  
2. Click **“Add to Chrome”**.
3. Confirm the installation dialog.
4. Pin the extension (optional but recommended) so the icon is always visible.

> Once installed, DoppelGit will be available on all GitHub tabs in that browser profile.

### Option 2 – Load unpacked (Developer Mode)

If you are building from source or contributing:

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/doppelgit.git
   cd doppelgit
   ```

2. Build (if required by this repo’s setup) or use the `dist/` folder if present.
3. Open `chrome://extensions` in your browser.
4. Enable **Developer mode** (toggle in the top-right).
5. Click **“Load unpacked”**.
6. Select the built extension directory (e.g. `dist/`).

The DoppelGit icon should now appear in your toolbar.

---

## Quickstart

1. **Open GitHub** in a new tab and sign in with any GitHub account (or none — PATs drive identities).
2. **Open the DoppelGit popup** by clicking the extension icon.
3. **Create your first identity**:
   - Give it a **name** (e.g. “Work – Org A”).
   - Optionally set a **color or icon** (if supported).
   - Paste a **GitHub PAT** with the recommended scopes (see below).
4. **Select that identity** as “active”.
5. Navigate to a **pull request, issue, or repo** on GitHub.
6. Use DoppelGit’s UI to open PRs, create issues, or comment on issues/PRs as the **active identity**.

All actions triggered via DoppelGit will use that identity’s PAT.

---

## How identities work

- **Identity = Label + PAT + Metadata**
  - A human-friendly label (e.g. “OSS Maintainer”, “Client X Bot”).
  - A GitHub **Personal Access Token**.
  - Optional extras like color, default repo, or tags (implementation-dependent).
- **Switching identities**
  - Only one identity is **active** at a time.
  - When you change the active identity, DoppelGit immediately starts using the new PAT for supported GitHub actions.
- **No shared state between identities**
  - Each identity is logically isolated.
  - DoppelGit always uses the active identity’s PAT when calling the GitHub API.

### Adding tokens (PATs)

1. In the DoppelGit popup or options page, click **“Add identity”**.
2. Enter:
   - **Display name** – how you’ll recognize this identity.
   - **PAT** – paste your GitHub Personal Access Token.
3. Choose the **plan**:
   - Free: you can have **1 identity**.
   - Pro: you can add **unlimited identities**.
4. Save the identity.

#### Recommended PAT scopes

For typical workflows:

- **Repos & PRs**
  - `repo` (or more fine-grained scopes for: pull requests, contents, issues).
- **Issues & comments**
  - `issues` (if using classic tokens).

Use **fine-grained PATs** where possible with the smallest set of scopes and repositories needed for your workflow.

---

## Using DoppelGit on GitHub

### Creating pull requests as another identity

1. Set the **active identity** in the DoppelGit popup.
2. Navigate to a repo where the PAT has appropriate permissions.
3. Create a branch and push your changes as usual.
4. Use DoppelGit’s UI to:
   - Open a pull request from your branch, or
   - Fill in the PR form and submit via the GitHub API using the active identity’s PAT.

The PR is owned by the **user behind the PAT**, regardless of which browser account you’re signed into.

### Creating issues

1. Select the **active identity**.
2. On GitHub, navigate to the repository’s **Issues** tab.
3. Use the DoppelGit UI to:
   - Open a new issue,
   - Fill out title/body,
   - Submit via the GitHub API with the active identity.

The issue is authored by that identity.

### Commenting on issues & PRs

1. Navigate to an issue or PR.
2. Choose an **active identity** in DoppelGit.
3. Use DoppelGit to post a **comment** (e.g. review, status update, bot message) using that identity’s PAT.

This is ideal for:

- Bot-style comments,
- Separating personal vs. work voice,
- Acting as “official” organization accounts.

---

## Security model

- **Local storage only**
  - Identities and PATs are stored in `chrome.storage.local` in your browser profile.
  - Data is never sent to any external server operated by DoppelGit.
- **Optional encryption (Pro)**
  - When enabled, DoppelGit encrypts sensitive data (e.g. PATs) using **AES-GCM**.
  - The encryption key is derived from a **passphrase** you choose.
  - Decryption only happens locally when you unlock DoppelGit.
- **No external servers**
  - All GitHub API calls go **directly from your browser to `api.github.com`**.
  - DoppelGit does not sit in the middle as a proxy or backend.
- **Browser sandbox**
  - DoppelGit runs within the Chrome Extension security model.
  - Permissions are limited to the domains and APIs necessary for GitHub integration.

For more details, see [`docs/SECURITY.md`](./docs/SECURITY.md) and [`docs/PRIVACY.md`](./docs/PRIVACY.md).

---

## Compatibility

DoppelGit is built for **Chromium-based browsers**:

- ✅ **Google Chrome**
- ✅ **Brave**
- ✅ **Microsoft Edge** (Chromium-based)

Other browsers:

- ⚠️ **Firefox** – not supported yet (planned).
- ⚠️ **Safari** – not supported yet (planned).

See the roadmap in [`docs/ROADMAP.md`](./docs/ROADMAP.md).

---

## FAQ (short version)

**Q: Is this safe to use?**  
**A:** DoppelGit is designed with a **local-first** security model: PATs stay in `chrome.storage.local`, and all requests go directly to GitHub. With Pro, you can additionally encrypt PATs using AES-GCM with a passphrase. You are still responsible for generating, scoping, and revoking your tokens.

**Q: Does this violate GitHub’s Terms of Service?**  
**A:** DoppelGit uses GitHub’s official APIs via your PATs, similar to any other client. You should always comply with GitHub’s Terms, Acceptable Use, and rate limits. Using multiple identities is common for maintainers, bot accounts, and work vs. personal accounts.

**Q: Will GitHub block me?**  
**A:** GitHub may rate-limit or restrict accounts that abuse the API or violate their policies. DoppelGit does not attempt to evade rate limits. Use reasonable PAT scopes and avoid spammy activity.

**Q: What’s the difference between Free and Pro?**  
**A:** Free gives you **1 identity**, **50 actions/day**, and no encryption. Pro unlocks **unlimited identities**, **unlimited actions**, **local encryption**, priority support, and early access to automation and customization features.

For the full FAQ, see [`docs/FAQ.md`](./docs/FAQ.md).

---

## Troubleshooting

- **Nothing happens when I try to create a PR/issue/comment**
  - Check that an **identity is active** in the DoppelGit popup.
  - Confirm your PAT has the **required scopes** and repo access.
  - Check the browser console or extension logs for GitHub API errors.
- **I get a 401 / 403 from GitHub**
  - PAT may be invalid, expired, or missing scopes.
  - If using fine-grained PATs, ensure the target repo is allowed.
- **My identities are gone**
  - Ensure you’re in the same browser profile where DoppelGit was originally configured.
  - Check if the browser or OS cleared extension data.
- **I forgot my encryption passphrase (Pro)**
  - DoppelGit cannot recover or reset the encryption key.
  - You may need to reset the extension and re-enter PATs.

More detail is available in [`docs/USAGE.md`](./docs/USAGE.md).

---

## Roadmap (preview)

High-level items:

- **Now**
  - Stability improvements.
  - Better GitHub DOM detection.
- **Next**
  - “Review as identity”.
  - Labels & assignees via identity.
  - Team/agency support.
  - UI improvements for the options page.
- **Later**
  - Firefox/Edge/Safari ports.
  - Organization-wide identity templates.
  - API access.
  - Offline mode.

See [`docs/ROADMAP.md`](./docs/ROADMAP.md) for details.

---

## Contributing

Contributions, bug reports, and feature ideas are welcome!

See [`docs/CONTRIBUTING.md`](./docs/CONTRIBUTING.md) for:

- How to clone and build the project,
- How to load the extension in Developer Mode,
- Coding style and conventions,
- How to propose features and open PRs.

---

## License

**DoppelGit is proprietary software.**  
All rights reserved.  
Unauthorized copying, modification, or distribution of this code or extension is prohibited except as explicitly permitted by the author or a separate commercial agreement.

---

## Call to action

If DoppelGit looks useful:

- **Star the repo ⭐** to support the project.
- **Open issues** with feedback or ideas.
- **Share it** with other developers who juggle multiple GitHub identities.

DoppelGit exists to make multi-identity GitHub workflows simpler, safer, and more pleasant.