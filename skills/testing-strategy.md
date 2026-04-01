---
name: testing-strategy
description: Testing strategies, methodologies, and best practices for software quality assurance. **TDD (Test-Driven Development) is MANDATORY** - write tests BEFORE implementation. USE FOR writing effective tests; choosing testing approaches; test-driven development; ensuring code quality through testing. DO NOT USE FOR code review (use code-review-principles) or debugging (use technical-problem-solving).
---

# Testing Strategy

Comprehensive guide to software testing approaches, strategies, and best practices.

**⚠️ CRITICAL: Test-Driven Development (TDD) is MANDATORY for all implementations**

---

## Quick TDD Reference Card

**The Golden Rule**: Write test BEFORE implementation (no exceptions)

### TDD in 3 Steps:
1. 🔴 **RED** - Write failing test
2. 🟢 **GREEN** - Make test pass (minimum code)
3. 🔵 **REFACTOR** - Clean up (keep tests green)

### Non-Negotiable Rules:
- ✅ Test first, code second
- ✅ See test fail before implementing
- ✅ All tests GREEN before proceeding
- ❌ Never commit with failing tests
- ❌ Never skip writing tests
- ❌ Never ignore test failures

### When Tests Fail:
**STOP 🛑 → DEBUG 🔍 → FIX ✅ → VERIFY 🟢**

---

## Testing Philosophy

### Why We Test

1. **Verify Correctness**: Ensure code works as intended
2. **Prevent Regressions**: Catch when changes break existing functionality
3. **Enable Refactoring**: Safely improve code structure
4. **Document Behavior**: Tests show how code should be used
5. **Design Feedback**: Hard-to-test code often indicates design problems

### Testing Pyramid

```
       /\
      /  \     E2E Tests (Few)
     /____\    - Full user workflows
    /      \   - Browser/UI automation
   /        \  - Slow, brittle, expensive
  /__________\ 

  /          \ Integration Tests (Some)
 /            \ - Multiple components together
/______________\ - API tests, database tests
                 - Moderate speed and complexity

/              \ Unit Tests (Many)
/                \ - Individual functions/classes
/                  \ - Fast, isolated, reliable
/____________________\ - Majority of test suite
```

**Principle**: Many fast unit tests, fewer integration tests, even fewer E2E tests

---

## Test Types & Levels

### Unit Tests

**What**: Test individual functions, methods, or classes in isolation

**Characteristics**:
- Fast (milliseconds per test)
- Isolated (no database, network, file system)
- Focused (one behavior per test)
- Deterministic (same result every time)

**When to Write**:
- For all business logic
- For utility functions
- For algorithms and calculations
- For data transformations
- For validation logic

**Example**:
```javascript
describe('calculateTotal', () => {
  test('sums item prices correctly', () => {
    const items = [
      { price: 10.00 },
      { price: 25.50 },
      { price: 5.00 }
    ];
    expect(calculateTotal(items)).toBe(40.50);
  });

  test('returns 0 for empty array', () => {
    expect(calculateTotal([])).toBe(0);
  });

  test('throws error for invalid input', () => {
    expect(() => calculateTotal(null)).toThrow();
  });
});
```

### Integration Tests

**What**: Test multiple components working together

**Characteristics**:
- Slower than unit tests (seconds per test)
- May use real dependencies (database, external services)
- Test component interactions
- Verify integration points work

**When to Write**:
- For API endpoints
- For database queries and transactions
- For service-to-service communication
- For third-party integrations
- For configuration loading

**Example**:
```javascript
describe('User API', () => {
  test('POST /users creates user in database', async () => {
    const response = await request(app)
      .post('/users')
      .send({ name: 'John', email: 'john@test.com' })
      .expect(201);

    const user = await db.users.findById(response.body.id);
    expect(user.name).toBe('John');
    expect(user.email).toBe('john@test.com');
  });
});
```

### End-to-End (E2E) Tests

**What**: Test complete user workflows through the UI

**Characteristics**:
- Slow (seconds to minutes per test)
- Full system integration
- Test from user perspective
- Most brittle and expensive

**When to Write**:
- For critical user journeys
- For smoke tests (basic functionality works)
- For scenarios spanning multiple pages/steps
- Sparingly - only most important flows

**Example** (using Playwright/Cypress):
```javascript
test('user can complete purchase', async ({ page }) => {
  await page.goto('/products');
  await page.click('[data-testid="add-to-cart"]');
  await page.click('[data-testid="checkout"]');
  await page.fill('#email', 'user@test.com');
  await page.fill('#card-number', '4242424242424242');
  await page.click('[data-testid="complete-purchase"]');
  
  await expect(page.locator('.success-message')).toBeVisible();
});
```

