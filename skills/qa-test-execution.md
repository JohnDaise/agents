---
name: qa-test-execution
description: Systematic QA test planning and execution methodology for validating features and bug fixes. USE FOR planning manual test cases; executing comprehensive testing; validating acceptance criteria; regression testing. DO NOT USE FOR writing automated tests (use testing-strategy skill) or code review (use code-review-principles).
---

# QA Test Execution

Systematic approach to planning and executing quality assurance testing.

## Testing Workflow

### Step 1: Understand What Changed
1. **Review the change**: What was implemented or fixed?
2. **Read requirements**: Original requirements or bug report
3. **Understand expectations**: What behavior is expected?
4. **Identify scope**: What needs testing?
5. **Note dependencies**: Prerequisites, related features, integrations

**Questions to Answer**:
- What changed in the codebase?
- Why was this change made?
- What should the user experience be?
- What could potentially break?
- What related areas might be affected?

---

### Step 2: Review Acceptance Criteria
1. **Check for criteria**: Are acceptance criteria defined?
2. **Verify clarity**: Is expected behavior clear?
3. **Seek clarification**: If unclear, ask stakeholders
4. **Define success**: How will you know it works?
5. **Note edge cases**: Special conditions or scenarios

**If Acceptance Criteria Missing**:
- Ask project manager or stakeholder to define them
- Propose criteria based on requirements
- Get explicit approval before proceeding
- Document agreed-upon criteria

**Acceptance Criteria Format**:
```
Given [initial context/state]
When [action or event]
Then [expected outcome]
And [additional outcome]
```

---

### Step 3: Plan Test Approach
1. **Determine testing levels**:
   - Automated tests (unit, integration, E2E)
   - Manual testing (exploratory, UI, workflows)
   - Performance testing
   - Security testing
   - Accessibility testing

2. **Identify test scenarios**:
   - Happy path (normal usage)
   - Alternative paths (different valid workflows)
   - Edge cases (boundaries, limits)
   - Error cases (invalid inputs, failures)
   - Regression scenarios (existing features)

3. **Prioritize testing**:
   - **Critical**: Core functionality, security, data integrity
   - **High**: Common user workflows, major features
   - **Medium**: Less common scenarios, minor features
   - **Low**: Cosmetic issues, rare edge cases

4. **Consider regression impact**:
   - What existing features might be affected?
   - What related functionality should be retested?
   - Which integration points need verification?

5. **Plan test data**:
   - What test data is needed?
   - How to set up test environment?
   - What cleanup is required?

---

### Step 4: Execute Tests

#### A. Run Automated Tests
```bash
# Run full test suite
npm test

# Check for failures
# Review test output
# Investigate any failures
```

**What to Check**:
- All tests pass
- No new failing tests
- No skipped tests that should run
- Code coverage is maintained or improved
- Test execution time is reasonable

#### B. Manual Testing

**Happy Path Testing**:
- Test the primary user workflow
- Verify core functionality works
- Check that common scenarios succeed
- Ensure user experience is smooth

**Edge Case Testing**:
- Empty/null/undefined inputs
- Very large values (strings, numbers, files)
- Very small/negative/zero values
- Boundary conditions (min/max)
- Special characters, unicode, emojis
- Whitespace (leading, trailing, only spaces)

**Error Scenario Testing**:
- Invalid input formats
- Missing required fields
- Out-of-range values
- Invalid state or timing
- Network failures
- System errors

**Exploratory Testing**:
- Use the feature naturally
- Try unexpected combinations
- Think creatively about edge cases
- Test as a user would (and as a hacker would)
- Document interesting findings

#### C. Integration Testing
- Feature works with related components
- API calls succeed
- Database operations work
- External services integrate correctly
- Cross-browser/cross-device compatibility

---

### Step 5: Document Results

#### Test Execution Record

