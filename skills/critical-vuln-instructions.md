# Security Triage Workflow

When asked to turn critical GitHub vulnerabilities into Jira work for the upFUNnel backlog, use this workflow.

## Source of truth
- Use GitHub Dependabot alerts as the vulnerability source.
- Prefer the GitHub API over local `npm audit` output for ticket creation.
- If GitHub MCP does not expose Dependabot alerts directly, use the existing Git credential helper with `curl` against the GitHub REST API.
- Only create tickets for open `critical` alerts.

## Repo-to-ticket mapping
- Create one Jira story per repository, not one story per package or advisory.
- Put every open critical alert for that repo into the same story description.
- Keep the wording simple and direct.
- Do not start the description with phrases like "As a manager" or "As a dev manager".
- Mention that dependency overrides are allowed for critical vulnerabilities.

## Jira defaults for this workflow
- Project: `DIG`
- Issue type: `Story`
- Priority: `Critical`
- Labels: `tech-debt`, `security`, `dependabot`, `critical-vulnerability`
- Assign to `Atinder Sohal` when not told otherwise.
- Use the required team/governance fields that match the existing DIG security stories:
  - `customfield_12604`: `Up FUNnel`
  - `customfield_13581`: `Tech Reliability, Site_App security, ADA - DG10082`

## Components
- Always add `Security / Performance`.
- Add the repo-specific DIG component when it exists.
- Known component mappings:
  - `product-search-mfe` -> `product-search-mfe`
  - `priority-access-mfe` -> `priority access`
  - `events-mfe` -> `events-mfe`
  - `priority-access-restate` -> `priority access`
  - `location-slideout-component` -> `location-slideout-component`
  - `rich-relevance-component` -> `rich-relevance-component`
  - `change-location-component` -> `change-location-component`

## Description template
- First line: `Fix the open critical Dependabot vulnerabilities in <repo>.`
- Then state this is tech debt work and that overrides are allowed for critical vulnerabilities.
- Add a `Critical vulnerabilities:` section with one bullet per alert.
- Each bullet should include package, GHSA, CVE when present, patched version, and alert URL.
- Add acceptance criteria to:
  - upgrade or override affected dependencies
  - resolve all open critical alerts for the repo
  - verify build/tests
  - document any override

## Duplicate cleanup
- If duplicate package-level stories were created by mistake, keep one canonical repo-level story and mark the others as duplicates.
- Before rejecting a duplicate parent story, reject its subtasks first.
- For duplicate subtasks, use the `Reject Subtask` transition.
- After all subtasks are rejected, reject the duplicate parent story.
- Add a Jira comment on duplicate tickets pointing to the canonical repo-level story.

## Validation
- After creating or updating stories, verify the final non-done ticket list so only one story per repo remains.
- If Jira workflow blocks rejection, inspect subtasks and transitions before retrying.