### Other Test Types

**Smoke Tests**: Basic tests to verify system is functional
**Regression Tests**: Tests for previously fixed bugs
**Performance Tests**: Verify speed and scalability
**Security Tests**: Check for vulnerabilities
**Accessibility Tests**: Ensure usability for all users

---

## Test-Driven Development (TDD)

### CRITICAL: TDD Is Mandatory for All Implementation

**You MUST follow TDD workflow when implementing features or fixing bugs.**

---

### The TDD Cycle: Red-Green-Refactor

```
🔴 RED    →    🟢 GREEN    →    🔵 REFACTOR    →    🔴 RED (next feature)
 Write         Make Test        Clean Up           Write Next
 Failing       Pass             Code               Test
 Test          (minimum)        (keep green)
```

1. **🔴 RED**: Write a failing test for desired behavior
2. **🟢 GREEN**: Write minimum code to make test pass
3. **🔵 REFACTOR**: Improve code while keeping tests green
4. **Repeat** for next small piece

---

### TDD Workflow for Agents (MANDATORY)

#### Before Writing ANY Implementation Code:

**Pre-Implementation Checklist:**
```
□ Requirements are clear (if not, ask project-manager)
□ I know what behavior to test
□ Test file is created/located
□ I'm ready to write a failing test
```

#### Step-by-Step TDD Process:

**Step 1: Write Failing Test (RED Phase)**
```
1. Write test describing expected behavior
2. Run test → MUST FAIL ❌
3. Verify failure message makes sense
4. If test passes unexpectedly:
   - Test is wrong, or
   - Feature already exists
   Fix before proceeding
```

**Step 2: Make Test Pass (GREEN Phase)**
```
1. Write MINIMUM code to make test pass
   - Don't add extra features
   - Don't perfect it yet
   - Simplest thing that works
2. Run test → MUST PASS ✅
3. Run ALL tests → ALL MUST PASS ✅
4. If tests fail:
   - Debug immediately
   - Fix code or test
   - Do NOT proceed until green
```

**Step 3: Refactor (REFACTOR Phase)**
```
1. Look for improvements:
   - Remove duplication
   - Improve naming
   - Simplify logic
   - Better structure
2. Make ONE small improvement
3. Run tests → MUST STAY GREEN ✅
4. If tests fail:
   - Refactor broke something
   - Undo change or fix
   - Never commit broken code
5. Repeat small refactorings as needed
```

**Step 4: Repeat for Next Piece**
```
1. Commit current working code (all tests green)
2. Start next feature/test
3. Return to RED phase
```

---

### TDD Non-Negotiable Rules

#### ✅ ALWAYS DO:
1. **Write test before implementation** - No exceptions
2. **See test fail (RED) before implementing** - Verify test works
3. **Run tests frequently** - After every small change
4. **Keep all tests passing** - GREEN is the only acceptable state
5. **Fix failing tests immediately** - They are critical blockers
6. **Commit only when all tests pass** - Never commit RED

#### ❌ NEVER DO:
1. **Write implementation before test** - Defeats TDD purpose
2. **Skip RED phase** - Must see test fail first
3. **Continue with failing tests** - STOP and fix immediately
4. **Comment out failing tests** - Fix them instead
5. **Ignore flaky tests** - Fix or delete them
6. **Write multiple features before testing** - Small increments only

---

### When Tests Fail: Action Required

**🚨 STOP IMMEDIATELY - Failing tests are BLOCKERS**

#### Scenario 1: Test Fails in RED Phase (Expected)
```
✅ Good! This is what should happen.
→ Proceed to GREEN phase (implement feature)
```

#### Scenario 2: Test Fails in GREEN Phase (Implementation Issue)
```
❌ Your implementation doesn't work correctly
→ Debug and fix implementation
→ Re-run tests until GREEN
→ Do NOT add more features until fixed
```

#### Scenario 3: Test Fails in REFACTOR Phase (Broke Something)
```
❌ Your refactor broke working code
→ Undo refactor or fix the break
→ Get back to GREEN before continuing
→ Make smaller refactor steps
```

#### Scenario 4: Previously Passing Test Now Fails (Regression)
```
🚨 CRITICAL: Your changes broke existing functionality
→ STOP all other work immediately
→ Debug what changed
→ Fix the regression
→ Get ALL tests GREEN before proceeding
→ Consider: Do you need more tests to catch this?
```

