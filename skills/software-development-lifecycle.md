---
name: software-development-lifecycle
description: Systematic workflow for implementing software features from requirements to deployment. USE FOR feature implementation; bug fixes; following structured development process; planning implementation steps. DO NOT USE FOR code review (use code-review-principles) or requirements gathering (use requirements-clarification).
---

# Software Development Lifecycle (SDLC)

A systematic, step-by-step approach to implementing software features and fixes.

## The 7-Step Development Workflow

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

**Objective**: Design the solution before writing code

**Activities**:
1. **Break down the work** into logical, manageable steps
2. **Identify affected components**: Files, classes, functions, databases
3. **Choose architecture/patterns**: Design patterns, data structures
4. **Plan error handling**: Edge cases, validation, error messages
5. **Design data flow**: Inputs, transformations, outputs
6. **Determine testing approach**: Unit, integration, manual tests
7. **Estimate complexity**: Consider if task should be broken into subtasks

**Questions to Answer**:
- What files/modules need to change?
- What new components need to be created?
- What design patterns apply?
- What are the error/edge cases?
- How will this be tested?
- Are there performance implications?
- Are there security considerations?

**Output**: Implementation plan with ordered steps and affected components

---

### Step 4: Implement Solution

**Objective**: Write clean, correct, maintainable code

**Activities**:
1. **Implement incrementally**: One logical piece at a time
2. **Follow clean code principles**: SRP, DRY, KISS, meaningful names
3. **Handle errors gracefully**: Validate inputs, handle exceptions
4. **Add logging**: For debugging and monitoring
5. **Write self-documenting code**: Clear structure and naming
6. **Add comments for complexity**: Explain WHY, not WHAT
7. **Test as you go**: Verify each piece works before moving on

**Best Practices**:
- Make small, focused commits
- Keep functions small (< 20 lines ideally)
- Avoid premature optimization
- Handle edge cases explicitly
- Use appropriate data structures
- Follow existing code patterns/style

**Output**: Working implementation that solves the problem

---

### Step 5: Test & Validate

**Objective**: Ensure the solution works correctly and meets requirements

**Activities**:
1. **Write automated tests**: Unit tests for logic, integration tests for interactions
2. **Test happy path**: Verify normal/expected usage works
3. **Test edge cases**: Boundary conditions, empty/null values, large datasets
4. **Test error scenarios**: Invalid inputs, system failures, timeouts
5. **Verify against requirements**: Check all acceptance criteria
6. **Perform manual testing**: Use the feature as a user would
7. **Check for regressions**: Ensure existing functionality still works

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

### Efficiency
- Don't over-engineer
- Reuse existing solutions when appropriate
- Automate repetitive tasks
- Balance perfection with pragmatism

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
