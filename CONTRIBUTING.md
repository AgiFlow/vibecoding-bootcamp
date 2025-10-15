# Contributing to Vibecoding Training

Thank you for your interest in contributing to the Vibecoding Training repository! This guide will help you contribute effectively.

## Table of Contents

- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Contribution Types](#contribution-types)
- [Development Workflow](#development-workflow)
- [Writing Lessons](#writing-lessons)
- [Code Quality Standards](#code-quality-standards)
- [Submitting Changes](#submitting-changes)
- [Community Guidelines](#community-guidelines)

## Getting Started

### Prerequisites

- Git installed
- Familiarity with an AI coding assistant (Claude Code, Cursor, GitHub Copilot, etc.)
- Basic understanding of Next.js, TypeScript, and React (for technical contributions)

### Setting Up Your Environment

1. **Fork the repository** on GitHub

2. **Clone your fork**:
   ```bash
   git clone https://github.com/YOUR-USERNAME/vibecoding-bootcamp.git
   cd vibecoding-bootcamp
   ```

3. **Add upstream remote**:
   ```bash
   git remote add upstream https://github.com/ORIGINAL-ORG/vibecoding-bootcamp.git
   ```

4. **Keep your fork synced**:
   ```bash
   git fetch upstream
   git checkout main
   git merge upstream/main
   ```

## How to Contribute

### Quick Start for Different Contribution Types

- **New Lesson Idea**: Open an issue with the `lesson-proposal` label
- **Bug Report**: Open an issue with the `bug` label
- **Documentation Fix**: Submit a PR directly
- **Lesson Improvement**: Create a proposal using OpenSpec (see below)

## Contribution Types

### 1. New Lessons

We welcome new lesson ideas! Before creating a new lesson:

1. **Check existing lessons and backlog** in chapter READMEs
2. **Open an issue** describing:
   - Lesson topic and learning objectives
   - Target difficulty level (beginner/intermediate/advanced)
   - Estimated completion time
   - Which chapter it belongs to
3. **Wait for maintainer feedback** before implementing

### 2. Lesson Improvements

For improvements to existing lessons:

1. **Test the lesson** yourself with an AI assistant
2. **Document issues** you encountered
3. **Propose specific improvements** (clearer instructions, better examples, etc.)
4. **Submit a PR** with your changes

### 3. Documentation

Documentation contributions are always welcome:

- Fix typos or unclear instructions
- Add missing examples or clarifications
- Improve README files
- Update learning paths

### 4. Templates and Boilerplates

Contributing to templates (under `/templates`):

1. Follow existing template structure
2. Include proper `architect.yaml` and `RULES.yaml` files
3. Test with scaffold-mcp tools
4. Document template usage in template README

### 5. Bug Fixes

Found a bug in lesson code or instructions?

1. **Open an issue** describing the bug
2. **Include**:
   - Steps to reproduce
   - Expected vs. actual behavior
   - AI assistant used (if relevant)
   - Environment details
3. **Submit a fix** if you can

## Development Workflow

For lesson contributions, keep it simple:

1. **Fork the repository**
2. **Create a feature branch**
3. **Add or port a lesson**
4. **Test it with an AI assistant**
5. **Submit a PR**

That's it! No complex workflows needed for lesson contributions.

## Understanding the Chapter + Lesson Structure

### Multi-Language Organization

The bootcamp is organized to support **multiple programming languages**. Each lesson is implemented separately per language:

```
chapters/
├── typescript/
│   ├── 01-bugfixes-01/        # Chapter 1, Lesson 1 in TypeScript
│   ├── 01-bugfixes-02/        # Chapter 1, Lesson 2 in TypeScript
│   ├── 02-feature-request-01/ # Chapter 2, Lesson 1 in TypeScript
│   └── 03-greenfield-01/      # Chapter 3, Lesson 1 in TypeScript
├── python/
│   ├── 01-bugfixes-01/        # Same lesson, Python version
│   └── ...
└── go/
    ├── 01-bugfixes-01/        # Same lesson, Go version
    └── ...
```

### Naming Convention

Lesson directories follow this pattern:
```
{chapter-number}-{chapter-type}-{lesson-number}/
```

**Examples:**
- `01-bugfixes-01` = Chapter 1 (Bug Fixes), Lesson 1
- `02-feature-request-03` = Chapter 2 (Feature Enhancements), Lesson 3
- `03-greenfield-01` = Chapter 3 (Greenfield Project), Lesson 1

**Chapter Types:**
- `01-bugfixes` = Chapter 1: Bug Fixes (5-10 min lessons)
- `02-feature-request` = Chapter 2: Feature Enhancements (30 min lessons)
- `03-greenfield` = Chapter 3: Greenfield Projects (60-90 min projects)

### Why This Structure?

1. **Language Independence**: Learners choose their preferred language
2. **Same Learning Objectives**: All languages teach the same concepts
3. **Consistent Naming**: Easy to find equivalent lessons across languages
4. **Scalability**: Adding new languages doesn't require restructuring

### Adding Lessons Across Languages

When contributing a new lesson:

1. **Choose which languages to support** (at minimum: TypeScript)
2. **Create equivalent implementations** for each language
3. **Ensure learning objectives remain identical**
4. **Adapt code to language idioms** (don't just translate)

Example - Same lesson, different languages:
- `typescript/01-bugfixes-01/` - Uses Next.js, TypeScript, React
- `python/01-bugfixes-01/` - Uses Flask/FastAPI, Python
- `go/01-bugfixes-01/` - Uses Go, net/http or Gin

## Calling All Language Experts!

**We need your help!** If you're proficient in a language and experienced with AI coding assistants, you can contribute lessons.

### What We're Looking For

Contributors who can port existing TypeScript lessons to:
- **Python** (Flask, FastAPI, Django)
- **Go** (Gin, Echo, standard library)
- **Rust** (Axum, Actix)
- **Java** (Spring Boot)
- **Ruby** (Rails, Sinatra)
- **PHP** (Laravel, Symfony)
- **Any other language you love!**

### Super Simple Contribution Process

#### Option 1: Port an Existing Lesson (Fastest!)

1. **Pick a TypeScript lesson** from `chapters/typescript/`
2. **Create equivalent in your language**:
   ```bash
   mkdir -p chapters/python/01-bugfixes-01
   # Port the lesson to Python
   ```
3. **Keep the same**:
   - Learning objectives
   - Bug or feature description
   - Time estimate
   - Success criteria
4. **Adapt to your language**:
   - Use idiomatic code for your language
   - Choose appropriate frameworks
   - Match the complexity level
5. **Submit PR** - We'll review and merge!

#### Option 2: Create a New Lesson

1. **Propose the lesson** (open an issue)
2. **Get approval** from maintainers
3. **Create it in your language first**
4. **Submit PR**
5. Others can port to other languages later!

### What Each Lesson Needs

Keep it minimal - you're busy, we get it:

1. **Working playground app** in your language
2. **README.md** with:
   - Learning objective (1-2 sentences)
   - Time estimate
   - Setup instructions (install + run)
   - The bug/feature description
   - 2-3 sample AI prompts
   - Success checklist
3. **Starter code** (with the bug or TODO markers)
4. **Solution** (in a branch or `solution/` folder)

That's it! No need to over-engineer.

### Time Commitment

- **Porting a simple bug fix**: 30-60 minutes
- **Porting a feature enhancement**: 2-3 hours
- **Creating a new lesson from scratch**: 3-5 hours

### Recognition

- Your name in the lesson README
- Listed as a contributor
- Mentioned in release notes
- Helping developers worldwide learn vibecoding!

### Get Started Right Now

1. **Browse existing lessons**: Check `chapters/typescript/`
2. **Pick one you like**: Bug fixes are easiest to start
3. **Port it to your language**: Use your favorite stack
4. **Open a PR**: We're friendly and fast with reviews!

**Questions?** Open a GitHub issue - we're here to help!

## Writing Lessons

### Lesson Structure

Each lesson playground should include:

1. **README.md** with:
   - Clear learning objectives
   - Estimated time to complete
   - Prerequisites and setup instructions
   - The bug/feature description
   - Sample prompts for AI assistants
   - Success criteria checklist

2. **Starter code**:
   - Working app with intentional bug OR
   - Working app ready for feature enhancement OR
   - Requirements doc for greenfield project
   - Clear TODO markers or bug indicators
   - Dependencies file (package.json, requirements.txt, go.mod, etc.)

3. **Solution** (separate branch or `solution/` directory):
   - Complete working solution
   - Comments explaining key decisions

### Lesson Quality Guidelines

**Keep it simple and focused**:
- One concept per lesson
- Clear success criteria
- Works with any AI assistant (Claude Code, Cursor, Copilot, etc.)
- Realistic time estimates

## Code Quality Standards

### For Template Code

When contributing code to templates:

1. **Follow existing patterns**:
   - Check `architect.yaml` for design patterns
   - Review `RULES.yaml` for coding standards
   - Use architect-mcp tools to validate

2. **Before editing files**:
   ```bash
   # Get design patterns for the file
   architect-mcp get-file-design-pattern <file-path>
   ```

3. **After editing files**:
   ```bash
   # Review your changes
   architect-mcp review-code-change <file-path>
   ```

### Code Style

- Use TypeScript for type safety
- Follow existing formatting conventions
- Add comments for complex logic
- Keep functions small and focused
- Write self-documenting code

### Testing

- Test lessons end-to-end with AI assistants
- Verify all sample prompts work
- Check success criteria are achievable
- Test on different operating systems if possible

## Submitting Changes

### Pull Request Process

1. **Create a feature branch**:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**:
   - Follow OpenSpec workflow for major changes
   - Follow coding standards
   - Test thoroughly

3. **Commit your changes**:
   ```bash
   git add .
   git commit -m "feat: Add lesson on authentication basics

   - Add new lesson to Chapter 2
   - Include sample prompts and success criteria
   - Test with Claude Code and Cursor

   Co-Authored-By: Your Name <your.email@example.com>"
   ```

4. **Push to your fork**:
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Open a Pull Request**:
   - Use a clear, descriptive title
   - Reference any related issues
   - Describe what changed and why
   - Include testing notes
   - Add OpenSpec proposal (if applicable)

### Commit Message Format

Follow conventional commits:

- `feat:` - New feature or lesson
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `refactor:` - Code refactoring
- `test:` - Adding tests
- `chore:` - Maintenance tasks

Examples:
```
feat: Add database migration lesson to Chapter 2
fix: Correct authentication example in lesson 2.1
docs: Update contributing guidelines for OpenSpec
refactor: Simplify lesson 1.3 instructions
```

### PR Review Process

Maintainers will review your PR for:

- Alignment with project goals
- Code quality and standards
- Lesson quality (if applicable)
- Testing completeness
- Documentation clarity

Be prepared to:
- Address feedback constructively
- Make requested changes
- Discuss design decisions

## Community Guidelines

### Code of Conduct

- Be respectful and inclusive
- Welcome newcomers
- Give constructive feedback
- Assume good intentions
- Help others learn

### Communication

- **GitHub Issues**: Bug reports, feature requests, lesson proposals
- **GitHub Discussions**: Questions, ideas, general discussion
- **Pull Requests**: Code and documentation contributions

### Getting Help

- Read existing documentation first
- Search closed issues for similar questions
- Ask in GitHub Discussions for general help
- Open issues for specific bugs or proposals

## Recognition

Contributors will be:
- Listed in project contributors
- Credited in relevant lesson READMEs
- Mentioned in release notes

Thank you for contributing to Vibecoding Training!

## License

By contributing, you agree that your contributions will be licensed under the AGPL-3.0 License.

## Questions?

- Open a GitHub Discussion for general questions
- Open an issue for specific bugs or proposals
- Check existing documentation and issues first

---

**Ready to contribute?** Pick an issue labeled `good-first-issue` or propose a new lesson idea!
