# Common Code Smells Quick Reference

Quick lookup guide for identifying code smells during reviews. Print this out or keep it handy!

## 🔴 Critical Smells (Fix Immediately)

| Smell | Sign | Quick Fix |
|-------|------|-----------|
| **Hardcoded Secrets** | API keys, passwords in code | Move to environment variables |
| **SQL Injection** | String concat in queries | Use parameterized queries |
| **XSS Vulnerability** | Unescaped user input | Escape output, use textContent |
| **No Error Handling** | No try-catch, no validation | Add proper error handling |

## 🟡 Structure Smells

| Smell | Description | Limit | Fix |
|-------|-------------|-------|-----|
| **Long Method** | Function too many lines | >30 lines | Extract methods |
| **Large Class** | Class doing too much | >300 lines | Split into smaller classes |
| **Long Parameter List** | Too many parameters | >3 params | Use object/builder pattern |
| **Deep Nesting** | Too many levels | >3 levels | Use guard clauses, extract |

## 🟠 Naming Smells

| Bad | Why | Good |
|-----|-----|------|
| `data`, `info`, `temp` | Too vague | `userProfile`, `orderDetails` |
| `doStuff()` | Unclear action | `processPayment()`, `validateEmail()` |
| `flag`, `check` | Unclear purpose | `isAuthenticated`, `hasPermission` |
| `mgr`, `ctrl` | Unclear abbreviation | `manager`, `controller` |

## 🔵 Logic Smells

### Duplicate Code
```javascript
// 🚩 Same code in multiple places
function getUserEmail() { return db.query('SELECT email FROM users...'); }
function getUserPhone() { return db.query('SELECT phone FROM users...'); }

// ✅ Unified approach
function getUserField(field) { return db.query(`SELECT ${field} FROM users...`); }
```

### Magic Numbers
```javascript
// 🚩 Unexplained constants
if (status === 2) { ... }
discount = price * 0.15;

// ✅ Named constants
const STATUS_ACTIVE = 2;
const DISCOUNT_RATE = 0.15;
```

### Feature Envy
```javascript
// 🚩 Using another object's data excessively
invoice.getTotal(customer.getAddress().getTaxRate());

// ✅ Tell, don't ask
invoice.getTotal(customer.getTaxRate());
```

### Shotgun Surgery
```javascript
// 🚩 One change requires editing many files
// Changing discount calculation touches:
// - checkout.js
// - cart.js
// - pricing.js
// - invoice.js

// ✅ Centralize related behavior
// Discount logic in one DiscountCalculator class
```

## 🟣 OOP Smells

### God Object
```javascript
// 🚩 Class does everything
class User {
  authenticate() { }
  saveToDatabase() { }
  sendEmail() { }
  generateReport() { }
  validateInput() { }
}

// ✅ Single Responsibility
class User { /* data */ }
class UserRepository { /* database */ }
class EmailService { /* email */ }
```

### Primitive Obsession
```javascript
// 🚩 Primitive types everywhere
function createOrder(userEmail, amount, date, address) { ... }

// ✅ Use domain objects
function createOrder(order: Order) { ... }
```

### Refused Bequest
```javascript
// 🚩 Child doesn't use parent's methods
class Bird { fly() { } }
class Penguin extends Bird {
  fly() { throw new Error(); } // Refuses to fly!
}

// ✅ Proper hierarchy
class Bird { }
class FlyingBird extends Bird { fly() { } }
class Penguin extends Bird { swim() { } }
```

## 🟢 Comment Smells

### Bad Comments
```javascript
// Increment i - USELESS
i++;

// TODO: Fix this - NO TICKET
function broken() { ... }

// HACK: Don't touch - TECHNICAL DEBT
if (status == 2) { ... }
```

### Good Comments
```javascript
// Using polling instead of websockets because
// corporate firewall blocks websocket connections
pollForUpdates();

// Performance: Benchmarked at 100ms for 10k items
// vs 2s for the naive approach
optimizedAlgorithm();
```

## Quick Identification Guide

### Ask These Questions:

**Is it too long?**
- Function >30 lines → Extract methods
- Class >300 lines → Split class
- File >500 lines → Reorganize

**Is it unclear?**
- Can't understand in 30 seconds → Add comments or refactor
- Unclear name → Rename
- Complex logic → Simplify or extract

**Is it duplicated?**
- Same code 2+ places → Extract to function
- Similar but slightly different → Parameterize

**Is it wrong?**
- Security issue → Fix immediately
- Bug → Fix before merge
- Performance issue → Optimize if measured

**Is it inflexible?**
- Hard to change → Apply SOLID
- Tightly coupled → Inject dependencies
- Rigid → Add abstraction

## Severity Guide

### 🔴 BLOCK (Must fix before merge)
- Security vulnerabilities
- Data loss risks
- Critical bugs
- Breaking changes

### 🟡 REQUEST CHANGE (Should fix)
- Clear code smells
- Missing tests
- Poor structure
- Maintainability issues

### 🟢 COMMENT (Nice to have)
- Minor improvements
- Style suggestions
- Learning opportunities

## Rule of Thumb

| Metric | Good | Acceptable | Refactor |
|--------|------|------------|----------|
| Function length | <15 lines | 15-30 lines | >30 lines |
| Function parameters | 0-2 | 3 | >3 |
| Class size | <200 lines | 200-300 | >300 |
| Nesting depth | 1-2 levels | 3 levels | >3 levels |
| Cyclomatic complexity | 1-5 | 6-10 | >10 |

## When Code Smells Are OK

Sometimes code smells are acceptable:
- ✅ Prototype/POC code
- ✅ Temporary debug code (clearly marked)
- ✅ Performance-critical sections (measured!)
- ✅ Generated code
- ✅ Simple scripts (<100 lines)

But NEVER OK:
- ❌ Security vulnerabilities
- ❌ Data loss risks
- ❌ Production bugs

## Quick Actions

### During Code Review
1. Scan for long methods (>30 lines)
2. Look for duplicate code
3. Check for magic numbers
4. Verify error handling
5. Check security (secrets, SQL, XSS)

### When Writing Code
1. Keep functions small
2. Name clearly
3. Extract constants
4. Handle errors
5. Test edge cases

### When Refactoring
1. Add tests first
2. Fix one smell at a time
3. Verify tests still pass
4. Commit frequently
5. Don't change behavior

## Remember

> "Code smells are surface indications of deeper problems."

- Smell ≠ Bug (but may hide bugs)
- Smell ≠ Must fix now (use judgment)
- Smell = Warning sign (investigate)

The goal is **maintainable code**, not perfect code.

---

**Full Details:**
- [[02 - Code Smells & Anti-Patterns]]
- [[03 - Clean Code Principles]]
- [[04 - SOLID Principles]]
