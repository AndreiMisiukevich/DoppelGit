## DoppelGit Roadmap

This roadmap outlines the planned evolution of DoppelGit. It is intentionally high-level and subject to change based on feedback and real-world usage.

---

## NOW

Focus: **Stability and reliability on GitHub.**

- **Stability improvements**
  - Harden core flows (creating PRs, issues, comments) against DOM changes and edge cases.  
  - Improve error handling and user-facing messages when GitHub returns 4xx/5xx responses.  
  - Add better diagnostics/logging toggles for debugging issues without leaking sensitive data.  

- **Better GitHub DOM detection**
  - More robust detection of:
    - PR pages  
    - Issue pages  
    - Repo context (owner/repo/branch)  
  - Graceful degradation when GitHub UI changes (feature flags, A/B tests, redesigns).  
  - Fewer false positives/negatives when injecting UI.

---

## NEXT

Focus: **Richer identity-aware workflows and team support.**

- **“Review as identity”**
  - Perform code reviews and submit review comments as a specific identity.  
  - Streamlined UI for switching reviewer identity before submitting.  
  - Support for:
    - Approve / Request changes / Comment reviews  
    - Inline comments via the identity’s PAT (subject to GitHub API capabilities).

- **Labels & assignees via identity**
  - Apply labels to issues and PRs as a chosen identity.  
  - Set assignees or reviewers where permissions allow.  
  - Useful for:
    - Bot workflows (triage, labeling, assignment).  
    - Team leads or agencies that manage multiple repos.

- **Team / agency support**
  - Opinionated workflows for:
    - Agencies and consultancies managing multiple client repos.  
    - Teams that want shared patterns but separate tokens and identities.  
  - Possible features:
    - Identity templates per client.  
    - Predefined label/assignee configurations.

- **UI improvements for options page**
  - Cleaner identity management interface:
    - Search and filter identities.  
    - Grouping by client, project, or type (human vs bot).  
  - Easier configuration of:
    - Encryption and lock settings (Pro).  
    - Plan status and upgrade flow.  
    - Default behavior and shortcuts.

---

## LATER

Focus: **Platform expansion and advanced workflows.**

- **Firefox / Safari ports**
  - Port DoppelGit to:
    - **Firefox** (WebExtension APIs).  
    - **Safari** (Safari Web Extensions).  
  - Maintain a mostly shared codebase with browser-specific adjustments.  

- **Organization-wide identity templates**
  - Allow organizations to define standard identity templates:
    - Recommended PAT scopes.  
    - Naming conventions and color-coding.  
    - Suggested labels and workflows.  
  - Help large teams adopt DoppelGit consistently.

- **API access**
  - Provide a (local) API surface that:
    - Exposes identity info to local scripts or tools, subject to permission.  
    - Allows advanced users to integrate DoppelGit with other automations or dashboards.  
  - Keep security front-and-center (no remote control without explicit consent).

- **Offline mode**
  - Improve behavior when offline:
    - Queue certain non-sensitive actions for later (where safe and applicable).  
    - Display clear “offline” indicators and re-try behavior.  
  - Maintain a predictable experience even with intermittent connectivity.

---

## Roadmap feedback

The roadmap is guided by:

- Real-world usage and bug reports.  
- Feedback from:
  - Individual developers,  
  - OSS maintainers,  
  - Agencies and teams.  

If you have suggestions:

- Open an issue in the repo describing:
  - Your workflow,  
  - Pain points,  
  - What you’d like DoppelGit to support.  

Your feedback helps prioritize what moves from **LATER** to **NEXT** to **NOW**.


