---
name: cherry-pick
description: Workflow for creating a branch from `master` by cherry-picking one or more user-provided existing commits. USE FOR backports, selective promotions, hotfix propagation, and partial feature rollout from existing commits. DO NOT USE FOR full-branch merges, rebases, or situations where the desired change has not been committed yet.
---

# Pull Request Cherry-Pick Workflow

Use this skill when the goal is to open a pull request containing specific existing commits, rather than merging an entire source branch.

## When to Use This Skill
- Backporting fixes from `main` to a release branch
- Promoting selected commits between long-lived branches
- Shipping a hotfix without unrelated changes from the source branch
- Reusing an already-reviewed commit in another branch

## When Not to Use This Skill
- The target change is not committed yet
- The user wants the full source branch history
- The work depends on many intertwined commits that should be rebased or merged together
- The source commit is a merge commit and there is no clear mainline parent to preserve

## Core Principle

Cherry-pick only the minimal verified commits required for the target branch. Validate each pick in the target branch context before opening the pull request.

## Inputs You Need

Before starting, identify:
- Exact commit SHA or SHAs to cherry-pick (provided by the user)
- Branch name for the new working branch
- Reason for the cherry-pick, such as backport, hotfix, or selective rollout

If any of these are unclear, stop and ask rather than guessing.

## Recommended Workflow

### 1. Inspect the Commits

Confirm each commit is the right one and understand scope before applying it.

```bash
git show --stat <sha>
git log --oneline --decorate -n 20
```

Check for:
- Hidden dependencies on earlier commits
- Generated files or lockfile changes that must travel with the commit
- Migrations, schema changes, or config changes that may not fit the target branch

### 2. Create a Fresh Branch from `master`

Always start from `master`, not the source branch.

```bash
git fetch origin
git switch master
git pull --ff-only origin master
git switch -c <new-branch>
```

Suggested branch names:
- `backport/<ticket-or-sha>`
- `hotfix/<short-description>`
- `cherry-pick/<short-description>`

### 3. Cherry-Pick the Commits

Apply the minimal set of commits in dependency order.

Single commit:

```bash
git cherry-pick -x <sha>
```

Multiple commits:

```bash
git cherry-pick -x <sha1> <sha2> <sha3>
```

Range when commits are contiguous and order matters:

```bash
git cherry-pick -x <oldest-sha>^..<newest-sha>
```

Why `-x` matters:
- It appends the source SHA to the commit message
- It improves traceability for reviewers and release managers

### 4. Resolve Conflicts Carefully

If conflicts occur:

```bash
git status
git add <resolved-files>
git cherry-pick --continue
```

If the cherry-pick is wrong or too risky:

```bash
git cherry-pick --abort
```

Conflict handling rules:
- Preserve target-branch compatibility over source-branch exactness
- Do not silently drop behavior during conflict resolution
- If the commit depends on missing code, either cherry-pick the dependency too or stop and explain the blocker

### 5. Validate in the Target Branch Context

Run the narrowest checks that can prove the cherry-picked change still works.

Examples:

```bash
npm test -- <affected-test>
go test ./path/to/package
```

Validation checklist:
- Tests covering the touched behavior pass
- Build or typecheck still passes for the affected slice
- No unintended files were changed

### 6. Review the Branch Diff and Cherry-Pick Log

Ensure the branch only contains the intended changes relative to `master`.

```bash
git diff --stat origin/master...HEAD
git log --oneline origin/master..HEAD
git cherry -v origin/master
```

Look for:
- Extra commits
- Accidental formatting churn
- Missing follow-up changes needed by the picked commit

### 7. Stop Before PR Creation

Do not create a pull request in this workflow.

After confirming the log shows all intended cherry-picked commits, hand off to the user to create the pull request.

Optional, if the user wants the branch published first:

```bash
git push -u origin <new-branch>
```

## Decision Rules

- Prefer cherry-picking individual commits over broad ranges unless the range is known to be self-contained
- Avoid cherry-picking merge commits unless the user explicitly wants that and the correct mainline parent is known
- If a commit is not self-contained, say so explicitly and name the dependent commit SHAs
- If the target branch has materially diverged, expect code adaptation and revalidation

## Common Failure Modes

### Missing Dependency Commit
Symptom: Build or tests fail after cherry-pick.

Action:
- Inspect the original branch history
- Identify prerequisite commits
- Cherry-pick only the necessary dependencies

### Conflict Resolved Incorrectly
Symptom: Cherry-pick completes but behavior regresses.

Action:
- Review the conflict hunks against both branches
- Re-run focused tests
- Compare final diff with the original commit intent

### Wrong Base Branch
Symptom: Pull request shows unrelated changes.

Action:
- Abort if still in progress, or reset by recreating the branch from the correct base
- Re-apply the intended commit set

## Agent Execution Notes

When performing this workflow as an agent:
- Confirm exact user-provided SHAs before making branch changes
- Create the branch from `master`
- Prefer non-interactive git commands
- Validate after cherry-picking and show the commit log before any PR creation
- Report the exact SHAs picked, any conflicts encountered, and the checks run