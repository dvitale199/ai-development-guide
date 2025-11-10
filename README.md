# AI-Assisted Development Guide

**A comprehensive, language-agnostic guide for developing software with AI coding assistants.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## Overview

This repository provides principles, practices, and patterns for developing high-quality software with AI assistance. Whether you're using Claude Code, GitHub Copilot, Cursor, or any other AI coding tool, these guidelines will help you:

- **Maintain code quality** while leveraging AI's speed
- **Ensure correctness** through empirical validation
- **Manage complexity** as AI generates code quickly
- **Balance human judgment** with AI capabilities
- **Deploy safely** with proper testing and rollback strategies

## Who This Is For

- **Developers** using AI coding assistants in their daily work
- **Code reviewers** evaluating AI-generated code
- **Team leads** establishing AI development practices
- **AI tools** (like Claude Code) that can read and apply these guidelines

---

## Quick Navigation

### 🚀 New to AI-Assisted Development?

Start here:
1. **[AI Development Guide](AI_DEVELOPMENT_GUIDE.md)** - Comprehensive principles and practices
2. **Getting Started Guide** - *(Coming in Phase 2)* Orientation and onboarding
3. **For AI Tools** - *(Coming in Phase 2)* How AI assistants should use this repo

### 📚 Core Principles

All core principles are covered in the [AI Development Guide](AI_DEVELOPMENT_GUIDE.md):
- **Empiricism** - Test everything, trust evidence over assumptions
- **Incrementalism** - Small, atomic, reversible changes
- **Testing** - Test-driven development with AI
- **Human-AI Collaboration** - Roles and responsibilities

*Standalone focused guides coming in Phase 3*

### 🛠️ Practical Guides

**Currently Available:**
- See [AI Development Guide](AI_DEVELOPMENT_GUIDE.md) for code review checklists, deployment strategies, and testing practices

**Coming in Phase 4:**
- Code Review - Detailed review process for AI-generated code
- Deployment - Feature flags, rollback, monitoring
- Testing Strategy - TDD workflow with AI
- Incident Response - Learning from production issues

### 💻 Language & Framework Guides

Stack-specific implementation guidance:
- **[Python/FastAPI](docs/stacks/python-fastapi/README.md)** - Python conventions, testing, deployment

*More stacks coming soon: Node.js, Rust, Go, Java, and others*

### 🚫 Anti-Patterns

