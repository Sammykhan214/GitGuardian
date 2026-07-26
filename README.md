# GitGuardian

**A demo project to enforce pull‑request workflows, branch discipline, and automatic version management in a team codebase.**

---

## 🚨 The Problem

In many AIML (or software) teams, members push code directly to the primary branch (often `main` / `master`).  
This leads to:

- Broken builds and undetected bugs
- No code review or knowledge sharing
- Messy version history
- No clear way to track which changes were approved or released

---

## 🛡️ What GitGuardian Does

GitGuardian is a fully automated system that:

- **Blocks direct pushes** to `main` using GitHub branch protection rules.
- **Requires all changes to come through Pull Requests** from named branches (`feature/*`, `bugfix/*`, `hotfix/*`).
- **Enforces branch naming conventions** with a custom GitHub Actions check.
- **Detects any direct push attempt** (even by admins) and automatically creates a GitHub issue flagging the violation.
- **Automatically bumps project versions** on every merge to `main`, using [Semantic Versioning](https://semver.org/).
- **Provides a clear, reproducible demo** that can be shown to the team and adapted for real‑world repositories.

Everything is transparent, self‑documented, and ready to be cloned and tested.

---

## 📁 Repository Structure
GitGuardian/
├── .github/
│ ├── workflows/
│ │ ├── enforcer-push-check.yml # Detects direct pushes to main
│ │ ├── enforcer-branch-naming.yml # Validates branch naming convention
│ │ └── version-bump.yml # Automatically bumps version on merge
│ └── (optional) CODEOWNERS, PULL_REQUEST_TEMPLATE.md
│
├── p1/ # Example: ATO_fraud_detection
│ ├── VERSION # Contains "0.1.0" (SemVer)
│ └── src/
│ └── train.py
│
├── p2/ # Example: Model_drift_detection
│ ├── VERSION
│ └── src/
│ └── evaluate.py
│
├── p3/ # Example: Data_preprocessing
│ ├── VERSION
│ └── src/
│ └── preprocess.py
│
├── p4/ # Example: Utils
│ ├── VERSION
│ └── src/
│ └── utils.py
│
├── p5/ # Example: Core_ML_model
│ ├── VERSION
│ └── src/
│ └── model.py
│
├── docs/
│ ├── branch-protection-settings.png # Screenshot of GitHub branch protection
│ └── test-scenario.md # Step-by-step demo test plan
│
├── .gitignore # Python gitignore
├── requirements.txt # Project dependencies (numpy)
└── README.md # This file


---

## 🔧 How It Works

### 1. Branch Protection (The Iron Wall)

Applied on the `main` branch via **Settings → Branches**:

- ✅ **Require a pull request before merging** – no direct pushes allowed.
- ✅ **Require approvals (≥ 1)** – at least one reviewer must sign off.
- ✅ **Dismiss stale pull request approvals when new commits are pushed** – keeps reviews meaningful.
- ✅ **Restrict who can push to matching branches** – only the team lead (and optionally CI bots) can push directly.

Any associate who tries `git push origin main` gets a `403` error.

### 2. Custom GitHub Actions (The Watchdog)

Even with branch protection, we added two safety nets:

#### 📛 Direct Push Detector (`enforcer-push-check.yml`)

- **Triggers:** on push to `main`.
- **Check:** examines the commit message. Merged PRs always start with `"Merge pull request #..."`.
- **If a direct commit is detected:** creates a GitHub issue tagging the pusher, and fails the workflow.
- **Purpose:** if an admin temporarily bypasses the rules, this acts as a loud alarm.

#### 📋 Branch Naming Convention Check (`enforcer-branch-naming.yml`)

- **Triggers:** on every PR (`opened`, `synchronize`, `reopened`).
- **Check:** validates that the source branch name starts with `feature/`, `bugfix/`, or `hotfix/`.
- **If invalid:** the CI check fails, preventing the merge (if status checks are required).
- **Purpose:** keeps the repository organised and makes the purpose of every branch instantly clear.

### 3. Automatic Version Bumping (`version-bump.yml`)

Every time a pull request is **merged to `main`**, the workflow:

1. Detects which project folders (`p1`–`p5`) were changed.
2. Reads the current version from `VERSION` (e.g., `0.1.0`).
3. Bumps the **patch** number by default (`0.1.0` → `0.1.1`).
4. If the merge commit contains a keyword, it bumps major or minor instead:
   - `[p1:major]` → `1.0.0`
   - `[p2:minor]` → `0.2.0`
5. Commits the updated `VERSION` file and creates a Git tag (e.g., `p1/v0.1.1`).

This ensures every merged change is traceable and every project’s release is automatically versioned.

---

## 🧪 Demo – Test It Yourself

1. **Clone the repo** and give a teammate (or a second GitHub account) write access (but do **not** add them to the push allowlist).
2. **Try a direct push to `main`** – it should be rejected.
3. **Create a branch with a wrong name** (e.g., `johns-fix`) and open a PR – the branch name check will fail.
4. **Temporarily remove the push restriction** (or comment it out), commit directly to `main`, and watch the direct‑push detector create an issue tagging you.
5. **Make a real change via a proper feature branch** (e.g., `feature/update-fraud-model`), open a PR, and merge it – the version bump will automatically increment the project’s version.

Full step‑by‑step test plan is available in [`docs/test-scenario.md`](docs/test-scenario.md).

---

## 🚀 Setting Up in Your Own Team’s Repo

1. Go to **Settings → Branches** and add a protection rule for `main` (or your default branch).
2. Enable **Require a pull request before merging** and **Require approvals**.
3. Restrict push access to only team leads / CI bots.
4. Copy the `.github/workflows/` folder into your repository.
5. Add the **enforcer-branch-naming** job as a required status check in the branch protection rule.
6. (Optional) Customise the allowed branch prefixes in `enforcer-branch-naming.yml`.
7. Add a `VERSION` file to each project/module you want to track.

---

## 🧰 Tech Stack

- **Python** (dummy AIML scripts)
- **GitHub Actions** (YAML workflows)
- **GitHub API** (issue creation via `gh` CLI)
- **Markdown** (documentation)

---

## 📸 Screenshot

![Branch protection settings](docs/branch-protection-settings.png)

---

## 🤝 Contributing

All changes must go through a Pull Request with a properly named branch (`feature/`, `bugfix/`, `hotfix/`).  
At least one review is required before merging into `main`.  
Yes, even this repository follows its own rules! 😄

---

## 📝 License

MIT – feel free to use and adapt for your own team.

---

*Built with ❤️ for teams that care about clean version control and automated release management.*