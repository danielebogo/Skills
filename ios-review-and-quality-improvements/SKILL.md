---
name: ios-review-and-quality-improvements
description: Staff-level iOS code review and quality improvements for Swift, SwiftUI, Core Data, and Swift Concurrency changes. Use when asked to review branch changes, route framework-specific analysis through the installed expert skills, and propose a plan to finalize changes, reduce tech debt, and improve code quality.
---

# iOS Code Review and Quality Improvements

## Workflow

- Act as a Staff iOS Engineer reviewing the code changes in the current branch.
- Ask the user to confirm the branch name before starting the review.
- Inspect the branch diff and identify which specialized domains are present before forming conclusions.
- Load and follow each relevant expert skill completely before analyzing that domain:
  - Core Data changes: use `core-data-expert` from `/Users/danielebogo/.agents/skills/core-data-expert/SKILL.md`.
  - SwiftUI changes: use `swiftui-expert:swiftui-expert-skill` from `/Users/danielebogo/.codex/plugins/cache/openai-curated-remote/swiftui-expert/4.2.0/skills/swiftui-expert-skill/SKILL.md`.
  - Swift Concurrency changes: use `swift-concurrency:swift-concurrency` from `/Users/danielebogo/.codex/plugins/cache/openai-curated-remote/swift-concurrency/2.3.0/skills/swift-concurrency/SKILL.md`.
- Load only the expert skills relevant to the diff. Use both `core-data-expert` and `swift-concurrency:swift-concurrency` when Core Data work crosses actor, task, context, or `Sendable` boundaries.
- Apply each expert skill's routing instructions and read only the references relevant to the reviewed changes.
- Identify correctness risks, regressions, performance concerns, and missing tests.
- Consolidate the specialized findings into one review instead of returning separate domain reports.
- Provide a concrete plan to finalize the changes, reduce tech debt, and improve code quality.

## Output Expectations

- Summarize key findings and risks, prioritized by severity.
- Call out test gaps and propose focused test additions.
- Provide a step-by-step plan with owners or next actions where possible.
