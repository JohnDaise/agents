# Copilot Agents Guide

This workspace includes custom Copilot agents to assist with software development tasks.

## How to Use Agents

### In VS Code Chat

Agents are invoked in the Copilot chat using the `@` symbol:

```
@code-reviewer - Review my recent code changes
@go-engineer - Implement the backend microservice
@fe-engineer - Build the user dashboard UI
@project-manager - Orchestrate delivery of the export feature
@product-owner - Clarify the requirements for the export feature
@qa-engineer - Test the login functionality
@doc-researcher - Find information about React hooks
```

### Agent Reference

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| **@code-reviewer** | Reviews code for quality, clean code principles, and security | After writing or modifying code |
| **@doc-researcher** | Researches technical documentation and provides detailed answers | When you need to understand APIs, libraries, or best practices |
| **@fe-engineer** | Expert frontend engineer for React and JavaScript development | When building UI components, implementing frontend features, or working with React |
| **@go-engineer** | Expert Go engineer for backend services and microservices | When building backend services, APIs, microservices, or Go applications |
| **@project-manager** | Orchestrates the full development lifecycle from requirements to delivery, coordinating engineers, reviewers, and QA via a 10-step workflow. | When you want automated end-to-end feature delivery |
| **@product-owner** | Clarifies requirements, defines acceptance criteria, and provides product guidance to engineers. | When requirements are unclear, when engineers have specification questions, or when tradeoffs need stakeholder input |
| **@qa-engineer** | Tests features and bug fixes. Can report bugs to Jira. | After implementation to validate functionality |

---

## NEW: Automated Orchestration Mode

The **@project-manager** agent now supports **proactive orchestration mode**, which automates the entire development lifecycle from requirements to delivery.

### How It Works

When you give @project-manager a feature request, it will:

1. ✅ **Analyze requirements** and create REQUIREMENTS.md
2. ✅ **Coordinate engineers** (@go-engineer or @fe-engineer)
3. ✅ **Manage code review** with @code-reviewer
4. ✅ **Coordinate testing** with @qa-engineer
5. ✅ **Iterate until complete** (review → fix → test → repeat)
6. ✅ **Demonstrate the solution** for your review

### When to Use Orchestration Mode

**Use orchestration when you want**:
- Automated end-to-end delivery with minimal manual coordination
- A complete feature built following best practices
- Systematic review and testing integrated into the workflow
- Documentation (REQUIREMENTS.md) created automatically

**Use manual mode when you want**:
- More control over each step
- To implement it yourself with guidance
- To learn the process by doing it manually
- Quick clarifications without full implementation

### Example

```bash
# Orchestration Mode (Automated)
You: @project-manager Build a user registration API with email validation

@project-manager: [Orchestrates entire workflow automatically]
                  Creates REQUIREMENTS.md → Invokes engineer → Reviews code → Tests → Delivers

# Manual Mode (Step-by-step)
You: @project-manager What should a user registration API include?
@project-manager: [Provides guidance]
You: @go-engineer Implement based on guidance above
You: @code-reviewer Review the implementation
You: @qa-engineer Test the implementation
```

---

## Available Skills

Skills are reusable methodologies that agents use. You can reference them when working with agents:

### Development Skills
- **software-development-lifecycle** - 7-step workflow for features and bug fixes
- **technical-problem-solving** - Debugging and troubleshooting strategies

### Code Quality Skills
- **code-review-principles** - Clean code principles (SRP, DRY, KISS, etc.)
- **security-review** - Security vulnerability identification
- **testing-strategy** - Testing approaches and best practices

### QA Skills
- **qa-test-execution** - Systematic testing workflow
- **defect-management** - Bug reporting and tracking
- **k6-performance-testing** - Performance testing with k6
- **testing-strategy** - Comprehensive testing methodology

### Accessibility Skills
- **evinced-sdk-setup** - Setup and manage Evinced MCP server for accessibility testing
- **accessibility-expertise** - Web accessibility best practices and WCAG compliance

### Project Management Skills
- **requirements-clarification** - Requirements analysis and acceptance criteria
- **documentation-research** - Systematic documentation research
- **jira-integration** - Creating, managing, and tracking Jira issues and work items
- **cherry-pick** - Create a PR by cherry-picking specific commits onto a target branch

### Data Transformation Skills
- **promotions-spreadsheet-to-product-badge-json** - Translate Browse Path promotions spreadsheets into product-badge JSON grouped by market and SKU

Policy for promotions transformations:
- Treat spreadsheet text as source-of-truth.
- Do not silently correct typos, wording, brand names, punctuation, or legal copy.
- Only whitespace normalization is allowed automatically.
- Ask the user for approval before applying any content correction, and report approved changes explicitly.

---

## Common Workflows

### 1. Backend Feature Development

**Option A: Fully Orchestrated (Automated)**

