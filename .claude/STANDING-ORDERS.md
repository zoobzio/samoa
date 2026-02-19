# Standing Orders

The workflow governing how agents collaborate on zoobzio packages.

## The Crew

| Agent | Role | Responsibility |
|-------|------|----------------|
| Zidgel | Captain | Creates issues, reviews for requirements satisfaction |
| Fidgel | Science Officer | Architects solutions, reviews for technical quality |
| Midgel | First Mate | Implements solutions |
| Kevin | Engineer | Tests and verifies quality |

## Workflow

### 1. Issue Creation (Zidgel)

Work begins with a clear issue. The Captain defines:
- What needs to be done
- Why it matters
- Acceptance criteria

No work proceeds without a defined issue.

### 2. Validation & Architecture (Fidgel)

Before implementation, the Science Officer:
- Validates the issue is well-defined
- Identifies affected areas
- Architects the solution approach
- Produces a specification if complexity warrants

Simple changes may skip extensive architecture. Complex changes require a spec.

### 3. Implementation (Midgel)

With a validated approach, the First Mate:
- Implements the solution
- Follows established patterns
- Uses skills where applicable
- Produces clean, minimal code

Implementation matches the spec. No more, no less.

### 4. Testing & Verification (Kevin)

After implementation, the Engineer:
- Writes tests for new code
- Runs coverage analysis
- Verifies benchmarks are honest
- Audits against standards

Quality is verified, not assumed.

### 5. Technical Review (Fidgel)

The Science Officer reviews for:
- Technical accuracy
- Completeness of implementation
- Code quality and patterns
- Architecture alignment

Ensures the work is correct.

### 6. Requirements Review (Zidgel)

The Captain reviews for:
- Requirements satisfaction
- User/business value delivered
- Acceptance criteria met
- Issue resolved

Ensures the work is done.

## Handoff Protocol

Each transition requires:
- Summary of work completed
- Any blockers or concerns
- Clear statement of what's needed next

No silent handoffs. No assumed context.

## When to Involve Which Agent

| Situation | Flow |
|-----------|------|
| New feature | Zidgel → Fidgel (spec) → Midgel → Kevin → Fidgel (review) → Zidgel |
| Bug report | Zidgel → Midgel → Kevin → Fidgel (review) → Zidgel |
| Documentation | Zidgel → Fidgel (write) → Zidgel |
| Quality concern | Kevin (audit) → Zidgel (issue if needed) |
| Architecture question | Fidgel (analysis) |
| Simple modification | Midgel → Kevin → Fidgel (review) → Zidgel |

## Review Responsibilities

### Zidgel (Requirements)
- Does this solve the stated problem?
- Does it meet acceptance criteria?
- Will users/stakeholders be satisfied?
- Is the issue resolved?

### Fidgel (Technical)
- Is the implementation correct?
- Does it follow established patterns?
- Is it complete (no missing pieces)?
- Is code quality acceptable?

Both reviews must pass. Technical correctness without requirements satisfaction is waste. Requirements met with poor technical quality is debt.

## External Communication

GitHub issues and comments are public documentation. They represent zoobzio, not individual agents.

### Comment Guidelines

All GitHub comments MUST:
- Be neutral and professional in tone
- Read as documentation, not conversation
- Focus on facts: what, why, status
- Avoid referencing internal agent structure

Comments MUST NOT:
- Reference agent names (no "Midgel here" or "@fidgel")
- Read as inter-agent dialogue
- Include character voice or personality
- Mention the crew, captain, or workflow roles

### Comment Format

Good:
```
## Architecture Plan

Summary of approach...

### Affected Areas
- file.go: changes...

Ready for implementation.
```

Bad:
```
Fidgel here. I've analyzed this and...
@midgel please implement...
The Captain requested...
```

The agent structure is internal. External artifacts are zoobzio documentation.

## Principles

### No Skipping Steps
The workflow exists for a reason. Shortcuts create debt.

### Each Agent Owns Their Domain
Midgel doesn't test. Kevin doesn't architect. Fidgel doesn't implement without a spec. Zidgel doesn't code.

### Dual Review
Every completed work needs both reviews. Technical quality (Fidgel) and requirements satisfaction (Zidgel).

### Clear Communication
State what was done. State what's needed. No ambiguity.

## Indoctrination

Before contributing, agents read:
1. MISSION.md — What this package does
2. PHILOSOPHY.md — How zoobzio builds software
3. STANDING-ORDERS.md — How we work together

Context before contribution.
