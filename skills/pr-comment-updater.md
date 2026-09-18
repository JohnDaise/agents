---
name: pr-comment-updater
description: "Use when updating an existing GitHub pull request comment with a contribution summary, Jira ticket context, testing steps, screenshots, and checklist. Select the matching template from the workspace templates directory and consult the jira-integration skill; use the MFE branch-cookie reference when applicable."
---

# Pull Request Comment Updater

Update an existing pull request comment using the team's contribution template. This skill is for editing an existing PR comment, not creating a new Jira issue or opening a new PR.

## Required References

- Jira workflow: read and follow `../jira-integration.md`.
- Templates: choose one of:
  - `../../templates/mfe-template.md` for micro-frontend changes.
  - `../../templates/shared-packages-template.md` for shared package changes.
  - `../../templates/go-service-template.md` for Go service changes.
- MFE branch cookies: consult [All Micro-frontend (MFE) Branch Cookies](https://totalwine.atlassian.net/wiki/spaces/Frontini/pages/4998463596/All+Micro-frontend+MFE+Branch+Cookies) when testing requires a feature branch cookie. Do not guess or invent cookie names or values.

## Workflow

1. Identify the repository, pull request number, and the existing comment to update. If the PR or comment is ambiguous, ask the user before editing.
2. Determine the change type and select the corresponding template. Preserve the template's headings, checklist, and testing structure.
3. Get the Jira issue key and title from the PR context, branch name, commit messages, or user-provided details. Use the Jira integration skill to retrieve the issue details when needed. Keep the Jira link in the format used by the selected template.
4. Replace every placeholder with factual information from the PR, Jira issue, test output, and user-provided screenshots. Do not claim tests, screenshots, or checklist items that are not verified.
5. For MFE or service testing, consult the branch-cookie reference above when a feature branch cookie is applicable. Include the exact applicable cookie instruction in the Testing Steps section, and include the relevant page, endpoint, or acceptance criteria.
6. For a Go service, retain both Develop and Feature Branch curl sections from `go-service-template.md`. For an MFE or shared package, retain the testing section from its selected template and adapt `{MFE PAGE}` only when an MFE page is involved.
7. Update only the intended existing PR comment through the GitHub pull-request comment API. Do not add a duplicate comment unless the user explicitly asks for one.
8. Report the PR URL, comment identifier if available, selected template, and a concise summary of the updated content.

## Content Rules

- Preserve source-of-truth wording from Jira and the PR; do not silently rewrite acceptance criteria or legal/product copy.
- Keep `Changes Made` to no more than seven focused bullets, as required by the templates.
- Keep the checklist accurate. Mark an item complete only when the repository or test results support it.
- If Jira authentication, PR-comment permissions, or the branch-cookie page is unavailable, stop before updating and tell the user exactly what information or access is missing.
- Never expose credentials, access tokens, or private cookie values in the comment unless the repository's established workflow explicitly requires a non-secret cookie value.

## Completion Checklist

- [ ] Correct PR and existing comment identified.
- [ ] Correct template selected from `templates/`.
- [ ] Jira issue details verified using `jira-integration`.
- [ ] Branch-cookie guidance checked when applicable.
- [ ] Testing steps and results are factual and complete.
- [ ] Existing PR comment updated without creating a duplicate.