```
You: @project-manager I need to implement user authentication API

@project-manager: [Analyzes requirements, creates REQUIREMENTS.md]
                  ↓
              [Shows requirements for approval]
                  ↓
              [Invokes @go-engineer to implement]
                  ↓
              [Monitors progress, answers questions]
                  ↓
              [Coordinates @code-reviewer for review]
                  ↓
              [Addresses review feedback with @go-engineer if needed]
                  ↓
              [Coordinates @qa-engineer for testing]
                  ↓
              [Addresses QA feedback with @go-engineer if needed]
                  ↓
              [Iterates review-test cycle until complete]
                  ↓
              [Demonstrates working solution]

You: [Review and commit the code]
```

**Option B: Manual Step-by-Step**

```
You: @product-owner I need to implement user authentication API

@product-owner: [Clarifies requirements, defines acceptance criteria]

You: @go-engineer Please implement based on the requirements above

@go-engineer: [Implements the authentication microservice]

You: @code-reviewer Review the authentication implementation

@code-reviewer: [Reviews code quality and security]

You: @qa-engineer Test the authentication API

@qa-engineer: [Tests and reports any issues]
```

### 2. Frontend Feature Development

**Option A: Fully Orchestrated (Automated)**

```
You: @project-manager I need a user dashboard with analytics

@project-manager: [Complete orchestrated workflow from requirements to delivery]

You: [Review and commit the code]
```

**Option B: Manual Step-by-Step**

```
You: @product-owner I need a user dashboard with analytics

@product-owner: [Clarifies requirements, defines acceptance criteria]

You: @fe-engineer Build the dashboard based on the requirements

@fe-engineer: [Implements the React components and integration]

You: @code-reviewer Review the dashboard implementation

@code-reviewer: [Reviews code quality, accessibility, and performance]

You: @qa-engineer Test the dashboard functionality

@qa-engineer: [Tests and reports any issues]
```

### 3. Bug Fix (Backend)

```
You: @go-engineer Fix: API endpoint returning 500 errors

@go-engineer: [Diagnoses and implements fix]

You: @qa-engineer Verify the API fix

@qa-engineer: [Tests and confirms fix works]
```

### 4. Bug Fix (Frontend)

```
You: @fe-engineer Fix: Form validation not working on mobile

@fe-engineer: [Diagnoses and implements fix]

You: @qa-engineer Verify the form validation fix

@qa-engineer: [Tests and confirms fix works]
```

### 5. Documentation Research

```
You: @doc-researcher How do I implement OAuth 2.0 authentication in Go?

@doc-researcher: [Researches and provides detailed guidance with citations]

You: @go-engineer Implement OAuth based on the research above

@go-engineer: [Implements using the documentation guidance]
```

### 6. Requirements Clarification

```
You: @product-owner Should the search feature support fuzzy matching?

@product-owner: [Clarifies requirements or escalates to stakeholder if needed]

You: @go-engineer Implement search with the clarified requirements

@go-engineer: [Implements accordingly]
```

### 7. Cherry-Pick Pull Request

```
You: @go-engineer Use the cherry-pick skill to backport commit abc1234 to release/1.2

@go-engineer: [Creates a branch from release/1.2]
              [Cherry-picks abc1234 with traceability]
              [Runs validation in the target branch context]
              [Pushes the branch and prepares the PR summary]
```

---

## Tips for Working with Agents

1. **Be Specific**: Provide context about what you're working on
   - ❌ "Review my code"
   - ✅ "@code-reviewer Review the authentication middleware in src/auth.js"

2. **Chain Agents**: Use multiple agents in sequence for complex workflows
   - First clarify requirements with @product-owner
   - Then implement with @go-engineer (backend) or @fe-engineer (frontend)
   - Finally test with @qa-engineer

3. **Reference Skills**: Agents automatically use skills, but you can reference them explicitly
   - "@go-engineer Follow the software-development-lifecycle skill to implement this feature"

4. **Provide Files**: Include relevant files or code snippets for context
   - Attach files to your chat message
   - Reference specific file paths

5. **Ask Follow-ups**: Agents maintain context in the conversation
   - You can ask clarifying questions
   - Request revisions or additional details

---

## Agent Capabilities

### Tools Available to Agents

All agents have access to:
- **read_file** - Read workspace files
- **grep_search** - Search for text in files
- **semantic_search** - Semantic code search
- **run_in_terminal** - Execute commands

Additionally, **@doc-researcher** has:
- **fetch_webpage** - Fetch online documentation

---

## Configuration Location

- Agents: `agents/`
- Skills: `skills/`

To modify an agent or skill, edit the corresponding markdown file in these directories.

---

## Next Steps

1. **Try an agent**: Open Copilot chat (Cmd/Ctrl + I) and type `@code-reviewer review my code`
2. **Explore skills**: Read through `skills/` to understand available methodologies
3. **Customize**: Edit agent files to adjust their behavior for your specific needs
4. **Create workflows**: Develop your own multi-agent workflows for common tasks

For questions or issues, refer to the individual agent files in `agents/` for detailed documentation.
