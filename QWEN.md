# AEGIS — Automated Epistemic Governance & Intelligence System

AEGIS is a multi-agent, multi-phase codebase auditing framework. It deploys specialized engineering personas across 14 audit domains to produce adversarially reviewed, severity-ranked findings on architecture, security, data integrity, scalability, and maintainability.

## Architecture

AEGIS operates through phased, tool-augmented reasoning:

| Phase | Name | Purpose |
|-------|------|---------|
| 0 | Context & Threat Modeling | Establish audit scope, constraints, and threat model |
| 1 | Automated Signal Gathering | Run external analysis tools (SonarQube, Semgrep, Trivy, etc.) |
| 2 | Deep Domain Audits | 10 domain-specific agents audit independently |
| 3 | Change Risk & Reality Gap | Staff Engineer + Reality Gap assess blast radius and config drift |
| 4 | Adversarial Review | Devil's Advocate attacks high-confidence claims |
| 5 | Synthesis & Report | Principal Engineer synthesizes final report |
| 6 | Remediation Synthesis | Generate playbooks at 4 specificity layers |
| 7 | Change Risk Validation | Risk scoring, guardrail generation |
| 8 | Execution Planning | PAUL-compatible project artifacts |

## 14 Audit Domains

| Domain | Name | Agent |
|--------|------|-------|
| 00 | Context & Intent | Principal Engineer |
| 01 | Architecture & System Design | Architect |
| 02 | Data & State Integrity | Data Engineer |
| 03 | Correctness & Logic | Senior Application Engineer |
| 04 | Security | Security Engineer |
| 05 | Compliance, Privacy & Governance | Compliance Officer |
| 06 | Testing Strategy & Verification | Test Engineer |
| 07 | Reliability & Resilience | SRE |
| 08 | Scalability & Performance | Performance Engineer |
| 09 | Maintainability & Code Health | Senior Application Engineer |
| 10 | Operability & Developer Experience | SRE |
| 11 | Change Risk & Evolvability | Staff Engineer |
| 12 | Team, Ownership & Knowledge Risk | Staff Engineer |
| 13 | Risk Synthesis & Forecasting | Principal Engineer |

## Epistemic Schema

Every finding follows a strict 7-layer structure:

1. **Observation** — raw signal from tool or code review
2. **Evidence Source** — where the signal came from
3. **Interpretation** — what the evidence means
4. **Assumptions** — unstated premises in the reasoning
5. **Risk Statement** — what could go wrong
6. **Impact/Likelihood** — severity and probability
7. **Judgment** — the agent's conclusion

## Intervention Levels

| Level | Meaning |
|-------|---------|
| Suggesting | Advisory only, no changes proposed |
| Planning | Specific changes proposed, human review required |
| Authorizing | Changes ready for execution with approval |
| Executing | Human-approved, ready to apply |

## Safety Model

AEGIS **never auto-executes code**. It proposes; humans approve and execute.

## Slash Commands

| Command | Purpose |
|---------|---------|
| `/aegis.init` | Initialize AEGIS in a target project |
| `/aegis.audit` | Start a full or targeted diagnostic audit |
| `/aegis.resume` | Resume an interrupted audit |
| `/aegis.status` | Display current audit state and progress |
| `/aegis.report` | Generate or view the final audit report |
| `/aegis.validate` | Verify tool installation and framework integrity |
| `/aegis.transform` | Initiate the full remediation pipeline (Phases 6-8) |
| `/aegis.remediate` | Generate remediation playbooks for findings |
| `/aegis.playbook` | Generate a playbook for a single finding |
| `/aegis.guardrails` | Generate project rules for AI coding assistants |

## Quick Start

```
/aegis.init          # Set up the project
/aegis.audit         # Start the diagnostic audit
```

The audit creates `.aegis/` directory with state tracking, findings, and reports.

## External Tools

AEGIS optionally integrates with these OSS analysis tools for signal gathering (Phase 1):

| Tool | Purpose | Install |
|------|---------|---------|
| SonarQube | Code quality, complexity, duplication | Docker or Cloud |
| Semgrep | Pattern-based security/correctness | pip/brew |
| Trivy | Vulnerability and misconfiguration scanning | curl/brew |
| Gitleaks | Secrets detection | curl/brew/go |
| Checkov | Infrastructure-as-code scanning | pip/brew |
| Syft | SBOM/dependency inventory | curl/brew |
| Grype | SBOM CVE matching | curl/brew |

If tools are not installed, AEGIS proceeds with manual/code-based analysis using its agent personas.
