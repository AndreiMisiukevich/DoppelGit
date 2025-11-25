## Contributing to DoppelGit

Thanks for your interest in contributing to **DoppelGit**!  
This document explains how to get set up, how to run the extension in development, and how to propose changes.

---

## 1. Getting started

### 1.1 Clone the repository

```bash
git clone https://github.com/your-username/doppelgit.git
cd doppelgit
```

If you are working from a fork:

```bash
git clone https://github.com/<your-github-username>/doppelgit.git
cd doppelgit
git remote add upstream https://github.com/your-username/doppelgit.git
```

---

### 1.2 Install dependencies

If the project uses Node.js for building:

```bash
npm install
```

Check the root `README.md` or project documentation for any project-specific prerequisites (Node version, package manager preference, etc.).

---

## 2. Running the extension in Developer Mode

### 2.1 Build (if required)

If the project uses a build step:

```bash
npm run build
```

This should produce a **build output directory**, commonly `dist/`, containing the Chrome extension bundle (manifest + scripts + assets).

If the repo already contains a ready-to-load directory, follow that instead.

---

### 2.2 Load unpacked extension

1. Open **Chrome** (or another Chromium-based browser such as Brave or Edge).  
2. Navigate to `chrome://extensions`.  
3. Enable **Developer mode** in the top-right corner.  
4. Click **“Load unpacked”**.  
5. Select the directory that contains the extension files (e.g. `dist/`).  

You should now see the **DoppelGit** icon in the toolbar.  
Click it to open the popup and verify that the extension loads without errors.

> Tip: Open the **background page** and **DevTools** for content scripts to see logs and debug issues while you work.

---

## 3. Project structure (typical)

The exact structure may evolve, but a typical layout looks like:

- `src/`
  - **background scripts** – extension lifecycle, messaging.  
  - **content scripts** – logic that runs on `github.com` pages.  
  - **options page** – identity management, encryption settings, plan status.  
  - **popup UI** – quick identity switching and shortcuts.  
  - **shared utilities** – API helpers, storage, crypto, types.  
- `dist/`
  - Built extension: manifest, JS bundles, CSS, icons.  
- `docs/`
  - Public documentation (usage, security, FAQ, roadmap, privacy, contributing).  

Check the repo’s main `README.md` and inline comments for additional details.

---

## 4. Coding style and guidelines

### 4.1 General principles

- **Readability first** – Code should be understandable by other contributors.  
- **Minimal surprise** – Follow established patterns already used in the project.  
- **Small, focused changes** – Avoid large, multi-concern PRs unless absolutely necessary.

### 4.2 Style

If there is an ESLint/Prettier or similar setup:

- Run the linter/formatter before opening a PR:

  ```bash
  npm run lint
  npm run format
  ```

- Match existing conventions:
  - Naming patterns,  
  - Folder/module organization,  
  - Error handling and logging approach.

If in doubt, mirror nearby code.

---

## 5. Feature proposals and issues

### 5.1 Filing issues

If you encounter a bug or have a feature idea:

1. Check existing **GitHub issues** to see if it’s already reported.  
2. If not, open a **new issue** with:
   - Clear title and description.  
   - Steps to reproduce (if a bug).  
   - Browser/version and operating system.  
   - DoppelGit version or commit hash.  

### 5.2 Proposing features

For non-trivial feature additions:

1. Open an issue titled **“Proposal: <short description>”**.  
2. Describe:
   - Problem you’re trying to solve.  
   - Rough UX and technical idea.  
   - How it fits into the current roadmap (see `ROADMAP.md`).  
3. Discuss the approach before submitting a large PR.

This reduces the risk of doing a lot of work that doesn’t align with project goals.

---

## 6. Pull request guidelines

### 6.1 Branching

1. Create a feature branch from `main` (or the canonical default branch):

   ```bash
   git checkout -b feature/my-new-feature
   ```

2. Keep your branch up-to-date with upstream changes:

   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

### 6.2 Scope of a PR

- Aim for **single-topic** PRs:
  - One bug fix,  
  - One small feature,  
  - One refactoring.  
- Avoid mixing:
  - Large refactors,  
  - New features,  
  - Formatting changes,
  all in the same PR.

### 6.3 Tests and manual verification

If the project uses automated tests:

- Run them before pushing:

  ```bash
  npm test
  ```

Manual checks:

- Load the extension via **Developer Mode**.  
- Verify:
  - Identities can be added/edited/deleted.  
  - GitHub actions you touched (PRs, issues, comments) still work.  
  - No obvious regressions in the popup or options page.

### 6.4 PR description

Include in your pull request:

- **Summary** of the change.  
- **Motivation** (what problem is solved).  
- **Implementation notes** (any architectural decisions).  
- **Testing** steps and environment.  
- Screenshots or GIFs (especially for UI changes).

---

## 7. Security and privacy considerations

Because DoppelGit handles **GitHub PATs**:

- Be cautious when:
  - Logging data,  
  - Adding analytics,  
  - Introducing new network calls.  
- Never:
  - Log raw PATs,  
  - Send PATs to third-party servers,  
  - Include PATs in crash reports or telemetry.

Any change that touches:

- Storage of identities,  
- Encryption,  
- Network requests,

should be clearly described in the PR and may need extra review.

See `SECURITY.md` and `PRIVACY.md` for the intended security and privacy model.

---

## 8. Documentation updates

If your change affects:

- User-facing behavior,  
- Security or privacy characteristics,  
- Pricing, plans, or feature tiers,

please update:

- `README.md` for high-level positioning and usage.  
- `docs/USAGE.md` for behavioral changes.  
- `docs/SECURITY.md` / `docs/PRIVACY.md` if security or privacy characteristics change.  
- `docs/ROADMAP.md` if you are moving or adding roadmap items.

---

## 9. Code of conduct

Please:

- Be respectful and constructive in issues, discussions, and PRs.  
- Assume good intent and seek to clarify misunderstandings.  
- Help maintain a welcoming environment for contributors at all experience levels.

If a separate **Code of Conduct** file exists in the repo, it applies to all interactions in this project.

---

## 10. Thank you

Your contributions — whether they are bug reports, documentation improvements, or new features — help make DoppelGit more useful for everyone who juggles multiple GitHub identities.

Thanks for helping improve the project!


