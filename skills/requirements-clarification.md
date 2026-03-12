---
name: requirements-clarification
description: Methodology for clarifying technical requirements, acceptance criteria, and project scope. USE FOR gathering requirements; clarifying ambiguous specifications; defining acceptance criteria; bridging technical and business perspectives. DO NOT USE FOR technical implementation (use software-development-lifecycle) or documentation research (use documentation-research).
---

# Requirements Clarification Methodology

Use this systematic approach to clarify requirements and eliminate ambiguity.

## Core Objectives

1. **Eliminate Ambiguity** - Ensure all parties have shared understanding
2. **Define Success** - Establish clear acceptance criteria
3. **Identify Constraints** - Surface limitations early
4. **Manage Scope** - Distinguish must-haves from nice-to-haves

## Requirements Analysis Process

### Step 1: Understand the Request
- What is the business goal or user need?
- What problem are we solving?
- Who are the stakeholders and users?
- What does success look like?

### Step 2: Identify Ambiguities
Look for unclear or undefined aspects:
- Undefined behaviors or edge cases
- Missing acceptance criteria
- Vague terms ("user-friendly", "fast", "secure")
- Implicit assumptions not stated
- Conflicting requirements

### Step 3: Ask Clarifying Questions
Use the 5 W's framework:
- **What**: What exactly should happen? What are the inputs/outputs?
- **Who**: Who will use this? Who is affected?
- **When**: When should this occur? What triggers it?
- **Where**: Where in the system? What context?
- **Why**: Why is this needed? What's the business value?

Additional questions:
- **How much**: What are the scale/volume requirements?
- **What if**: What happens in edge cases or error scenarios?

### Step 4: Define Acceptance Criteria
Document clear, testable criteria:
- Use "Given-When-Then" format when appropriate
- Make criteria specific and measurable
- Include both positive and negative cases
- Cover edge cases and error conditions
- Define what's explicitly out of scope

### Step 5: Confirm Understanding
- Summarize requirements back to stakeholder
- Verify acceptance criteria are sufficient
- Document any assumptions made
- Get explicit approval to proceed

## Question Patterns

### For Functional Requirements
```
- "What should happen when [scenario]?"
- "Should the system support [behavior]?"
- "What's the expected output when [input]?"
- "How should [edge case] be handled?"
- "Are there any exceptions to [rule]?"
```

### For Non-Functional Requirements
```
- "What's the expected response time/throughput?"
- "How many concurrent users should be supported?"
- "What are the availability/uptime requirements?"
- "What security/compliance standards apply?"
- "What's the acceptable error rate?"
```

### For Scope Definition
```
- "Is [feature] in scope for this iteration?"
- "Should we handle [scenario] now or later?"
- "What's the minimum viable version?"
- "Can [complex feature] be simplified or phased?"
- "What's the priority of [requirement] vs [other requirement]?"
```

### For Technical Constraints
```
- "Are there technology/platform constraints?"
- "What's the timeline and when is delivery needed?"
- "What resources (time, budget, people) are available?"
- "Are there existing systems we must integrate with?"
- "What are the scaling/performance requirements?"
```

## Acceptance Criteria Formats

### Given-When-Then Format
```
Given [initial context/state]
When [action or event occurs]
Then [expected outcome]
```

Example:
```
Given a user is logged in
When they submit a valid comment
Then the comment should appear immediately
And the user should see a success message
And an email notification should be sent to subscribed users
```

### Checklist Format
```
Feature must:
- [ ] Do X when Y happens
- [ ] Support Z scenario
- [ ] Display error message for invalid A
- [ ] Complete within B milliseconds
- [ ] Work on C browsers/platforms
```

### Scenario-Based Format
```
Scenario 1: Happy Path
- User inputs: [details]
- Expected behavior: [details]
- Expected output: [details]

Scenario 2: Error Case
- User inputs: [invalid data]
- Expected behavior: [validation]
- Expected output: [error message]
```

## Managing Ambiguity

### When Requirements Are Unclear
1. **Ask specific questions** to narrow down the ambiguity
2. **Propose options** with pros/cons for stakeholder to choose
3. **Suggest reasonable defaults** if decision isn't critical
4. **Document assumptions** and get them validated
5. **Prototype if needed** to visualize and clarify

### When Requirements Conflict
1. **Document the conflict** clearly
2. **Explain the tradeoffs** of each interpretation
3. **Present options** to stakeholder
4. **Request prioritization** if both can't be satisfied
5. **Document the decision** and rationale

### When Information Is Missing
1. **Identify the specific gap** in requirements
2. **Explain the impact** of the missing information
3. **Suggest where to find it** (documentation, stakeholders, etc.)
4. **Propose interim solution** if answer will take time
5. **Track as open question** until resolved

## Scope Management

### Defining Scope Boundaries
- **In Scope**: Features/behaviors explicitly required
- **Out of Scope**: Features/behaviors explicitly deferred or excluded
- **Future Consideration**: Features/behaviors to consider later
- **Assumptions**: Working assumptions that may change

### Managing Scope Creep
- Document all scope changes
- Assess impact on timeline/resources
- Require explicit approval for additions
- Suggest deferring non-critical items
- Maintain "backlog" of future enhancements

## Communication Best Practices

### With Engineers
- Provide technical context and constraints
- Explain the "why" behind requirements
- Be specific about behaviors and edge cases
- Reference existing patterns when applicable
- Acknowledge technical feasibility concerns

### With Stakeholders
- Translate technical constraints to business impact
- Present options with clear tradeoffs
- Use business language, avoid jargon
- Focus on outcomes and value
- Confirm understanding before proceeding

### Documentation
- Write requirements in clear, concise language
- Use examples and scenarios liberally
- Include diagrams/mockups when helpful
- Keep requirements version-controlled
- Update as decisions are made

## Red Flags

Watch for these signs of unclear requirements:
- ❌ Vague terms: "user-friendly", "intuitive", "fast"
- ❌ Undefined edge cases: "usually", "normally", "typically"
- ❌ Missing actors: Who initiates? Who is affected?
- ❌ Missing outcomes: What happens after the action?
- ❌ Implicit assumptions: Unstated expectations
- ❌ Conflicting statements: Requirements that contradict
- ❌ No acceptance criteria: How to verify completion?

## Example: Clarifying Vague Requirement

**Original (Vague)**:
"Users should be able to export their data"

**Clarifying Questions**:
1. What format should the export be? (CSV, JSON, XML, PDF?)
2. What data should be included? (All data or user-selectable?)
3. When does export happen? (Real-time download or async delivery?)
4. Are there size limits? (What if user has 1GB of data?)
5. Does exported data include deleted items?
6. Are there privacy/security concerns with exports?
7. Should export be audited/logged?

**Clarified Requirement**:
```
Feature: Data Export

Users can export their account data in CSV format.

Acceptance Criteria:
- Export includes: profile info, posts, comments, settings
- Does NOT include: deleted items, private messages, admin notes
- Format: Standard CSV with headers
- Triggered by: User clicks "Export Data" button in settings
- For datasets > 10MB: Generate async and email download link
- For datasets < 10MB: Provide immediate download
- Export action is logged for audit purposes
- Exported file expires after 7 days
- User can request export once per 24 hours

Out of Scope:
- Other formats (JSON, XML) - future consideration
- Selective field export - future consideration
```
