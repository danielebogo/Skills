# Skills
My personal collections of Skills

## AGENTS.md
- Inspired by ([@afterxleep](https://github.com/afterxleep/agents))

## Personal
- ios-review-and-quality-improvements ([@twannl](https://x.com/twannl/status/2019016628676030935?s=20))
- plan-prompt-grill ([@LLMJunky](https://x.com/LLMJunky/status/2019079131284066656?s=20))

## Plugins

### Apple Xcode Skills
- Local Codex plugin bundling the Apple development skills exported from Xcode.
- Manifest: `Plugins/apple-xcode-skills/.codex-plugin/plugin.json`
- Skills: `Plugins/apple-xcode-skills/skills/`

## Third Party

### ASC
- [ASC](https://asccli.sh/)
```bash
brew install asc
```
```bash
asc install-skills
```

### Point-Free
- [Point-Free Way](https://github.com/pointfreeco/pfw)
```bash
brew install pointfreeco/tap/pfw
```
```bash
pfw login
```
```bash
pfw install --tool agents
```

### Egonex-AI
- [Understand Anything](https://github.com/Egonex-AI/Understand-Anything)
```bash
curl -fsSL https://raw.githubusercontent.com/Egonex-AI/Understand-Anything/main/install.sh | bash -s codex
```

### Emil Kowalski
- [Skills for Designers and Engineers](https://github.com/emilkowalski/skills)
```bash
npx skills@latest add emilkowalski/skills
```

### Steipete
- [Skills](https://github.com/steipete/agent-scripts/skills)
  - codex-review
  ```bash
  npx skills add https://github.com/steipete/agent-scripts --skill codex-review
  ```
  - codex-debugging
  ```bash
  npx skills add https://github.com/steipete/agent-scripts --skill codex-debugging
  ```

### Dimillian
- [Skills](https://github.com/dimillian/skills)
  - app-store-changelog
  ```bash
  npx skills add https://github.com/dimillian/skills --skill app-store-changelog
  ```
  - swiftui-liquid-glass
  ```bash
  npx skills add https://github.com/dimillian/skills --skill swiftui-liquid-glass
  ```
  - swiftui-performance-audit
  ```bash
  npx skills add https://github.com/dimillian/skills --skill swiftui-performance-audit
  ```

### AvdLee
- [swiftui-expert-skill](https://github.com/AvdLee/SwiftUI-Agent-Skill)
```bash
npx skills add https://github.com/avdlee/swiftui-agent-skill --skill swiftui-expert-skill
```
- [swift-concurrency](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill)
```bash
npx skills add https://github.com/avdlee/swift-concurrency-agent-skill --skill swift-concurrency
```
- [core-data-expert](https://github.com/AvdLee/Core-Data-Agent-Skill)
```bash
npx skills add https://github.com/avdlee/core-data-agent-skill --skill core-data-expert
```
- [swift-testing-expert](https://github.com/AvdLee/Swift-Testing-Agent-Skill)
```bash
npx skills add https://github.com/avdlee/swift-testing-agent-skill --skill swift-testing-expert
```

### TwoStraws
- [swift-testing-agent-skill](https://github.com/twostraws/Swift-Testing-Agent-Skill)
```bash
npx skills add https://github.com/twostraws/swift-testing-agent-skill --skill swift-testing-pro
```

### Jamesrochabrun
- [swiftui-animation](https://github.com/jamesrochabrun/skills/tree/main/skills/swiftui-animation)
- [leetcode-teacher](https://github.com/jamesrochabrun/skills/tree/main/skills/leetcode-teacher)

### Erikote04
- [swift-api-design-guidelines-skill](https://github.com/Erikote04/Swift-API-Design-Guidelines-Agent-Skill)
```bash
npx skills add https://github.com/Erikote04/Swift-API-Design-Guidelines-Agent-Skill --skill swift-api-design-guidelines-skill
```

### XCDocs
- [XCDocs](https://github.com/BitrigApp/XCDocs)
```bash
brew install BitrigApp/tap/xcdocs
```
- Clone the repo and symlink the `[PATH_TO_THE_REPO]/.agents/skills/xcdocs to ~/.agents/skills/xcdocs` 
```bash
ln -sfn [PATH_TO_THE_REPO]/.agents/skills/xcdocs ~/.agents/skills/xcdocs
```

### Data
- [Axiom - Data](https://github.com/charleswiltgen/axiom)
```bash
npx skills add https://github.com/charleswiltgen/axiom --skill axiom-data
```

### TvOS
- [Mhaviv - Focus](https://github.com/mhaviv/Swift-FocusEngine-Agent-Skill)
```bash
npx skills add https://github.com/mhaviv/Swift-FocusEngine-Agent-Skill --skill swift-focusengine-pro --agent codex
```
