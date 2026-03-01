# Core Engineering Principles

1.  **Clarity over cleverness** — Write code that’s maintainable, not impressive.
2.  **Explicit over implicit** — No magic. Make behavior obvious.
3.  **Simplicity first** — Prefer the simplest solution that works.
4.  **Minimal impact** — Change only what is necessary. Avoid ripple effects and introducing bugs..
5.  **Composition over inheritance** — Build small units that combine well.
6.  **Fail fast, fail loud** — Surface errors at the source. Never hide failures.
7.  **Verify, don’t assume** — Run it. Test it. Prove it works.
8.  **Delete code** — Less code means fewer bugs. Question every addition.
9.  **Fix root causes** — No temporary fixes. No laziness. Senior standards only.

---

## Feature Development Workflow

### Before Writing Code

1. **Clarify requirements** — Restate the goal. Ask questions if ambiguous.
2. **Identify failure modes** — Identify what can go wrong:
   - Invalid inputs
   - Missing dependencies
   - Network/IO failures
   - Concurrency issues
   - Memory Leaks / Resource exhaustion
3. **Classify work by priority**:
   - **A. Core flow**: Happy path + direct error cases
   - **B. Edge cases**: Unusual but valid scenarios
   - **C. Out of scope**: Document, don't implement
4. **Check existing code** — Is this already solved? Can you extend rather than create?

### Implementation Order

1. Write failing test for core happy path
2. Implement minimum code to pass
3. Write failing tests for core error cases
4. Implement error handling
5. Refactor if needed (tests stay green)
6. Add edge case tests only after core is solid

### Before Submitting

- [ ] All tests pass
- [ ] No commented-out code
- [ ] No TODO without context (`// TODO: [reason] description`)
- [ ] Error messages are actionable
- [ ] No secrets, credentials, or hardcoded environment-specific values
- [ ] Formatting/linting passes

---

## Debugging Process

### When Something Fails

1. **Reproduce reliably** — Can you trigger it consistently?
2. **Isolate the scope** — What's the smallest input that fails?
3. **Read the error** — Actually read it. Full stack trace.
4. **Form a hypothesis** — One specific guess about the cause
5. **Test the hypothesis** — Add logging, write a test, or inspect state
6. **Fix and verify** — Change one thing. Confirm it's fixed.
7. **Add regression test** — Ensure it can't silently break again

### Don't

- Change multiple things at once
- Assume you know the cause without evidence
- Delete error handling to "simplify"
- Fix symptoms instead of root causes

---

## Engineering Standards

### Autonomous Ownership

When given a bug or task:

- Own it fully.
- Do not stop at diagnosis.
- Resolve logs, errors, and failing tests without hand-holding.
- Minimize context switching for the user.
- Escalate only with findings and concrete hypotheses.

Drive problems to resolution.

---

### Demand Elegance

For non-trivial changes:

- Ask: is there a simpler, cleaner approach?
- Prefer structural fixes over layered patches.
- Avoid over-engineering trivial work.
- Optimize for long-term clarity.

Elegance is proportional to complexity.

---

### Continuous Improvement

After completing work or receiving corrections:

1. **Extract the lesson.**
2. **Update `tasks/lessons.md`.**
3. **Convert mistakes into explicit rules.**
4. **Prevent recurrence through tests or standards.**
5. **Review relevant lessons at session start.**

Mistakes are acceptable.  
Repeated mistakes are not.

---

## Code Organization

### File Structure

- One type per file
- Filename matches type name exactly
- Group by feature/domain, not by layer
- Shared utilities in `Core/` or `Common/` — zero domain dependencies

### Dependency Direction

```
Features → Services → Core
    ↓          ↓        ↓
   UI      Business   Utilities
           Logic
```

- `Core/` depends on nothing internal
- `Services/` depends only on `Core/`
- `Features/` depends on `Services/` and `Core/`
- No circular dependencies
- Features are deletable without breaking unrelated code

---

## Code Style

### Naming

| Type | Convention |
|------|------------|
| Types/Classes | `UpperCamelCase` |
| Functions/Variables | `lowerCamelCase` |
| Constants | `lowerCamelCase` or `SCREAMING_SNAKE_CASE` (follow language convention) |
| Protocols/Interfaces | `UpperCamelCase`, noun or adjective |

---

### If and guard statement

- Prefer direct `if` checks for simple empty/non-empty conditions.

Preferred:
```swift
if array.isEmpty {
    return
}
```

