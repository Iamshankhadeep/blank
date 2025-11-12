# Code Quality Checklist

Use this before submitting your own code for review. Better to catch issues yourself than have them pointed out in review.

## Before You Commit

### Names & Clarity
- [ ] All variables have clear, meaningful names
- [ ] Function names are verbs (e.g., `getUserData`, `calculateTotal`)
- [ ] Class names are nouns (e.g., `User`, `OrderProcessor`)
- [ ] No single-letter variables (except simple loop counters)
- [ ] No abbreviations that aren't obvious
- [ ] Names reveal intent without comments

### Functions
- [ ] Each function does ONE thing
- [ ] Functions are small (<20 lines ideally, <30 max)
- [ ] No more than 3 parameters (use object if more needed)
- [ ] No side effects (or clearly named if unavoidable)
- [ ] One level of abstraction per function
- [ ] Can you explain what it does in one sentence?

### Code Smells
- [ ] No duplicate code
- [ ] No magic numbers or strings (use constants)
- [ ] No deep nesting (>3 levels)
- [ ] No long parameter lists
- [ ] No commented-out code
- [ ] No TODO comments without tickets
- [ ] No dead code

### SOLID Principles
- [ ] **S**: Each class has one responsibility
- [ ] **O**: Can extend without modifying
- [ ] **L**: Subtypes can replace base types
- [ ] **I**: No unused interface methods
- [ ] **D**: Depend on abstractions, not concrete classes

### Error Handling
- [ ] All errors are handled (no silent failures)
- [ ] Error messages are helpful
- [ ] No empty catch blocks
- [ ] Errors are logged appropriately
- [ ] No swallowing exceptions
- [ ] Fail fast when appropriate

### Security
- [ ] No secrets, keys, or passwords in code
- [ ] User input is validated
- [ ] SQL queries are parameterized
- [ ] User data is escaped when displayed
- [ ] Authentication checks present
- [ ] Authorization checks present
- [ ] No `eval()` or similar dangerous functions

### Testing
- [ ] Critical paths have tests
- [ ] Edge cases are tested
- [ ] Error cases are tested
- [ ] Tests are readable
- [ ] Tests test behavior, not implementation
- [ ] Tests will catch regressions
- [ ] All tests pass

### Performance
- [ ] No obvious performance issues
- [ ] No N+1 queries
- [ ] Appropriate data structures
- [ ] No unnecessary loops
- [ ] Async operations where appropriate
- [ ] Resources cleaned up properly

### Comments & Documentation
- [ ] Comments explain WHY, not WHAT
- [ ] Complex logic is explained
- [ ] No obvious comments (code is self-documenting)
- [ ] Public APIs are documented
- [ ] README updated if needed

## Self-Review Process

### Step 1: Review Your Own PR First (5 min)
Before requesting review:
1. Read your changes as if you're the reviewer
2. Add comments to explain complex parts
3. Check for any missed issues
4. Ensure PR description is clear

### Step 2: Run These Checks (10 min)

```bash
# Linting
npm run lint  # or your linter
pylint .      # Python
rubocop .     # Ruby

# Tests
npm test      # Run all tests
npm run coverage  # Check coverage

# Build
npm run build  # Ensure it builds

# Format
npm run format  # Auto-format
```

### Step 3: Ask Yourself (5 min)
- Would I be proud to maintain this code in 6 months?
- Is this the simplest solution that works?
- Can a new team member understand this?
- Have I introduced technical debt?
- Is this production-ready?

## Common Quality Issues

### ❌ Poor Quality
```javascript
// Unclear names, magic numbers, no error handling
function p(d) {
  let r = d * 0.15;
  if (d > 100) r = d * 0.20;
  return r;
}
```