#### Scenario 5: Failing Test and You're Stuck
```
🆘 Need help - escalate
→ Document what you've tried
→ Ask project-manager for help
→ Explain: test written, implementation attempted, still failing
→ Don't struggle alone for >30 minutes
```

---

### TDD Benefits

- **Better Design**: Forces thinking about API/interface first
- **Immediate Feedback**: Know if code works within seconds
- **Confidence**: Know when done (tests pass)
- **Documentation**: Tests show intended usage
- **Regression Prevention**: Tests already exist
- **Less Debugging**: Catch issues immediately, not later
- **Refactoring Safety**: Tests catch breaks during refactoring

---

### When TDD Works Well (Use Always)

✅ **Business logic and algorithms** - Perfect for TDD
✅ **Bug fixes** - Write failing test that reproduces bug first
✅ **API endpoints** - Test contract before implementation
✅ **Data transformations** - Clear inputs/outputs
✅ **Validation logic** - Test all edge cases
✅ **New features** - Define behavior first

### When TDD Is Challenging (Still Recommended)

⚠️ **UI/design exploration** - Write tests after exploring, before finalizing
⚠️ **Prototyping/learning** - Quick spike, then TDD for real implementation
⚠️ **Unclear requirements** - Clarify with project-manager FIRST, then TDD
⚠️ **Complex integration** - Start with integration test, fill in unit tests

**Note**: Even in challenging cases, write tests before considering feature "done"

---

## Writing Effective Tests

### Test Structure: AAA Pattern

**Arrange**: Set up test data and preconditions
**Act**: Execute the code under test
**Assert**: Verify the expected outcome

```javascript
test('withdrawing money updates balance', () => {
  // Arrange
  const account = new Account(100);
  
  // Act
  account.withdraw(30);
  
  // Assert
  expect(account.balance).toBe(70);
});
```

### Test Naming Conventions

**Pattern**: `should [expected behavior] when [condition]`

Examples:
- ✅ `should return 404 when user not found`
- ✅ `should validate email format`
- ✅ `should throw error when amount is negative`
- ❌ `test1` (not descriptive)
- ❌ `testUserFunction` (vague)

### One Assertion Per Test (Guideline)

**Prefer**: Each test verifies one logical behavior

```javascript
// Good: Focused tests
test('should set status to active', () => {
  expect(user.status).toBe('active');
});

test('should set activation date', () => {
  expect(user.activatedAt).toBeInstanceOf(Date);
});

// Acceptable: Related assertions
test('should activate user account', () => {
  expect(user.status).toBe('active');
  expect(user.activatedAt).toBeInstanceOf(Date);
});

// Bad: Testing multiple unrelated things
test('user tests', () => {
  expect(user.name).toBe('John');
  expect(user.delete()).toBe(true);
  expect(user.preferences.theme).toBe('dark');
});
```

### Test Independence

Each test should:
- Run independently of others
- Not depend on test execution order
- Clean up after itself
- Start from known state

```javascript
// Good: Each test independent
beforeEach(() => {
  database.clear();
  user = createTestUser();
});

afterEach(() => {
  database.cleanup();
});

// Bad: Tests depend on each other
test('create user', () => {
  globalUser = createUser(); // Don't use global state
});

test('update user', () => {
  updateUser(globalUser); // Depends on previous test
});
```

---

## What to Test

### Always Test

✅ **Business Logic**: Core functionality and rules
✅ **Edge Cases**: Boundary conditions, empty/null values
✅ **Error Handling**: Invalid inputs, exception paths
✅ **Public APIs**: Methods/functions used by other code
✅ **Bug Fixes**: Add test for each bug before fixing

### Consider Testing

⚠️ **Complex Conditionals**: Multiple if/else branches
⚠️ **Data Transformations**: Mapping, filtering, reducing
⚠️ **Integration Points**: API calls, database queries
⚠️ **Critical Paths**: Payment, authentication, data loss scenarios

### Usually Don't Test

❌ **Private Methods**: Test through public interface
❌ **Trivial Code**: Simple getters/setters
❌ **Third-Party Libraries**: Assume they're tested
❌ **Framework Code**: Framework handles its own testing
❌ **Configuration**: Unless complex logic involved

---

## Testing Edge Cases

### Common Edge Cases to Test

**Numeric Inputs**:
- Zero
- Negative numbers
- Very large numbers
- Decimals vs. integers
- NaN, Infinity

**Strings**:
- Empty string ("")
- Single character
- Very long string
- Special characters
- Unicode/emoji
- Null vs. undefined

