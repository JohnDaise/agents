---
name: defect-management
description: Defect reporting, classification, and tracking methodology for quality assurance. USE FOR documenting bugs; classifying severity; creating bug reports; tracking defect lifecycle. DO NOT USE FOR fixing bugs (that's for developers) or test execution (use qa-test-execution).
---

# Defect Management

Comprehensive guide to identifying, documenting, and managing software defects.

## Defect Lifecycle

```
New → Assigned → In Progress → Fixed → Verified → Closed
                                   ↓
                              Not Fixed (Reopened) → Assigned
```

### Lifecycle Stages

1. **New**: Defect reported, awaiting review
2. **Assigned**: Assigned to developer
3. **In Progress**: Developer is working on fix
4. **Fixed**: Developer completed fix, ready for verification
5. **Verified**: QA confirmed fix works
6. **Closed**: Defect resolved and deployed
7. **Reopened**: QA found issue still exists or regression

---

## Severity Classification

### Critical (P0)
**Definition**: 
- System crash or data loss
- Security vulnerability exposing sensitive data
- Complete feature failure blocking all users
- Data corruption or integrity issues
- No workaround available

**Examples**:
- Application crashes on launch for all users
- SQL injection vulnerability discovered
- Payment processing completely broken
- Database corruption causing data loss
- Authentication system down

**Timeline**: Fix immediately, deploy hotfix ASAP

---

### High (P1)
**Definition**:
- Major feature broken or severely degraded
- Significant impact on user experience
- Affects large percentage of users
- No reasonable workaround available
- Core functionality impaired

**Examples**:
- Search feature returns no results
- Users cannot submit forms
- Major UI element not rendering
- Critical API endpoint failing
- Reports showing incorrect data

**Timeline**: Fix within 24-48 hours, include in next release

---

### Medium (P2)
**Definition**:
- Feature partially works but has issues
- Moderate impact on user experience
- Affects some users or specific scenarios
- Workaround exists but inconvenient
- Non-critical functionality broken

**Examples**:
- Error message displays but feature works
- UI element misaligned in certain browsers
- Optional feature not working
- Performance degradation in specific cases
- Minor data display issues

**Timeline**: Fix in next sprint/release cycle

---

### Low (P3)
**Definition**:
- Minor cosmetic issues
- Minimal user impact
- Easy workaround available
- Affects very few users
- Nice-to-have improvements

**Examples**:
- Spelling/grammar errors
- Minor visual inconsistencies
- Color contrast slightly off
- Tooltip wording unclear
- Console warning messages

**Timeline**: Fix when convenient, may defer

---

## Defect Report Template

### Standard Bug Report

```markdown
## Bug: [Short Descriptive Title]

**ID**: BUG-[Number]
**Severity**: Critical / High / Medium / Low
**Status**: New
**Reported By**: [Your name]
**Date Reported**: YYYY-MM-DD
**Environment**: [Details]

### Environment Details
- **Browser**: Chrome 120.0 / Firefox 121.0 / Safari 17.0
- **OS**: macOS 14.1 / Windows 11 / Ubuntu 22.04
- **Device**: Desktop / Mobile / Tablet
- **Screen Resolution**: 1920x1080
- **Version/Build**: v1.2.3 (build 456)

### Steps to Reproduce
1. Navigate to [URL/Page]
2. Click on [Element/Button]
3. Enter [Input data]
4. Submit/Save
5. Observer [Issue]

### Expected Behavior
[Clear description of what should happen]
- The form should submit successfully
- User should see confirmation message
- Data should save to database

### Actual Behavior
[Clear description of what actually happens]
- Form submission fails
- Error message displays: "[exact error text]"
- Data is not saved

### Screenshots/Videos
[Attach visual evidence]
- Screenshot of error: [filename]
- Video of reproduction: [link]
- Console logs: [paste]

### Browser Console Errors
```
[Paste exact error messages from console]
TypeError: Cannot read property 'name' of undefined
    at validateForm (app.js:123)
    at handleSubmit (app.js:456)
```

### Network Requests
- **Request**: POST /api/users
- **Status**: 500 Internal Server Error
- **Response**: { "error": "Validation failed" }

### Additional Context
- Issue started after deployment on [date]
- Only affects users with [specific condition]
- Related to feature [feature name]
- May be connected to bug [BUG-123]

### Impact Assessment
- **Users Affected**: ~50% of users / All users / 10 users
- **Frequency**: Every time / Intermittent / Rare
- **Business Impact**: Blocks sales / Confuses users / Minor annoyance

### Possible Cause (Optional)
[Your hypothesis if you have one]
Appears to be null reference when user object is undefined

### Suggested Fix (Optional)
[If you have suggestions]
Add null check before accessing user.name property

### Related Issues
- Similar to BUG-345
- Blocks feature FT-789
- Regression of BUG-234
```

---

## Defect Report Best Practices

### Writing Effective Bug Reports

#### DO ✅
- **Be specific**: Exact steps, exact inputs, exact errors
- **Be objective**: Describe facts, not opinions
- **Reproduce first**: Ensure bug is consistent before reporting
- **One bug per report**: Separate issues separately
- **Include evidence**: Screenshots, logs, videos
- **Provide context**: When started, what changed, who's affected
- **Classify accurately**: Appropriate severity level
- **Update promptly**: Add new information as discovered

#### DON'T ❌
- **Be vague**: "It doesn't work" is not helpful
- **Be emotional**: "This is terrible!" doesn't help fix it
- **Assume**: Report what you observe, not what you think
- **Combine issues**: Multiple bugs in one report get confusing
- **Forget evidence**: "I saw it once" isn't reproducible
- **Exaggerate severity**: Crying wolf reduces credibility
- **Report duplicates**: Check for existing reports first
- **Abandon reports**: Follow through until resolution

### Title Guidelines

**Good Titles** ✅
- "Login fails with valid credentials on mobile"
- "Search returns no results when query contains special characters"
- "Payment form allows submission with invalid credit card"
- "User profile image upload fails for files over 5MB"

**Bad Titles** ❌
- "Bug in login" (too vague)
- "It's broken" (not descriptive)
- "URGENT!!!" (severity in title, no description)
- "This needs to be fixed ASAP" (not a description)

### Steps to Reproduce

**Good Steps** ✅
```
1. Log in as user with email "test@example.com"
2. Navigate to Settings > Profile
3. Click "Upload Photo" button
4. Select image file larger than 5MB
5. Click "Save"
→ Result: Error message appears, image not saved
```

**Bad Steps** ❌
```
1. Go to the profile page
2. Try to upload an image
→ Result: Doesn't work
```

---

## Defect Classification Matrix

| Type | Description | Examples |
|------|-------------|----------|
| **Functional** | Feature doesn't work as specified | Button doesn't submit, calculation wrong |
| **UI/Visual** | Display or layout issues | Misaligned text, wrong colors |
| **Performance** | Speed or efficiency problems | Slow page load, timeout errors |
| **Security** | Vulnerability or security flaw | XSS, SQL injection, exposed data |
| **Usability** | User experience issues | Confusing workflow, unclear labels |
| **Compatibility** | Works in some env, not others | Browser-specific, mobile issues |
| **Data** | Data integrity or accuracy issues | Wrong calculations, corrupted data |
| **Integration** | Problems with external systems | API failures, third-party errors |

---

## Defect Triage

### Questions to Answer

1. **Is it really a bug?**
   - Expected behavior vs. actual behavior
   - Check requirements/specifications
   - Confirm with stakeholders if needed

2. **Can it be reproduced?**
   - Consistent: High priority
   - Intermittent: Need more investigation
   - Cannot reproduce: Need more info

3. **What's the severity?**
   - Critical: Immediate action
   - High: Next release
   - Medium: Planned release
   - Low: Backlog

4. **Who's affected?**
   - All users: Higher priority
   - Specific users: May be lower priority
   - No users yet: Depends on launch timeline

5. **Is there a workaround?**
   - No workaround: Higher priority
   - Easy workaround: Lower priority
   - Workaround is acceptable: May defer

6. **What's the root cause?**
   - Clear cause: Easier to fix
   - Unknown cause: Needs investigation
   - Complex cause: May need more time

---

## Verifying Bug Fixes

### Verification Checklist

- [ ] **Reproduce original bug**: Confirm bug existed
- [ ] **Test the fix**: Follow exact steps from report
- [ ] **Verify bug is gone**: Issue no longer occurs
- [ ] **Test variations**: Similar scenarios work correctly
- [ ] **Check edge cases**: Boundaries around the fix
- [ ] **Regression test**: Related features still work
- [ ] **Review fix details**: Understand what changed
- [ ] **Test in multiple environments**: Different browsers/devices
- [ ] **Verify acceptance criteria**: If specified

### Verification Outcomes

**Pass ✅**
```
Verification: PASSED

Tested fix for BUG-123:
✓ Original issue no longer occurs
✓ Tested 5 variations - all pass
✓ No regressions detected
✓ Works in Chrome, Firefox, Safari

Recommendation: Approve for deployment
Status: Verified → Closed
```

**Fail ❌**
```
Verification: FAILED

Tested fix for BUG-123:
✗ Original issue still occurs in scenario X
✓ Works in scenario Y
✗ New issue introduced: [description]

Reopening bug with additional details.
Status: Fixed → Reopened
```

**Partial Fix ⚠️**
```
Verification: PARTIAL

Tested fix for BUG-123:
✓ Primary scenario fixed
✗ Edge case still failing: [description]

Discussed with developer - edge case is separate issue.
Opening new bug BUG-456 for edge case.

This bug: Approved
Status: Verified → Closed
```

---

## Defect Metrics & Tracking

### Key Metrics

**Defect Density**
- Number of defects per feature/module
- Indicates quality of specific areas

**Defect Discovery Rate**
- How quickly bugs are found
- Shows testing effectiveness

**Fix Rate**
- How quickly bugs are resolved
- Shows development velocity

**Reopen Rate**
- Percentage of bugs reopened
- Indicates fix quality

**Escape Rate**
- Bugs found in production (not in testing)
- Shows testing coverage gaps

### Tracking Template

```markdown
## Sprint Defect Summary

**Sprint**: Sprint 12
**Period**: Jan 1-14, 2026

### Defects by Severity
- Critical: 2 (Fixed: 2, Open: 0)
- High: 5 (Fixed: 4, Open: 1)
- Medium: 12 (Fixed: 8, Open: 4)
- Low: 8 (Fixed: 3, Open: 5)

### Defects by Status
- New: 3
- In Progress: 7
- Fixed: 17
- Verified: 12
- Closed: 12
- Reopened: 1

### Top Issues
1. BUG-234 (High): Login validation regression
2. BUG-245 (High): Search performance degradation
3. BUG-256 (Medium): UI alignment on mobile

### Trends
- 📈 Defect discovery up 20% (good - more thorough testing)
- 📉 Reopen rate down to 5% (good - better fixes)
- ⚠️ High severity bugs up - needs attention
```

---

## Communication Guidelines

### When Reporting to Developers

✅ **Be collaborative**
- "I found an issue that needs attention"
- "Here's detailed information to help debug"
- "Let me know if you need more information"

❌ **Not accusatory**
- "You broke this feature"
- "This code is terrible"
- "Why did you do it this way?"

✅ **Be helpful**
- Provide clear reproduction steps
- Include screenshots/logs
- Suggest possible cause if you have insight
- Offer to pair with developer to reproduce

✅ **Be respectful**
- Acknowledge complexity of development
- Recognize time constraints
- Understand prioritization decisions
- Thank developers for fixes

### When Escalating Issues

**Escalate when:**
- Critical issue not getting attention
- Timeline at risk
- Repeated reopens of same bug
- Cross-team coordination needed
- Severity reclassification needed

**How to escalate:**
1. First try direct communication
2. Provide data and context
3. Explain business impact
4. Suggest resolution path
5. Loop in appropriate stakeholders

---

## Bug Prevention Mindset

While QA finds bugs, everyone should prevent them:

### Developer Prevention
- Write comprehensive tests
- Follow code review process
- Test locally before committing
- Handle edge cases in code

### QA Prevention
- Test early and often
- Provide fast feedback
- Suggest testability improvements
- Share patterns observed

### Process Prevention
- Clear requirements
- Good acceptance criteria
- Adequate testing time
- Regression test suites

**Remember**: The goal is not finding more bugs, but shipping with fewer bugs.
