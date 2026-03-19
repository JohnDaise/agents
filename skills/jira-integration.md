---
name: jira-integration
description: "Workflow for creating, managing, and tracking Jira issues. Use when: creating Jira tickets, updating Jira issues, searching Jira, reporting bugs to Jira, creating stories/tasks in Jira, managing Jira workflow transitions."
---

# Jira Integration

## Purpose
This skill provides comprehensive guidance for working with Jira through the Atlassian MCP tools. Use this skill to create issues, update tickets, search for existing issues, manage workflow transitions, and track work in Jira.

## When to Use This Skill
- Creating bug reports in Jira from test results
- Creating user stories or tasks in Jira
- Updating existing Jira issues
- Searching for related issues before creating new ones
- Adding comments to Jira tickets
- Transitioning issues through workflow states
- Assigning issues to team members
- Linking issues or adding metadata

## Prerequisites

Before using Jira MCP tools, you need:
1. **Cloud ID**: Your Atlassian site identifier (can use site URL like `yoursite.atlassian.net`)
2. **Project Key**: The project where you're creating/managing issues
3. **Authentication**: MCP server handles OAuth, but you must be logged in to Atlassian

### Getting Your Cloud ID

If you don't know your Cloud ID, use:
```javascript
mcp_atlassian_getAccessibleAtlassianResources()
```

This returns all accessible Atlassian sites with their Cloud IDs.

Alternatively, if you have a Jira URL (e.g., `https://mycompany.atlassian.net/browse/PROJ-123`), try using the hostname (`mycompany.atlassian.net`) as the Cloud ID first.

## Important: User Approval Required

**CRITICAL WORKFLOW REQUIREMENT:**

Before creating any Jira issue (bug, story, task, etc.), you MUST:
1. **Show the user** the complete details of the ticket you plan to create
2. **Ask for explicit permission** to create the ticket
3. **Wait for user approval** before invoking any create/edit Jira tools

**Example Approval Request:**
```
I found a bug and would like to create a Jira ticket with the following details:

**Summary:** Login button not responding on mobile Safari

**Issue Type:** Bug

**Priority:** High

**Description:**
[Show full description]

**Labels:** mobile, safari, login

May I create this Jira ticket? (yes/no)
```

Only after receiving user approval should you proceed with `mcp_atlassian_createJiraIssue`.

## Core Workflows

### 1. Find Available Projects

Before creating issues, find the correct project:

```javascript
mcp_atlassian_getVisibleJiraProjects({
  cloudId: "yoursite.atlassian.net",
  action: "create"  // Filter to projects where you can create issues
})
```

**Returns**: List of projects with keys, names, and available issue types.

### 2. Create a Bug Report

**Use Case**: QA Engineer reports a bug found during testing

**IMPORTANT**: Always show the ticket details to the user and get approval before creating.

```javascript
mcp_atlassian_createJiraIssue({
  cloudId: "yoursite.atlassian.net",
  projectKey: "PROJ",
  issueTypeName: "Bug",
  summary: "Login button not responding on mobile Safari",
  description: `## Bug Description
User cannot tap the login button on iPhone Safari. Button appears clickable but no action occurs.

## Environment
- Device: iPhone 14 Pro
- OS: iOS 17.2
- Browser: Safari
- Screen size: 390x844

## Steps to Reproduce
1. Navigate to /login page on iPhone Safari
2. Enter valid credentials
3. Tap the "Login" button
4. Observe: Nothing happens

## Expected Behavior
User should be logged in and redirected to dashboard

## Actual Behavior  
Button tap does not trigger any action. No error message displayed.

## Severity
High - Blocks mobile users from logging in`,
  contentFormat: "markdown",
  additional_fields: {
    priority: { name: "High" },
    labels: ["mobile", "safari", "login"]
  }
})
```

**Response**: Returns the created issue key (e.g., PROJ-123) and full issue details.

### 3. Create a User Story or Task

**Use Case**: Project Manager creates work items

**IMPORTANT**: Always show the ticket details to the user and get approval before creating.