```markdown
## Test Execution: [Feature/Bug Name]
**Date**: YYYY-MM-DD
**Tester**: [Name]
**Environment**: [Environment details]

### Tests Executed
- [x] Automated test suite - PASS
- [x] Happy path testing - PASS
- [x] Edge case testing - PASS
- [ ] Error scenario testing - FAIL (Bug found)
- [x] Regression testing - PASS

### Test Results Summary
- Total scenarios tested: 25
- Passed: 23
- Failed: 2
- Blocked: 0

### Defects Found
1. [Bug title] - Severity: High
2. [Bug title] - Severity: Medium

### Observations
- [Any concerns or notes]
- [Performance observations]
- [UX feedback]

### Recommendation
[ ] Approve for deployment
[x] Requires fixes before deployment
[ ] Needs more testing
```

---

### Step 6: Report Findings

#### If Tests Pass ✅
```
Test Results: [Feature Name]

All tests passed successfully:
✓ Automated tests: All passing
✓ Manual testing: All scenarios verified
✓ Acceptance criteria: All met
✓ Regression: No issues found

Recommendation: Approved for deployment
```

#### If Defects Found ❌
```
Test Results: [Feature Name]

Issues found during testing. See defect reports:
- Bug #1: [Critical] [Title]
- Bug #2: [Medium] [Title]

Not approved for deployment until issues are resolved.
```

#### If Requirements Unclear ⚠️
```
Testing Blocked: [Feature Name]

Cannot complete testing due to unclear requirements:
- [Specific question 1]
- [Specific question 2]

Need clarification from project-manager to proceed.
```

---

### Step 7: Verify Fixes

After developers fix issues:

1. **Re-test the specific issue**:
   - Follow exact steps from bug report
   - Verify the issue no longer occurs
   - Test variations of the scenario

2. **Test related scenarios**:
   - Verify fix didn't break related functionality
   - Test similar scenarios
   - Check edge cases around the fix

3. **Run regression tests**:
   - Re-run full automated test suite
   - Spot-check critical features
   - Verify no new issues introduced

4. **Final approval**:
   - All issues resolved: ✅ Approve
   - New issues found: ❌ Report and iterate
   - Partial fix: ⚠️ Clarify status

---

## Test Coverage Checklist

### Functional Testing
- [ ] Happy path works as expected
- [ ] All acceptance criteria are met
- [ ] Feature works in different scenarios
- [ ] Integration with other components works
- [ ] User workflows complete successfully
- [ ] Error messages are clear and helpful
- [ ] Success messages display correctly

### Edge Cases & Boundaries
- [ ] Empty/null/undefined inputs handled
- [ ] Very large inputs (strings, numbers, files)
- [ ] Very small/zero/negative values
- [ ] Boundary conditions (min/max values)
- [ ] Special characters and unicode
- [ ] Whitespace handling
- [ ] Duplicate entries
- [ ] Concurrent operations

### Error Handling
- [ ] Invalid inputs rejected with clear errors
- [ ] Missing required data handled
- [ ] System errors caught and handled gracefully
- [ ] User sees appropriate error messages
- [ ] Errors don't expose sensitive information
- [ ] Error states are recoverable
- [ ] Validation messages are helpful

### Negative Testing
- [ ] Unauthorized access prevented
- [ ] Invalid operations blocked
- [ ] Malformed data rejected
- [ ] SQL injection attempts blocked
- [ ] XSS attempts prevented
- [ ] CSRF protection working
- [ ] Rate limiting enforced

### Regression Testing
- [ ] Existing functionality still works
- [ ] No previously passing tests now fail
- [ ] Related features unaffected
- [ ] Performance hasn't degraded
- [ ] No new warnings or errors in logs
- [ ] Previous bugs haven't reappeared

### Non-Functional Testing
- [ ] Performance is acceptable
- [ ] Response times are reasonable
- [ ] Page load times acceptable
- [ ] UI is usable and intuitive
- [ ] Works across browsers/devices
- [ ] Accessibility considerations met
- [ ] Mobile responsiveness works
- [ ] Security best practices followed

### Data Validation
- [ ] Data persists correctly
- [ ] Data format is correct
- [ ] Data integrity maintained
- [ ] Transactions are atomic
- [ ] Rollback works on errors
- [ ] Data migration successful (if applicable)

---

## Test Planning Templates

### Feature Test Plan

