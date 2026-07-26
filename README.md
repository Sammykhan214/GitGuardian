# \# GitGuardian

# 

# \*\*A demo project to enforce pull-request workflows and branch discipline in a team codebase.\*\*

# 

# \---

# 

# \## 🚨 The Problem

# 

# In many AIML (or software) teams, members often push code directly to the `master` branch without review.  

# This leads to:

# \- Broken builds and undetected bugs

# \- No code review or knowledge sharing

# \- Messy version history

# \- No way to track which changes were approved

# 

# \---

# 

# \## 🛡️ What GitGuardian Does

# 

# GitGuardian is a fully automated system that:

# 

# \- \*\*Blocks direct pushes\*\* to `master` using GitHub branch protection rules.

# \- \*\*Requires all changes to come through Pull Requests\*\* from named branches (`feature/\*`, `bugfix/\*`, `hotfix/\*`).

# \- \*\*Enforces branch naming conventions\*\* with a custom CI check (GitHub Actions).

# \- \*\*Detects any direct push attempt\*\* (even by admins) and automatically creates an issue flagging the violation.

# \- \*\*Provides a clear, reproducible demo\*\* that can be shown to the team and adapted for a real production repository.

# 

# Everything is transparent, self‑documented, and ready to be cloned and tested.

# 

# \---

# 

# \## 📁 Repository Structure



GitGuardian/

├── p1/src/train.py # Simulated ML training module

├── p2/src/evaluate.py # Evaluation placeholder

├── p3/src/preprocess.py # Data preprocessing stub

├── p4/src/utils.py # Utility functions (loader)

├── p5/src/model.py # ML model class

├── requirements.txt

├── .github/

│ └── workflows/

│ ├── enforcer-push-check.yml # Direct push detector

│ └── enforcer-branch-naming.yml # Branch name enforcer

└── docs/

├── branch-protection-settings.png # Screenshot of protection rule

└── test-scenario.md # Step-by-step test plan





The `p1`–`p5` folders simulate a monorepo where different team members own different components.



\---



\## 🔧 How It Works



\### 1. Branch Protection (The Iron Wall)

\- Applied on `master` via GitHub \*\*Settings → Branches\*\*.

\- \*\*Require a pull request before merging\*\* – no direct pushes.

\- \*\*Require approvals (≥1)\*\* – at least one reviewer must approve.

\- \*\*Restrict who can push\*\* – only the team lead (and optionally a manager) can push directly (for emergencies).

\- Any associate who tries `git push origin master` gets a `403` error.



\### 2. Custom GitHub Actions (The Watchdog)

Even with branch protection, we added two safety nets:



\#### 📛 Direct Push Detector

\- \*\*Triggers:\*\* On every push to `master`.

\- \*\*Check:\*\* Examines the commit message. Merged PRs always start with `"Merge pull request #..."`.

\- \*\*If a direct commit (non‑merge) is detected\*\* → creates a GitHub issue tagging the pusher, and fails the workflow.

\- \*\*Why?\*\* If an admin temporarily bypasses the rules, this acts as a loud alarm.



\#### 📋 Branch Naming Convention Check

\- \*\*Triggers:\*\* On every PR (`opened`, `synchronize`, `reopened`).

\- \*\*Check:\*\* Validates that the source branch name starts with `feature/`, `bugfix/`, or `hotfix/`.

\- \*\*If invalid\*\* → the CI check fails and prevents merging (if status checks are required).

\- \*\*Why?\*\* Keeps the repository organised and makes it easy to see the purpose of each branch.



\---



\## 🧪 Demo – Test It Yourself



1\. \*\*Clone the repo\*\* (or ask a teammate to do it with write access).

2\. \*\*Try to push directly to `master`\*\* – it should be rejected.

3\. \*\*Create a branch with a wrong name\*\* (e.g. `johns-fix`) and open a PR – the branch name check will fail.

4\. \*\*Temporarily lift the push restriction for yourself\*\*, commit directly to `master`, and watch the direct‑push detector create an issue.



Full step‑by‑step test plan is available in \[`docs/test-scenario.md`](docs/test-scenario.md).



\---



\## 🚀 Setting Up in Your Own Team’s Repo



To adapt this for a real project:



1\. Go to \*\*Settings → Branches\*\* and add a rule for your default branch (`master` or `main`).

2\. Enable \*\*Require a pull request before merging\*\* and \*\*Require approvals\*\*.

3\. Restrict push access to only team leads / CI bots.

4\. Copy the `.github/workflows/` folder into your repository.

5\. Add the branch name check as a \*\*required status check\*\* in the branch protection rule.

6\. (Optional) Customise the allowed branch prefixes in `enforcer-branch-naming.yml`.



\---



\## 🧰 Tech Stack



\- \*\*Python\*\* (dummy AIML scripts, no real dependencies)

\- \*\*GitHub Actions\*\* (YAML workflows)

\- \*\*GitHub API\*\* (issue creation via `gh` CLI)

\- \*\*Markdown\*\* (documentation)



\---



\## 📸 Screenshot



!\[Branch protection settings](docs/branch-protection-settings.png)



\---



\## 📝 License



MIT – feel free to use and adapt for your own team.



\---



\## 🤝 Contributing



All changes must go through a Pull Request with a properly named branch (`feature/`, `bugfix/`, `hotfix/`).  

At least one review is required before merging into `master`.  

Yes, even this repo follows its own rules! 😄



\---



\*Built with ❤️ for teams that care about clean version control.\*

