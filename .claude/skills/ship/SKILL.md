---
name: ship
description: Safely review, scan, commit, and publish repository changes with Git and GitHub CLI.
allowed-tools: Read Glob Grep Bash(git:*) Bash(gh:*) Bash(gitleaks:*) Bash(npm:*) Bash(npx:*) Bash(pnpm:*) Bash(yarn:*) Bash(uv:*) Bash(pytest:*) Bash(python:*) Bash(dotnet:*) Bash(mvn:*) Bash(gradle:*)
---

# Safe Repository Shipping

Use this skill when the user asks to review, commit, publish, push, ship, or upload project changes to GitHub.

The goal is to make every publication deliberate, reviewable, reproducible, and resistant to accidental secret disclosure.

Do not skip review, verification, or secret scanning merely because the repository is small or described as a prototype.

## Core safety rules

Never perform any of the following without explicit user approval:

* force push;
* rewrite shared Git history;
* delete a local or remote branch;
* replace or modify an existing remote;
* create a public repository;
* change repository visibility;
* publish files that may contain secrets;
* bypass a real Gitleaks finding;
* bypass failed tests without clearly reporting the failure;
* include unrelated changes in a commit;
* discard local work;
* reset or clean the working tree;
* amend, squash, or rewrite commits not created during the current approved workflow.

Do not use these commands as part of the normal workflow:

```bash
git push --force
git push --force-with-lease
git reset --hard
git clean -fd
git checkout -- .
git restore .
git restore --staged .
```

Only execute a destructive command when the user explicitly requests that exact action after the consequences have been explained.

Prefer reversible operations.

## Phase 1: Confirm repository context

Run:

```bash
git rev-parse --show-toplevel
git status --short --branch
git branch --show-current
```

Confirm:

* the current directory belongs to the intended repository;
* the repository root is known;
* the current branch is known;
* modified and untracked files are visible;
* the repository is not in the middle of a merge, rebase, cherry-pick, revert, or conflicted operation.

Check for an interrupted Git operation when the status indicates one.

If the directory is not a Git repository, stop and explain that Git initialization is required.

Do not run `git init` automatically unless the user approves initialization.

## Phase 2: Inspect all changes

Run:

```bash
git status --short
git diff --stat
git diff
git diff --cached --stat
git diff --cached
```

Review tracked modifications, staged modifications, deletions, renames, and untracked files.

Pay particular attention to:

* `.env` and environment-specific files;
* passwords, tokens, API keys, and connection strings;
* private certificates and key files;
* credentials or service-account files;
* local Claude configuration;
* MCP configuration containing credentials;
* browser sessions and storage state;
* generated reports;
* database files;
* logs;
* archives;
* large binaries;
* editor-specific files;
* operating-system files;
* files outside the approved task scope.

Untracked files do not appear in a normal Git diff.

Read relevant untracked text files directly before proposing that they be staged.

Do not stage a file merely to inspect it.

Do not assume a file is safe merely because `.gitignore` does not exclude it.

## Phase 3: Verify ignored and local files

Inspect ignored files and rules:

```bash
git status --ignored --short
git check-ignore -v .env .env.local .claude/settings.local.json .mcp.local.json .playwright 2>/dev/null
```

Files that normally must not be committed include:

* `.env`;
* `.env.local`;
* `.env.development.local`;
* `.env.production.local`;
* `.claude/settings.local.json`;
* `.mcp.local.json`;
* Playwright browser authentication state;
* browser storage-state files;
* private connection strings;
* credential files;
* generated secrets;
* private certificates;
* local databases containing real data;
* local logs and reports.

`.env.example` may be committed only when it contains variable names, documentation, and fictional placeholder values.

Inspect `.mcp.json` before publication and confirm that it does not contain:

* tokens;
* API keys;
* passwords;
* private headers;
* embedded credentials;
* private connection strings.

Do not ignore the complete `.claude` directory when it contains project skills or shared project configuration that should be versioned.

## Phase 4: Run deterministic secret scanning