Preferred:
```swift
if !array.isEmpty {
    // Use array
}
```

- Avoid `== true` / `== false` unless required (for example with optional chaining).

Preferred:
```swift
if viewModel.isReady {
    // Continue
}
```

Allowed when required:
```swift
if viewModel?.isReady == true {
    // Continue
}
```

- Avoid `guard` with double negation for simple early returns.

Preferred:
```swift
if string.isEmpty {
    return
}
```

Not preferred:
```swift
guard !string.isEmpty else {
    return
}
```

- Prefer multiline `guard` formatting for wrapped conditions and function calls, with `guard` and `else` on dedicated lines.

Preferred:
```swift
guard
    var playQueue = try queue(
        for: playlistHashId
    )
else {
    return false
}
```

Not preferred:
```swift
guard var playQueue = try queue(for: playlistHashId) else {
    return false
}
```

---

### Functions

- Do one thing
- Name describes what, not how
- Max 3-4 parameters — beyond that, use a config object
- Avoid boolean parameters — they obscure intent at call sites
- Prefer multiline function declaration and call formatting when parameters are present.

Preferred declaration:
```swift
func testFunction(
    a: Int,
    b: Int
) {
    //Implementation
}
```

Not preferred declaration:
```swift
func testFunction(a: Int, b: Int) {
    //Implementation
}
```

Preferred call:
```swift
testFunction(
    a: 0,
    b: 1
)
```

Not preferred call:
```swift
testFunction(a: 0, b: 1)

```

#### Trailing closure syntax

- Prefer Swift trailing-closure syntax lookups.

Preferred:
```swift
queue.channels.firstIndex { $0.primaryKey == channel.primaryKey }
```

Not preferred:
```swift
queue.channels.firstIndex(where: { $0.primaryKey == channel.primaryKey })
```

#### Multiline Formatting Rule

Use multiline formatting when:

- Parameters exceed one per line
- The declaration or call wraps due to length
- Readability benefits from vertical alignment

Avoid forcing multiline formatting for trivial single-parameter declarations or short calls that fit comfortably on one line.

The goal is consistency and readability, not mechanical formatting. Multiline style should improve clarity, reduce diff noise, and make parameter intent visually obvious.

##### Preferred (single short parameter, no wrapping)
```swift
func load(id: String)
```

##### Not Preferred (forced multiline for trivial case)
```swift
func load(
    id: String
)
```

##### Preferred (multiple parameters, improved alignment)
```swift
func configure(
    with playlist: Playlist,
    user: User,
    isPreview: Bool
) {
    // Implementation
}
```

##### Not Preferred (long horizontal line reducing readability)
```swift
func configure(with playlist: Playlist, user: User, isPreview: Bool) {
    // Implementation
}
```

##### Preferred call (wrapped for clarity)
```swift
configure(
    with: playlist,
    user: currentUser,
    isPreview: true
)
```

##### Not Preferred call (wrapped but not vertically aligned)
```swift
configure(with: playlist,
          user: currentUser,
          isPreview: true)
```

---

### Comments

- Explain WHY, not WHAT
- Delete comments that restate code
- TODO format: `// TODO: [context] description`
- Document non-obvious behavior and workarounds

---

## Error Handling

### Rules

1. Define domain-specific error types per module
2. Include context: what failed, with what inputs
3. Map external errors at boundaries — don't leak implementation details
4. Fail at the source — don't pass invalid state hoping someone handles it
5. Errors are API — design them like success paths

### Error Checklist

- [ ] Message helps diagnose the problem
- [ ] Includes relevant context (IDs, paths, values)
- [ ] Caller can distinguish error types programmatically
- [ ] Transient vs permanent failures are distinguishable

---

## Testing

### Principles

- Tests are isolated: no shared state, no execution order dependencies
- One behavior per test, descriptive names: `test_build_failsWhenSchemeNotFound`
- Tests run in parallel — design for it
- Test behavior, not implementation
- Fast tests get run; slow tests get skipped

### Before Writing a Test

1. Check if the **behavior** is already tested (not just the code path — multiple tests can hit the same lines testing different behaviors)
2. Identify the mock in `Tests/Mocks/` — create only if missing
3. Identify the fixture in `Tests/Fixtures/` — create only if missing

### When Adding Features

1. **Document failure modes first** — List edge cases and failure scenarios in a comment block before writing implementation code:
   ```
   // Failure modes:
   // - Scheme not found in project
   // - Build timeout exceeded  
   // - Simulator unavailable
   // - Concurrent build in progress
   ```
