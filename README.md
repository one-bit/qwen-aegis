# AEGIS for Qwen Code

> **Automated Epistemic Governance & Intelligence System** — multi-agent, multi-phase codebase auditing for Qwen Code.

<p align="center">
  <strong>12 engineering personas · 14 audit domains · 10 slash commands · adversarial review</strong>
</p>

### Install

```bash
qwen extensions install https://github.com/YOUR_USERNAME/qwen-aegis
```

---

## What Is AEGIS?

AEGIS deploys **specialized engineering personas** across **14 audit domains** to produce **adversarially reviewed, severity-ranked findings** on architecture, security, data integrity, scalability, maintainability, and more.

It is **not a linter or static analyzer** — it is a **coordinated reasoning system** that answers:

> *Can this system be trusted, survive change, scale, operate safely, and be understood by new engineers?*

## Origins & Attribution

This project is an adaptation of the original **[ChristopherKahler/aegis](https://github.com/ChristopherKahler/aegis)**, created by [Christopher Kahler](https://github.com/ChristopherKahler). The original was designed as a Claude Code extension. This repository adapts it for **[Qwen Code](https://github.com/QwenLM/qwen-code)**.

All framework specifications, agent personas, workflow definitions, domain knowledge, schemas, rules, and tool adapters are derived from the original AEGIS project. The adaptation work (tool name mapping, file reference resolution, YAML frontmatter conversion, Qwen Code extension manifest) was done to make the system natively compatible with Qwen Code's extension architecture.

**Original repository:** [ChristopherKahler/aegis](https://github.com/ChristopherKahler/aegis)
**Original license:** See the original repository for licensing terms.

## Quick Start

```
/aegis:init       # Initialize AEGIS in your project
/aegis:audit      # Start the diagnostic audit
/aegis:validate   # Verify framework integrity
```

## Slash Commands

| Command | Purpose |
|---------|---------|
| `/aegis:init` | Initialize AEGIS in a target project — creates `.aegis/` directory with state tracking |
| `/aegis:audit` | Start a full or targeted diagnostic audit — guides scope, domain, and tool selection |
| `/aegis:resume` | Resume an interrupted audit from the last checkpoint |
| `/aegis:status` | Display current audit state, phase progress, finding counts, and next action |
| `/aegis:report` | Generate or view the final audit report (requires all phases 0–4 complete) |
| `/aegis:validate` | Verify tool installation and framework integrity — read-only, safe to run anytime |
| `/aegis:transform` | Initiate the full remediation pipeline (Phases 6–8) on a completed audit |
| `/aegis:remediate` | Generate remediation playbooks for all findings or a specific domain |
| `/aegis:playbook` | Generate a remediation playbook for a single specific finding |
| `/aegis:guardrails` | Generate project rules for AI coding assistants (CLAUDE.md, .cursorrules, QWEN.md) |

## Architecture

### Diagnostic Phases (0–5)

| Phase | Name | Agent(s) | Purpose |
|-------|------|----------|---------|
| **0** | Context & Threat Modeling | Principal Engineer | Establish audit scope, constraints, and threat model |
| **1** | Automated Signal Gathering | External tools | Run SonarQube, Semgrep, Trivy, Gitleaks, Checkov, Syft, Grype, git-history |
| **2** | Deep Domain Audits | 10 domain agents | Each agent independently audits its domain(s) |
| **3** | Change Risk & Reality Gap | Staff Engineer, Reality Gap Analyst | Assess blast radius, config drift, git patterns |
| **4** | Adversarial Review | Devil's Advocate | Attack high-confidence claims, surface blind spots |
| **5** | Synthesis & Report | Principal Engineer | Synthesize final report from all findings and critique |

### Transform Phases (6–8) — Optional Remediation Pipeline

| Phase | Name | Purpose |
|-------|------|---------|
| **6** | Remediation Synthesis | Generate playbooks at 4 specificity layers (abstract → framework → language → project) |
| **7** | Change Risk Validation | Risk scoring, guardrail generation |
| **8** | Execution Planning | PAUL-compatible project artifacts with dependency sequencing |

## 14 Audit Domains

| Domain | Name | Subagent |
|--------|------|----------|
| **00** | Context & Intent | Principal Engineer |
| **01** | Architecture & System Design | aegis-architect |
| **02** | Data & State Integrity | aegis-data-engineer |
| **03** | Correctness & Logic | aegis-senior-app-engineer |
| **04** | Security | aegis-security-engineer |
| **05** | Compliance, Privacy & Governance | aegis-compliance-officer |
| **06** | Testing Strategy & Verification | aegis-test-engineer |
| **07** | Reliability & Resilience | aegis-sre |
| **08** | Scalability & Performance | aegis-performance-engineer |
| **09** | Maintainability & Code Health | aegis-senior-app-engineer |
| **10** | Operability & Developer Experience | aegis-sre |
| **11** | Change Risk & Evolvability | aegis-staff-engineer |
| **12** | Team, Ownership & Knowledge Risk | aegis-staff-engineer |
| **13** | Risk Synthesis & Forecasting | Principal Engineer |

## 12 Subagents

| Subagent | Domains | Phases | Key Tools |
|----------|---------|--------|-----------|
| `aegis-architect` | 01 | 1, 2, 3, 4 | SonarQube, Semgrep, git-history |
| `aegis-security-engineer` | 04 | 1, 2, 3, 4 | Semgrep, Gitleaks, Trivy, Syft, Grype, SonarQube |
| `aegis-data-engineer` | 02 | 1, 2, 3, 4 | SonarQube, Semgrep, Checkov |
| `aegis-senior-app-engineer` | 03, 09 | 1, 2, 3, 4 | SonarQube, Semgrep, git-history, Gitleaks |
| `aegis-test-engineer` | 06 | 1, 2, 3, 4 | SonarQube, Semgrep, git-history, Trivy |
| `aegis-sre` | 07, 10 | 1, 2, 3, 4 | SonarQube, Semgrep, Checkov, git-history, Gitleaks |
| `aegis-performance-engineer` | 08 | 1, 2, 3, 4 | SonarQube, Semgrep, git-history |
| `aegis-compliance-officer` | 05 | 1, 2, 3, 4 | Semgrep, Gitleaks, Checkov, Trivy, SonarQube, git-history |
| `aegis-staff-engineer` | 11, 12 | 2, 3 | git-history, SonarQube, Semgrep, Syft, Grype |
| `aegis-principal-engineer` | 00, 13 | 0, 5 | None (meta-reasoner) |
| `aegis-devils-advocate` | All (adversarial) | 4 | None (reads all findings) |
| `aegis-reality-gap-analyst` | Cross-domain | 3 | Checkov, git-history, SonarQube |

## Epistemic Schema

Every finding follows a strict **7-layer structure** to separate raw signals from opinions and enforce disciplined doubt:

1. **Observation** — Raw signal from tool or code review
2. **Evidence Source** — Where the signal came from
3. **Interpretation** — What the evidence means in context
4. **Assumptions** — Unstated premises in the reasoning
5. **Risk Statement** — What could go wrong if this is true
6. **Impact/Likelihood** — Severity (critical/high/medium/low/info) and confidence (1–5)
7. **Judgment** — The agent's conclusion

## Intervention Levels

| Level | Meaning | Auto-execute? |
|-------|---------|---------------|
| **Suggesting** | Advisory only, no changes proposed | No |
| **Planning** | Specific changes proposed, human review required | No |
| **Authorizing** | Changes ready for execution with approval | No |
| **Executing** | Human-approved, ready to apply | Only with explicit authorization |

**Hard safety rule:** AEGIS never auto-executes code. It proposes; humans approve and execute.

## External Tools (Optional)

AEGIS optionally integrates with OSS analysis tools for Phase 1 signal gathering. If not installed, AEGIS proceeds with manual/code-based analysis.

| Tool | Purpose | Install |
|------|---------|---------|
| SonarQube | Code quality, complexity, duplication | Docker or Cloud |
| Semgrep | Pattern-based security/correctness scanning | pip / brew |
| Trivy | Vulnerability and misconfiguration scanning | curl / brew |
| Gitleaks | Secrets detection in code and history | curl / brew / go install |
| Checkov | Infrastructure-as-code scanning | pip / brew |
| Syft | SBOM/dependency inventory | curl / brew |
| Grype | SBOM CVE matching | curl / brew |

## Extension Structure

```
qwen-aegis/
├── qwen-extension.json          # Qwen Code extension manifest
├── QWEN.md                       # Top-level context loaded by Qwen Code
├── README.md                     # This file
├── commands/                     # 10 slash commands
│   ├── audit.md
│   ├── guardrails.md
│   ├── init.md
│   ├── playbook.md
│   ├── remediate.md
│   ├── report.md
│   ├── resume.md
│   ├── status.md
│   ├── transform.md
│   └── validate.md
├── agents/                       # 12 Qwen subagent definitions
│   ├── aegis-architect.md
│   ├── aegis-compliance-officer.md
│   ├── aegis-data-engineer.md
│   ├── aegis-devils-advocate.md
│   ├── aegis-performance-engineer.md
│   ├── aegis-principal-engineer.md
│   ├── aegis-reality-gap-analyst.md
│   ├── aegis-security-engineer.md
│   ├── aegis-senior-app-engineer.md
│   ├── aegis-sre.md
│   ├── aegis-staff-engineer.md
│   └── aegis-test-engineer.md
├── skills/
│   └── aegis-audit/
│       └── SKILL.md              # Audit orchestration skill
├── src/                          # 83 framework reference files
│   ├── core/                     # Agent manifests, personas, workflows
│   ├── transform/                # Transform phase workflows, schemas, rules
│   ├── domains/                  # 14 audit domain knowledge modules
│   ├── schemas/                  # Shared schema definitions
│   ├── rules/                    # Shared rules
│   └── tools/                    # Tool adapter specifications
└── docs/                         # Standards and validation reports
```

## Adaptation Notes

This extension was adapted from the original AEGIS Claude Code extension with the following key changes:

| Claude Code | Qwen Code |
|---|---|
| `allowed-tools: [Read, Write, Edit, Bash, ...]` | `tools: [read_file, write_file, edit, run_shell_command, ...]` |
| `@~/.claude/aegis/` file references | `@${extensionPath}/src/` (Qwen extension variable) |
| `@~/.claude/commands/aegis/` | `@${extensionPath}/commands/` |
| `$ARGUMENTS` | `${arguments}` |
| Separate agent manifests + persona files | Combined into single Qwen subagent `.md` with YAML frontmatter |
| `~/.claude/` framework directory | `${extensionPath}/src/` within the extension |

Additionally, the `/aegis:guardrails` command was extended to support Qwen Code's `.qwen/QWEN.md` format alongside Claude's `CLAUDE.md` and Cursor's `.cursorrules`.

## Core Principles

- **Disciplined Doubt:** Optimizes for correctness under uncertainty, not helpful narratives.
- **Reality Gap Framework:** Detects divergence between code-as-written and system-as-run.
- **Language-Specific Failure Models:** Applies ecosystem-specific anti-pattern catalogs.
- **Disagreement Resolution:** Conflicts are first-class objects. The Principal Engineer must explicitly respond to every Devil's Advocate critique.
- **No Auto-Execution:** AEGIS produces plans only. Humans approve and execute.

## License

The original AEGIS framework is the work of [Christopher Kahler](https://github.com/ChristopherKahler). See [ChristopherKahler/aegis](https://github.com/ChristopherKahler/aegis) for licensing terms.

This adaptation is provided under the same licensing terms as the original.
