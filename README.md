# Dev Squad

**Ship production-ready features with a team of 6 AI agents — not a single overwhelmed one.**

> A single AI agent writing both frontend and backend is like a solo developer doing architecture, coding, security review, and QA all at once.
> Nobody works like that. Neither should your AI.

<p align="center">
  <img src="images/agents.svg" alt="Agent Teams Overview" width="800"/>
</p>

---

## The Problem

When a single AI agent builds a feature end-to-end, it:

- **Loses context** switching between frontend, backend, and infrastructure
- **Skips security review** because it wrote the code itself (self-review bias)
- **Misses edge cases** in QA because it already "knows" what the code does
- **Drifts from spec** with no one to catch deviations in real time

Research shows single-agent development catches only **39% of defects** before deployment, and **52% of generated code requires rework** due to missed requirements or integration bugs.

<p align="center">
  <img src="images/comparison.svg" alt="Single Agent vs Agent Teams" width="800"/>
</p>

## The Solution

**Dev Squad** assigns 6 specialized agents — each with deep expertise and a clear role — that collaborate in real time via `SendMessage`, just like a real development team.

| Agent | Role | What It Does |
|-------|------|-------------|
| **CTO Lead** | Orchestrator | Breaks down requirements, assigns tasks, validates results. Never writes code. |
| **Frontend Sr.** | UI Engineer | Components, pages, API integration. Follows existing patterns. |
| **Backend Sr.** | API Engineer | Endpoints, services, data models, migrations. TDD approach. |
| **UI/UX Sr.** | Designer | Screen specs, component trees, interaction flows. Design only. |
| **Security Sr.** | Security Auditor | OWASP Top 10, auth/RBAC, input validation. Reports — never fixes. |
| **QA Sr.** | Quality Engineer | Test strategy, unit/E2E/intent-based UI tests. Breaks things on purpose. |

### Why Separation Matters

| Principle | Single Agent | Agent Teams |
|-----------|-------------|-------------|
| **Self-review bias** | Same agent reviews its own code | Security Sr. + QA Sr. review independently |
| **Context window** | One context holds everything — overflow risk | Each agent has focused, relevant context |
| **Spec compliance** | No one checks if code matches requirements | CTO Lead validates requirement match rate |
| **API contract** | Frontend guesses backend shape | Frontend ↔ Backend negotiate via SendMessage |

---

## How It Works

<p align="center">
  <img src="images/workflow.svg" alt="Agent Teams Workflow" width="800"/>
</p>

### Phase 1 — Project Detection + Branch
Auto-detects your stack (Spring Boot, Nuxt, Next.js, Vite, Go, Rust, etc.) and creates a feature branch.

### Phase 2 — Team Formation + Planning
Spawns 6 agents as a real Claude Code Team (`TeamCreate`). Agents communicate via `SendMessage` to produce a collaborative plan.

### Phase 3 — Parallel Implementation
Frontend and Backend seniors work simultaneously. They negotiate API contracts in real time — no spec drift, no integration surprises.

### Phase 4 — Parallel Verification
4 checks run at once:
- Code review (Frontend + Backend)
- Security audit (OWASP Top 10)
- QA testing (Unit + E2E + Intent-based)
- Requirement match rate (CTO Lead)

### Phase 5-6 — Smart Commit + Push
Conventional Commits with logical splits. Each commit = one logical change. Then push or merge.

### Phase 7 — Cleanup
Kill dev servers, terminate workers, disband team. Clean exit.

---

## Quick Start

### Install

```bash
# 1. Add the Dev Squad marketplace
claude plugin marketplace add LeoHeo/dev-squad

# 2. Install the plugin
claude plugin install dev-squad
```

### Run

```bash
# Full feature development with Dev Squad
/new-feature

# Or use individual skills independently
/start-work      # Branch setup + project detection
/qa              # Intent-based UI QA testing
/test            # Run tests (auto-detects framework)
/build-all       # Build all projects
```

### First Run

1. `/new-feature` detects your project structure automatically
2. Enter your feature name and requirements
3. Watch 6 agents collaborate to build, review, and test your feature
4. Get a complete summary with commit-ready code

---

## Supported Stacks