```javascript
mcp_atlassian_createJiraIssue({
  cloudId: "yoursite.atlassian.net",
  projectKey: "PROJ",
  issueTypeName: "Story",
  summary: "Add export to CSV functionality for user reports",
  description: `## User Story
As a data analyst, I want to export user reports to CSV format so that I can analyze data in Excel.

## Acceptance Criteria
- [ ] Export button visible on user reports page
- [ ] CSV file includes all visible columns
- [ ] File name includes report type and current date
- [ ] Export works for up to 10,000 rows
- [ ] Loading indicator shown during export
- [ ] Success message displayed on completion

## Technical Notes
- Use existing report query logic
- Generate CSV server-side to handle large datasets
- Consider implementing background job for exports >5000 rows`,
  contentFormat: "markdown",
  additional_fields: {
    priority: { name: "Medium" },
    labels: ["enhancement", "reporting"],
    customfield_10016: 5  // Story points (example, field ID varies by instance)
  }
})
```

### 4. Search for Existing Issues

**Use Case**: Check if bug already reported before creating new ticket

```javascript
mcp_atlassian_searchJiraIssuesUsingJql({
  cloudId: "yoursite.atlassian.net",
  jql: 'project = PROJ AND issuetype = Bug AND status != Closed AND text ~ "login button"',
  fields: ["summary", "status", "priority", "assignee", "created"],
  maxResults: 20,
  responseContentFormat: "markdown"
})
```

**Common JQL Patterns**:
```jql
// Find open bugs in a project
project = PROJ AND issuetype = Bug AND status != Done

// Find my assigned issues
assignee = currentUser() AND status != Closed

// Find recently created high priority issues
project = PROJ AND priority = High AND created >= -7d

// Find issues with specific text
project = PROJ AND text ~ "authentication"

// Find issues by label
project = PROJ AND labels = "frontend"

// Complex query with multiple conditions
project = PROJ AND issuetype IN (Bug, Story) AND status = "In Progress" AND assignee = currentUser()
```

### 5. Get Issue Details

**Use Case**: Retrieve full details of a specific issue

```javascript
mcp_atlassian_getJiraIssue({
  cloudId: "yoursite.atlassian.net",
  issueIdOrKey: "PROJ-123",
  responseContentFormat: "markdown",
  fields: ["summary", "description", "status", "priority", "assignee", "reporter", "created", "updated"]
})
```

### 6. Add Comment to Issue

**Use Case**: Provide updates or additional information

```javascript
mcp_atlassian_addCommentToJiraIssue({
  cloudId: "yoursite.atlassian.net",
  issueIdOrKey: "PROJ-123",
  commentBody: `## Test Results

Retested the login functionality after the latest deployment:

✅ Desktop Chrome: Working correctly
✅ Desktop Firefox: Working correctly  
✅ Desktop Safari: Working correctly
❌ Mobile Safari: Still not working (same issue)
✅ Mobile Chrome: Working correctly

The issue appears to be specific to Mobile Safari. Testing on iOS 17.2 and 17.3.`,
  contentFormat: "markdown"
})
```

### 7. Update Issue Fields

**Use Case**: Update priority, assignee, or custom fields

```javascript
mcp_atlassian_editJiraIssue({
  cloudId: "yoursite.atlassian.net",
  issueIdOrKey: "PROJ-123",
  fields: {
    priority: { name: "Critical" },  // Escalate priority
    labels: ["mobile", "safari", "login", "critical"],  // Add labels
    description: "Updated description with new information..."
  }
})
```

### 8. Assign Issue to User

**Use Case**: Assign bug to developer

First, look up the user's account ID:
```javascript
mcp_atlassian_lookupJiraAccountId({
  cloudId: "yoursite.atlassian.net",
  searchString: "john.doe@company.com"  // Or user's display name
})
```

Then assign the issue:
```javascript
mcp_atlassian_editJiraIssue({
  cloudId: "yoursite.atlassian.net",
  issueIdOrKey: "PROJ-123",
  fields: {
    assignee: { accountId: "5b10ac8d82e05b22cc7d4ef5" }  // From lookup result
  }
})
```

