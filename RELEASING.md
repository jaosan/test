# Releasing

## Contents

- [1. Overview](#1-overview)
- [2. Version format](#2-version-format)
- [3. Versioning guidelines](#3-versioning-guidelines)
- [4. Rules](#4-rules)
- [5. Hotfix releases](#5-hotfix-releases)
- [6. JIRA traceability](#6-jira-traceability)

## 1. Overview

Releases mark stable, deployable versions of the codebase. Every release must be tagged in Git using a consistent naming convention to ensure traceability between versions, deployments, and JIRA tasks.

Releases are always made from the `main` branch. Before tagging, confirm that all pull requests intended for the release have been merged and that the application has been validated in the dev environment.

After each release, recreate the `dev` branch from the updated `main` to start the next release cycle clean.

---

## 2. Version format

Release tags must follow the `<YEAR>.<RELEASE>.<PATCH>` versioning scheme with a `v` prefix:

```
v<YEAR>.<RELEASE>.<PATCH>
```

Examples:

```
v26.1.0
v26.1.1
v26.2.0
```

---

## 3. Versioning guidelines

| Component | When to increment                                                                 | Example           |
|-----------|-----------------------------------------------------------------------------------|-------------------|
| YEAR      | At the start of each new calendar year                                            | v25.9.0 → v26.1.0 |
| RELEASE   | A new planned release is deployed (new features, improvements or maintenance)     | v26.1.0 → v26.2.0 |
| PATCH     | A hotfix or bug fix is applied on top of an existing release without new features | v26.1.0 → v26.1.1 |

Examples:

```
v26.1.0   → First release of 2026
v26.1.1   → Hotfix on top of the first release
v26.2.0   → Second planned release of 2026
v27.1.0   → First release of 2027
```

---

## 4. Rules

- Always tag releases from the `main` branch.
- Use annotated tags with a short release description.
- Tag names must always start with a lowercase `v`.
- Do not delete or reuse existing release tags.
- Each release tag should correspond to a merge into `main`.

---

## 5. Hotfix releases

A hotfix is a targeted fix for a critical issue in production, deployed outside of the normal release cycle.

1. Branch from `main` using the `fix/JIRA-XXX` prefix — see [CONTRIBUTING.md](CONTRIBUTING.md) for the full pull request workflow.
2. Merge the pull request into `main`.
3. Tag a patch release immediately after merging:

```bash
git checkout main
git pull
git tag -a v26.1.1 -m "v26.1.1 - JIRA-123: fix null reference when loading customer data"
git push origin v26.1.1
```

4. Recreate the `dev` branch from the updated `main` so the fix is included in the next release cycle:

```bash
git branch -D dev
git checkout -b dev main
git push --force origin dev
```

---

## 6. JIRA traceability

Release tags should reference all JIRA tasks included in the release. This enables full traceability from task to branch, commit, pull request, and release.

```bash
git tag -a v26.2.0 -m "v26.2.0 - JIRA-999: add approval workflow, JIRA-456: update Dataverse SDK dependencies"
git push origin v26.2.0
```

Keeping JIRA IDs in the tag message makes it straightforward to answer "what shipped in v26.2.0?" without reading the full commit log.