**Currently Available:**
- See [AI Development Guide - Anti-Patterns section](AI_DEVELOPMENT_GUIDE.md#anti-patterns--red-flags) for common pitfalls and red flags

**Coming in Phase 5:**
- Comprehensive anti-patterns guide
- Detection guide for code review

### 📋 Templates

**Coming in Phase 5:**
- PR Template - Structure for AI-assisted pull requests
- Review Checklist - Comprehensive code review guide
- Commit Messages - How to document AI contributions
- Retrospective Template - Blameless incident reviews

*In the meantime, see examples in the [AI Development Guide](AI_DEVELOPMENT_GUIDE.md)*

---

## Repository Structure

```
ai-development-guide/
├── README.md                          # You are here
├── AI_DEVELOPMENT_GUIDE.md            # Comprehensive guide (language-agnostic)
├── docs/
│   └── stacks/                        # Language/framework specific guides
│       └── python-fastapi/            # Python/FastAPI implementation
│           ├── README.md              # Stack guide
│           └── CLAUDE.md              # Quick reference
└── PROJECT_PLAN.md                    # Development roadmap
```

**Coming soon:** getting-started/, core-principles/, guides/, anti-patterns/, templates/

---

## Philosophy

### AI as a Tool, Not a Replacement

AI coding assistants are powerful tools that can dramatically increase development velocity, but they require discipline:

- **AI generates** - Humans verify, validate, and take accountability
- **AI suggests** - Humans decide based on domain expertise and context
- **AI scaffolds** - Humans refine, test, and ensure correctness

### Core Principles

1. **Empiricism** - All engineering decisions grounded in observable evidence
2. **Incrementalism** - Small, reversible changes over large batch updates
3. **Human Oversight** - Critical review and domain expertise remain non-negotiable
4. **Continuous Testing** - Automated validation catches AI hallucinations and errors
5. **Transparency** - Document what's AI-generated and track decision rationale

---

## Key Concepts

### The Learning Cycle

AI-assisted development optimizes for learning velocity, not just code generation speed:

```
Hypothesis → Generate → Test → Measure → Learn → Iterate
```

**The bottleneck shifts from "writing code" to "validating correctness."**

### Test-Driven Development (TDD) with AI

1. Human defines requirements and success criteria
2. AI generates test cases (including edge cases)
3. Human reviews tests for completeness
4. AI generates implementation to pass tests
5. Run tests, iterate until all pass
6. Human reviews for security, clarity, maintainability

### Code Review Essentials

Before approving AI-generated code, verify:
- ✅ **Correctness** - Does it solve the problem? Are edge cases handled?
- ✅ **Security** - No hardcoded secrets, proper input validation, auth checks
- ✅ **Maintainability** - Clear naming, follows conventions, appropriate complexity
- ✅ **Architecture** - Respects boundaries, doesn't introduce coupling
- ✅ **Documentation** - Docstrings, comments explaining "why", API docs updated

---

## Language-Agnostic vs Stack-Specific

### Language-Agnostic Content

The **core principles**, **philosophy**, and **practices** in this guide apply to any programming language. Concepts like:
- Empirical validation
- Incrementalism and atomic changes
- Test-driven development
- Code review standards
- Deployment practices

...work whether you're writing Python, JavaScript, Rust, Go, or any other language.

### Stack-Specific Guides

For practical implementation, see **[docs/stacks/](docs/stacks/)** for language and framework-specific guidance:
- Conventions and patterns
- Testing frameworks and examples
- Deployment strategies
- Quick reference for AI tools

Each stack guide follows a consistent structure, making it easy to apply learnings across different technologies.

---

## AI Tool Support

### General AI Coding Assistants

This guide works with any AI coding tool:
- GitHub Copilot
- Cursor
- Claude Code
- Tabnine
- Amazon CodeWhisperer
- And others

**See:** [For AI Tools](docs/getting-started/for-ai-tools.md) for general guidance on how AI assistants should read and apply these docs.

### Claude Code Specific

Claude Code users get additional guidance on:
- File reference syntax (`[filename.py:42](path/to/file.py#L42)`)
- Tool usage patterns (Read, Edit, Bash, etc.)
- Workflow optimization

**Claude-specific guidance coming in Phase 2.**

---

## Getting Started

### For Developers

1. **Read [AI_DEVELOPMENT_GUIDE.md](AI_DEVELOPMENT_GUIDE.md)** - Start with core principles
2. **Check your stack guide** - See [Python/FastAPI](docs/stacks/python-fastapi/README.md) or your language
3. **Apply incrementally** - Start with code review checklist from the guide
4. **Test everything** - Follow TDD practices outlined in the guide
5. **Review [PROJECT_PLAN.md](PROJECT_PLAN.md)** to see what's coming next

### For AI Coding Assistants

If you're an AI tool reading this repository:

1. **Read [AI_DEVELOPMENT_GUIDE.md](AI_DEVELOPMENT_GUIDE.md)** first for principles
2. **Apply core principles** when generating code (empiricism, incrementalism, testing)
3. **Follow stack conventions** from [docs/stacks/python-fastapi/](docs/stacks/python-fastapi/) or relevant guide
4. **Mark your contributions** with `[ai-assisted]` or `[ai-generated]` tags
5. **Reference the code review checklist** before suggesting large changes

### For Teams

1. **Start with [AI_DEVELOPMENT_GUIDE.md](AI_DEVELOPMENT_GUIDE.md)** - Establish shared understanding
2. **Adopt the code review checklist** - Use it for all AI-generated PRs
3. **Customize for your stack** - Create or extend stack guides in [docs/stacks/](docs/stacks/)
4. **Track progress** - See [PROJECT_PLAN.md](PROJECT_PLAN.md) for roadmap
5. **Contribute back** - Share improvements and new stack guides

---

## Contributing

We welcome contributions! Whether you want to:
- Add a new stack guide (Node.js, Rust, Go, etc.)
- Improve existing documentation
- Share code examples or case studies
- Fix typos or broken links

*(Contribution guidelines coming soon - see [PROJECT_PLAN.md](PROJECT_PLAN.md) for roadmap)*

---

## License

This project is licensed under the **MIT License** - see [LICENSE](LICENSE) file for details.

You are free to:
- Use this guide in your projects
- Modify and adapt for your needs
- Share with your team
- Contribute improvements back to the community

---

## FAQ

### How is this different from general coding best practices?

AI introduces unique challenges:
- **Speed vs Quality** - AI generates code faster than humans can review it
- **Plausibility vs Correctness** - AI code often looks right but may be subtly wrong
- **Hidden Assumptions** - AI training data may not match your domain
- **Coupling Risk** - AI may create dependencies without understanding implications

This guide addresses these AI-specific challenges while reinforcing timeless engineering principles.

### Do I need to follow every guideline?

Start with the **Core Requirements**:
1. **Empirical validation** - Always test AI-generated code
2. **Human review** - Never merge without domain expert approval
3. **Incrementalism** - Keep changes small and atomic
4. **Testing** - Require tests for all significant code

Other guidelines are best practices to adopt over time.

### Can I use this with my existing development workflow?

Yes! This guide complements standard practices:
- Works with Git workflows (feature branches, PRs)
- Integrates with CI/CD pipelines
- Compatible with Agile/Scrum processes
- Enhances existing code review practices

### What if my language/framework isn't covered?

The core principles are language-agnostic. Start with:
1. **[AI Development Guide](AI_DEVELOPMENT_GUIDE.md)** for universal concepts
2. **[Core Principles](docs/core-principles/)** for fundamental practices
3. **Create a stack guide** following the Python/FastAPI example
4. **Contribute back** so others can benefit

---

## Resources

### Related Projects
- [Anthropic Claude Documentation](https://docs.anthropic.com/)
- [GitHub Copilot Best Practices](https://docs.github.com/en/copilot)

### Further Reading
- Software Engineering Best Practices
- Test-Driven Development
- Code Review Guidelines
- Deployment Strategies

---

## Acknowledgments

This guide synthesizes best practices from:
- Software engineering principles (modularity, testing, incrementalism)
- AI safety and human-in-the-loop systems
- Production deployment practices (feature flags, monitoring, rollback)
- Real-world experience with AI coding assistants

---

## Questions or Feedback?

- **Issues**: Open an issue on GitHub
- **Discussions**: Start a discussion for questions or ideas
- **Pull Requests**: Contribute improvements

**Last Updated**: 2025-11-10
