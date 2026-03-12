# Copilot Agents Guide

This workspace includes custom Copilot agents to assist with software development tasks.

## How to Use Agents

### In VS Code Chat

Agents are invoked in the Copilot chat using the `@` symbol:

```
@code-reviewer - Review my recent code changes
@senior-engineer - Implement the user authentication feature
@project-manager - Clarify the requirements for the export feature
@qa-engineer - Test the login functionality
@doc-researcher - Find information about React hooks
```

### Agent Reference

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| **@code-reviewer** | Reviews code for quality, clean code principles, and security | After writing or modifying code |
| **@doc-researcher** | Researches technical documentation and provides detailed answers | When you need to understand APIs, libraries, or best practices |
| **@project-manager** | Clarifies requirements and makes technical decisions | When requirements are unclear or you need guidance on priorities |
| **@senior-engineer** | Implements features and fixes bugs | When you need code written or issues resolved |
| **@qa-engineer** | Tests features and bug fixes | After implementation to validate functionality |

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
- **testing-strategy** - Comprehensive testing methodology

### Project Management Skills
- **requirements-clarification** - Requirements analysis and acceptance criteria
- **documentation-research** - Systematic documentation research

---

## Common Workflows

### 1. Feature Development

```
You: @project-manager I need to implement user authentication

@project-manager: [Clarifies requirements, defines acceptance criteria]

You: @senior-engineer Please implement based on the requirements above

@senior-engineer: [Implements the feature]

You: @code-reviewer Review the authentication implementation

@code-reviewer: [Reviews code quality and security]

You: @qa-engineer Test the authentication feature

@qa-engineer: [Tests and reports any issues]
```

### 2. Bug Fix

```
You: @senior-engineer Fix: Users can't submit the contact form

@senior-engineer: [Diagnoses and implements fix]

You: @qa-engineer Verify the contact form fix

@qa-engineer: [Tests and confirms fix works]
```

### 3. Documentation Research

```
You: @doc-researcher How do I implement OAuth 2.0 authentication in Express.js?

@doc-researcher: [Researches and provides detailed guidance with citations]

You: @senior-engineer Implement OAuth based on the research above

@senior-engineer: [Implements using the documentation guidance]
```

### 4. Requirements Clarification

```
You: @project-manager Should the search feature support fuzzy matching?

@project-manager: [Clarifies requirements or escalates to stakeholder if needed]

You: @senior-engineer Implement search with the clarified requirements

@senior-engineer: [Implements accordingly]
```

---

## Tips for Working with Agents

1. **Be Specific**: Provide context about what you're working on
   - ❌ "Review my code"
   - ✅ "@code-reviewer Review the authentication middleware in src/auth.js"

2. **Chain Agents**: Use multiple agents in sequence for complex workflows
   - First clarify requirements with @project-manager
   - Then implement with @senior-engineer
   - Finally test with @qa-engineer

3. **Reference Skills**: Agents automatically use skills, but you can reference them explicitly
   - "@senior-engineer Follow the software-development-lifecycle skill to implement this feature"

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

- Agents: `.github/copilot/agents/`
- Skills: `.github/copilot/skills/`

To modify an agent or skill, edit the corresponding markdown file in these directories.

---

## Next Steps

1. **Try an agent**: Open Copilot chat (Cmd/Ctrl + I) and type `@code-reviewer review my code`
2. **Explore skills**: Read through `.github/copilot/skills/` to understand available methodologies
3. **Customize**: Edit agent files to adjust their behavior for your specific needs
4. **Create workflows**: Develop your own multi-agent workflows for common tasks

For questions or issues, refer to the individual agent files in `.github/copilot/agents/` for detailed documentation.
