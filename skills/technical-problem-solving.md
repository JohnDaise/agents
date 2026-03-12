---
name: technical-problem-solving
description: Problem-solving strategies and approaches for technical challenges during software development. USE FOR debugging difficult issues; overcoming implementation blockers; evaluating technical tradeoffs; making architectural decisions. DO NOT USE FOR requirements clarification (use requirements-clarification) or code review (use code-review-principles).
---

# Technical Problem-Solving

Systematic approaches for solving technical challenges encountered during development.

## General Problem-Solving Framework

### 1. Define the Problem
- What exactly is the problem? (Be specific)
- What is the expected behavior vs. actual behavior?
- When does it occur? (Always, sometimes, specific conditions?)
- What is the impact? (Severity, affected users/systems)
- What constraints exist? (Time, resources, technology)

### 2. Gather Information
- What error messages appear?
- What are the relevant logs?
- What is the system state when problem occurs?
- What changed recently?
- Can the problem be reproduced?

### 3. Form Hypotheses
- What are possible root causes?
- Which is most likely based on symptoms?
- What evidence supports/contradicts each hypothesis?

### 4. Test Hypotheses
- Design experiments to test each hypothesis
- Test one variable at a time
- Document results of each test
- Eliminate possibilities systematically

### 5. Implement Solution
- Choose the best solution approach
- Implement carefully and incrementally
- Verify the fix resolves the problem
- Ensure no new problems introduced

### 6. Reflect & Document
- What was the root cause?
- Why did the solution work?
- How can we prevent this in the future?
- Document for future reference

---

## Debugging Strategies

### When You Have a Bug

**Step 1: Reproduce Consistently**
- Create minimal reproduction case
- Identify precise conditions that trigger bug
- Document steps to reproduce
- Note if issue is intermittent or consistent

**Step 2: Isolate the Problem**
- Binary search: Comment out code sections to narrow down
- Add logging/debugging statements
- Use debugger to step through execution
- Check assumptions with print/log statements
- Verify inputs are what you expect

**Step 3: Form Hypothesis About Root Cause**
- What could cause these symptoms?
- Where in the code would that happen?
- What changed recently that might affect this?

**Step 4: Test Your Hypothesis**
- Set breakpoints and inspect variables
- Add temporary logging
- Make small targeted changes
- Verify hypothesis with evidence

**Step 5: Fix and Verify**
- Implement the fix
- Verify the bug no longer occurs
- Add regression test to prevent recurrence
- Check for similar bugs elsewhere

### Debugging Techniques

**Rubber Duck Debugging**
- Explain the problem out loud (to a duck, colleague, or yourself)
- Often clarifies thinking and reveals solution

**Binary Search Debugging**
- Comment out half the code
- Does problem still occur?
- Narrow down location systematically

**Logging & Print Debugging**
- Add strategic log statements
- Track variable values through execution
- Verify assumptions about program flow

**Debugger Usage**
- Set breakpoints at key locations
- Step through code line by line
- Inspect variable values at each step
- Evaluate expressions in debug console

**Diff Analysis**
- What changed since it last worked?
- Review git diff or version history
- Revert changes one at a time to isolate

**Compare Working vs Broken**
- Find a working version or similar code
- Identify differences
- Understand why working version works

---

## When You're Stuck on Implementation

### Strategy 1: Break It Down
- Divide problem into smaller, manageable pieces
- Solve simplest piece first
- Build up to full solution incrementally
- Each piece should be testable independently

### Strategy 2: Simplify
- Remove complexity temporarily
- Get basic version working first
- Add features/complexity incrementally
- Sometimes simpler approach is better anyway

### Strategy 3: Research
- Search for similar problems/solutions
- Read documentation thoroughly
- Look for examples and code samples
- Check Stack Overflow, GitHub issues, blogs
- Review existing codebase for similar patterns

### Strategy 4: Change Perspective
- Try a different approach entirely
- Ask: "What if I did the opposite?"
- Look at problem from user's perspective
- Take a break and come back fresh

### Strategy 5: Seek Help
**Time-box your struggle**: If stuck for 30+ minutes without progress:
- Explain problem to someone (rubber duck or colleague)
- Ask specific questions about what you've tried
- Show your code and attempts
- Be clear about what you don't understand

---

## Evaluating Technical Approaches

When multiple solutions are possible, evaluate systematically:

### Evaluation Criteria Matrix

| Criterion | Weight | Option A | Option B | Option C |
|-----------|--------|----------|----------|----------|
| Simplicity | High | | | |
| Performance | Medium | | | |
| Maintainability | High | | | |
| Time to Implement | Medium | | | |
| Testability | High | | | |
| Scalability | Low | | | |
| **Total Score** | | | | |

### Common Tradeoffs

**Simplicity vs. Flexibility**
- Simple: Faster to implement, easier to understand
- Flexible: Handles more cases, more future-proof
- Consider: Current needs vs. anticipated future needs

**Performance vs. Readability**
- Performant: Optimized, may be complex
- Readable: Clear, maintainable, may be slower
- Consider: Is performance actually a bottleneck?

**Build vs. Use Library**
- Build: Full control, no dependencies, more time
- Library: Faster, maintained by others, less control
- Consider: Is this core business logic or commodity functionality?