### 9. Transition Issue Through Workflow

**Use Case**: Move issue from "To Do" to "In Progress" to "Done"

First, get available transitions:
```javascript
mcp_atlassian_getTransitionsForJiraIssue({
  cloudId: "yoursite.atlassian.net",
  issueIdOrKey: "PROJ-123"
})
```

Then transition the issue:
```javascript
mcp_atlassian_transitionJiraIssue({
  cloudId: "yoursite.atlassian.net",
  issueIdOrKey: "PROJ-123",
  transition: {
    id: "31"  // Transition ID from getTransitions result (e.g., "In Progress")
  },
  fields: {
    // Optionally set fields required by the transition
    resolution: { name: "Done" }  // If transitioning to Done
  }
})
```

**Common Workflow Transitions**:
- **To Do → In Progress**: Developer starts work
- **In Progress → Code Review**: Code ready for review
- **Code Review → Testing**: Passed code review, ready for QA
- **Testing → Done**: QA verified, issue resolved
- **Any → Won't Do**: Issue closed without implementation

### 10. Get Project Issue Types and Metadata

**Use Case**: Find out what issue types and fields are available in a project

```javascript
mcp_atlassian_getJiraProjectIssueTypesMetadata({
  cloudId: "yoursite.atlassian.net",
  projectIdOrKey: "PROJ"
})
```

**Returns**: All issue types (Bug, Story, Task, Epic, etc.) with their available fields and configurations.

## Integration with Other Skills

### With defect-management Skill
When reporting bugs, use both skills together:
1. **defect-management** - Structure the bug report with proper severity, steps to reproduce, etc.
2. **jira-integration** - Create the Jira ticket with the structured information

### With qa-test-execution Skill
After testing:
1. **qa-test-execution** - Perform testing and document results
2. **jira-integration** - Report failures as Jira bugs
3. **jira-integration** - Add test results as comments to existing tickets

### With requirements-clarification Skill
For project management:
1. **requirements-clarification** - Define clear acceptance criteria
2. **jira-integration** - Create Jira story/task with defined criteria

## Best Practices

### User Approval Workflow

**NEVER create Jira tickets automatically.** Always follow this workflow:

1. **Prepare**: Structure the ticket with all details (summary, description, priority, labels)
2. **Present**: Show the complete ticket information to the user in a clear, readable format
3. **Request**: Explicitly ask "May I create this Jira ticket?" or "Should I create this in Jira?"
4. **Wait**: Wait for explicit user approval (yes/confirm/approved)
5. **Create**: Only after approval, invoke the Jira creation tool
6. **Confirm**: After creation, show the user the Jira issue key (e.g., PROJ-123)

**Example Interaction:**
```
Agent: "I've identified a bug that should be reported. Here's the Jira ticket I would create:

📋 **Jira Ticket Details**

**Project:** PROJ
**Issue Type:** Bug
**Summary:** Login button not responding on mobile Safari
**Priority:** High

**Description:**
## Bug Description
[Full details here...]

**Labels:** mobile, safari, login

Would you like me to create this Jira ticket?"

User: "Yes, please create it"

Agent: [Creates ticket]
"✓ Jira ticket created: PROJ-123"
```

### Creating Issues

**✅ DO:**
- **Always get user approval before creating tickets**
- Search for duplicates first using JQL
- Use clear, descriptive summaries
- Include all relevant context in description
- Use markdown formatting for readability
- Add appropriate labels for categorization
- Set correct priority based on impact
- Include steps to reproduce for bugs
- Define clear acceptance criteria for stories
- Use consistent formatting across issues

**❌ DON'T:**
- **NEVER create tickets without user approval**
- Create duplicate issues without checking
- Use vague summaries like "Fix bug" or "Update feature"
- Leave description empty
- Report multiple unrelated issues in one ticket
- Set everything as "Critical" priority
- Forget to specify environment details for bugs

### Content Format

Use `contentFormat: "markdown"` for:
- Better readability in both Jira and tools
- Easier to write structured content
- Lists, code blocks, and headings work naturally

