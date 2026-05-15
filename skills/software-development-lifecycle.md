---
name: software-development-lifecycle
description: Systematic workflow for implementing software features from requirements to deployment. USE FOR feature implementation; bug fixes; following structured development process; planning implementation steps. DO NOT USE FOR code review (use code-review-principles) or requirements gathering (use requirements-clarification).
---

# Software Development Lifecycle (SDLC)

A systematic, step-by-step approach to implementing software features and fixes.

**KEY PRINCIPLE**: Test-Driven Development (TDD) is MANDATORY in Step 4, except for documented exemptions (throwaway spikes, config-only changes, or codebases with no test infrastructure). When skipping TDD, document the reason.

## The 7-Step Development Workflow

**IMPORTANT**: Step 4 uses Test-Driven Development (TDD) - write tests BEFORE implementation

```
1. Understand Requirements    → Know what to build
2. Clarify Ambiguities        → Remove uncertainties  
3. Plan Implementation        → Design the approach
4. Implement with TDD         → 🔴 RED → 🟢 GREEN → 🔵 REFACTOR (repeat)
5. Validate & Verify          → Ensure all tests pass
6. Review (Optional)          → Get feedback
7. Document & Deploy          → Prepare for production
```

---

### Step 1: Understand Requirements

**Objective**: Fully comprehend what needs to be built or fixed

**Activities**:
1. Read requirements, user stories, or bug reports thoroughly
2. Identify the core objective and desired outcome
3. Understand the context and affected users
4. Note constraints (time, technical, business)
5. Identify dependencies on other systems/components
6. List any unclear or ambiguous points

**Questions to Answer**:
- What problem are we solving?
- Who is the user/audience?
- What's the expected behavior?
- What are the success criteria?
- What are the constraints?

**Output**: Clear understanding of the goal, or list of clarification questions

---

### Step 2: Clarify Ambiguities

**Objective**: Resolve all uncertainties before implementation

**Activities**:
1. Review list of unclear points from Step 1
2. Formulate specific questions
3. Ask stakeholders/PM for clarification
4. Document answers and decisions
5. Document any assumptions if full clarity isn't possible
6. Confirm understanding before proceeding

**When to Seek Clarification**:
- Undefined edge cases or error scenarios
- Ambiguous acceptance criteria
- Missing requirements for specific scenarios
- Conflicting requirements
- Technical constraints affecting feasibility

**Output**: Complete, unambiguous requirements or documented assumptions

---

### Step 3: Plan Implementation

**Objective**: Design the simplest solution that meets requirements

**Activities**:
1. **Start with the simplest approach** - resist over-engineering
2. **Break down the work** into logical, manageable steps
3. **Identify affected components**: Files, classes, functions, databases
4. **Choose the straightforward path**: Avoid complex patterns unless necessary
5. **Plan error handling**: Edge cases, validation, error messages
6. **Design data flow**: Inputs, transformations, outputs
7. **Determine testing approach**: Unit, integration, manual tests
8. **Estimate complexity**: Consider if task should be broken into subtasks

**Questions to Answer**:
- What's the SIMPLEST way to solve this?
- What files/modules need to change?
- What new components need to be created?
- Do I really need this abstraction/pattern, or can I keep it simple?
- What are the error/edge cases?
- How will this be tested?
- Are there performance implications?
- Are there security considerations?

**Anti-Patterns to Avoid**:
- ❌ Adding abstractions before you have 3+ similar cases
- ❌ Using frameworks/libraries when simple code works
- ❌ Building for scale you don't have yet
- ❌ Adding features "just in case" (YAGNI)
- ❌ Complex design patterns when straightforward code suffices

**Output**: Simple, clear implementation plan that solves the actual problem

---

### Step 4: Implement Solution (Using TDD)

**Objective**: Write clean, correct, maintainable code using Test-Driven Development

**CRITICAL: Follow the Red-Green-Refactor Cycle**

#### TDD Workflow (MANDATORY)

**For each small piece of functionality:**

1. **🔴 RED: Write a Failing Test**
   - Write test that defines expected behavior
   - Run test → verify it FAILS (if it passes, test is wrong)
   - Failure message should be clear and helpful
   - **NEVER skip this step**

2. **🟢 GREEN: Make Test Pass**
   - Write the MINIMUM code to make test pass
   - Don't add extra features or perfect it yet
   - Run test → verify it PASSES
   - All other tests should still pass (no regressions)

3. **🔵 REFACTOR: Improve Code**
   - Clean up code while keeping tests green
   - Remove duplication
   - Improve naming and structure
   - Run tests after each refactor → verify still PASSING

4. **Repeat** for next small piece of functionality