**Comprehensive vs. Minimal**
- Comprehensive: Handles all cases, more complex
- Minimal: Solves immediate need, simpler
- Consider: YAGNI (You Aren't Gonna Need It)

### Decision Framework

1. **Identify constraints** (time, performance, maintainability)
2. **List viable options** (3-5 approaches)
3. **Score each option** against criteria
4. **Consider second-order effects** (what else is affected?)
5. **Make recommendation** with rationale
6. **Document the decision** and tradeoffs

---

## Overcoming Technical Constraints

### When Technology Doesn't Support What You Need

**Option 1: Workaround**
- Find alternative way to achieve goal
- Use available features creatively
- May be less elegant but gets job done

**Option 2: Extend/Customize**
- Build on top of existing technology
- Create wrapper or adapter
- Maintain compatibility with updates

**Option 3: Different Technology**
- Evaluate alternatives that support requirement
- Consider migration costs and timeline
- Get stakeholder approval for change

**Option 4: Negotiate Requirements**
- Explain technical limitation to PM
- Propose alternative that's technically feasible
- Find compromise that satisfies business need

### When Performance is Insufficient

**Step 1: Measure**
- Profile to find actual bottlenecks
- Don't optimize without data
- Measure before and after changes

**Step 2: Identify Bottleneck**
- CPU-bound, I/O-bound, network-bound?
- Which operation is slowest?
- Is it algorithm or implementation?

**Step 3: Optimize**
- Algorithm: Better complexity (O(n²) → O(n log n))
- Caching: Store expensive computations
- Database: Add indexes, optimize queries
- Async: Make blocking operations non-blocking
- Batching: Reduce number of operations

**Step 4: Verify**
- Measure improvement
- Ensure correctness maintained
- Check if "good enough" now

### When Dependencies Conflict

**Identify the conflict:**
- Which dependencies conflict?
- What versions are involved?
- What features are needed from each?

**Resolution options:**
1. Update one or both dependencies to compatible versions
2. Find alternative dependency
3. Vendor (copy) one dependency
4. Isolate in separate module/service
5. Negotiate with PM to drop conflicting feature

---

## Making Architectural Decisions

### When to Architect vs. When to Code

**Architect first when:**
- Large feature affecting multiple components
- Public API that will be hard to change
- Performance/scaling is critical
- Multiple developers will work on it
- Decision has long-term implications

**Code first when:**
- Small, isolated change
- Requirements likely to change
- Learning/exploring solution space
- Prototyping to validate approach
- Easy to refactor later

### Architectural Decision Record (ADR)

Document significant decisions:

```markdown
## Decision: [Title]

**Date**: YYYY-MM-DD
**Status**: Proposed | Accepted | Rejected | Superseded

### Context
[Describe the problem and constraints]

### Options Considered
1. Option A: [description]
   - Pros: ...
   - Cons: ...
   
2. Option B: [description]
   - Pros: ...
   - Cons: ...

### Decision
[Which option chosen and why]

### Consequences
- Positive: ...
- Negative: ...
- Risks: ...
```

---

## Common Problem Patterns & Solutions

### Pattern: "Works on My Machine"
**Causes**: Environment differences, missing dependencies, configuration
**Solutions**: 
- Use containers (Docker)
- Document environment requirements
- Use environment variable checks
- Automate setup scripts

### Pattern: Intermittent Failures
**Causes**: Race conditions, timing issues, external dependencies
**Solutions**:
- Add logging to capture state
- Increase frequency to reproduce
- Check for async/threading issues
- Verify external service reliability

### Pattern: "It Worked Before"
**Causes**: Recent changes, updated dependencies, data changes
**Solutions**:
- Check version control history
- Review recent changes (git log, git diff)
- Use git bisect to find breaking commit
- Check dependency updates

### Pattern: Performance Degradation
**Causes**: Data growth, inefficient queries, resource leaks
**Solutions**:
- Profile to find bottleneck
- Check database query performance
- Look for memory leaks
- Review recent data size changes

### Pattern: Undefined Behavior
**Causes**: Missing validation, unconsidered edge cases
**Solutions**:
- Add input validation
- Handle null/undefined explicitly
- Add defensive checks
- Test edge cases thoroughly

---

## Problem-Solving Mindset

### Effective Attitudes

✅ **Curious**: "I wonder why this happens?"
✅ **Systematic**: "Let me test one thing at a time"
✅ **Patient**: "Debugging takes time, that's normal"
✅ **Humble**: "I might be wrong about my assumption"
✅ **Persistent**: "I'll try another approach"

### Ineffective Attitudes

❌ **Random**: Changing things without hypothesis
❌ **Rushed**: Not thinking through the problem
❌ **Stubborn**: Refusing to try different approach
❌ **Assuming**: Not verifying assumptions with evidence
❌ **Giving up**: Stopping before truly exploring options

### When to Ask for Help

**Ask soon when:**
- Security or data integrity issues
- Stuck for 30+ minutes without progress
- Need domain knowledge you don't have
- Multiple approaches failed without understanding why
- Time-sensitive issue affecting users

**Before asking, prepare:**
- What have you already tried?
- What is your current understanding?
- What specific question do you have?
- Can you show a minimal reproduction?

---

## Cognitive Strategies

### Take Breaks
- Stuck for 45+ minutes? Take a 10-minute break
- Fresh perspective often reveals solution
- Walk, stretch, different activity

### Write It Down
- Explain problem in writing
- Create diagram of data flow
- List what you know vs. don't know
- Writing clarifies thinking

### Explain to Someone
- Rubber duck debugging
- Often realize solution while explaining
- Fresh perspective from listener

### Sleep On It
- Subconscious processes problems
- Next morning often brings clarity
- Don't force solution when exhausted

### Reduce Complexity
- Too many variables? Remove some
- Simplify until it works, then add back
- Isolate the failing component