Verify that Gitleaks is available:

```bash
gitleaks version
```

Run the repository scan:

```bash
gitleaks detect --source . --no-banner
```

When the repository already contains Git history, ensure that the installed Gitleaks behavior scans the relevant committed history as well as the current worktree.

If Gitleaks reports a finding:

1. stop the publication workflow;
2. identify the affected file and finding type;
3. do not reproduce the full secret in the response;
4. determine whether the finding appears real or is a likely false positive;
5. remove or replace the sensitive value safely;
6. recommend rotating the credential when exposure is plausible;
7. rerun Gitleaks;
8. continue only after the finding is resolved or safely documented as a verified false positive.

Never bypass a real secret finding.

Never add a broad allowlist merely to silence a result without understanding it.

A clean Gitleaks result does not replace manual review.

## Phase 5: Determine applicable verification

Inspect the repository for available tooling and documented commands.

Relevant files may include:

* `package.json`;
* `pnpm-lock.yaml`;
* `yarn.lock`;
* `package-lock.json`;
* `pyproject.toml`;
* `requirements.txt`;
* `.csproj`;
* `.sln`;
* `pom.xml`;
* `build.gradle`;
* `gradlew`;
* `Makefile`;
* `README.md`;
* the current Spec Kit plan;
* continuous-integration workflows.

Run the smallest relevant checks first and expand according to risk.

Possible commands include:

```bash
npm test
npm run lint
npm run typecheck
npm run build
pnpm test
pnpm lint
yarn test
pytest
uv run pytest
dotnet test
mvn test
gradle test
```

Do not invent commands that the project does not define.

Do not install dependencies merely to complete the shipping workflow unless the user approves the installation.

Do not modify package manifests or lockfiles merely to make publication easier.

When no application code or test configuration exists, state that no code-level tests were applicable.

If a relevant check fails:

1. stop before commit or push;
2. retain the useful error details;
3. explain the likely cause;
4. propose the smallest safe correction;
5. do not silently bypass the failure.

Proceed despite a failed check only after explicit user approval, and document the unresolved failure in the final report.

## Phase 6: Apply security and differential review

For meaningful code changes, use the differential-review capability before committing.

Use insecure-defaults when changes affect:

* configuration;
* authentication;
* authorization;
* validation;
* environment variables;
* network exposure;
* database access;
* cryptography;
* secret handling;
* error handling;
* security controls.

Treat findings as review inputs:

* fix confirmed problems;
* explain verified false positives;
* surface unresolved risks;
* do not silently ignore material findings.

If one of these capabilities is unavailable, state that clearly.

Do not claim that a review capability ran unless it actually ran.

## Phase 7: Prepare a pre-publication summary

Before staging, committing, creating a repository, or pushing, present a summary containing:

* repository root;
* current branch;
* modified files;
* deleted or renamed files;
* untracked files proposed for inclusion;
* files deliberately excluded;
* Gitleaks result;
* tests and checks executed;
* differential-review result when applicable;
* insecure-defaults result when applicable;
* unresolved failures or risks;
* proposed commit message;
* whether a remote exists;
* current remote destination;
* intended push destination;
* intended repository visibility when repository creation is required.

Do not stage, commit, create a repository, or push until the user explicitly approves the proposed publication plan.

Approval to inspect changes is not approval to commit or publish them.

Approval to commit is not automatically approval to create a public repository.

## Phase 8: Stage only approved files

After approval, stage explicit files whenever practical:

```bash
git add path/to/file1 path/to/file2
```

Avoid:

```bash
git add .
git add -A
```

unless every changed and untracked file has been reviewed and the user explicitly approved including all of them.

After staging, run:

```bash
git status --short
git diff --cached --stat
git diff --cached
```

Confirm that:

* every staged file belongs to the approved scope;
* no secret or local configuration is staged;
* no unrelated change is staged;
* no unexpected generated file is staged;
* the staged diff matches the proposed commit.

If unexpected content appears, stop and correct the staging set using a non-destructive approach.