2. Write tests in this order:
   - Happy path for core flow
   - Error/failure cases for core flow
   - Edge cases (only after core flow is solid)
3. Don't skip ahead — core flow must be solid before edge cases

### Test Naming

```
test_{unit}_{condition}_{expectedResult}
```

Examples:
- `test_build_failsWhenSchemeNotFound`
- `test_parse_returnsNilForMalformedJSON`
- `test_cache_fetchesNewDataAfterExpiration`

### Structure: Arrange-Act-Assert

```
func test_example() {
    // Arrange — setup preconditions
    
    // Act — execute behavior under test
    
    // Assert — verify outcomes
}
```

One blank line between sections. No other structure.

### What to Test

| Test | Don't Test |
|------|------------|
| Public interface behavior | Private implementation details |
| Error handling paths | Framework/language behavior |
| State transitions | Trivial getters/setters |
| Business logic | Third-party library internals |

### Unit vs Integration

- **Unit**: Single component, mocked dependencies, fast
- **Integration**: Multiple components, real dependencies, slower

Default to unit tests. Use integration tests for:
- Critical paths that must work end-to-end
- Complex component interactions
- External service contract validation

---

## Mocks

**Location:** `Tests/Mocks/`  
**Naming:** `Mock{ProtocolName}` (e.g., `MockBuildService`)

### Rules

1. One mock per protocol/interface
2. Mocks must be stateless or resettable between tests
3. Track all inputs received (for verification)
4. Allow stubbing return values (for control)
5. Provide `reset()` method to clear state

### Structure

```
MockBuildService
├── stubbedResult           // Control what it returns
├── receivedConfigurations  // Verify what it received
├── callCount               // Verify how many times called
└── reset()                 // Clear state between tests
```

### Don't

- Create test-specific mocks — reuse shared mocks
- Let mocks accumulate state across tests
- Mock what you don't own (wrap it first, mock the wrapper)

---

## Fixtures

**Location:** `Tests/Fixtures/`

- Static, deterministic test data
- Named descriptively: `validUser`, `expiredToken`, `malformedResponse`
- Minimal — only fields relevant to tests
- JSON/data files in `Fixtures/` folder
- Code-based fixtures in `{Type}+Fixtures` extension files

---

## Refactoring

### When to Refactor

- Before adding a feature (make the change easy, then make the easy change)
- After tests pass (not during implementation)
- When you touch code that's hard to understand

### When NOT to Refactor

- While debugging
- Without test coverage
- Unrelated to the current task
- "While I'm here" changes — make a separate commit or ticket

### How to Refactor

1. Ensure tests exist and pass
2. Make one structural change
3. Run tests
4. Repeat

Never change behavior and structure in the same step.

---

## Dependencies

### Before Adding

1. Can we solve this in <100 lines ourselves?
2. Is it actively maintained?
3. What's the transitive dependency cost?
4. What's the license?
5. What if it disappears tomorrow?

### Rules

- Wrap third-party APIs behind interfaces you control
- Pin versions explicitly
- Isolate imports to wrapper modules
- Update deliberately, not automatically

---

## Git Hygiene

### Commits

- One logical change per commit
- Present tense, imperative: "Add caching" not "Added caching"
- First line ≤50 chars, blank line, then details
- Follow GIT templates if present in the repository

### Branches

- `main` is always deployable
- Feature: `feature/{description}`
- Fix: `fix/{description}`
- Hotfix: `hotfix/{hotfix version}`
- Delete after merge

---

## Token Efficiency
- Never re-read files you just wrote or edited. You know the contents.
- Never re-run commands to "verify" unless the outcome was uncertain.
- Don't echo back large blocks of code or file contents unless asked.
- Batch related edits into single operations. Don't make 5 edits when 1 handles it.
- Skip confirmations like "I'll continue..."  Just do it.
- If a task needs 1 tool call, don't use 3. Plan before acting.
- Do not summarize what you just did unless the result is ambiguous or you need additional input.

---

## When Uncertain

1. **Check existing patterns** — How does the codebase solve similar problems?
2. **Ask** — Ambiguity is expensive. Clarify before implementing.
3. **Smallest change** — Prefer minimal diff that solves the problem.
4. **Reversibility** — Prefer changes easy to undo.
5. **Prove it** — Run the code. Pass the tests. Don't guess.