#### Implementation Activities:
1. **Write test first** - RED phase (failing test)
2. **Implement to pass test** - GREEN phase (minimum code)
3. **Refactor if needed** - REFACTOR phase (clean up)
4. **Start with the simplest approach** - resist over-engineering
5. **Follow clean code principles**: SRP, DRY, KISS, meaningful names
6. **Handle errors gracefully**: Validate inputs, handle exceptions
7. **Add logging**: For debugging and monitoring
8. **Write self-documenting code**: Clear structure and naming
9. **Add comments for complexity**: Explain WHY, not WHAT

**Best Practices**:
- Make small, focused commits
- Keep functions small (< 20 lines ideally)
- Avoid premature optimization
- Handle edge cases explicitly
- Use appropriate data structures
- Follow existing code patterns/style
- **Start simple, refactor later if needed**
- **Don't abstract until you have 3+ similar cases**

**TDD Red Flags - Stop Immediately If This Happens**:
🚨 **Writing implementation before writing test** - STOP, write test first
🚨 **Test passing on first run** - Test is wrong or not testing new code
🚨 **Continuing with failing tests** - STOP, fix tests before proceeding
🚨 **Skipping the RED phase** - Must see test fail first
🚨 **Not running tests frequently** - Run after every small change
🚨 **Committing failing tests** - All tests must be green before commit

**Simplicity Red Flags**:
⛔ Adding frameworks/libraries before trying simple solutions
⛔ Creating abstractions before patterns emerge
⛔ Building features "just in case" (YAGNI violation)
⛔ Deep inheritance hierarchies or complex class structures
⛔ Over-modularizing - don't make everything a separate module/package
⛔ Premature optimization without profiling
⛔ Adding middleware, interceptors, decorators upfront
⛔ Complex design patterns when straightforward code works

**Output**: Working, tested implementation with all tests passing (GREEN)

---

### Step 5: Validate & Verify

**Objective**: Ensure all tests pass and solution meets requirements

**If you followed TDD properly, most testing is already done!**

**Activities**:
1. **Run full test suite**: All tests should be GREEN ✅
2. **Verify test coverage**: Ensure critical paths are tested
3. **Test edge cases**: If any edge cases were missed, add tests now (TDD cycle)
4. **Check acceptance criteria**: Verify all requirements met
5. **Perform manual testing**: Quick smoke test as a user
6. **Check for regressions**: Ensure existing functionality still works
7. **Review test quality**: Are tests clear, focused, and valuable?

**If Tests Are Failing**:
- ❌ **DO NOT PROCEED** - Failing tests are blockers
- 🔍 **Debug immediately** - Find root cause
- 🔧 **Fix the issue** - Either test or implementation is wrong
- ✅ **Get to green** - All tests must pass before moving forward
- 📢 **Escalate if stuck** - Ask project-manager if blocked

**Testing Levels**:
- **Unit Tests**: Individual functions/methods in isolation
- **Integration Tests**: Multiple components working together
- **End-to-End Tests**: Full user workflows
- **Manual Testing**: Exploratory testing, visual verification

**Output**: Verified, tested solution with passing test suite

---

### Step 6: Review (Optional)

**Objective**: Get feedback on code quality and approach

**Activities**:
1. **Request code review**: From peer, team lead, or code-reviewer agent
2. **Provide context**: Explain the problem and your approach
3. **Respond to feedback**: Address concerns and suggestions
4. **Refactor if needed**: Improve based on review comments
5. **Re-test after changes**: Ensure refactoring didn't break anything

**When to Request Review**:
- Critical or security-sensitive code
- Complex algorithms or architecture
- Uncertain about approach or quality
- Team policy requires review
- Learning opportunity

**Output**: Reviewed, refined code meeting quality standards

---

### Step 7: Document & Deploy

**Objective**: Make the solution understandable and ready for production

**Activities**:
1. **Update documentation**: README, API docs, configuration guides
2. **Add inline documentation**: For complex logic or public APIs
3. **Update CHANGELOG**: Note what changed and why
4. **Prepare deployment notes**: Configuration changes, migrations, rollback plan
5. **Update tests documentation**: If test setup changed
6. **Commit with clear message**: Describe what and why

**Documentation Types**:
- **Code comments**: For complex algorithms or business logic
- **README updates**: For new features or setup changes
- **API documentation**: For public interfaces
- **Architecture diagrams**: For significant structural changes
- **Deployment notes**: For operations team

**Output**: Documented, deployment-ready solution

---

## Workflow Variations

### For Bug Fixes

1. **Understand**: Read bug report, understand expected vs actual behavior
2. **Reproduce**: Verify the bug occurs as reported
3. **Diagnose**: Debug to find root cause
4. **Plan**: Determine fix approach, consider side effects
5. **Implement**: Fix the bug without introducing regressions
6. **Test**: Verify fix works, add regression test
7. **Document**: Explain the issue and resolution

### For Refactoring

