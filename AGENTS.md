# Core Engineering Principles

1.  **Clarity over cleverness** — Write code that’s maintainable, not impressive.
2.  **Explicit over implicit** — No magic. Make behavior obvious.
3.  **Simplicity first** — Prefer the simplest solution that works.
4.  **Minimal impact** — Change only what is necessary. Avoid ripple effects and introducing bugs..
5.  **Composition over inheritance** — Build small units that combine well.
6.  **Fail fast, fail loud** — Surface errors at the source. Never hide failures.
7.  **Verify, don’t assume** — Run it. Test it. Prove it works.
8.  **Never modify** files you did not create unless the task explicitly requires it.
9.  **Delete code** — Less code means fewer bugs. Question every addition.
10.  **Fix root causes** — No temporary fixes. No laziness. Senior standards only.

---

## Scoped Implementation Guidance

When a task involves implementing, debugging, or reviewing code, read [docs/engineering-guidelines.md](docs/engineering-guidelines.md) for the detailed workflow, coding conventions, testing, and dependency guidance.

## Engineering Standards

### Autonomous Ownership

When given a bug or task:

- Own it fully.
- Do not stop at diagnosis.
- Resolve logs, errors, and failing tests without hand-holding.
- Minimize context switching for the user.
- Escalate only with findings and concrete hypotheses.
- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions).
- Use plan mode for verification steps, not just building.

Drive problems to resolution.

---

### Subagent Strategy

- Use subagents liberally to keep main context window clean
- Offload research, exploration, and parallel analysis to subagents
- For complex problems, throw more compute at it via subagents
- One task per subagent for focused execution

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

## Scoped Code Guidance

Detailed file organization, Swift style, error handling, tests, mocks, fixtures, refactoring, and dependency guidance is in [docs/engineering-guidelines.md](docs/engineering-guidelines.md). Load it when the task touches code.

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

- Search before reading; read only the relevant ranges. Reuse context already read. Re-read when a file has changed, earlier output was incomplete, or verification requires it.
- When validation is requested or required by the repository, start with the narrowest relevant check. Broaden it as the results and task risk warrant; run the full suite once at the end when appropriate.
- For long-running commands, prefer one supported wait or watch operation over repeated sleep-and-check steps.
- Keep command output compact. Use existing summaries or filters while preserving exit status, warnings, errors, and failing test details. Read full logs when needed to diagnose a failure.
- For Apple UI tasks, inspect the accessibility hierarchy first; use screenshots to check visual details the hierarchy cannot show.
- Load the best-fit skill. Add another only when it provides distinct or required guidance; avoid loading the same skill twice.
- Delegate bounded, independent searches when parallelism or context isolation is useful. Use a lightweight model for exploration when available.
- Lead with the result and skip routine tool narration. The final report should still state changes and requested validation, including failures or unverified results.
- After two failed attempts at the same fix, stop repeating it without new evidence. Summarize the findings, update the hypothesis, and choose the next diagnostic step.

---

## When Uncertain

1. **Check existing patterns** — How does the codebase solve similar problems?
2. **Ask** — Ambiguity is expensive. Clarify before implementing.
3. **Smallest change** — Prefer minimal diff that solves the problem.
4. **Reversibility** — Prefer changes easy to undo.
5. **Prove it** — Run the code. Pass the tests. Don't guess.
