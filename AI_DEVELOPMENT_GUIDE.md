# AI-Driven Software Engineering: Comprehensive Development Guide

**Purpose:** This document provides detailed principles, practices, and patterns for developing software with AI assistance. These are language-agnostic guidelines that apply to any programming language or framework.

**Target Audience:** AI assistants, human developers, and code reviewers working with AI coding tools.

**Note:** For language/framework-specific guidance, see [docs/stacks/](docs/stacks/).

---

## Table of Contents

1. [Philosophy & Core Principles](#philosophy--core-principles)
2. [Development Practices](#development-practices)
3. [Deployment & Operations](#deployment--operations)
4. [Anti-Patterns & Red Flags](#anti-patterns--red-flags)
5. [Definitions & Glossary](#definitions--glossary)

---

## Philosophy & Core Principles

### 1. Empirical Foundation

**Principle:** All engineering decisions must be grounded in observable, measurable evidence.

**Why this matters with AI:**
- AI generates plausible-looking code that may not be correct
- Assumptions baked into AI training data may not match your domain
- "It looks right" is insufficient for production systems

**In practice:**
- Run code before claiming it works
- Measure performance, correctness, and behavior with real or synthetic data
- Document what you tested and what the results showed
- Every PR must include evidence: test output, benchmark data, or validation logs

**Example workflow:**
```
1. AI generates data processing function
2. Write test with known input/output pairs
3. Run test → observe failure modes
4. Iterate with AI on fixes
5. Document edge cases discovered
6. Include test results in PR description
```

### 2. Human-AI Collaboration Model

**Principle:** Humans provide goals, context, judgment, and accountability. AI provides generation, exploration, and scaffolding.

**Division of responsibility:**

**Human responsibilities (non-delegable):**
- Define business requirements and success criteria
- Provide domain expertise (business logic, security, privacy, regulations)
- Review AI output for correctness, safety, and appropriateness
- Make final merge decisions
- Own production outcomes

**AI responsibilities:**
- Generate code scaffolding and boilerplate
- Suggest multiple implementation approaches
- Identify potential edge cases
- Draft tests and documentation
- Refactor and optimize within constraints

**Critical rule:** AI output is a DRAFT until human-reviewed and validated.

### 3. Optimizing for Learning Velocity

**Core insight:** In AI-assisted development, the bottleneck shifts from "writing code" to "validating correctness." Optimize for rapid learning cycles, not raw code generation speed.

**The learning cycle:**
```
Hypothesis → Generate → Test → Measure → Learn → Iterate
```

**Key practices:**
- **Iteration:** Ship smallest possible increments (single function, single API route)
- **Feedback:** Automate everything validatable (tests, linters, type checkers, security scans)
- **Incrementalism:** Each change must be independently reviewable and revertible
- **Experimentation:** Use feature flags and branches to test multiple approaches safely
- **Empiricism:** Measure actual behavior, not intended behavior

**Anti-pattern:** Generating large features end-to-end without intermediate validation.

### 4. Managing Complexity

**Challenge:** AI can generate complex, coupled code quickly. Without discipline, codebases become unmaintainable.

**Core strategies:**

**Modularity:**
- Enforce clear module boundaries with defined APIs
- Each module has a single, well-defined responsibility
- AI should generate within existing module patterns, not create new coupling

**Separation of Concerns:**
- Business logic ≠ API layer ≠ Data access ≠ UI
- Review AI code for "scope creep" (e.g., API endpoint that also formats UI strings)
- Reject changes that blur architectural boundaries

**Information Hiding & Abstraction:**
- Internal implementation details stay internal
- Public APIs are stable, documented contracts
- AI should not expose internals just because it's convenient

**Coupling Management:**
- New dependencies between modules require explicit justification
- Use dependency injection and interfaces to manage coupling
- Automated tools (linters, static analyzers) can flag tight coupling

**Example review question:** "Why does this API endpoint import from the UI layer?"

---

## Development Practices

### Test-Driven Development (TDD) with AI

**Workflow:**
1. Human defines the requirement and success criteria
2. AI generates test cases (including edge cases)
3. Human reviews tests for completeness and domain correctness
4. AI generates implementation to pass tests
5. Run tests, iterate until all pass
6. Human reviews final code for clarity, security, and maintainability

**Key principle:** Tests are written FIRST, ideally before implementation exists.

**Why TDD matters more with AI:**
- AI often generates "happy path" code that fails on edge cases
- Tests document intended behavior independent of implementation
- Tests catch AI hallucinations (e.g., using APIs that don't exist)

**Test requirements:**
- All public functions and API endpoints require tests
- Use clear, descriptive test names: `test_endpoint_returns_404_for_missing_user`
- Include both positive cases (expected success) and negative cases (expected failures)
- Use synthetic data fixtures that mimic real data structure when working with sensitive data

**Marking AI-generated tests:**
```
# [ai-generated] Test cases for edge cases
def test_function_with_empty_input():
    """Edge case: empty input should return empty result."""
    result = process_data([])
    assert result == []
```

**Note:** For language-specific testing frameworks and examples, see your stack guide in [docs/stacks/](docs/stacks/).

### Code Review Checklist for AI Output

**Before approving any AI-generated code, verify:**

**Correctness:**
- [ ] Does it solve the stated problem?
- [ ] Are edge cases handled (null/empty/invalid input)?
- [ ] Do tests pass and cover key scenarios?
- [ ] Does it match domain requirements and business logic?

**Security & Privacy:**
- [ ] No hardcoded secrets or API keys
- [ ] No exposure of sensitive data in logs or errors
- [ ] Input validation for all user-supplied data
- [ ] SQL injection / command injection risks mitigated
- [ ] Authentication/authorization checks present where required

**Maintainability:**
- [ ] Code follows project conventions
- [ ] Function/variable names are clear and domain-appropriate
- [ ] Comments explain "why," not just "what"
- [ ] No unnecessary complexity or over-engineering
- [ ] Linting and type checking pass (per project standards)

**Architecture:**
- [ ] Respects module boundaries and separation of concerns
- [ ] Doesn't introduce unexpected coupling
- [ ] Consistent with existing patterns in the codebase
- [ ] API contracts are stable and documented

**Documentation:**
- [ ] Docstrings/comments for public functions (with types, args, returns, raises)
- [ ] Comments for non-obvious logic (especially domain-specific algorithms)
- [ ] API changes reflected in project documentation
- [ ] Marked as `[ai-assisted]` or `[ai-generated]` where substantial

### Incrementalism: What Qualifies as "Atomic"?

**Good atomic changes (approve these):**
- Add a single new API endpoint with tests
- Refactor one function to improve readability
- Fix a specific bug with a test that reproduces it
- Add validation to a single data model/schema
- Update one processing stage with before/after validation

**Bad non-atomic changes (split these up):**
- "Refactor entire codebase to use new pattern"
- "Add feature X and also fix unrelated bugs Y and Z"
- "Update API, database schema, UI, and docs in one PR"
- "Rewrite multiple pipeline stages with new algorithm"

**Rule of thumb:** If you can't explain the change in one sentence, it's too large.

### Documentation Standards

**What to document:**

**In code (docstrings and comments):**
```
function calculate_score(items, options):
    """
    Calculate score using weighted sum algorithm.

    Args:
        items: List of items to process
        options: Configuration options for calculation

    Returns:
        Score between 0.0 and 1.0, where higher indicates better match

    Raises:
        ValueError: If options are invalid

    Notes:
        - Uses standard weighting algorithm
        - Weights configurable via options parameter
        - [ai-assisted] Algorithm implementation based on domain requirements
    """
```

**In PRs:**
- What problem does this solve?
- What approach did you take and why?
- What alternatives were considered?
- What testing was performed?
- What are the risks or limitations?

**In project documentation:**
- All API endpoints, request/response schemas, status codes, error conditions
- Public interfaces and their contracts
- Architecture decisions and rationale

**When using AI:**
- Mark substantial AI contributions: `[ai-assisted]` or `[ai-generated]`
- Document any domain assumptions the AI made
- Note if AI suggested the approach vs. human-directed implementation

---

## Deployment & Operations

### Feature Flags and Experimentation

**Principle:** Separate deployment (code is in production) from release (users see the feature).

**How feature flags enable safe AI-assisted development:**
- Deploy AI-generated features behind flags, disabled by default
- Enable for internal testing first
- Gradually roll out to small user percentage
- Compare metrics (performance, errors) between flag on/off
- Instant rollback if issues detected (flip flag, no redeploy)

**Implementation pattern:**
```
# Pseudocode example
function handle_request(user_id):
    if not is_feature_enabled("new_algorithm_v2", user_id):
        # Fall back to old implementation
        return old_algorithm(user_id)

    # [ai-generated] New AI-assisted implementation
    return new_algorithm_v2(user_id)
```

**Configuration:**
```yaml
# feature_flags.yml
new_algorithm_v2:
  enabled: false
  rollout_percentage: 5  # Only 5% of users initially
  allow_list: ["test-user-1", "test-user-2"]
```

### Rollback Strategies

**Every deployment must have a tested rollback plan.**

**Rollback mechanisms (in order of speed):**
1. **Feature flag toggle** (instant, no redeploy)
2. **Blue-green deployment switch** (seconds, requires identical infra)
3. **Previous container/image redeploy** (minutes)
4. **Git revert + CI/CD pipeline** (slowest, but always available)

**Pre-deployment checklist:**
- [ ] Rollback plan documented
- [ ] Database migrations are reversible (or compatible with old code)
- [ ] No destructive data operations without backups
- [ ] Feature flags configured for new features
- [ ] Monitoring and alerts configured

**Rollback triggers:**
- Error rate increases >2x baseline
- Latency increases >50% above baseline
- Any data corruption or privacy breach detected
- Failed health checks or smoke tests

### Monitoring AI-Generated Code in Production

**Key metrics to track:**

**Performance:**
- Response time (p50, p95, p99)
- Throughput (requests/second)
- Resource usage (CPU, memory, database connections)

**Correctness:**
- Error rates by endpoint/function
- Failed validations (schema/type errors)
- Data pipeline success/failure rates

**Business impact:**
- Feature adoption (flag-on usage)
- User-reported issues
- Downstream data quality (for pipelines)

**AI-specific monitoring:**
- Track which features are AI-generated (via tags/labels)
- Compare metrics between AI-generated and human-written code
- Flag anomalies for human review

**Alerting rules:**
```
# Example alert configuration (pseudocode)
if error_rate > baseline * 2 and feature_flag == "ai_generated":
    alert(
        severity="high",
        message="AI-generated feature showing elevated errors",
        action="Review logs and consider rollback"
    )
```

---

## Anti-Patterns & Red Flags

### Common AI Coding Pitfalls

**1. The "AI Dump"**
- **Pattern:** AI generates hundreds of lines of code at once without human validation
- **Why it's bad:** Impossible to review thoroughly; likely contains hidden bugs
- **Fix:** Require incremental generation with validation at each step

**2. The "Looks Good to Me" Review**
- **Pattern:** Human approves AI code because it "looks professional" without testing
- **Why it's bad:** AI excels at generating plausible-looking but incorrect code
- **Fix:** Mandate empirical testing and domain expert review

**3. The "Invisible Coupling"**
- **Pattern:** AI imports and uses modules without understanding their contracts
- **Why it's bad:** Creates hidden dependencies; breaks when upstream changes
- **Fix:** Require explicit justification for new dependencies; use dependency injection

**4. The "Over-Engineered Solution"**
- **Pattern:** AI generates complex, abstract solutions for simple problems
- **Why it's bad:** Harder to maintain, test, and debug
- **Fix:** Ask "What's the simplest solution?" before accepting AI's first answer

**5. The "Hallucinated API"**
- **Pattern:** AI uses functions or libraries that don't exist or don't work as described
- **Why it's bad:** Code looks correct but fails at runtime
- **Fix:** Always test generated code; verify third-party API usage against docs

**6. The "Silent Failure"**
- **Pattern:** AI catches exceptions without proper logging or error handling
- **Why it's bad:** Bugs are hidden; impossible to debug in production
- **Fix:** Require explicit error handling and logging for all failure modes

### When to Reject AI Suggestions

**Automatic rejection criteria:**
- Hardcoded secrets or credentials
- Exposure of sensitive data in logs or responses
- Bypassing authentication or authorization checks
- Disabling security features (e.g., SQL parameterization)
- Skipping input validation
- Large, unreviewed code blocks (>100 lines without intermediate validation)

**Requires strong justification:**
- New external dependencies (libraries, APIs)
- Changes to core data models or schemas
- Modifications to authentication/authorization logic
- Performance-critical code without benchmarks
- Algorithmic changes in data processing pipelines without validation data

**When in doubt, ask:**
- "What problem does this solve?"
- "What are the risks?"
- "Is there a simpler approach?"
- "How would we test this?"
- "What happens if this fails in production?"

### Red Flags in AI-Generated PRs

**Review with extra scrutiny if:**
- No tests included ("I'll add tests later")
- Comments are generic or auto-generated ("This function does X")
- Multiple unrelated changes in one PR
- New patterns inconsistent with existing codebase
- No explanation of why this approach was chosen
- Touches security, privacy, or compliance-related code
- Modifies database schemas or migrations
- Changes API contracts without documentation updates

---

## Definitions & Glossary

### Empiricism
**Definition:** Decision-making based on observed evidence rather than assumptions or intuition.

**In practice:** Before merging code, you must test it and observe its behavior. "It should work" is not sufficient; "I ran it and here's the output" is required.

### Incrementalism
**Definition:** Advancing in small, reversible, independently-reviewable steps rather than large batch changes.

**Atomic change:** A change that solves one problem, can be reviewed in <30 minutes, and can be reverted without breaking other features.

**Counter-example:** "Refactor entire module + add feature + fix bugs" is not incremental.

### Test-Driven Development (TDD)
**Definition:** Write tests before implementation; let failing tests drive development.

**Workflow:**
1. Write a failing test that defines desired behavior
2. Write minimal code to make test pass
3. Refactor while keeping tests green
4. Repeat

**With AI:** Human defines test cases; AI generates implementation; human reviews both.

### Modularity
**Definition:** Code organized into self-contained units with clear boundaries and minimal dependencies.

**Good modularity:**
- Each module has a single responsibility
- Modules interact through defined APIs
- Internal implementation can change without affecting other modules

**Bad modularity:**
- Circular dependencies between modules
- Global state shared across modules
- Unclear boundaries (where does this logic belong?)

### Coupling
**Definition:** The degree to which modules depend on each other.

**Loose coupling (good):**
- Modules communicate through interfaces
- Changes in one module rarely require changes in others
- Easy to test modules in isolation

**Tight coupling (bad):**
- Module A directly accesses Module B's internals
- Can't change B without breaking A
- Hard to test or replace components

### Cohesion
**Definition:** The degree to which elements within a module belong together.

**High cohesion (good):**
- All functions in a module serve a related purpose
- Module has a clear, single responsibility
- Easy to understand and maintain

**Low cohesion (bad):**
- Module contains unrelated functionality
- "Utility module" with random helper functions
- Unclear what the module is for

### Separation of Concerns
**Definition:** Each component should address a separate concern; different types of logic should not be mixed.

**Example separation in typical projects:**
- `/routes` or `/controllers`: API routes, request/response handling
- `/services`: Business logic, data processing
- `/models` or `/schemas`: Data validation, type definitions
- `/views` or `/ui`: UI and presentation logic

**Anti-pattern:** API endpoint that contains SQL queries, business logic, and HTML generation.

### Information Hiding & Abstraction
**Definition:** Hide implementation details behind clean interfaces; expose only what's necessary.

**Good abstraction:**
```
function calculate_score(items):
    """Calculate score. Implementation details hidden."""
    ...
```

**Bad abstraction (leaky):**
```
function calculate_score_using_weighted_algorithm_with_caching(
    items,
    use_cache,
    database_connection,
    algorithm_version
):
    """Exposes too many implementation details in API."""
    ...
```

### Feature Flags
**Definition:** Runtime toggles that enable/disable features without code deployment.

**Use cases:**
- Test new features with subset of users
- Instant rollback (disable flag)
- A/B testing
- Gradual rollouts

**Implementation:**
```
if is_enabled("new_feature", user_id):
    return new_implementation()
else:
    return old_implementation()
```

### Experimentation
**Definition:** Testing multiple approaches to find what works best, using scientific methods.

**In AI-assisted development:**
- Prompt AI for 2-3 different implementation approaches
- Implement behind feature flags
- Measure performance, correctness, maintainability
- Choose based on evidence, not intuition

**Requirements:**
- Clear hypothesis ("Approach A will be 2x faster than approach B")
- Measurable outcomes (benchmarks, error rates, user metrics)
- Fair comparison (same test data, same environment)

### Traceability
**Definition:** The ability to track which changes were made, why, and by whom (or what).

**In AI-assisted development:**
- Mark AI-generated code: `[ai-generated]` or `[ai-assisted]`
- Document in commit messages: "Used AI assistant to generate initial implementation"
- Track in PR descriptions: "AI suggested this approach; human validated with domain expert"

**Why it matters:**
- Debugging (was this bug in AI-generated code?)
- Accountability (who's responsible for reviewing?)
- Learning (which AI suggestions work well vs. poorly?)

### Human-in-the-Loop
**Definition:** Humans remain actively involved in decisions, not just passively approving AI output.

**Active involvement:**
- Review AI code line-by-line
- Test with domain-specific scenarios
- Validate against business requirements
- Make final merge decision with accountability

**Passive (insufficient):**
- "AI wrote it, looks fine, approved"
- No testing or validation
- Assume AI is correct by default

### Blue-Green Deployment
**Definition:** Running two identical production environments (blue and green); switch traffic between them for instant rollbacks.

**Process:**
1. Blue is serving production traffic
2. Deploy new version to green environment
3. Test green with smoke tests
4. Switch traffic from blue to green
5. If issues, switch back to blue (instant rollback)

**Benefit:** Zero-downtime deployments and instant rollback without redeploying.

### Canary Release
**Definition:** Gradually rolling out changes to a small percentage of users before full deployment.

**Process:**
1. Deploy new version to 5% of servers
2. Monitor metrics (errors, latency, etc.)
3. If metrics are good, increase to 25%
4. Continue until 100%
5. If metrics are bad at any point, rollback

**With feature flags:** You can canary without multiple deployments by gradually increasing the flag percentage.

### Blameless Retrospective
**Definition:** Post-incident review focused on learning and process improvement, not assigning blame.

**Structure:**
- What happened? (timeline of events)
- Why did it happen? (root causes, not "who made a mistake")
- How do we prevent this? (process improvements, automation)
- What did we learn?

**For AI-assisted development:**
- Did AI generate incorrect code? Why did it pass review?
- What tests would have caught this?
- How can we improve our AI code review process?

### Innovation Tokens
**Definition:** The concept that teams have a limited budget for adopting new, unproven technologies.

**Principle:** Don't spend innovation tokens unnecessarily. Use proven, boring technologies for most things; save tokens for areas where innovation provides clear value.

**Examples:**
- **Good use of token:** New AI model for core business functionality (provides clear value)
- **Bad use of token:** Rewriting database layer in new framework because it's trendy

### Scope Creep
**Definition:** Gradual expansion of a project's requirements beyond its original intent.

**In AI-assisted development:**
- AI generates code that solves the stated problem PLUS additional "nice to have" features
- Each addition seems small, but collectively they make the change unreviewable

**Prevention:**
- Define clear requirements upfront
- Reject AI code that goes beyond requirements
- Follow up features should be separate PRs

### Regulatory Alignment
**Definition:** Ensuring code and processes comply with relevant regulations (HIPAA, GDPR, SOX, PCI-DSS, etc.).

**Common regulatory requirements:**
- Data privacy and security (GDPR, CCPA, HIPAA)
- Financial data protection (SOX, PCI-DSS)
- Industry-specific regulations
- Access controls and audit logging

**AI-specific concerns:**
- AI training data may include copyrighted or private information
- AI-generated code may violate compliance rules
- Require human expert review for compliance-critical code

### Sociotechnical Feedback
**Definition:** Feedback loops that include both technical systems and human/organizational factors.

**Examples:**
- Retrospectives after incidents
- Team learning from AI code review mistakes
- Cross-team sharing of effective AI prompts and patterns
- User feedback on AI-generated features

**Why it matters:** Technical solutions alone are insufficient; human processes and culture must evolve alongside AI tools.

---

## Summary: North Star Principles

Use AI to amplify learning, feedback, and delivery speed, but always enforce:

1. **Human judgment** - AI assists; humans decide and own outcomes
2. **Code discipline** - Tests, reviews, incremental changes, clear architecture
3. **Transparency** - Document what's AI-generated, why decisions were made
4. **Ethical standards** - Security, privacy, regulatory compliance, fairness
5. **Continuous improvement** - Learn from outcomes, adapt practices, share knowledge

**Remember:** AI is a powerful tool for writing code faster. But faster code generation without disciplined validation creates faster accumulation of technical debt and production bugs. The goal is not to write more code; it's to deliver more value safely and sustainably.

---

## Next Steps

**For language/framework-specific guidance:**
- See [docs/stacks/](docs/stacks/) for implementation guides
- Python/FastAPI guide available at [docs/stacks/python-fastapi/](docs/stacks/python-fastapi/)
- More stacks coming soon

**For practical templates:**
- PR templates, checklists, and examples in [docs/templates/](docs/templates/)

**For getting started:**
- [Getting Started Guide](docs/getting-started/README.md)
- [For AI Tools](docs/getting-started/for-ai-tools.md)

---

**Last Updated:** 2025-11-10
