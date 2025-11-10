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
1. **[Getting Started Guide](docs/getting-started/README.md)** - Orientation and onboarding
2. **[For AI Tools](docs/getting-started/for-ai-tools.md)** - How AI assistants should use this repo
3. **[AI Development Guide](AI_DEVELOPMENT_GUIDE.md)** - Comprehensive principles and practices

### 📚 Core Principles

Fundamental concepts for AI-assisted development:
- **[Empiricism](docs/core-principles/empiricism.md)** - Test everything, trust evidence over assumptions
- **[Incrementalism](docs/core-principles/incrementalism.md)** - Small, atomic, reversible changes
- **[Testing](docs/core-principles/testing.md)** - Test-driven development with AI
- **[Human-AI Collaboration](docs/core-principles/human-ai-collaboration.md)** - Roles and responsibilities

### 🛠️ Practical Guides

Task-specific guidance:
- **[Code Review](docs/guides/code-review.md)** - How to review AI-generated code
- **[Deployment](docs/guides/deployment.md)** - Feature flags, rollback, monitoring
- **[Testing Strategy](docs/guides/testing-strategy.md)** - TDD workflow with AI
- **[Incident Response](docs/guides/incident-response.md)** - Learning from production issues

### 💻 Language & Framework Guides

Stack-specific implementation guidance:
- **[Python/FastAPI](docs/stacks/python-fastapi/README.md)** - Python conventions, testing, deployment

*More stacks coming soon: Node.js, Rust, Go, Java, and others*

### 🚫 Anti-Patterns

Learn what to avoid:
- **[Common Pitfalls](docs/anti-patterns/README.md)** - AI coding mistakes
- **[Detection Guide](docs/anti-patterns/detection-guide.md)** - How to spot issues

### 📋 Templates

Ready-to-use resources:
- **[PR Template](docs/templates/pr-template.md)** - Structure for AI-assisted pull requests
- **[Review Checklist](docs/templates/review-checklist.md)** - Comprehensive code review guide
- **[Commit Messages](docs/templates/commit-messages.md)** - How to document AI contributions
- **[Retrospective Template](docs/templates/retrospective-template.md)** - Blameless incident reviews

---

## Repository Structure

```
ai-development-guide/
├── README.md                          # You are here
├── AI_DEVELOPMENT_GUIDE.md            # Comprehensive guide (language-agnostic)
├── docs/
│   ├── getting-started/               # Onboarding for developers and AI tools
│   ├── core-principles/               # Fundamental concepts
│   ├── guides/                        # Task-specific guidance
│   ├── stacks/                        # Language/framework specific guides
│   │   └── python-fastapi/            # Python/FastAPI implementation
│   ├── anti-patterns/                 # Common mistakes to avoid
│   └── templates/                     # Reusable checklists and templates
└── PROJECT_PLAN.md                    # Development roadmap
```

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

**See:** [For Claude](docs/getting-started/for-claude.md) for Claude-specific instructions.

---

## Getting Started

### For Developers

1. **Read the [Getting Started Guide](docs/getting-started/README.md)**
2. **Review core principles** - Start with [Empiricism](docs/core-principles/empiricism.md)
3. **Check your stack guide** - See [Python/FastAPI](docs/stacks/python-fastapi/README.md) or your language
4. **Use templates** - Adopt [PR templates](docs/templates/pr-template.md) and [review checklists](docs/templates/review-checklist.md)
5. **Read the comprehensive guide** - [AI_DEVELOPMENT_GUIDE.md](AI_DEVELOPMENT_GUIDE.md) for deep understanding

### For AI Coding Assistants

If you're an AI tool reading this repository:

1. **Read [For AI Tools](docs/getting-started/for-ai-tools.md)** first
2. **Apply core principles** when generating code
3. **Follow stack conventions** from relevant guide
4. **Reference templates** when helping users create PRs or reviews
5. **Mark your contributions** with `[ai-assisted]` or `[ai-generated]` tags

### For Teams

1. **Adopt incrementally** - Start with code review checklist
2. **Customize templates** - Adapt PR and commit message templates to your workflow
3. **Train reviewers** - Share anti-patterns guide with code reviewers
4. **Establish conventions** - Create a stack guide for your tech stack if not present
5. **Share learnings** - Use retrospective template to learn from incidents

---

## Contributing

We welcome contributions! Whether you want to:
- Add a new stack guide (Node.js, Rust, Go, etc.)
- Improve existing documentation
- Share code examples or case studies
- Fix typos or broken links

Please see **CONTRIBUTING.md** for guidelines.

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
