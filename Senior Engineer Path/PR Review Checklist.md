# PR Review Checklist

Use this checklist for every code review. Not everything applies to every PR, but consider each point.

## Before You Start

- [ ] Read the PR description and linked issues
- [ ] Understand the problem being solved
- [ ] Check if this is the right approach architecturally

## 🔴 Critical (MUST block if issues found)

### Security
- [ ] No secrets, API keys, or credentials in code
- [ ] No SQL injection vulnerabilities (parameterized queries?)
- [ ] No XSS vulnerabilities (user input escaped?)
- [ ] Authentication checks present for protected routes
- [ ] Authorization checks present (can THIS user do THIS?)
- [ ] No command injection (`exec` with user input)
- [ ] No path traversal vulnerabilities
- [ ] Passwords hashed, never plaintext

### Data Safety
- [ ] No risk of data loss
- [ ] Database migrations are reversible
- [ ] Breaking changes have migration path

### Correctness
- [ ] Code does what PR claims
- [ ] Edge cases handled
- [ ] Error handling present
- [ ] No obvious bugs

## 🟡 Important (Should address before merge)

### Code Quality
- [ ] Functions are small (<30 lines ideally)
- [ ] Each function does one thing
- [ ] No duplicate code
- [ ] Names are clear and reveal intent
- [ ] No magic numbers/strings
- [ ] Code smells addressed (see [[02 - Code Smells & Anti-Patterns]])

### Design
- [ ] Follows SOLID principles (see [[04 - SOLID Principles]])
- [ ] Proper abstraction levels
- [ ] Appropriate design patterns used
- [ ] Not over-engineered
- [ ] Not under-engineered

### Error Handling
- [ ] All errors are handled
- [ ] Error messages are helpful
- [ ] No swallowed exceptions
- [ ] Proper logging in place

### Testing
- [ ] Critical paths are tested
- [ ] Tests are meaningful (not just for coverage)
- [ ] Edge cases tested
- [ ] Tests are maintainable
- [ ] Tests will catch regressions

### Performance
- [ ] No obvious performance issues
- [ ] No N+1 query problems
- [ ] Appropriate data structures used
- [ ] No unnecessary loops/iterations
- [ ] Async operations handled properly

### Dependencies
- [ ] New dependencies are justified
- [ ] Dependencies are up to date
- [ ] No known vulnerable dependencies

## 🟢 Nice to Have (Approve with comments)

### Documentation
- [ ] Complex logic has comments explaining WHY
- [ ] Public APIs are documented
- [ ] README updated if needed
- [ ] Breaking changes documented

### Readability
- [ ] Code is self-documenting
- [ ] Consistent with codebase style
- [ ] No unnecessary complexity
- [ ] Positive conditionals used

### Maintainability
- [ ] Easy to understand
- [ ] Easy to change
- [ ] Easy to test
- [ ] Future-friendly

## Language-Specific Checks

### JavaScript/TypeScript
- [ ] Proper async/await usage
- [ ] No callback hell
- [ ] Promises handled correctly
- [ ] TypeScript types are meaningful (not `any`)
- [ ] No `==`, always `===`

### Python
- [ ] PEP 8 compliance
- [ ] Proper exception handling
- [ ] Context managers used for resources
- [ ] List comprehensions not too complex

### Java
- [ ] Proper exception handling
- [ ] Resources closed (try-with-resources)
- [ ] Null checks where needed
- [ ] Proper use of streams

### Go
- [ ] Errors checked
- [ ] Defer used for cleanup
- [ ] Goroutines don't leak
- [ ] Proper context usage

## Questions to Ask

### Architecture
- Does this fit with existing design?
- Is this in the right place?
- Are we creating the right abstractions?

### Impact
- Who is affected by this change?
- What's the blast radius if this breaks?
- Do we need a feature flag?

### Testing
- How would we test this in production?
- What metrics should we monitor?
- Can we roll this back easily?

### Alternatives
- Is there a simpler approach?
- Have we considered other solutions?
- What are the trade-offs?

## Review Sizes

### Small PR (<100 lines)
- ⏱️ Time: 10-15 min
- Focus: Correctness, security, tests

### Medium PR (100-300 lines)
- ⏱️ Time: 20-30 min
- Focus: Architecture, design patterns, full checklist

### Large PR (300-500 lines)
- ⏱️ Time: 45-60 min
- Consider: Ask for split into smaller PRs

### Extra Large PR (>500 lines)
- 🚨 Request breakdown into smaller PRs
- Or schedule live review session

## Giving Feedback

### Use Prefixes
- `❌ BLOCK:` Critical issue preventing merge
- `⚠️ CONCERN:` Should be addressed
- `💡 SUGGESTION:` Nice to have
- `❓ QUESTION:` Seeking clarification
- `🎓 LEARNING:` Teaching moment
- `✨ PRAISE:` Something done well

### Example Comments

#### Good
```
❌ BLOCK: SQL injection vulnerability
This query is vulnerable to SQL injection. Please use
parameterized queries instead:
  db.query('SELECT * FROM users WHERE id = ?', [userId])
```

```
💡 SUGGESTION: Extract this logic
This function is doing multiple things. Consider extracting
the validation logic into a separate `validateInput()` function.
This would make it easier to test and reuse.
```

```
❓ QUESTION: Error handling
What happens if the API call fails? Should we retry or
surface the error to the user?
```

#### Bad
```
"This is wrong." (No explanation)
"We don't do it this way." (No reasoning)
"nit: rename this" (Blocking on style)
```

## After Your Review

- [ ] Left specific, actionable feedback
- [ ] Explained the "why" behind suggestions
- [ ] Found at least one thing to praise
- [ ] Categorized feedback (block/concern/suggestion)
- [ ] Checked your tone (is it helpful?)

## When to Approve

✅ Approve when:
- No critical issues
- Minor issues can be addressed in follow-up
- You trust the author to handle comments

## When to Block

❌ Request changes when:
- Security vulnerabilities
- Critical bugs
- Breaking changes without plan
- Major design issues
- Missing critical tests

## Remember

- **Be kind** - You're reviewing code, not the person
- **Be specific** - Point to exact lines, suggest alternatives
- **Be timely** - Review within 24 hours
- **Be thorough** - Don't rush, quality matters
- **Be consistent** - Apply same standards to everyone

> "The code you write makes you a programmer. The code you review makes you a senior engineer."

---

**Related:**
- [[01 - Code Review Fundamentals]]
- [[08 - Security in Code Reviews]]
- [[Code Quality Checklist]]