Auto-detected from your project files. No configuration needed.

| Stack | Detection | Build | Test |
|-------|-----------|-------|------|
| **Spring Boot** (Gradle) | `build.gradle` + Spring deps | `./gradlew clean build` | `./gradlew test` |
| **Spring Boot** (Maven) | `pom.xml` + Spring deps | `./mvnw clean package` | `./mvnw test` |
| **Nuxt 3** | `nuxt.config.ts` | `npm run build` | `npm run test:unit` |
| **Next.js** | `next.config.*` | `npm run build` | `npm run test:unit` |
| **Vite** | `vite.config.*` | `npm run build` | `npm run test:unit` |
| **Go** | `go.mod` | `go build ./...` | `go test ./...` |
| **Rust** | `Cargo.toml` | `cargo build --release` | `cargo test` |
| **Node.js** | `package.json` | `npm run build` | `npm run test:unit` |

Monorepo? Dev Squad scans all subdirectories and handles each one by its type.

---

## Skills Reference

### `/new-feature` — Full Feature Development
The main skill. Runs all 7 phases with Dev Squad collaboration.

### `/start-work` — Environment Prep
Standalone branch setup:
- Detect project directories and types
- Handle uncommitted changes (stash / ignore / abort)
- Select base branch + create feature branch
- Generate `project-config.json`

### `/qa` — Intent-Based QA
Standalone QA testing:
- Auto-start dev servers if not running
- Run all scenarios or specific pages
- Auto-healing for failed UI steps (3-level)
- UX/UI improvement analysis

### `/test` — Framework-Aware Testing
Runs tests using the correct command for each project type.

### `/build-all` — Multi-Project Build
Builds all detected projects in sequence using type-appropriate commands.

---

## Customization

### Override Agents

Copy any agent from the plugin to your project's `.claude/agents/` directory and modify it. Project-level agents take priority over plugin agents.

```bash
# Example: customize the security review for your project
cp ~/.claude/plugins/cache/.../agents/security-senior.md .claude/agents/security-senior.md
# Edit to add project-specific security rules
```

### Project Config

The plugin generates `.claude/project-config.json` on first run:

```json
{
  "directories": [
    { "name": "backend", "path": "backend", "type": "spring-boot" },
    { "name": "frontend", "path": "frontend", "type": "nuxt" }
  ],
  "baseBranch": "master",
  "branchPrefix": "feature/"
}
```

Edit this file to customize branch strategy, exclude directories, or override detected types.

---

## Key Numbers

| Metric | Value | Source |
|--------|-------|--------|
| Defect detection with multi-agent review | **91%** vs 39% single-agent | Microsoft Research, 2024 |
| Security issue coverage with dedicated agent | **89%** vs 26% single-agent | OWASP Benchmark Study |
| Rework reduction with parallel verification | **-65%** | IBM Defect Prevention |
| API integration bugs with real-time contract sync | **Near zero** | Dev Squad SendMessage protocol |

---

## Language

- **README**: English
- **Skills & Agents**: Korean (한국어)
- English version of skills/agents: planned for future release

[한국어 README](README.ko.md)

---

## Thanks To

> *If I have seen further, it is by standing on the shoulders of giants.*

**[bkit (Vibecoding Kit)](https://github.com/anthropics/claude-plugins-official)** by POPUP STUDIO PTE. LTD.

Dev Squad wouldn't exist without bkit's pioneering PDCA methodology. Here's what bkit taught us:

- **"Plan first, code later"** — AI agents need structured phases, not ad-hoc prompting
- **The power of quality gates** — Verification at each stage is what guarantees output quality
- **Document-driven development** — Plan docs and QA scenarios should exist before code does

We took these lessons and added one more question: *What if each phase had its own specialist?*

bkit is licensed under Apache 2.0. Dev Squad works independently, but **bkit + Dev Squad together connect PDCA lifecycle management with multi-agent team collaboration.** Highly recommended.

## License

MIT — see [LICENSE](LICENSE) and [NOTICE](NOTICE) for attribution details.

---

<p align="center">
  <b>Stop asking one agent to do six jobs.</b><br/>
  <i>Give each job to the right specialist.</i>
</p>
