# Intent-as-Infrastructure: When the Compiler Is Claude

**Michael Shatny**
ORCID: [0009-0006-2011-3258](https://orcid.org/0009-0006-2011-3258)

June 2026

---

## Abstract

Methodology-as-Infrastructure established that analytical frameworks can be compiled into deterministic execution layers — infrastructure other systems build upon. This paper proposes a related but distinct paradigm: **Intent-as-Infrastructure (IaI)**. Where MaI encodes *how to think* into pre-compiled runtimes, IaI encodes *what to reach into* as bounded vocabulary — and delegates compilation to an AI model. The distinction is not a failure of determinism. It is a deliberate architectural choice: the compiler is Claude, the artifact is sovereign, and the human is a first-class architectural primitive, not an edge case.

REACH (Runtime Executable Adaptive C# Handler) and OCTO (On-demand Contextual Task Orchestrator) serve as the primary case studies. REACH validates the single-arm pattern: one `.reach` file, Claude compiles typed C#, live systems answer, one `.reach-artifact` records the result. OCTO validates the orchestration layer: multiple arms run simultaneously, a contextual decision surface appears, and human judgment closes the loop at the moment it belongs — not as a reluctant safety check, as the architecture.

Eighteen POC validations across Windows live systems — Outlook COM, Teams Planner, SQL Server, Playwright, git, FlaUI — and two production workflows validate the approach in two days.

---

## 1. The Problem: Intent Has No Home in the Automation Stack

The automation stack as it exists today handles two categories of work well:

**Fully automated work.** Database backups, CI/CD pipelines, monitoring alerts. Deterministic, repeatable, no judgment required. These are the tasks that MaI addresses at the methodology layer — structure the reasoning, pre-compile it, let the pipeline run.

**Conversational AI work.** Open-ended questions, exploratory reasoning, content generation. The AI model handles the full interaction; the human reviews and acts on output. No structure required because no repeatable pipeline is implied.

The gap is **knowledge work** — the tasks that require reaching into live systems, assembling structured findings, and then applying human judgment at the moment that matters. The sprint health review. The morning inbox brief. The client status read. The insurance pre-authorization draft.

These tasks are:
- Too specific for general AI conversation (the system context is live, not described)
- Too judgment-dependent for full automation (the decision belongs to the person who knows the context)
- Too variable for pre-compiled runtimes (the sources change, the cadence changes, the relevance changes)

No paradigm addresses this gap precisely. Infrastructure-as-Code provisions servers. Policy-as-Code enforces rules. Methodology-as-Infrastructure executes analytical frameworks. None of them express *workflow intent* — the declaration of what to reach into, what to surface, and where the human enters.

Intent-as-Infrastructure is the answer to that gap.

---

## 2. The Concept: Intent-as-Infrastructure

### 2.1 Definition

**Intent-as-Infrastructure (IaI)** is the practice of encoding workflow intent as a bounded declarative vocabulary — delegating compilation to an AI model, producing sovereign artifacts at each execution, and reserving human judgment as a first-class architectural primitive at the moment of consequence.

IaI is not a variant of existing paradigms. It is the next row in the "-as-Infrastructure" progression:

| Paradigm | What it encodes | Execution model | Human role |
|---|---|---|---|
| Infrastructure-as-Code | Server provisioning | Declarative config → cloud resources | Designs config |
| Policy-as-Code | Compliance rules | Rule evaluation → pass/fail | Sets thresholds |
| Configuration-as-Code | System settings | Key-value → runtime behavior | Defines keys |
| Methodology-as-Infrastructure | Analytical framework | Methodology → deterministic analysis | Receives output |
| **Intent-as-Infrastructure** | **Workflow intent** | **Declaration → compiled execution → sovereign artifact** | **First-class architectural primitive** |

The practitioner writes the *what*. The compiler — an AI model — writes the *how*. The artifact records the *result*. The human decides what the artifact means.

### 2.2 The Four Required Properties

For a workflow system to qualify as Intent-as-Infrastructure, it must satisfy four properties:

**1. Declarative.** The vocabulary is bounded and encodes *what to reach into and why*, not *how to execute*. The practitioner expresses intent; implementation is not expressible in the vocabulary. A `.reach` file can name a source, a qualifier, an output format. It cannot specify which COM method to call, which retry policy to apply, or how to handle a null result.

**2. Compiled.** An AI model generates typed, executable artifacts from the declaration per run. No pre-built executor exists. The intent is the specification; the compiler reads it fresh on each invocation. Compilation is not bitwise identical across runs — but the vocabulary constrains the output space to what the intent declared.

**3. Sovereign artifacts.** Every meaningful execution produces a human-readable, git-trackable artifact. The artifact is the primary output — not a log, not a side effect, not a database record. It contains the finding, the source, the timestamp, and enough context to stand alone. A `.reach-artifact` is citable, diffable, and complete.

**4. Human-as-primitive.** The architecture explicitly reserves space for human judgment at the moment of consequence. This is not an approval step added when the system is nervous. It is the designed-in role of the human — the entity the system is built for, not routed around. OCTO's surface is not a modal dialog in the way of automation; it is the moment the system was always building toward.

### 2.3 Relationship to Methodology-as-Infrastructure

IaI and MaI share DNA — both are declarative, both produce sovereign artifacts, both cite their authors — but they differ on the two properties that matter most:

| Property | MaI | IaI |
|---|---|---|
| Determinism | Same input → same output | Same intent → compiled differently per run; output space constrained by vocabulary |
| Human role | Designed out of execution path | Designed into execution as first-class primitive |
| Executor | Pre-compiled runtime (CAL, Phoenix) | AI model (Claude), compiling per invocation |
| Closed loop | DRIFT measurement closes the loop | Human judgment closes the loop |
| Appropriate when | Decisions are threshold-driven, no judgment required | Decisions require context only the human has |

MaI is not a superset of IaI. IaI is not a weaker form of MaI. They answer different questions:

```
MaI asks: How do we remove interpretation from execution?
IaI asks: How do we ensure interpretation enters at the right moment,
          by the right entity?
```

A methodology where human interpretation at execution time is a flaw becomes MaI. A workflow where human interpretation at execution time is the architecture becomes IaI.

---

## 3. Case Study: REACH

### 3.1 The Vocabulary

REACH is a DSL for local workflow automation. A `.reach` file has a fixed vocabulary scoped to three concepts:

- **Sources** — what to reach into: `outlook.inbox`, `teams.planner`, `git.log`, `sql.query`, `screen.capture`, `file.read`
- **Qualifiers** — how to filter and frame: `since`, `where`, `limit`, `format`
- **Outputs** — what to produce: `surface`, `artifact`, `draft`, `send`

A `.reach` file cannot express control flow, error handling, COM Interop method signatures, or API parameters. The vocabulary has no `if`, no `try`, no `foreach`. This is not an omission — it is the constraint that makes the vocabulary intent-only.

A morning brief declaration looks like this:

```reach
sources:
  outlook.inbox since yesterday
  outlook.calendar today
  teams.planner sprint current

surface:
  format: brief
  close: "Good morning. {N} items need attention."

artifact: morning-brief
```

What this file does not contain: which Exchange endpoint to query, how to authenticate against Teams, what COM dispatch IDs to use, how to handle a slow Planner API response. The declaration expresses *what to reach into*. Nothing else.

### 3.2 The Compiler

Claude reads the `.reach` file and compiles a self-contained C# script — a single `.cs` file with `#:package` directives for any NuGet dependencies. The compiled output:

- Uses the Microsoft Graph API or Outlook COM for email
- Uses the Teams REST API or Planner COM for task data
- Uses LibGit2Sharp for git history
- Uses System.Data.SqlClient for SQL queries
- Uses Playwright for browser-based sources
- Uses FlaUI for Windows UI automation

Claude selects the appropriate library, writes the integration code, handles authentication, maps the results to the declared output format, and closes the artifact.

The practitioner runs:

```
dotnet script morning-brief.cs
```

The system reaches. The surface appears. The brief is delivered.

### 3.3 The Sovereign Artifact

Every REACH execution writes a `.reach-artifact` — a structured finding record. For the morning brief:

```
reach-artifact: morning-brief
generated: 2026-06-16T08:47:23Z
source: outlook.inbox, outlook.calendar, teams.planner
---
Inbox: 12 items since yesterday. 3 require action today.
Calendar: 4 meetings. First at 10:00am (Sprint Review).
Planner: 8 tasks active. 2 overdue. Sprint closes Friday.

Action items:
- Reply to Chen re: deployment window (flagged)
- Insurance pre-auth for Johnson (due today)
- Sprint demo prep — 2 items not committed
```

This artifact is:
- **Human-readable** — a colleague can read it and understand the situation
- **Git-trackable** — diff yesterday's artifact against today's; the delta is the news
- **Self-contained** — the sources, the timestamp, the finding, and the context are all present
- **Citable** — a finding in a `.reach-artifact` can be referenced in a decision record

The artifact is not a log. A log records what happened to the system. An artifact records what the system found for the human.

### 3.4 Validation

Eighteen POC cases validated the REACH pattern across Windows live systems in Day 1 of development:

| Source | Validation | Result |
|---|---|---|
| Outlook COM | Inbox scan, draft send, reply | Confirmed |
| Teams Planner | Sprint task list, overdue detection | Confirmed |
| SQL Server | Query, aggregate, surface result | Confirmed |
| git | Commit log, author filter, recent delta | Confirmed |
| Playwright | Web page extraction, structured output | Confirmed |
| FlaUI | Windows desktop UI automation | Confirmed |
| Screen capture | Vision analysis, structured finding | Confirmed |

Production workflow: timesheet for week of June 15 submitted via REACH. Sprint health read surfaced 30% after-hours activity across ten weeks. The finding was acted on. The closed loop — data to finding to artifact to decision — completed.

---

## 4. Case Study: OCTO

### 4.1 Orchestration as Intent

OCTO (On-demand Contextual Task Orchestrator) is the orchestration layer built on REACH arms. Where a `.reach` file declares intent for one source, a `.octo` file declares intent for multiple arms running simultaneously:

```octo
arms:
  - outlook.inbox since yesterday
  - outlook.calendar today
  - teams.planner sprint current
  - git.commits since monday

surface:
  type: morning-brief
  format: decision-surface

close:
  tone: grounded
  format: one line, what matters most today
```

The `.octo` vocabulary is the same constraint as `.reach` — sources, qualifiers, surface type, close tone. Not implementation, not coordination logic, not error routing. Claude compiles the orchestration: arms run concurrently, results are assembled, the surface is rendered, and the session closes with personality.

### 4.2 The Human Surface

The OCTO surface is not a dashboard. It is a decision interface built for one moment.

```
┌─────────────────────────────────────────────────────────┐
│  Morning Brief — Monday June 16, 08:47                 │
├─────────────────────────────────────────────────────────┤
│  Inbox (12 items)          Calendar (4 meetings)        │
│  ● Chen — deploy window    ● 10:00am Sprint Review      │
│  ● Johnson pre-auth (!)    ● 2:00pm Client call         │
│  ● Sprint demo prep        ● 4:30pm 1:1                 │
│                                                          │
│  Planner (8 tasks, 2 overdue)   Git (since Monday)      │
│  ● Auth middleware — due Fri    ● 7 commits, 3 authors   │
│  ● DB migration — blocked       ● 2 PRs open            │
│                                                          │
│  [Send confirmation to Chen]  [Draft Johnson pre-auth]  │
│  [Flag sprint items]          [Dismiss]                 │
│                                                          │
│  Grounded: Two items need action before 10am.           │
└─────────────────────────────────────────────────────────┘
```

The surface presents findings. The human decides which actions to take and in what order. Buttons map to `.reach` actions that execute on click. The human is not approving what the system decided — the human is making the decision the system assembled context for.

When the human clicks [Send confirmation to Chen], REACH executes a `.reach` arm: `outlook.email draft reply to Chen confirm window`. Claude compiles it. The draft appears in Outlook. The human sends it. The artifact records: "Confirmation sent to Chen at 08:49."

The system reached. The human decided. The artifact recorded.

### 4.3 The Cormorant Model vs the Data Center Model

Enterprise agentic frameworks inherit their architecture from server infrastructure:

```
Start it. Walk away. Hope it finishes.
```

A database backup is the right use case for that model — deterministic, bounded, no judgment required, binary success criteria. Always-on multi-agent loops burn tokens continuously: agents poll, evaluate, decide whether to act. Most cycles produce nothing. The budget is unpredictable. The human is routed around by design.

OCTO follows a different model — the cormorant:

```
Perch. Observe. Dive with precision. Surface with something. Done.
```

Nothing runs until intent is declared. Tokens burn per task, bounded by the declaration. The dive is intentional. The surface is meaningful. The human is the point.

```
Always-on enterprise agent    OCTO on-demand
──────────────────────────────────────────────────────────
Tokens: continuous        →   Tokens: per task — bounded
Human: edge case          →   Human: first-class primitive
Output: whenever          →   Output: when you ask
Control: low              →   Control: high
Complexity: accumulates   →   Complexity: never starts
```

Most agentic complexity exists to solve problems the architecture created. OCTO has none of these problems. It never created them.

### 4.4 The Dental Practice Extension

The OCTO pattern scales to practitioners who cannot write code. A dental receptionist's morning brief:

```octo
arms:
  - outlook.calendar today appointments
  - outlook.inbox confirmations outstanding
  - outlook.inbox cancellations since yesterday

surface:
  type: morning-brief
  persona: dental-receptionist

close:
  tone: warm
  format: "Good morning. {N} confirmations needed. {M} patients today."
```

The receptionist sees a surface at 8:45am. Confirmation buttons map to pre-drafted Outlook emails. Cancellations surface with waitlist slot detection. The human clicks; the system reaches; the artifact records.

The receptionist does not know what `.NET` is. They do not know what COM Interop is. They see: buttons, results, one click, done.

This is the extensibility property of IaI: the vocabulary is bounded, so the configuration is teachable. The compilation is Claude's responsibility, so the execution is reliable. The human-as-primitive property means the architecture scales to practitioners at any technical level — because the human is always where the design puts them: at the moment of decision.

---

## 5. Discussion

### 5.1 When Does a Workflow Qualify for IaI?

Not every workflow benefits from the IaI pattern. IaI is appropriate when:

- The workflow requires **reaching into live systems** with variable state (inbox, calendar, tasks, git history)
- The result requires **human judgment** at the point of action — not just at the point of configuration
- The **sources change over time** — new APIs, new tools, new data schemas — but the *intent* is stable
- **Token cost per task** is more appropriate than always-on monitoring
- The practitioner is **one person** with deep context, not a platform serving many users

IaI is not appropriate when:
- Decisions are bitwise deterministic and judgment-free (use MaI)
- The workflow requires continuous monitoring across long time windows (use always-on agents)
- The domain is a closed platform with no live system access (use a pre-built integration)
- The compilation reliability of an AI model is insufficient for the stakes involved

### 5.2 The Vocabulary Constraint as Design

The IaI vocabulary's narrowness is its primary mechanism. A `.reach` file that cannot express `if/else` cannot accidentally become a program. A practitioner who cannot write control flow cannot produce a broken control flow.

This constraint has three effects:

1. **The practitioner is forced to express intent.** When the vocabulary cannot express implementation, the declaration is pure intent. The discipline is the design.
2. **The compiler has a constrained problem space.** Claude compiles from a bounded declaration, not from open-ended prose. The vocabulary is a contract between the practitioner and the compiler.
3. **The artifact is interpretable.** A `.reach` file is readable by anyone who understands the domain. No programming knowledge is required to read `outlook.inbox since yesterday`. The vocabulary's simplicity is the artifact's legibility.

This mirrors MaI's keyword constraint. CAL's 10 keywords cannot express arbitrary computation — they can only express cascade analysis methodology. The constraint forces rigorous application of the methodology. REACH's vocabulary constraint forces rigorous expression of intent.

### 5.3 The Self-Limiting Property and AI Compilation

A criticism of AI-compiled execution is reliability: Claude's compilation of a `.reach` file is not bitwise identical across runs. A `.reach` arm that fetches Outlook inbox items might compile differently on Monday than it does on Friday.

This is correct. And it is acceptable within the IaI model for four reasons:

1. **The intent is the specification.** The compiled output must satisfy the intent declaration. If the intent says `outlook.inbox since yesterday`, the output must be inbox items from the past 24 hours. Whether Claude uses `GraphServiceClient` or Outlook COM to achieve this is an implementation detail.
2. **The artifact is the validation.** The `.reach-artifact` records what was found. If the artifact contains the correct data, the compilation was correct. The human reviews the artifact, not the compiled C#.
3. **The vocabulary constrains the output space.** Bounded inputs produce bounded outputs. A vocabulary that cannot express arbitrary computation produces compilations that cannot express arbitrary computation. The error surface is constrained by the declaration.
4. **The human is the loop.** Unlike MaI, IaI does not require closed-loop automated correction. The human sees the surface, evaluates the finding, and decides. The human is the validation mechanism — and they are designed in, not bolted on.

---

## 6. Conclusion

Intent-as-Infrastructure proposes that the gap between fully automated pipelines and open-ended AI conversation is not a failure of either — it is a space that requires a distinct paradigm.

When the workflow requires live system reach, human judgment at the moment of consequence, and sovereign artifacts as the record of what was found, the appropriate model is:

- Declare the intent in a bounded vocabulary
- Delegate compilation to an AI model
- Record the finding as a sovereign artifact
- Reserve the decision for the human

REACH validates the single-arm pattern. OCTO validates the orchestration layer. Eighteen POC cases and two production workflows validate the approach across Windows live systems. The vocabulary, the compiler, the artifact, and the human surface all function as a coherent system.

The field is moving rapidly toward full autonomy — always-on agents, autonomous decision-making, human-as-edge-case architectures. IaI is the precise, documented counterposition: not because autonomy is wrong in its domain, but because human knowledge work is not that domain.

The human is not the bottleneck. The human is the point.
Build the system around that.

---

## References

[1] M. Shatny, "REACH: Runtime Executable Adaptive C# Handler and OCTO: On-demand Contextual Task Orchestrator," 2026. DOI: [10.5281/zenodo.20680385](https://doi.org/10.5281/zenodo.20680385)

[2] M. Shatny, "Methodology-as-Infrastructure: From Framework to Runtime," 2026. DOI: [10.5281/zenodo.18946631](https://doi.org/10.5281/zenodo.18946631)

[3] M. Shatny, "Semantic Intent SSOT," 2025. DOI: [10.5281/zenodo.17114972](https://doi.org/10.5281/zenodo.17114972)

[4] M. Shatny, "OCTO vs Enterprise Agentic Frameworks: A Positioning Document," 2026. Available: [https://reach.semanticintent.dev/octo/positioning](https://reach.semanticintent.dev/octo/positioning)

---

## Appendix A: OCTO vs Enterprise Agentic Frameworks — The Full Comparison

The positioning document [4] provides the complete comparison. Key dimensions reproduced here for completeness.

**The two questions:**

```
Enterprise agentic frameworks ask:
  "How do we build autonomous AI systems at scale?"

OCTO asks:
  "How does one person work better tomorrow morning?"
```

Both questions are valid. The second is asked by far more people. Almost nobody is answering it well.

**The philosophy comparison:**

| Enterprise agentic frameworks | OCTO |
|---|---|
| Solves coordination at scale | Solves one person's workflow |
| Requires infrastructure | Requires .NET 10 SDK |
| Tokens burn continuously | Tokens burn per task — bounded |
| Human routed around | Human designed in — first-class |
| You operate the system | You develop the practice |
| Framework is the product | Practice is the product |

**The architectural distinction:**

Most agentic complexity exists to solve problems the architecture created:

```
Add agents to handle scale
  ↓
Add orchestration to manage the agents
  ↓
Add monitoring to watch the orchestration
  ↓
Add guardrails to prevent failures
  ↓
Add infrastructure to run all of it
  ↓
Add budget management for continuous token consumption
  ↓
Add failure recovery for runaway loops
```

OCTO has none of these problems. It never created them. The morning brief does not need to scale to 10,000 users. It needs to work for you. At 8:45am. Every morning. Reliably. Without burning your token budget while you sleep.

**The use case split:**

| Good fit for always-on enterprise agents | Good fit for OCTO on-demand |
|---|---|
| Continuous monitoring (infrastructure, pipelines) | Knowledge work (judgment is the product) |
| High volume processing (batch jobs) | Personal workflows (one practitioner, deep context) |
| Closed domains (clear success criteria, no judgment) | Consequential actions (email, timesheets, stakeholder comms) |
| Platform products (serving many users) | Variable cadence (some days 3 items, some days 12) |

**The IaI alignment:**

OCTO is the first expression of Intent-as-Infrastructure where the human moment is not optional — it is the methodology. The decision surface is not a safety valve. It is the architecture. The human is the loop.
