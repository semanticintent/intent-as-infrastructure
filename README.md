# Intent-as-Infrastructure: When the Compiler Is Claude

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20681523.svg)](https://doi.org/10.5281/zenodo.20681523)

**Author:** Michael Shatny ([ORCID](https://orcid.org/0009-0006-2011-3258))

Where Methodology-as-Infrastructure encodes *how to think* into pre-compiled runtimes, Intent-as-Infrastructure encodes *what to reach into* as bounded vocabulary — and delegates compilation to an AI model. The compiler is Claude. The artifact is sovereign. The human is a first-class architectural primitive, not an edge case.

## Read the Paper

- [Intent-as-Infrastructure](docs/intent-as-infrastructure.md)

## Key Argument

| Paradigm | Execution model | Human role |
|---|---|---|
| Methodology-as-Infrastructure | Methodology → deterministic analysis | Receives output |
| **Intent-as-Infrastructure** | **Declaration → compiled execution → artifact** | **First-class architectural primitive** |

IaI is not a failure to satisfy MaI's determinism requirement. It is a deliberate choice — a paradigm where the compiler is intelligent and the human is the loop by design.

## The Four Properties

For a workflow system to qualify as Intent-as-Infrastructure:

1. **Declarative** — vocabulary is bounded and encodes intent (what to reach into), never implementation
2. **Compiled** — an AI model generates typed, executable artifacts from the declaration per run
3. **Sovereign artifacts** — every execution produces a human-readable, git-trackable finding record
4. **Human-as-primitive** — architecture reserves human judgment at the moment of consequence — by design, not reluctance

## Evidence Base

- **REACH** — Runtime Executable Adaptive C# Handler. `.reach` files declare workflow intent; Claude compiles to typed C# at runtime. 18 POC validations across Outlook, Teams, SQL Server, git, Playwright, FlaUI. Production timesheet and sprint health workflows. ([DOI](https://doi.org/10.5281/zenodo.20680385))
- **OCTO** — On-demand Contextual Task Orchestrator. Multiple REACH arms run simultaneously, surfaces a contextual decision interface, human closes the loop. Morning brief operational June 2026.

## The Paradigm Progression

| Paradigm | What it encodes | Human role |
|---|---|---|
| Infrastructure-as-Code | Server provisioning | Designs config |
| Policy-as-Code | Compliance rules | Sets thresholds |
| Methodology-as-Infrastructure | Analytical framework | Receives output |
| **Intent-as-Infrastructure** | **Workflow intent** | **First-class architectural primitive** |

## Related Work

- [Methodology-as-Infrastructure](https://doi.org/10.5281/zenodo.18946630) — the preceding paradigm; IaI extends the progression
- [Semantic Intent SSOT](https://doi.org/10.5281/zenodo.17114971)
- [REACH + OCTO Documentation](https://reach.semanticintent.dev)

## License

MIT
