## DoppelGit FAQ

This FAQ covers common questions about DoppelGit’s behavior, safety, and pricing.

---

### Does this violate GitHub’s Terms of Service?

**Short answer:** DoppelGit uses GitHub’s official APIs via **Personal Access Tokens (PATs)**, just like any other API client.

Important points:

- You authenticate with GitHub using your own PATs.  
- Actions (PRs, issues, comments) are performed via standard GitHub API endpoints.  
- Many developers already use multiple accounts, bot users, and automation workflows.

That said:

- You are responsible for ensuring your own usage complies with:
  - GitHub’s Terms of Service,  
  - GitHub’s Acceptable Use Policies, and  
  - Your organization’s internal policies.  
- Abuse of the API (spam, harassment, evasion of rate limits or enforcement) may still result in action from GitHub.

---

### Is it safe to paste my GitHub token into DoppelGit?

DoppelGit is designed with a **local-first** and **transparent** security model:

- PATs are stored in **`chrome.storage.local`** in your browser profile.  
- PATs are **never sent to a third-party server** operated by DoppelGit.  
- GitHub API requests go **directly from your browser to `api.github.com`**.  
- On the **Pro plan**, you can enable **local AES-GCM encryption** with a passphrase, so PATs are encrypted at rest.

However, no tool can be “absolutely safe”:

- If your machine or browser is compromised, anything running in it may be at risk.  
- You should always:
  - Use **fine-grained tokens** with minimal scopes.  
  - Rotate and revoke tokens periodically.  
  - Review the source if you have strict security requirements.

See [`SECURITY.md`](./SECURITY.md) for details.

---

### Will GitHub block me for using this?

GitHub may restrict or block:

- Accounts that abuse the API (e.g. spam, scraping, ToS violations).  
- Tokens that violate security policies or are compromised.

DoppelGit itself:

- Does not attempt to bypass rate limits.  
- Does not evade GitHub’s security controls.  
- Simply makes API calls using your PATs with the scopes you granted.

If you use DoppelGit responsibly:

- Following GitHub guidelines,  
- Keeping activity reasonable and non-spammy,  
- And using proper bot or service accounts where appropriate,  

you should be operating within typical, supported usage.

---

### Can I use bot accounts with DoppelGit?

Yes. DoppelGit is a great fit for **bot-style** or **service** accounts:

- Create a GitHub user or machine account with clear naming.  
- Generate a PAT with minimal, appropriate scopes.  
- Add it as an identity in DoppelGit (e.g. `Repo Bot`, `Release Notifier`).  

Use cases:

- Automated comments (status, reminders, triage).  
- Filing templated issues.  
- Posting release notes on PRs or issues.

Always ensure:

- You comply with GitHub’s policies around bot/service accounts.  
- Bots are clearly labeled and non-deceptive.

---

### Can I use this at work?

It depends on your company’s policies.

Things to check:

- **Security & compliance policies** – Some organizations restrict:
  - Use of browser extensions,  
  - Use of third-party tools with production repositories,  
  - Storage of access tokens outside approved systems.  
- **Data classification rules** – PATs may be considered highly sensitive credentials.

If in doubt:

- Talk to your security/compliance team.  
- Share the docs (`README.md`, `SECURITY.md`, `PRIVACY.md`) and source code.  
- Consider using:
  - Dedicated work-specific PATs,  
  - Fine-grained tokens,  
  - The Pro encryption feature for local protection.

---

### Does DoppelGit send analytics?

By design, DoppelGit does **not** require analytics to function.

If analytics exist at all:

- They should be **strictly opt-in**.  
- They should **never** include:
  - PATs,  
  - Repository contents,  
  - Raw API request payloads.  

Instead, analytics (if enabled) might focus on:

- Anonymous counts (e.g. feature adoption, general usage patterns).  
- Error types (e.g. how often GitHub returns 401/403).  

See [`PRIVACY.md`](./PRIVACY.md) for the detailed stance on analytics and telemetry.

---

### Why doesn’t GitHub already do this?

GitHub has:

- First-class support for:
  - Organizations,  
  - Teams,  
  - SSO,  
  - Personal vs. work accounts (via browser/session switching).

But for many real-world workflows:

- Developers need more **granular per-identity behavior**.  
- Agencies and consultants work with **many separate clients**.  
- Maintainers want **bot-style users** that comment and act differently.

DoppelGit exists to:

- Make it easy to act as **multiple identities** from one browser profile.  
- Keep actions clearly separated in audit logs and UI.  
- Provide a **developer-focused** workflow for identities based on PATs.

---

### What happens if the extension breaks?

If DoppelGit fails (bug, version change, browser update):

- GitHub continues to work as normal in your browser.  
- You can still:
  - Use GitHub’s native web UI,  
  - Use any other tools that rely on your PATs (CLI, CI, etc.).  

Your options:

- Check the **GitHub repo issues** for known problems or fixes.  
- Temporarily disable or remove the extension if needed.  
- As a Pro user, you can continue to **revoke PATs** in GitHub if you no longer trust the extension.

The extension being broken does **not** automatically revoke your tokens; you manage them directly in GitHub.

---

### What’s the difference between Free and Pro?

**Free plan**

- **Price:** $0  
- **Identities:** 1  
- **Actions/day:** 50 actions per day  
- **Encryption:** Not available  
- **Support:** Basic (best-effort GitHub issues)  

Best for:

- Trying DoppelGit.  
- Simple one-off alternate identity usage.  

**Pro plan**

- **Price:** **$5/month** or **$49/year**  
- **Identities:** Unlimited  
- **Actions/day:** Unlimited (within GitHub’s rate limits and TOS)  
- **Encryption:** Local encryption with AES-GCM, passphrase-based  
- **Support:** Priority support  
- **Extras:** Early access to automation features and more customization options  

Best for:

- Consultants, agencies, and teams.  
- Heavy multi-identity users (work, personal, client, bot).  
- Users with elevated security needs who want local encryption.

---

### What happens to my data if I uninstall DoppelGit?

When you uninstall the extension:

- The browser typically removes its `chrome.storage.local` data.  
- PATs and identities stored by DoppelGit are no longer accessible to the extension.

For maximum safety:

- **Revoke your PATs in GitHub** if you’re done using them or no longer trust the environment.  
- If you reinstall DoppelGit later, you’ll need to configure identities and tokens again.

---

### Can I use DoppelGit on Firefox or Safari?

Currently:

- DoppelGit targets **Chromium-based browsers** (Chrome, Brave, Edge).  

Planned:

- **Firefox** and **Safari** support are on the longer-term roadmap.  

See [`ROADMAP.md`](./ROADMAP.md) for more.

---

### Where can I report bugs or request features?

Please use the project’s **GitHub issues**:

- Look for an existing issue that matches your problem/feature.  
- If none exists:
  - Open a **new issue**,  
  - Include clear steps to reproduce,  
  - Mention browser version and DoppelGit version.

For PR guidelines and contribution details, see [`CONTRIBUTING.md`](./CONTRIBUTING.md).