1. **Understand**: Identify what needs improvement and why
2. **Clarify**: Confirm scope - what should/shouldn't change
3. **Plan**: Design the improved structure
4. **Implement**: Refactor incrementally with tests
5. **Test**: Ensure behavior unchanged (green tests throughout)
6. **Review**: Get feedback on improvements
7. **Document**: Update affected documentation

### For Prototypes/Spikes

1. **Understand**: Define what needs to be learned/proven
2. **Clarify**: Set time limit and success criteria
3. **Plan**: Outline exploration approach
4. **Implement**: Build quick proof of concept
5. **Test**: Validate the concept works
6. **Review**: Present findings and recommendations
7. **Document**: Document learnings and decision

---

## Best Practices Throughout SDLC

### Communication
- Ask questions early, not late
- Provide status updates on complex work
- Document decisions and their rationale
- Seek help when stuck (time-boxed problem solving)

### Quality
- Test throughout, not just at the end
- Make small, incremental changes
- Review your own code before requesting review
- Follow existing patterns and conventions

### Efficiency & Simplicity
- **Start simple, add complexity only when needed**
- **YAGNI**: Don't build features you don't need today
- **Wait for patterns**: Abstract after 3+ similar cases, not before
- **Question complexity**: If it feels complex, find a simpler way
- Reuse existing solutions when appropriate
- Automate repetitive tasks
- Balance perfection with pragmatism
- **Prefer boring, proven solutions over novel approaches**

### Risk Management
- Identify risks early in planning
- Have rollback strategy for changes
- Consider security implications
- Think about edge cases upfront

---

## Common Pitfalls to Avoid

❌ **Jumping to implementation** without understanding requirements
❌ **Skipping planning** and coding without a clear approach
❌ **Writing tests after** instead of during/before implementation
❌ **Making changes without tests** - how do you know it works?
❌ **Ignoring edge cases** until they cause bugs
❌ **Committing untested code** - always verify before commit
❌ **Unclear commit messages** - future you (and team) will thank you
❌ **Forgetting documentation** - code is read more than written
❌ **Over-engineering solutions** - starting complex instead of simple
❌ **Premature abstraction** - abstracting before patterns emerge
❌ **Adding "just in case" features** - YAGNI violations
❌ **Optimizing before profiling** - guessing at performance problems

---

## Metrics for Success

A successful implementation:
- ✅ Meets all stated requirements and acceptance criteria
- ✅ Has comprehensive test coverage (unit, integration, e2e)
- ✅ Follows code quality standards and best practices
- ✅ Includes proper error handling and validation
- ✅ Is documented (code comments, README, API docs)
- ✅ Has been reviewed (self-review minimum, peer review ideal)
- ✅ Doesn't introduce regressions or break existing tests
- ✅ Is ready for deployment with deployment notes

---

## Definition of Done

### ✅ Success Conditions (ALL must be true to consider work complete)
1. All acceptance criteria from requirements are met
2. All tests pass (unit, integration, e2e) — zero failing tests
3. No regressions in existing test suite
4. Code has been self-reviewed for clean code principles
5. Edge cases and error paths are handled and tested
6. Documentation updated where applicable

### 🛑 Failure Conditions (STOP and escalate if any occur)
These are signals to stop iterating and report to the orchestrator or user rather than continuing:

| Condition | Action |
|-----------|--------|
| Tests failing after 3 fix attempts | Stop. Report root cause and what was tried. Ask for guidance. |
| Acceptance criteria conflict or are impossible to satisfy simultaneously | Stop. Surface the conflict. Do not pick a winner silently. |
| Required dependency is missing or unavailable (library, API, service) | Stop. List what is missing and what it blocks. |
| Scope of work expanded significantly during implementation | Stop. Report new scope. Do not implement undiscussed requirements. |
| Architectural decision required that is outside implementation scope | Stop. Describe the decision needed. Do not make it unilaterally. |
| Blocked for > 30 minutes with no path forward | Stop. Summarize what was tried and where you are blocked. |

### ⚠️ Escalation Format
When a failure condition is hit, report:
```
🛑 BLOCKED: <one-line description of the blocker>

What I attempted:
- <attempt 1>
- <attempt 2>

Current state:
- <what is working>
- <what is not working>

Decision needed:
- <specific question or decision required to continue>
```

---

## Time Management

### Time Boxing
- Set time limits for exploration/debugging
- If stuck for > 30 minutes, seek help or try different approach
- Don't let perfect be the enemy of good

### Prioritization
- Implement core functionality first
- Add error handling before edge cases
- Polish after correctness
- Remember: Make it work, make it right, make it fast (in that order)

### Breaking Down Work
If a task feels too large:
1. Break into smaller, independent pieces
2. Implement minimum viable version first
3. Add enhancements incrementally
4. Consider if task needs to be split into multiple deliverables
