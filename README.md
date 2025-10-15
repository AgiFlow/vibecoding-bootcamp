# Vibecoding Bootcamp

[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL%203.0-blue.svg)](https://opensource.org/licenses/AGPL-3.0)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Made with AI](https://img.shields.io/badge/Made%20with-AI-orange.svg)](https://github.com/AgiFlow/aicode-toolkit)

> Learn to build software effectively using AI coding assistants

A comprehensive, tool-agnostic bootcamp for developers who want to master "vibecoding" - using AI assistants like Claude Code, Cursor, or GitHub Copilot to build software faster and better.

## Bootcamp Structure

The bootcamp is organized into **3 chapters**, progressing from simple to complex:

### [Chapter 1: Bug Fixes](./chapters/01-bugfixes/)
**Time:** ~30-40 minutes | **Lessons:** 5 | **Duration:** 5-10 mins each

Learn systematic debugging with AI assistants.

**Lessons:**
1. Simple Logic Error
2. Null/Undefined Handling
3. Type Error Bug
4. Off-by-One Error
5. UI Event Handler Bug

---

### [Chapter 2: Feature Enhancements](./chapters/02-feature-enhancements/)
**Time:** ~2.5 hours | **Lessons:** 5 | **Duration:** 30 mins each

Master common, high-impact enhancements that professional developers make.

**Lessons:**
1. Add Authentication
2. Database Migration for New Field
3. Refactor Duplicate Code
4. Add Tests (Unit + Integration)
5. Security Enhancement

---

### [Chapter 3: Greenfield Projects](./chapters/03-greenfield-project/)
**Time:** 2-3 hours | **Lessons:** 1 comprehensive project

Build a complete application from scratch.

**Main Project:**
1. Leave Request System (full-stack app with auth, API, database, UI)

---

## Learning Paths

### Beginner Path
**Goal:** Build confidence in reading and modifying code

1. Chapter 1: All 5 bug fix lessons (~40 mins)
2. Chapter 2: Lesson 1 (Authentication)
3. Chapter 2: Lesson 4 (Testing)

**Total Time:** ~2 hours

---

### Intermediate Path
**Goal:** Learn to build features and complete apps

1. Chapter 1: Complete (~40 mins)
2. Chapter 2: Complete (~2.5 hours)
3. Chapter 3: Leave Request System (2-3 hours)

**Total Time:** ~4-5 hours

---

### Advanced Path
**Goal:** Master production-ready development

1. All lessons from Beginner & Intermediate
2. Apply lessons to your own projects
3. Extend the leave request system with:
   - Admin approval workflow
   - Email notifications
   - Team calendar view
   - Deployment to production

**Total Time:** 10+ hours with projects

---

## Tool Independence

These lessons work with **any** AI coding assistant:
- Claude Code
- Cursor
- GitHub Copilot
- Gemini CLI
- Windsurf
- Any other AI coding tool

## Methodology Independence

Complete lessons using:
- **No spec framework** - Just describe what you want
- **OpenSpec** - Structured spec-driven development
- **SpecKit** - GitHub's spec-driven workflow
- **Any methodology** - Lessons adapt to your approach

---

## Quick Start

1. **Choose your AI assistant** (Claude Code, Cursor, etc.)
2. **Choose the methodology** (Spec driven or None)
3. **Start with Chapter 1** - Quick wins with bug fixes
4. **Follow the prompts** - Each lesson includes sample prompts
5. **Build real things** - Apply to your own projects

---

## What You'll Learn

### Core Skills
- Debugging with AI assistance
- Enhancing existing codebases
- Building applications from scratch
- Writing tests and maintaining quality
- Security best practices

### Technical Topics
- Authentication & authorization
- Database migrations
- API development
- Code refactoring
- Input sanitization
- Testing strategies

### Soft Skills
- Working effectively with AI assistants
- Breaking down complex problems
- Iterative development
- Understanding code patterns
- Making architectural decisions

---

## Bootcamp Features

### Practical & Hands-On
Every lesson includes working code examples and real scenarios.

### Tool-Agnostic
Works with any AI coding assistant - not locked to one tool.

### Progressive Difficulty
Start simple (5-min bug fixes) → Build to complex (full apps).

### Time-Boxed
Clear time estimates for each lesson (5 mins to 90 mins).

### Sample Prompts
Each lesson includes proven prompts to use with AI assistants.

### Success Criteria
Clear checklists to verify you've completed each lesson.

---

## Repository Structure

```
vibecoding-bootcamp/
├── README.md (this file)
├── LESSONS.md (detailed bootcamp overview)
├── playgrounds/
│   ├── typescript/
│   │   ├── 01-bugfixes-01/ (Simple Logic Error)
│   │   ├── 01-bugfixes-02/ (Null/Undefined Handling)
│   │   ├── 01-bugfixes-03/ (Type Error Bug)
│   │   ├── 01-bugfixes-04/ (Off-by-One Error)
│   │   ├── 01-bugfixes-05/ (UI Event Handler Bug)
│   │   ├── 02-feature-request-01/ (Add Authentication)
│   │   ├── 02-feature-request-02/ (Database Migration)
│   │   ├── 02-feature-request-03/ (Refactor Duplicate Code)
│   │   ├── 02-feature-request-04/ (Add Tests)
│   │   ├── 02-feature-request-05/ (Security Enhancement)
│   │   └── 03-greenfield-01/ (Leave Request System)
│   ├── python/
│   │   ├── 01-bugfixes-01/
│   │   ├── 02-feature-request-01/
│   │   └── ... (same lesson structure)
│   └── go/
│       ├── 01-bugfixes-01/
│       └── ... (same lesson structure)
├── templates/ (boilerplates for different languages)
```

---

## Related Projects

- **[AI Code Toolkit](https://github.com/AgiFlow/aicode-toolkit)** - MCP servers for scaffolding, architecture patterns, and validation rules. Use these tools to enforce consistency as you learn vibecoding.
---

## Getting Started

```bash
# Clone and install
git clone https://github.com/AgiFlow/vibecoding-bootcamp.git
cd vibecoding-bootcamp
pnpm install
```

---

## Contributing

Have ideas for new lessons or improvements? Contributions welcome!

See [CONTRIBUTING.md](./CONTRIBUTING.md) for detailed guidelines and individual chapter READMEs for backlog lessons we're planning to add.

---

## License

[AGPL-3.0](./LICENSE) © 2025

See [LICENSE](./LICENSE) file for details.

---

## Support

- **Issues**: Report bugs or request features in GitHub Issues
- **Discussions**: Ask questions in GitHub Discussions

---

**Built for developers learning to work effectively with AI coding assistants.**
