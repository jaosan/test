# Contributing

## Contents

- [1. Getting started](#1-getting-started)
- [2. Branch names](#2-branch-names)
- [3. Commit messages](#3-commit-messages)
- [4. Pull Requests](#4-pull-requests)
- [5. JIRA traceability](#5-jira-traceability)

## 1. Getting started

### Prerequisites

> List the tools and versions required to work on this project.

- [.NET SDK](https://dotnet.microsoft.com/download) — version X.X or later
- [Git](https://git-scm.com/)
- Access to the team's JIRA board

### Setup

```bash
# Clone the repository
git clone https://github.com/<org>/<repo>.git
cd <repo>

# Restore dependencies
dotnet restore
```

### Running the project

```bash
dotnet run
```

---

## 2. Branch names

### 2.1 Permanent branches

Every repository has two permanent branches:

| Branch | Purpose |
|--------|---------|
| `main` | Reflects the code currently running in production. Only updated via pull requests. |
| `dev`  | Integration branch for work targeting the next release. All task branches are merged here first and deployed to the dev/quality environment for testing. |

The `dev` branch is recreated from `main` after each release. This keeps it clean and ensures it only contains work planned for the next release. Any task that was in the previous `dev` but did not make the release must be re-merged into the new `dev`.

### 2.2 Format

All task branches must follow the format `<prefix>/<JIRA-ID>`.

### 2.3 Prefixes

| Prefix      | When to use                                                | Example             |
|-------------|------------------------------------------------------------|---------------------|
| feature     | Developing new features or updating existing features      | feature/JIRA-156    |
| fix         | Fixing bugs/errors                                         | fix/JIRA-888        |
| maintenance | Refactoring, improvements, updates or code cleanup         | maintenance/JIRA-91 |
| revert      | Reverting a previously merged pull request                 | revert/JIRA-456     |

### 2.4 Rules

- Always use lowercase for prefix.
- Always use a forward slash after the prefix.
- Always use the JIRA task identifier in uppercase.
- Do not use spaces.
- Do not include long descriptions.
- Create a branch for each task.
- Task branches must be created from `main`, or from another task branch if the work depends on it.
- Keep code changes in task branches small where possible.
- Merged branches are deleted automatically by GitHub. Run `git fetch --prune` regularly to fetch the latest changes and remove stale remote-tracking references from your local repository.

### 2.5 Valid and invalid examples

| ✅ Valid               | ❌ Invalid                        | Reason                                 |
|------------------------|------------------------------------|----------------------------------------|
| `feature/JIRA-999`     | `Feature/jira-999`                 | Prefix must be lowercase, ID uppercase |
| `fix/JIRA-123`         | `fix/JIRA-123-fix-login-bug`       | No long descriptions allowed           |
| `maintenance/JIRA-456` | `bugfix/JIRA-456`                  | Invalid prefix                         |
| `feature/JIRA-001`     | `feature/001`                      | JIRA prefix required                   |
| `fix/JIRA-888`         | `fix/jira-888`                     | JIRA ID must be uppercase              |
| `revert/JIRA-456`      | `revert/remove-bad-feature`        | JIRA ID required                       |

### 2.6 Branch protection

The `main` branch is protected with the following rules enforced by GitHub:

- Pull request required before merging — direct pushes to `main` are not allowed.
- At least **1 approval** from a reviewer is required before a pull request can be merged.
- All review conversations must be **resolved** before merging.
- Only the **merge** method is allowed — squash and rebase merges are disabled.
- **Signed commits** are required on all branches.

---

## 3. Commit messages

### 3.1 Format

Commit messages should follow the format: `<JIRA-ID>: <short work description>`.

Examples:

```
JIRA-999: add purchase approval workflow validation
JIRA-123: fix null reference when updating purchase data
JIRA-456: update packages
```

### 3.2 Rules

- Always start with the JIRA task identifier.
- Use English for descriptions.
- Use short and descriptive sentences.
- Use the imperative mood or direct description of the change.
- Avoid generic messages such as *fix, changes, update, wip*.
- Keep commits small.
- Don't mix unrelated changes in the same commit.

### 3.3 Intermediate commits

During development, intermediate commits are acceptable but must be cleaned up with an interactive rebase (`git rebase -i`) before opening the pull request. The final commit history on the branch should only contain clean, meaningful commits.

Example of an acceptable intermediate commit (to be squashed before the PR):

```
JIRA-678: work in progress on approval handler
```

---

## 4. Pull Requests

### 4.1 Format

The pull request title should follow the format: `<JIRA-ID>: <short task description>`.

Examples:

```
JIRA-999: add purchase approval workflow
JIRA-123: fix purchase update error
JIRA-456: update Dataverse SDK dependencies
```

### 4.2 Rules

- Always start with the JIRA identifier followed by a short description.
- Detail all the changes in the description.
- Create the pull request from a valid task branch.
- Keep the pull request as small as possible and focused on a single task.
- Always use **merge** (merge commit) as the method for merging a pull request into `main`.
- If the task branch is out of date with `main`, rebase it onto `main` before opening the pull request. This resolves conflicts on the task branch and keeps `main` history clean.
- If the work is not yet ready for review, open the pull request as a **draft** to signal that it is still in progress. Convert it to a regular pull request when it is ready for review.

### 4.3 Hotfix pull requests

Hotfixes are used to fix critical issues in production without waiting for the next planned release.

- Branch from `main` directly (never from `dev`).
- Use the `fix/JIRA-XXX` prefix.
- Open the pull request targeting `main`.
- After merging, a patch release is tagged — see [RELEASING.md](RELEASING.md).
- Recreate `dev` from the updated `main` so the fix is included in the next release cycle.

### 4.4 Template

```markdown
## Summary

Brief description of the change.

## Changes

- Added ...
- Updated ...
- Fixed ...

## JIRA task

JIRA-999

## Notes

Any relevant notes, risks, or deployment considerations.
```

### 4.5 Author checklist

- [ ] Branch follows the naming format: `<prefix>/<JIRA-ID>`
- [ ] Prefix is valid (feature, fix, maintenance or revert)
- [ ] Intermediate commits have been cleaned up (squashed/rebased)
- [ ] Commits include the JIRA ID
- [ ] Commit messages are clear and detail work done
- [ ] Branch is up to date with `main` (rebased if needed)
- [ ] Pull request title includes the JIRA ID
- [ ] Pull request description details all the changes included
- [ ] Pull request targets the correct base branch (main or dev)
- [ ] Pull request is merged using the **merge** method

### 4.6 Reviewer checklist

Pull requests must be reviewed and approved by a senior team member before merging. Reviewers should check the following:

- [ ] The pull request addresses the JIRA task it claims to
- [ ] The code is correct and free of obvious bugs or logic errors
- [ ] No sensitive data (credentials, tokens, personal data) is exposed
- [ ] The change does not break existing functionality
- [ ] The implementation follows the project's patterns and coding standards
- [ ] The code is readable and any non-obvious logic is adequately commented
- [ ] The pull request scope is appropriate — it does not include unrelated changes
- [ ] The branch naming and commit messages follow the conventions in this document

---

## 5. JIRA traceability

Use the JIRA task ID consistently across branches, commits, and pull requests to maintain full traceability. This makes it easy to understand what was changed, why, and when.

### 5.1 New feature

```
Branch:       feature/JIRA-999
Commit:       JIRA-999: add xyz field to xxx model
Pull Request: JIRA-999: update xyz endpoint to include xxx
Release tag:  v26.2.0
```

### 5.2 Bug fix / hotfix

```
Branch:       fix/JIRA-123
Commit:       JIRA-123: fix null reference when loading customer data
Pull Request: JIRA-123: fix customer data load error (targets main)
Release tag:  v26.1.1
```

### 5.3 Maintenance

```
Branch:       maintenance/JIRA-456
Commit:       JIRA-456: update Dataverse SDK to latest version
Pull Request: JIRA-456: update Dataverse SDK dependencies
Release tag:  v26.2.0
```

### 5.4 Revert

```
Branch:       revert/JIRA-456
Commit:       JIRA-456: revert Dataverse SDK update — caused plugin compatibility issue
Pull Request: Revert JIRA-456: remove Dataverse SDK update
Release tag:  v26.2.0
```
