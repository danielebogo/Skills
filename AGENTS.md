# Workflow Orchestration

### 1. Plan Mode Default
- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions)
- If something goes sideways, STOP and re-plan immediately – don’t keep pushing
- Use plan mode for verification steps, not just building
- Write detailed specs upfront to reduce ambiguity

### 2. Subagent Strategy
- Use subagents liberally to keep main context window clean
- Offload research, exploration, and parallel analysis to subagents
- For complex problems, throw more compute at it via subagents
- One task per subagent for focused execution

### 3. Self-Improvement Loop
- After ANY correction from the user: update `tasks/lessons.md` (or create it first if doesn't exist) with the pattern
- Write rules for yourself that prevent the same mistake
- Ruthlessly iterate on these lessons until mistake rate drops
- Review lessons at session start for relevant project

### 4. Verification Before Done
- Never mark a task complete without proving it works
- Diff behavior between main and your changes when relevant
- Ask yourself: "Would a staff engineer approve this?"
- Run tests, check logs, demonstrate correctness

### 5. Demand Elegance (Balanced)
- For non-trivial changes: pause and ask "is there a more elegant way?"
- If a fix feels hacky: "Knowing everything I know now, implement the elegant solution"
- Skip this for simple, obvious fixes – don’t over-engineer
- Challenge your own work before presenting it

### 6. Autonomous Bug Fixing
- When given a bug report: just fix it. Don’t ask for hand-holding
- Point at logs, errors, failing tests – then resolve them
- Zero context switching required from the user
- Go fix failing CI tests without being told how

---

# Task Execution Protocol

This section operationalizes Workflow Orchestration into file-based task tracking.

1. **Plan First**  
   Write plan to `tasks/todo.md` with checkable items.

2. **Verify Plan**  
   Check in before starting implementation.

3. **Track Progress**  
   Mark items complete as you go.

4. **Explain Changes**  
   Provide a high-level summary at each step.

5. **Document Results**  
   Add a review section to `tasks/todo.md`.

6. **Capture Lessons**  
   Update `tasks/lessons.md` after corrections (reinforces the Self-Improvement Loop above).

---

# Core Engineering Principles

- **Simplicity First**  
  Make every change as simple as possible. Impact minimal code.

- **Minimal Impact**  
  Changes should only touch what is necessary. Avoid introducing bugs.

- **No Laziness**  
  Find root causes. No temporary fixes. Senior developer standards.

- **Clarity Over Cleverness**  
  Prefer readability and explicitness over abstraction or trickiness.

---

# Agent Skill Usage

Skills are globally stored in `~/.agents/skills`. Always check if the Skills are present in the repository `.agents/skills` folder.
When operating prefer the following skills where applicable:

- Use the `app-store-changelog` skill when generating App Store release notes from git history or tags.
- Use the `core-data-expert` skill for advanced CoreData guidance and review.
- Use the `ios-review-and-quality-improvements` skill for iOS review and quality improvements.
- Use the `swift-concurrency` skill for async/await, actors, data races, Swift 6 concurrency warnings, or concurrency refactors.
- Use the `swiftui-liquid-glass` skill when adopting or reviewing iOS 26+ Liquid Glass APIs in SwiftUI.
- Use the `swiftui-animation` skill for SwiftUI animation design and implementation.
- Use the `swiftui-expert-skill` skill for advanced SwiftUI guidance and reviews.
- Use the `swift-testing-expert` skill for writing and refactoring unit tests.

---

# Coding Standards

## Commit & Pull Request Guidelines

- Commit messages are short, imperative, and scoped to the change (e.g., "Fix compile error on iOS").
- PRs should include a clear summary, testing notes, and screenshots for UI-facing changes.
- Link relevant issues or feature requests when applicable.

---

## General Rules

- Don't modify files if they are not requested or not related to the request.
- Always ask before modifying anything that is not directly related to the request.
- Platform: iOS / tvOS; UIKit with some SwiftUI; Swift + Objective-C interop.
- Prefer Swift/SwiftUI for all new work; UIKit is acceptable where SwiftUI is not in use.
- Do not add new Objective-C files unless required for bridging, legacy integration, or platform constraints.
- Objective-C legacy code: bug fixes only unless a Swift change requires parity.
- Follow existing conventions and avoid new dependencies without justification.
- Document public APIs, cross-module types, and non-obvious logic with English doc comments.
- Match the surrounding file style; the codebase mixes Swift and Objective-C.

---

## Naming Conventions

- Use UpperCamelCase for types.
- Use lowerCamelCase for methods and properties.
- Keep file names aligned with class/struct names.

---

## Swift Formatting Conventions

- Prefer Swift trailing-closure syntax for `firstIndex` lookups.

Preferred:
```swift
queue.channels.firstIndex { $0.primaryKey == channel.primaryKey }
```

Not preferred:
```swift
queue.channels.firstIndex(where: { $0.primaryKey == channel.primaryKey })
```

---

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

---

### Multiline Formatting Rule

Use multiline formatting when:

- Parameters exceed one per line
- The declaration or call wraps due to length
- Readability benefits from vertical alignment

Avoid forcing multiline formatting for trivial single-parameter declarations or short calls that fit comfortably on one line.

The goal is consistency and readability, not mechanical formatting. Multiline style should improve clarity, reduce diff noise, and make parameter intent visually obvious.

#### Preferred (single short parameter, no wrapping)
```swift
func load(id: String)
```

#### Not Preferred (forced multiline for trivial case)
```swift
func load(
    id: String
)
```

#### Preferred (multiple parameters, improved alignment)
```swift
func configure(
    with playlist: Playlist,
    user: User,
    isPreview: Bool
) {
    // Implementation
}
```

#### Not Preferred (long horizontal line reducing readability)
```swift
func configure(with playlist: Playlist, user: User, isPreview: Bool) {
    // Implementation
}
```

#### Preferred call (wrapped for clarity)
```swift
configure(
    with: playlist,
    user: currentUser,
    isPreview: true
)
```

#### Not Preferred call (wrapped but not vertically aligned)
```swift
configure(with: playlist,
          user: currentUser,
          isPreview: true)
```

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

---

## Concurrency Rules

- Be explicit about threading and concurrency (actors, `async/await`).
- Prefer structured concurrency.
- Avoid hidden thread hops.
- Do not mix runtime device checks with compile-time target macros.
- When reviewing SwiftUI code, refer to `~/.agents/SWIFTUI_GUIDE.md` and use `swiftui-expert-skill`.

---

## Realm Rules

- Realm `Object`s are thread-confined; never pass live objects across threads.
- For cross-thread access: pass primary keys + re-fetch, or use `freeze()` for read-only.
- All mutations of persisted objects must happen inside `try realm.write { }`.
- Temporary/in-memory objects must not be wrapped in `ThreadSafeReference`.

---

## Cross-Platform Target Macros

The app supports both iOS and tvOS. Some code should compile for only one target. Use the correct compile-time macros whenever you add or modify platform-specific code.

Swift:
```
#if TARGET_IOS
// iOS-only code
#else
// tvOS-only code
#endif
```

```
#if TARGET_TVOS
// tvOS-only code
#else
// iOS-only code
#endif
```

Objective-C:
```
#ifdef TARGET_IOS
// iOS-only code
#else
// tvOS-only code
#endif
```

```
#ifdef TARGET_TVOS
// tvOS-only code
#else
// iOS-only code
#endif
```

Note: If the change applies to only one target, omit `#else` and keep just the single platform block.

---

# Top 5 Structural Improvements Suggested

1. Clarify the relationship between Workflow Orchestration and Task Execution Protocol to reduce perceived duplication.
2. Explicitly reinforce that "Capture Lessons" operationalizes the Self-Improvement Loop rather than introducing a separate concept.
3. Group Coding Standards into logical subsections (General, Naming, Formatting, Concurrency, Persistence, Platform).
4. Clarify when multiline formatting is required versus optional to avoid pathological formatting in trivial cases.
5. Separate behavioral rules from formatting rules to keep Coding Standards focused strictly on code-level consistency.
