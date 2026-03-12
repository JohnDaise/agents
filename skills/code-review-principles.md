---
name: code-review-principles
description: Clean code principles and code review methodology. USE FOR reviewing code for maintainability, readability, and quality; evaluating code against clean code principles; providing structured feedback on code quality. DO NOT USE FOR security-specific reviews (use security-review skill) or runtime debugging.
---

# Code Review Principles

Apply these fundamental principles when reviewing code:

## 1. Meaningful Names
- Variables, functions, and classes use descriptive, searchable, and pronounceable names
- Names reveal intent without requiring comments
- Avoid abbreviations and single-letter names (except loop counters)

## 2. Single Responsibility Principle (SRP)
- Each function does one thing well
- Classes have only one reason to change
- Functions are small (ideally < 20 lines)
- Extract complex logic into well-named helper functions

## 3. Don't Repeat Yourself (DRY)
- No duplicate code blocks
- Common functionality is abstracted appropriately
- Repeated patterns suggest missing abstractions

## 4. Keep It Simple, Stupid (KISS)
- Prefer simple, straightforward solutions over "clever" code
- Avoid premature optimization
- Complex logic is broken into simpler components

## 5. Code Readability
- Code is self-documenting through clear structure and naming
- Comments explain WHY, not WHAT
- Proper indentation and formatting
- Logical code organization

## 6. Error Handling
- Proper error handling and validation
- Edge cases are considered
- No silent failures
- Meaningful error messages

## 7. Testing & Maintainability
- Code is testable and has appropriate test coverage
- Dependencies are manageable
- No tight coupling
- Changes won't cause ripple effects

## Review Output Format

Organize feedback by priority level:

### 🔴 Critical Issues (must fix)
- Bugs or potential runtime errors
- Major architecture violations

### 🟡 Warnings (should fix)
- Code smells and maintainability concerns
- Performance issues
- Inconsistent patterns
- Missing error handling

### 🟢 Suggestions (consider improving)
- Readability enhancements
- Better naming opportunities
- Simplification possibilities
- Documentation improvements

## For Each Issue

Provide:
1. **Location**: Specific file and line numbers
2. **Current Code**: Show the problematic code
3. **Issue**: Explain what's wrong and why it matters
4. **Improved Version**: Provide corrected code example
5. **Rationale**: Reference which clean code principle applies
