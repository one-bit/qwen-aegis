# AEGIS Audit Skill

Automated Epistemic Governance & Intelligence System — multi-agent, multi-phase codebase auditing.

## When to Use

Invoke this skill when the user wants to audit a codebase for architectural, security, data, testing, reliability, performance, maintainability, compliance, change risk, team, and overall risk concerns.

## How It Works

AEGIS deploys 12 specialized engineering personas across 14 audit domains. Each agent independently analyzes its domain, produces severity-ranked findings with confidence scores, and submits to adversarial review.

## Phases

### Phase 0: Context & Threat Modeling
- **Agent**: Principal Engineer
- **Goal**: Establish audit scope, constraints, and threat model
- **Input**: Repository structure, README, deployment configs, business context
- **Output**: Audit scope document, threat model, technology inventory

### Phase 1: Automated Signal Gathering
- **Tools**: SonarQube, Semgrep, Trivy, Gitleaks, Checkov, Syft, Grype, git-history
- **Goal**: Run external analysis tools and normalize signals
- **Output**: Signal data in `.aegis/signals/` per tool
- **Fallback**: If tools not installed, perform manual code analysis

### Phase 2: Deep Domain Audits
10 agents audit in parallel, each owning specific domains:

| Agent | Domains |
|-------|---------|
| Architect | 01 — Architecture & System Design |
| Data Engineer | 02 — Data & State Integrity |
| Senior App Engineer | 03 — Correctness & Logic, 09 — Maintainability & Code Health |
| Security Engineer | 04 — Security |
| Compliance Officer | 05 — Compliance, Privacy & Governance |
| Test Engineer | 06 — Testing Strategy & Verification |
| SRE | 07 — Reliability & Resilience, 10 — Operability & DevEx |
| Performance Engineer | 08 — Scalability & Performance |
| Staff Engineer | 11 — Change Risk & Evolvability, 12 — Team, Ownership & Knowledge Risk |

### Phase 3: Change Risk & Reality Gap
- **Agents**: Staff Engineer (change risk), Reality Gap Analyst (code-as-written vs system-as-run)
- **Goal**: Assess blast radius, git history patterns, config/runtime divergence
- **Input**: All Phase 2 findings, deployment configs, environment files

### Phase 4: Adversarial Review
- **Agent**: Devil's Advocate
- **Goal**: Attack high-confidence claims, surface blind spots, break consensus bias
- **Input**: ALL findings from ALL agents, ALL disagreement records
- **Output**: Critique set, blind spot analysis, missing investigation areas

### Phase 5: Synthesis & Report
- **Agent**: Principal Engineer
- **Goal**: Synthesize final report from all findings and adversarial critique
- **Output**: `.aegis/report/` containing:
  - `executive-summary.md`
  - `findings-by-domain.md`
  - `disagreements.md`
  - `remediation-roadmap.md`

## Epistemic Schema

Every finding MUST follow this 7-layer structure:

1. **Observation** — Raw signal from tool or code review
2. **Evidence Source** — Where the signal came from (file, tool output, pattern)
3. **Interpretation** — What the evidence means in context
4. **Assumptions** — Unstated premises in the reasoning
5. **Risk Statement** — What could go wrong if this is true
6. **Impact/Likelihood** — Severity (critical/high/medium/low/info) and confidence (1-5)
7. **Judgment** — The agent's conclusion

## Finding File Format

Each agent writes findings to `.aegis/findings/{agent-id}/`. Each finding is a markdown file:

```markdown
# Finding: {F-XX-NNN}

## Observation
{raw signal}

## Evidence Source
{where it came from}

## Interpretation
{what it means}

## Assumptions
{unstated premises}

## Risk Statement
{what could go wrong}

## Impact & Likelihood
- Severity: {critical|high|medium|low|info}
- Confidence: {1-5}
- Impact: {description}
- Likelihood: {description}

## Judgment
{conclusion}
```

## Disagreement Protocol

When two agents reach conflicting conclusions about overlapping concerns:
1. Create `.aegis/disagreements/{agent-a}-{agent-b}-NNN.md`
2. Record both positions with evidence
3. Identify the specific point of disagreement
4. Principal Engineer resolves during Phase 5

## Execution Flow

1. Check for existing `.aegis/STATE.md` — resume or start fresh
2. If no `.aegis/` exists, run `/aegis.init` first
3. For each phase in scope:
   - Execute the phase workflow
   - Run the phase checkpoint
   - Ask user: Continue, Pause, or Abort
4. After all phases: generate report with `/aegis.report`

## Available Subagents

All agents are defined in this extension's `agents/` directory:
- `aegis-architect` — Domain 01
- `aegis-security-engineer` — Domain 04
- `aegis-data-engineer` — Domain 02
- `aegis-senior-app-engineer` — Domains 03, 09
- `aegis-test-engineer` — Domain 06
- `aegis-sre` — Domains 07, 10
- `aegis-performance-engineer` — Domain 08
- `aegis-compliance-officer` — Domain 05
- `aegis-staff-engineer` — Domains 11, 12
- `aegis-principal-engineer` — Meta-reasoner, Phases 0 & 5
- `aegis-devils-advocate` — Adversarial review, Phase 4
- `aegis-reality-gap-analyst` — Code vs runtime divergence, Phase 3