**Collections (Arrays, Lists)**:
- Empty collection
- Single item
- Many items
- Duplicates
- Null items within collection

**Dates/Times**:
- Past, present, future
- Leap years
- Daylight saving time
- Time zones
- Invalid dates

**Boundaries**:
- Minimum value
- Maximum value
- Just below boundary
- Just above boundary

**States**:
- Initial state
- Final state
- Error state
- Intermediate states

---

## Test Doubles (Mocks, Stubs, Fakes)

### Stub
Returns predefined values, ignores calls

```javascript
const userStub = {
  getName: () => 'John Doe',
  getRole: () => 'admin'
};
```

### Mock
Records calls, verifies interactions

```javascript
const emailMock = jest.fn();
sendWelcomeEmail(user, emailMock);
expect(emailMock).toHaveBeenCalledWith('john@test.com');
```

### Fake
Working implementation, simplified

```javascript
class FakeDatabase {
  constructor() {
    this.data = [];
  }
  save(item) {
    this.data.push(item);
  }
  findById(id) {
    return this.data.find(item => item.id === id);
  }
}
```

### When to Use Each

- **Stub**: When you need predetermined responses
- **Mock**: When you need to verify interactions
- **Fake**: When you need somewhat realistic behavior

### Mocking Best Practices

✅ **Mock external dependencies** (APIs, databases)
✅ **Keep mocks simple** - don't over-complicate
✅ **Verify important interactions** - was API called?
❌ **Don't mock everything** - tests become brittle
❌ **Don't mock code under test** - defeats purpose

---

## Test Coverage

### What is Coverage?

Percentage of code executed by tests:
- **Line Coverage**: % of lines executed
- **Branch Coverage**: % of if/else branches taken
- **Function Coverage**: % of functions called
- **Statement Coverage**: % of statements executed

### Coverage Goals

- **80%+ is good** for most projects
- **90%+ is great** for critical systems
- **100% is overkill** - diminishing returns

### Coverage Is Not Everything

✅ High coverage + good assertions = confident
⚠️ High coverage + weak assertions = false confidence
❌ Low coverage = risky

```javascript
// 100% coverage but weak test
test('processes user', () => {
  processUser(); // Runs but doesn't verify anything
});

// Better test
test('processes user', () => {
  const result = processUser({ name: 'John' });
  expect(result.processed).toBe(true);
  expect(result.user.name).toBe('John');
});
```

---

## Testing Best Practices

### General Principles

1. **Fast**: Tests should run quickly (< 1 sec for unit tests)
2. **Isolated**: Tests don't affect each other
3. **Repeatable**: Same result every time
4. **Self-Validating**: Pass/fail, no manual verification
5. **Timely**: Written at the right time (with code, not months later)

### Practical Tips

✅ **Use descriptive names** - test name explains what's tested
✅ **Test behavior, not implementation** - test what, not how
✅ **One logical assertion per test** - focused tests
✅ **Use test fixtures/factories** - consistent test data
✅ **Keep tests simple** - easier to understand than production code
✅ **Run tests frequently** - catch issues early
✅ **Fix failing tests immediately** - don't accumulate

❌ **Don't test private methods directly** - test through public API
❌ **Don't make tests dependent on each other** - isolation
❌ **Don't test implementation details** - brittle tests
❌ **Don't duplicate production logic in tests** - copy-paste errors
❌ **Don't ignore flaky tests** - fix or delete them

---

## Testing Anti-Patterns

### The Giant Setup
**Problem**: 100 lines of setup for 1 line of test
**Solution**: Extract to helper functions, use factories

### The Mystery Guest
**Problem**: Test depends on external data not visible in test
**Solution**: Make all test data explicit in test

### The Hidden Dependency
**Problem**: Test depends on specific order or previous test
**Solution**: Make tests independent, use beforeEach

### The Slow Test
**Problem**: Test takes minutes to run
**Solution**: Mock external dependencies, use test doubles

### The Fragile Test
**Problem**: Test breaks with unrelated changes
**Solution**: Test behavior not implementation

### The Assertion Roulette
**Problem**: Multiple assertions, unclear which failed
**Solution**: One logical assertion per test, clear messages

---

## Continuous Testing

### Pre-Commit
- Run fast unit tests
- Run linter/formatter
- Quick feedback loop

### CI/CD Pipeline
```
Commit → Unit Tests → Integration Tests → E2E Tests → Deploy
         (seconds)    (minutes)           (minutes)
```

