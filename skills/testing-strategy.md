---
name: testing-strategy
description: Testing strategies, methodologies, and best practices for software quality assurance. USE FOR writing effective tests; choosing testing approaches; test-driven development; ensuring code quality through testing. DO NOT USE FOR code review (use code-review-principles) or debugging (use technical-problem-solving).
---

# Testing Strategy

Comprehensive guide to software testing approaches, strategies, and best practices.

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

### The TDD Cycle: Red-Green-Refactor

1. **Red**: Write failing test for desired behavior
2. **Green**: Write minimum code to make test pass
3. **Refactor**: Improve code while keeping tests green

### TDD Benefits

- **Better Design**: Forces thinking about API/interface first
- **Confidence**: Know when done (tests pass)
- **Documentation**: Tests show intended usage
- **Regression Prevention**: Tests already exist

### TDD Process

```
1. Write test for next small piece of functionality
2. Run test → it fails (Red)
3. Write simplest code to make test pass
4. Run test → it passes (Green)
5. Refactor if needed (keeping tests green)
6. Repeat for next piece
```

### When TDD Works Well

- Clear requirements
- Business logic and algorithms
- Bug fixes (write failing test first)
- API design
- Well-understood domain

### When TDD Is Challenging

- UI/design exploration
- Prototyping/learning
- Unclear requirements
- High-level architecture
- External dependencies

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

Even if not doing strict TDD, think:
- "How will I test this?"
- "Is this testable?"
- "What are the inputs/outputs?"
- "What could go wrong?"

This leads to better design.