### ✅ Good Quality
```javascript
// Clear names, constants, documented
const STANDARD_DISCOUNT_RATE = 0.15;
const BULK_DISCOUNT_RATE = 0.20;
const BULK_DISCOUNT_THRESHOLD = 100;

/**
 * Calculate discount based on purchase amount.
 * Bulk purchases (>$100) get 20% off, others get 15%.
 */
function calculateDiscount(purchaseAmount) {
  if (purchaseAmount < 0) {
    throw new Error('Purchase amount cannot be negative');
  }

  const rate = purchaseAmount > BULK_DISCOUNT_THRESHOLD
    ? BULK_DISCOUNT_RATE
    : STANDARD_DISCOUNT_RATE;

  return purchaseAmount * rate;
}
```

## Quality Metrics

### File Size
- 🟢 Good: <200 lines
- 🟡 Acceptable: 200-500 lines
- 🔴 Refactor: >500 lines

### Function Size
- 🟢 Good: <15 lines
- 🟡 Acceptable: 15-30 lines
- 🔴 Refactor: >30 lines

### Cyclomatic Complexity
- 🟢 Simple: 1-5
- 🟡 Moderate: 6-10
- 🔴 Complex: >10 (needs refactoring)

### Test Coverage
- 🟢 Good: >80%
- 🟡 Acceptable: 60-80%
- 🔴 Poor: <60%

(But quality > quantity for tests!)

## The Boy Scout Rule

> "Leave the code better than you found it."

When touching existing code:
- [ ] Fix one code smell
- [ ] Improve one name
- [ ] Add one missing test
- [ ] Extract one duplicated piece
- [ ] Simplify one complex function

## Language-Specific Quality Checks

### JavaScript/TypeScript
- [ ] Use `const` by default, `let` when needed, never `var`
- [ ] Use `===` instead of `==`
- [ ] Proper async/await (no callback hell)
- [ ] TypeScript types are meaningful (not `any`)
- [ ] No console.logs (use proper logging)

### Python
- [ ] PEP 8 compliant
- [ ] Type hints for function signatures
- [ ] Proper use of list/dict comprehensions
- [ ] Context managers for resources
- [ ] No bare `except:` clauses

### Java
- [ ] Proper access modifiers
- [ ] Try-with-resources for cleanup
- [ ] Proper exception hierarchy
- [ ] No null returns (use Optional)
- [ ] Streams used appropriately

### Go
- [ ] All errors checked
- [ ] Defer used for cleanup
- [ ] Proper context usage
- [ ] Exported functions documented
- [ ] `gofmt` applied

## Pre-Commit Ritual

Before every commit:
1. ✅ Run tests
2. ✅ Run linter
3. ✅ Review your diff
4. ✅ Check this list
5. ✅ Write clear commit message

## Pre-PR Ritual

Before requesting review:
1. ✅ Self-review the entire PR
2. ✅ Ensure all checks pass
3. ✅ Write thorough PR description
4. ✅ Link related issues
5. ✅ Add screenshots/videos if UI changes
6. ✅ Consider: Can this be smaller?

## Questions to Ask Yourself

### Clarity
- Can I understand this after 6 months?
- Would a new team member understand this?
- Is the intent obvious?

### Simplicity
- Is this the simplest solution?
- Am I over-engineering?
- Can I remove anything?

### Maintainability
- Will this be easy to change?
- Will this be easy to debug?
- Will this be easy to test?

### Quality
- Am I proud of this code?
- Would I want to review this?
- Is this my best work?

## Red Flags

🚩 If you find yourself thinking:
- "I'll clean this up later" → Do it now
- "This is temporary" → It will become permanent
- "Nobody will notice" → They will
- "It works, that's enough" → Not for senior level
- "I'm the only one who touches this" → You won't be

## Remember

> "First make it work, then make it right, then make it fast."
> - Kent Beck

Quality is not:
- ❌ Writing clever code
- ❌ Using every design pattern
- ❌ Perfect abstractions
- ❌ Zero duplication at all costs

Quality is:
- ✅ Code that works correctly
- ✅ Code that's easy to understand
- ✅ Code that's easy to change
- ✅ Code that's well-tested

---

**Related:**
- [[03 - Clean Code Principles]]
- [[04 - SOLID Principles]]
- [[02 - Code Smells & Anti-Patterns]]
- [[PR Review Checklist]]
