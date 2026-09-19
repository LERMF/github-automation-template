# GitHub Automation Template

> **Autonomous CI/CD governance and label-driven repository lifecycle management.**

Every manual merge, unformatted diff, and stalled dependency PR drains engineering momentum. Repository maintenance should not require human micromanagement. This repository provides a turnkey, production-hardened template implementing autonomous pull request lifecycle automation, deterministic code styling, proactive conflict detection, and automated issue triage.

---

## ✦ Architectural Topology

```
                  ┌──────────────────────────────────────────────┐
                  │            GitHub Actions Gateway            │
                  └──────┬──────────────┬──────────────┬─────────┘
                         │              │              │
           ┌─────────────▼────┐   ┌─────▼────────┐   ┌─▼──────────────────┐
           │   Auto-Merge     │   │   Auto-Fix   │   │  Conflict Detector │
           │ (Dependabot/bot) │   │ (Prettier/CI)│   │  (Label: conflict) │
           └──────────────────┘   └──────────────┘   └────────────────────┘
                         │              │              │
           ┌─────────────▼────┐   ┌─────▼────────┐   ┌─▼──────────────────┐
           │   Auto-Issue     │   │ Copilot-Fix  │   │     Dependabot     │
           │(Triage & Labels) │   │(Audit & Hook)│   │ (npm, pip, GHA)    │
           └──────────────────┘   └──────────────┘   └────────────────────┘
```

---

## ✦ Core Automation Matrix

| Workflow | File | Trigger Condition | Automated Action |
|---|---|---|---|
| **Auto Merge** | [`.github/workflows/auto-merge.yml`](.github/workflows/auto-merge.yml) | Dependabot PR or `auto-merge` label | Executes squash merge once branch checks pass |
| **Auto Fix** | [`.github/workflows/auto-fix.yml`](.github/workflows/auto-fix.yml) | Label `auto-fix` or manual dispatch | Runs Prettier/linters and commits formatted code directly |
| **Conflict Detection** | [`.github/workflows/auto-conflict-resolve.yml`](.github/workflows/auto-conflict-resolve.yml) | Target branch push / PR sync | Labels PR with `conflict` and alerts author to rebase |
| **Issue Triage** | [`.github/workflows/auto-issue.yml`](.github/workflows/auto-issue.yml) | `issues: [opened]` | Parses body for bug/feature markers and assigns labels |
| **Copilot Audit Hook** | [`.github/workflows/auto-copilot-fix.yml`](.github/workflows/auto-copilot-fix.yml) | Label `auto-copilot-fix` | Dispatches verification runner and leaves diagnostic status |
| **Dependabot Engine** | [`.github/dependabot.yml`](.github/dependabot.yml) | Weekly schedule | Automated vulnerability and upgrade scanning across 3 ecosystems |

---

## ✦ Quickstart Setup

### 1. Instantiate from Template
Click **"Use this template"** $\to$ **"Create a new repository"** on GitHub, or clone locally:
```bash
git clone https://github.com/LERMF/github-automation-template.git my-service
cd my-service
```

### 2. Configure Repository Permissions
Navigate to **Settings $\to$ Actions $\to$ General**:
- Under **Workflow permissions**, select **"Read and write permissions"**.
- Check **"Allow GitHub Actions to create and approve pull requests"**.

### 3. Establish Branch Protection (`main`)
Navigate to **Settings $\to$ Branches $\to$ Add branch protection rule**:
- **Branch name pattern**: `main`
- Enable **"Require status checks to pass before merging"**
- Enable **"Allow auto-merge"**

---

## ✦ CLI Operation & Label Triggers

Trigger workflows deterministically using the GitHub CLI:

```bash
# Trigger automated merge once CI passes:
gh pr edit <PR_NUMBER> --add-label "auto-merge"

# Format code and push formatting commit to branch:
gh pr edit <PR_NUMBER> --add-label "auto-fix"

# Request automated Copilot audit triage:
gh pr edit <PR_NUMBER> --add-label "auto-copilot-fix"
```

---

## ✦ Security & Hygiene Invariants

1. **Zero Secret Leakage**: Workflows rely exclusively on scoped `GITHUB_TOKEN` credentials with explicit job-level permission boundaries.
2. **Minimal Surface Area**: Dependencies are audited on a weekly schedule across `npm`, `pip`, and `github-actions`.
3. **Reproducible Execution**: Node runtime pinned to v22 LTS on `ubuntu-latest`.

## ✦ License
[MIT](LICENSE) © LERMF