Use `contentFormat: "adf"` (Atlassian Document Format) only when:
- Need advanced Jira features (@mentions, Smart Links, panels)
- Interfacing with Confluence or other Atlassian products

### Searching Issues

**Effective JQL Queries:**
```jql
// Find unassigned bugs
project = PROJ AND issuetype = Bug AND assignee is EMPTY

// Find stale issues (no updates in 30 days)
project = PROJ AND updated < -30d AND status != Done

// Find issues in sprint
project = PROJ AND sprint = "Sprint 42"

// Find high priority issues not in progress
project = PROJ AND priority IN (Critical, High) AND status = "To Do"

// Find issues by component
project = PROJ AND component = "Authentication"
```

### Workflow Transitions

- Always get available transitions first before attempting to transition
- Some transitions may require specific fields to be set
- Transition IDs are environment-specific (differ between Jira instances)
- Add comments when transitioning to explain state change

### Labels and Custom Fields

- Use consistent label naming conventions (lowercase, hyphenated)
- Check project metadata to find custom field IDs
- Custom field IDs vary by Jira instance (e.g., `customfield_10016`)
- Document your project's custom fields for reuse

## Common Workflows by Role

### QA Engineer Workflow

1. **During Testing**: Search for existing bugs before reporting
2. **Found Bug**: Structure bug report with defect-management skill
3. **Show to User**: Display proposed Jira ticket and ask for approval
4. **Create Ticket**: Only after approval, create bug report with jira-integration
5. **Verification**: Add comment with test results
6. **Bug Fixed**: Transition to "Done" or reopen if issue persists

### Project Manager Workflow

1. **Requirements Gathering**: Use requirements-clarification skill
2. **Create Work Items**: Structure stories/tasks with acceptance criteria
3. **Show to User**: Display proposed Jira ticket and ask for approval
4. **Create Ticket**: Only after approval, create stories/tasks in Jira
5. **Assignment**: Assign to appropriate team member
6. **Tracking**: Search and monitor in-progress work
7. **Closure**: Transition completed work through workflow

### Engineer Workflow

While engineers won't typically invoke Jira Integration directly:
- May reference Jira issues during implementation
- Can add comments with technical details
- Transition issues as work progresses

## Troubleshooting

### Issue: Can't Find Projects
**Solution**: Use `getVisibleJiraProjects` to list accessible projects

### Issue: Invalid Custom Field
**Solution**: Use `getJiraProjectIssueTypesMetadata` to find valid field IDs for your project

### Issue: Transition Failed
**Solution**: Use `getTransitionsForJiraIssue` to see available transitions for the current issue state

### Issue: User Not Found
**Solution**: Use `lookupJiraAccountId` to search for users by email or display name

### Issue: Permission Denied
**Solution**: Verify you have the correct permissions in the Jira project (ask Jira admin)

## Quick Reference

### Essential Tools

| Tool | Purpose | When to Use |
|------|---------|-------------|
| `createJiraIssue` | Create new issue | Report bug, create story/task |
| `searchJiraIssuesUsingJql` | Search issues | Find duplicates, query status |
| `getJiraIssue` | Get issue details | Review existing issue |
| `editJiraIssue` | Update fields | Change priority, labels, description |
| `addCommentToJiraIssue` | Add comment | Provide updates, test results |
| `transitionJiraIssue` | Change status | Move through workflow |
| `getTransitionsForJiraIssue` | List transitions | See available next states |
| `getVisibleJiraProjects` | List projects | Find project keys |
| `lookupJiraAccountId` | Find user | Get account ID for assignment |

### Issue Types

- **Bug**: Defect or problem in existing functionality
- **Story**: User-facing feature or enhancement
- **Task**: Technical work item (refactoring, setup, etc.)
- **Epic**: Large body of work spanning multiple stories
- **Subtask**: Child task of a story or bug

### Priority Levels (Typical)

- **Critical/Blocker**: Production down, data loss, security issue
- **High**: Major feature broken, significant user impact
- **Medium**: Moderate impact, workaround available
- **Low**: Minor issue, cosmetic, nice-to-have

Note: Priority names and levels vary by Jira configuration.
