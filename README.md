# Open Source Compliance Kit

A self-service toolkit for World Bank Group GitHub organization members to prepare their repositories for open source publication. It includes a reusable GitHub Actions compliance workflow and an interactive VS Code Copilot skill that audits repos against the WBG open source checklist and helps fix any gaps — so teams can get their repos ready before submitting for OSPO review.

## Getting Started

This repo provides two tools you can copy into your own repository:

1. **Compliance workflow** — Copy [`.github/compliance/sample-per-repo-caller.yml`](.github/compliance/sample-per-repo-caller.yml) to your repo as `.github/workflows/compliance.yml` and push. The workflow runs automatically and creates a compliance issue with results.

2. **Copilot skill** — Copy the [`.github/skills/wbg-oss-review/`](.github/skills/wbg-oss-review/) directory into your repo, open it in VS Code, and ask Copilot to "prepare my repo for open source". The skill will audit your repo and help fix any gaps.

See [How Teams Adopt](#how-teams-adopt) for the full walkthrough.

## Checks

- **License** — Whitelist: MIT, Apache-2.0. Requires WB IGO Rider for each.
- **README** — Presence and content checks
- **Secrets** — Queries GitHub Secret Scanning API (public repos); TruffleHog fallback (private repos)
- **Data Files** — Warns on large data files; suggests data catalog
- **Dependencies** — Queries Dependabot Alerts API
- **Dependency Licenses** — Trivy filesystem scan; fails on restricted licenses (GPL family, etc.). Works on public + private repos with no GHAS required
- **Code Quality (Basic)** — Local linters (eslint/ruff/flake8) or Super-Linter fallback
- **Code Quality (CodeQL)** — Queries GitHub Code Scanning API (public repos); skipped for private repos
- **Tech Stack** — GitHub Languages API + framework detection (informational)
- **AI Usage** — Detects AI library usage (informational)

For Secret Scanning, Dependabot, and CodeQL, the workflow **queries native GitHub APIs**.

### Where results show up

| Trigger | Surface |
|---|---|
| Pull request | A **PR comment** with the checklist is upserted on each push, so teams can iterate on fixes pre-merge without thrashing the persistent issue. |
| Push to default branch · daily cron · manual dispatch | The **persistent compliance issue** is created/updated as the authoritative paper trail. It stays open while any required check fails and is closed (with a final summary) once all pass. |
| Any other push (e.g. a feature branch with no open PR) | Workflow summary only — no issue or comment. |

Code quality checks are optional and don't block the issue from closing.

## Org Prerequisites

1. Enable **Dependabot alerts** org-wide: Settings > Code security > Dependabot alerts

2. *(Optional)* In **GitHub Org Secrets**, add:
   - `DEPENDABOT_TOKEN` — PAT with `security_events` scope for Dependabot API access on private repos

## How It Works

This kit provides two complementary tools that teams can use to prepare their repositories for open source publication.

1. **Compliance Workflow** (automated CI): A reusable GitHub Actions workflow that runs on every push to the default branch, every PR, and a daily schedule. It checks license, README, secrets, data files, dependencies, dependency licenses, and code quality. PR runs comment the checklist directly on the PR; default-branch runs update a persistent compliance issue (paper trail). The team iterates until checks pass.
2. **Copilot Skill** (interactive self-service): A Copilot skill that audits the repo interactively, generates a structured report, and helps create or fix compliance files. Teams copy the skill into their repo and invoke it in VS Code.

The intranet **public repository review request form** links to this repo so that teams can self-service their preparation before submitting for review.

After the team prepares their repo, they submit for OSPO review via the intranet request form. Once approved, the OSPO **changes the repo visibility to public**. GitHub native features (Secret Scanning, CodeQL default setup) activate automatically on public repos.

The compliance workflow continues running after publication as an ongoing check.

## How Teams Adopt

### Step 1a: Add the compliance workflow

Copy [`.github/compliance/sample-per-repo-caller.yml`](.github/compliance/sample-per-repo-caller.yml) to the target repo as `.github/workflows/compliance.yml`. Push to the default branch. The workflow runs automatically and opens a persistent compliance issue with a checklist of results.

Teams that work through pull requests get the same checklist as a **PR comment** on each PR push, so fixes can be iterated and verified before merging. The persistent compliance issue only reflects the state of the default branch.

### Step 1b: Use the Copilot skill for interactive review

For an interactive, guided review, copy the [`.github/skills/wbg-oss-review/`](.github/skills/wbg-oss-review/) directory into the target repo. Then open the repo in VS Code and invoke the **wbg-oss-review** Copilot skill (e.g., "prepare my repo for open source" or "audit my repo for WBG").

The skill will:

1. Scan the repo and generate a structured audit report against the WBG checklist
2. Identify gaps and offer to create or fix files (README, LICENSE, IGO rider, CITATION.cff, etc.)
3. Guide you through next steps — including submitting for OSPO review

### Step 2: Submit for OSPO review

When the repo is ready, submit a **public repository review request** via the intranet. The OSPO will review the repo and approve it for publication.

### Step 3: Go public

Once approved, the OSPO changes the repo visibility to public in GitHub Settings. GitHub native features (Secret Scanning, CodeQL default setup) activate automatically.

## WB IGO Riders

Repos using MIT license must include [WB-IGO-RIDER.md](https://github.com/worldbank/.github/blob/main/WB-IGO-RIDER.md).

Repos using Apache-2.0 license must include [WB-IGO-RIDER-APACHE.md](https://github.com/worldbank/.github/blob/main/WB-IGO-RIDER-APACHE.md).

The compliance workflow validates that the rider content matches the canonical version.

## Contact

For more information or help with the Open Source Kit, please contact [github@worldbank.org](mailto:github@worldbank.org).

## License

This project is licensed under the MIT License together with the [World Bank IGO Rider](https://github.com/worldbank/.github/blob/main/WB-IGO-RIDER.md). The Rider is purely procedural: it reserves all privileges and immunities enjoyed by the World Bank, without adding restrictions to the MIT permissions. Please review both files before using, distributing or contributing.