Do not discard the underlying working-tree changes.

## Phase 9: Create the commit

Use a concise commit message that describes the intent of the change.

Good examples:

```text
chore: establish secure AI engineering foundation
docs: document foundation workflow and safety rules
feat: add controlled repository shipping skill
fix: exclude local browser and Claude state
```

Avoid vague messages such as:

```text
update files
changes
fix stuff
commit
work
```

Commit only the reviewed staged changes:

```bash
git commit -m "<approved commit message>"
```

After committing, run:

```bash
git status --short --branch
git log -1 --oneline
```

If the commit fails, report the reason and stop before pushing.

Do not amend the commit automatically.

## Phase 10: Inspect repository remotes

Run:

```bash
git remote -v
git status --short --branch
```

If `origin` exists:

* confirm its fetch and push destinations;
* verify that it belongs to the intended repository;
* report the destination before pushing.

Do not replace, remove, or modify an existing remote without explicit approval.

If no remote exists, explain that a remote must be configured or a GitHub repository must be created.

## Phase 11: Create a GitHub repository only when approved

When the user explicitly approves repository creation, verify authentication:

```bash
gh auth status
```

Confirm:

* repository name;
* GitHub account or organization;
* intended visibility;
* whether `origin` already exists;
* whether the current contents passed review and scanning.

Default to a private repository.

A typical command is:

```bash
gh repo create <repository-name> --private --source=. --remote=origin
```

Do not use `--public` unless the user explicitly requests public visibility after reviewing the repository contents and associated risks.

Do not overwrite an existing remote.

Do not create the repository under an organization unless the user explicitly selected that organization and has permission.

After creation, verify:

```bash
git remote -v
gh repo view
```

## Phase 12: Push

Before pushing, confirm:

* the commit succeeded;
* the branch is correct;
* the working-tree state is understood;
* the remote destination is correct;
* tests and scans were reported;
* the user approved the push.

Use a normal push:

```bash
git push -u origin <current-branch>
```

Never use force push as part of the standard shipping workflow.

If the push is rejected:

1. report the rejection;
2. explain the likely cause;
3. do not automatically pull, merge, rebase, reset, or force push;
4. ask for approval before applying a reconciliation strategy.

Do not report publication as successful unless the push command actually succeeds.

## Phase 13: Final report

After a successful push, report:

* repository name;
* repository visibility when known;
* current branch;
* commit hash;
* commit message;
* remote destination;
* files included;
* tests performed;
* security scans performed;
* review capabilities used;
* unresolved warnings;
* whether the working tree is clean or still contains changes.

Use these verification commands:

```bash
git status --short --branch
git log -1 --oneline
git remote -v
```

When available, confirm the GitHub repository:

```bash
gh repo view
```

Never claim that GitHub contains a change unless the push succeeded.

## First publication of AI Engineering Foundation

For the initial publication of `ai-engineering-foundation`:

1. verify `CLAUDE.md`;
2. verify `.gitignore`;
3. verify `.env.example`;
4. verify `README.md`;
5. verify `.mcp.json` contains no credentials;
6. verify `.claude/settings.json` contains only shared project configuration;
7. verify `.claude/settings.local.json` is ignored;
8. verify `.playwright/` is ignored;
9. verify the project skills that should travel with the template;
10. scan the complete repository with Gitleaks;
11. inspect all files proposed for the first commit;
12. review the complete staged diff;
13. create the GitHub repository as private unless the user explicitly chooses public visibility;
14. perform a normal push;
15. verify the remote result;
16. validate the repository from GitHub or from a fresh clone;
17. enable the GitHub template-repository setting only after validation.

## Completion standard

The workflow is complete only when:

* the intended files were reviewed;
* secrets were scanned;
* applicable checks were executed;
* the staged diff was inspected;
* the commit succeeded;
* the approved push succeeded;
* the final repository state was reported accurately.

Never finish with only “done” or “published.”

State exactly what was reviewed, committed, tested, scanned, and pushed.