```markdown
## Test Plan: [Feature Name]

### Scope
- What's being tested
- What's NOT being tested
- Testing environment

### Acceptance Criteria
1. [Criterion 1]
2. [Criterion 2]
3. [Criterion 3]

### Test Scenarios

#### Scenario 1: [Happy Path]
**Preconditions**: [Setup needed]
**Steps**:
1. [Step 1]
2. [Step 2]
3. [Step 3]
**Expected**: [Expected outcome]
**Priority**: Critical

#### Scenario 2: [Edge Case]
**Preconditions**: [Setup needed]
**Steps**: [...]
**Expected**: [Expected outcome]
**Priority**: High

#### Scenario 3: [Error Case]
**Preconditions**: [Setup needed]
**Steps**: [...]
**Expected**: [Error message/handling]
**Priority**: Medium

### Test Data Required
- [Data set 1]
- [Data set 2]

### Dependencies
- [Dependency 1]
- [Dependency 2]

### Risks
- [Risk 1 and mitigation]
- [Risk 2 and mitigation]
```

### Bug Verification Test Plan

```markdown
## Bug Verification Plan: [Bug ID]

### Original Bug
**Title**: [Bug title]
**Severity**: [Critical/High/Medium/Low]
**Description**: [What was wrong]

### Fix Description
[What was changed to fix it]

### Verification Tests

#### Test 1: Original Scenario
**Steps**: [Exact steps from bug report]
**Expected**: [Bug should not occur]

#### Test 2: Variation 1
**Steps**: [Similar scenario]
**Expected**: [Correct behavior]

#### Test 3: Edge Case
**Steps**: [Edge case related to fix]
**Expected**: [Correct behavior]

### Regression Tests
- [ ] Related feature A still works
- [ ] Related feature B still works
- [ ] Integration point C unaffected
```

---

## Testing Environment Setup

### Before Testing
1. **Verify environment is ready**:
   - Correct version deployed
   - Database in known state
   - Required services running
   - Test data available

2. **Clear state**:
   - Clear cache/cookies
   - Reset database to baseline (if needed)
   - Log out and log in fresh
   - Close and reopen browser

3. **Document environment**:
   - Version/build number
   - Browser and version
   - Operating system
   - Any special configuration

### During Testing
- Keep detailed notes
- Take screenshots of issues
- Save error messages/logs
- Document steps taken
- Note timestamps of issues

### After Testing
- Clean up test data
- Reset environment if needed
- Save test artifacts
- Archive test results

---

## Testing Best Practices

### Be Systematic
✅ Follow test plan
✅ Don't skip scenarios
✅ Test one thing at a time
✅ Document as you go
✅ Track coverage

### Be Thorough
✅ Test happy path first
✅ Then test edge cases
✅ Don't forget error scenarios
✅ Verify error messages
✅ Check all user paths

### Be Realistic
✅ Test as a real user would
✅ Use realistic data
✅ Test common workflows
✅ Consider real-world constraints
✅ Think about production usage

### Be Creative
✅ Try unexpected combinations
✅ Think of ways to break it
✅ Test unusual scenarios
✅ Explore beyond test cases
✅ Question assumptions

### Be Efficient
✅ Prioritize critical tests
✅ Automate repetitive tests
✅ Batch similar tests
✅ Use test data wisely
✅ Balance coverage vs. time

---

## Common Testing Mistakes to Avoid

❌ **Testing only happy path** - Edge cases find bugs
❌ **Skipping regression** - Old bugs can resurface
❌ **Assuming it works** - Always verify
❌ **Not reproducing bugs** - Ensure consistency
❌ **Testing in isolation** - Test integration too
❌ **Ignoring error messages** - They reveal issues
❌ **Not documenting steps** - Others need to reproduce
❌ **Testing too late** - Test early and often
❌ **Testing without requirements** - Know what's expected
❌ **Not verifying fixes** - Confirm bugs are actually fixed

---

## Test Status Definitions

**Not Started**: Testing hasn't begun
**In Progress**: Currently testing
**Blocked**: Cannot proceed (missing info, environment issues)
**Failed**: Issues found, needs fixes
**Passed**: All tests successful
**Approved**: Ready for deployment
**Deployed**: In production, monitoring for issues