### Test Automation
- Automatically run on every commit
- Block merges if tests fail
- Generate coverage reports
- Fast feedback to developers

---

## Testing Mindset

### Think Like a Tester

- **Question assumptions**: What could go wrong?
- **Consider edge cases**: What's unusual?
- **Break things**: Try to make it fail
- **Think maliciously**: How could this be abused?
- **Verify, don't assume**: Does it actually work?

### When Writing Code

1. Write test (if TDD)
2. Write code
3. Write test (if not TDD)
4. Consider edge cases and add tests
5. Run all tests
6. Fix any failures

### Test-First Thinking

With TDD, you ALWAYS think:
- "What behavior should this have?" → Write test
- "What are the inputs/outputs?" → Define in test
- "What could go wrong?" → Test edge cases
- "How will I know it works?" → The test tells you

This leads to better design and more reliable code.

---

## TDD Pre-Implementation Checklist

**Before writing ANY implementation code, verify:**

### ✅ Requirements Ready
```
□ I understand what needs to be built
□ Requirements are clear and unambiguous
□ I know what "done" looks like
□ Acceptance criteria are defined
□ If unclear: I've asked project-manager for clarification
```

### ✅ Test Ready
```
□ Test file exists or is created
□ Test framework is set up and working
□ I can run tests and see results
□ I know what behavior to test
□ I know what the test assertion should be
```

### ✅ Mindset Ready
```
□ I will write test BEFORE implementation
□ I will see test FAIL before implementing
□ I will write minimum code to pass test
□ I will run tests frequently
□ I will not continue with failing tests
□ I will commit only when all tests pass
```

### ❌ Warning Signs - Do NOT Proceed If:
```
⛔ Requirements are unclear or ambiguous
⛔ I don't know what to test
⛔ I'm thinking "I'll write tests later"
⛔ I'm tempted to skip the test
⛔ Test framework isn't set up
⛔ I don't have time to do it right

→ If any of these: STOP and resolve first
```

---

## TDD Examples By Scenario

### Example 1: New Feature (Backend)
```
✅ TDD APPROACH:

// 1. RED: Write failing test
test('should create user with valid data', async () => {
  const userData = { email: 'test@example.com', name: 'John' };
  const user = await createUser(userData);
  
  expect(user.id).toBeDefined();
  expect(user.email).toBe('test@example.com');
  expect(user.name).toBe('John');
});

// Run test → ❌ FAILS (createUser doesn't exist)

// 2. GREEN: Minimum implementation
function createUser(data) {
  return {
    id: generateId(),
    email: data.email,
    name: data.name
  };
}

// Run test → ✅ PASSES

// 3. REFACTOR: Add validation (with test first!)
test('should reject invalid email', async () => {
  await expect(createUser({ email: 'invalid' }))
    .rejects.toThrow('Invalid email');
});

// Run test → ❌ FAILS (no validation yet)

// Add validation
function createUser(data) {
  if (!isValidEmail(data.email)) {
    throw new Error('Invalid email');
  }
  return {
    id: generateId(),
    email: data.email,
    name: data.name
  };
}

// Run test → ✅ PASSES
```

### Example 2: Bug Fix
```
✅ TDD APPROACH:

// 1. RED: Write test that reproduces bug
test('should handle empty cart checkout', () => {
  const cart = [];
  const result = calculateTotal(cart);
  
  expect(result).toBe(0);
});

// Run test → ❌ FAILS (throws error on empty cart - the bug!)

// 2. GREEN: Fix the bug
function calculateTotal(items) {
  if (!items || items.length === 0) return 0; // Fix
  return items.reduce((sum, item) => sum + item.price, 0);
}

// Run test → ✅ PASSES (bug fixed)

// 3. Add more edge case tests
test('should handle null cart', () => {
  expect(calculateTotal(null)).toBe(0);
});

// Already passes! Our fix handles this.
```

### Example 3: Refactoring Existing Code
```
✅ TDD APPROACH:

// 1. BEFORE refactoring: Ensure tests exist and pass
test('parseDate handles various formats', () => {
  expect(parseDate('2024-01-15')).toEqual(new Date(2024, 0, 15));
  expect(parseDate('01/15/2024')).toEqual(new Date(2024, 0, 15));
});

// Run tests → ✅ All PASS

// 2. Refactor code (improve implementation)
function parseDate(dateStr) {
  // Old messy code → refactor to cleaner version
  // ... improved implementation ...
}

// 3. Run tests after refactor → ✅ All PASS
// If tests fail: refactor broke something, fix or undo
```
