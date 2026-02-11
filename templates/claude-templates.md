# Claude Templates — On-Demand Reference

> **Do NOT read this file at session start.** Read it only when you need to write a summary, handoff, decision record, or subagent output. This file is referenced from CLAUDE.md.

---

## Lightweight Templates (Projects Under 5 Sessions)

For smaller projects — quick coding tasks, short research, bug investigations — the full templates below are overkill. Use these instead.

### Light Source Summary

**Write to:** `./docs/summaries/source-[filename].md`

```markdown
# Source: [Document Name]
**Processed:** [YYYY-MM-DD] | **Confidence:** [high/medium/low]

## Key Facts & Numbers
- [fact or metric]: [exact value]
- [fact or metric]: [exact value]

## Requirements (IF/THEN format)
- IF [condition] THEN [requirement] — priority: [must/should/nice]

## Open Items
- OPEN: [unresolved question]
- ASSUMED: [assumption made] — verify with [whom/how]
```

### Light Session Handoff

**Write to:** `./docs/summaries/handoff-[YYYY-MM-DD]-[topic].md`

```markdown
# Handoff: [Topic]
**Date:** [YYYY-MM-DD] | **Focus:** [one sentence]

## Done
- [what was completed] → `[file path]`

## In Progress
- [what's mid-stream] — stopped at [specific point], next: [specific action]

## Next Session Should
1. [specific action with file paths]
2. [specific action]

## Open Questions
- [ ] [question] — impacts [what]

## Don't Re-Read
- `[file]` — summarized in `[summary file]`
```

### Light Decision Record

**Write to:** `./docs/summaries/decision-[number]-[topic].md`

```markdown
# Decision: [What Was Decided]
**Date:** [YYYY-MM-DD] | **Status:** CONFIRMED / PROVISIONAL

- CHOSE: [option] BECAUSE [reason]
- REJECTED: [alternative] BECAUSE [reason]
- REVISIT IF: [trigger condition]
```

> **When to upgrade to full templates:** If your project grows past 5 sessions, has multiple stakeholders, or involves processing 4+ documents — switch to the full templates below. The light versions don't capture cross-document relationships, stakeholder input, or detailed conditional logic.

---

## Full Templates

---

## Template 1: Source Document Summary

**Use when:** Processing any input document (requirements spec, research report, design doc, brief, proposal)

**Write to:** `./docs/summaries/source-[filename].md`

```markdown
# Source Summary: [Original Document Name]
**Processed:** [YYYY-MM-DD]
**Source Path:** [exact file path]
**Archived From:** [original path, if moved to docs/archive/]
**Document Type:** [brief / requirements / research / proposal / transcript / other]
**Confidence:** [high = I understood everything / medium = some interpretation needed / low = significant gaps]

## Exact Numbers & Metrics
<!-- List EVERY specific number, dollar amount, percentage, date, count, measurement.
     Do NOT round. Do NOT paraphrase. Copy exactly as stated in source. -->
- [metric]: [exact value] (page/section reference if available)
- [metric]: [exact value]

## Key Facts (Confirmed)
<!-- Only include facts explicitly stated in the document. Tag source. -->
- [fact] — stated in [section/page]
- [fact] — stated in [section/page]

## Requirements & Constraints
<!-- Use IF/THEN/BUT/EXCEPT format to preserve conditional logic -->
- REQUIREMENT: [what is needed]
  - CONDITION: [when/if this applies]
  - CONSTRAINT: [limitation or exception]
  - PRIORITY: [must-have / should-have / nice-to-have / stated by whom]

## Decisions Referenced
<!-- Any decisions mentioned in the document -->
- DECISION: [what was decided]
  - RATIONALE: [why, as stated in document]
  - ALTERNATIVES MENTIONED: [what else was considered]
  - DECIDED BY: [who, if stated]

## Relationships to Other Documents
<!-- How this document connects to other known project documents -->
- SUPPORTS: [other document/decision it reinforces]
- CONTRADICTS: [other document/decision it conflicts with]
- DEPENDS ON: [other document/decision it requires]
- UPDATES: [other document/decision it supersedes]

## Open Questions & Ambiguities
<!-- Things that are NOT resolved in this document -->
- UNCLEAR: [what is ambiguous] — needs clarification from [whom]
- ASSUMED: [interpretation made] — verify with [whom]
- MISSING: [information referenced but not provided]

## Verbatim Quotes Worth Preserving
<!-- 2-5 direct quotes that capture key language, priorities, or constraints.
     Useful for aligning deliverables with the original author's intent. -->
- "[exact quote]" — [speaker/author], [context]
```

---

## Template 2: Analysis / Research Summary

**Use when:** Completing competitive analysis, market research, technical evaluation

**Write to:** `./docs/summaries/analysis-[topic].md`

```markdown
# Analysis Summary: [Topic]
**Completed:** [YYYY-MM-DD]
**Analysis Type:** [competitive / market / technical / financial / feasibility]
**Sources Used:** [list source paths or URLs]
**Confidence:** [high / medium / low — and WHY this confidence level]

## Core Finding (One Sentence)
[Single sentence: the most important conclusion]

## Evidence Base
<!-- Specific data points supporting the finding. Exact numbers only. -->
| Data Point | Value | Source | Date of Data |
|-----------|-------|--------|-------------|
| [metric]  | [exact value] | [source] | [date] |

## Detailed Findings
### Finding 1: [Name]
- WHAT: [the finding]
- SO WHAT: [why it matters for this project]
- EVIDENCE: [specific supporting data]
- CONFIDENCE: [high/medium/low]

### Finding 2: [Name]
[same structure]

## Conditional Conclusions
<!-- Use IF/THEN format -->
- IF [condition], THEN [conclusion], BECAUSE [evidence]
- IF [alternative condition], THEN [different conclusion]

## What This Analysis Does NOT Cover
<!-- Explicit scope boundaries to prevent future sessions from over-interpreting -->
- [topic not addressed and why]
- [data not available]

## Recommended Next Steps
1. [action] — priority [high/medium/low], depends on [what]
2. [action]
```

---

## Template 3: Decision Record

**Use when:** Any significant decision is made during a session

**Write to:** `./docs/summaries/decision-[number]-[topic].md`

```markdown
# Decision Record: [Short Title]
**Decision ID:** DR-[sequential number]
**Date:** [YYYY-MM-DD]
**Status:** CONFIRMED / PROVISIONAL / REQUIRES_VALIDATION

## Decision
[One clear sentence: what was decided]

## Context
[2-3 sentences: what situation prompted this decision]

## Rationale
- CHOSE [option] BECAUSE: [specific reasons with data]
- REJECTED [alternative 1] BECAUSE: [specific reasons]
- REJECTED [alternative 2] BECAUSE: [specific reasons]

## Quantified Impact
- [metric affected]: [expected change with numbers]
- [cost/time/resource implication]: [specific figures]

## Conditions & Constraints
- VALID IF: [conditions under which this decision holds]
- REVISIT IF: [triggers that should cause reconsideration]
- DEPENDS ON: [upstream decisions or facts this relies on]

## Stakeholder Input
- [name/role]: [their stated position, if known]

## Downstream Effects
- AFFECTS: [what other decisions, documents, or deliverables this impacts]
- REQUIRES UPDATE TO: [specific files or deliverables that need revision]
```

---

## Template 4: Session Handoff

**Use when:** A session is ending (context limit approaching OR phase complete)

**Write to:** `./docs/summaries/handoff-[YYYY-MM-DD]-[topic].md`

**LIFECYCLE**: After writing a new handoff, move the PREVIOUS handoff to `docs/archive/handoffs/`.

```markdown
# Session Handoff: [Topic]
**Date:** [YYYY-MM-DD]
**Session Duration:** [approximate]
**Session Focus:** [one sentence]
**Context Usage at Handoff:** [estimated percentage if known]

## What Was Accomplished
<!-- Be specific. Include file paths for every output. -->
1. [task completed] → output at `[file path]`
2. [task completed] → output at `[file path]`

## Exact State of Work in Progress
<!-- If anything is mid-stream, describe exactly where it stopped -->
- [work item]: completed through [specific point], next step is [specific action]
- [work item]: blocked on [specific issue]

## Decisions Made This Session
<!-- Reference decision records if created, otherwise summarize here -->
- DR-[number]: [decision] (see `./docs/summaries/decision-[file]`)
- [Ad-hoc decision]: [what] BECAUSE [why] — STATUS: [confirmed/provisional]

## Key Numbers Generated or Discovered This Session
<!-- Every metric, estimate, or figure produced. Exact values. -->
- [metric]: [value] — [context for where/how this was derived]

## Conditional Logic Established
<!-- Any IF/THEN/BUT/EXCEPT reasoning that future sessions must respect -->
- IF [condition] THEN [approach] BECAUSE [rationale]

## Files Created or Modified
| File Path | Action | Description |
|-----------|--------|-------------|
| `[path]`  | Created | [what it contains] |
| `[path]`  | Modified | [what changed and why] |

## What the NEXT Session Should Do
<!-- Ordered, specific instructions. The next session starts by reading this. -->
1. **First**: [specific action with file paths]
2. **Then**: [specific action]
3. **Then**: [specific action]

## Open Questions Requiring User Input
<!-- Do NOT proceed on these without explicit user confirmation -->
- [ ] [question] — impacts [what downstream deliverable]
- [ ] [question]

## Assumptions That Need Validation
<!-- Things treated as true this session but not confirmed -->
- ASSUMED: [assumption] — validate by [method/person]

## What NOT to Re-Read
<!-- Prevent the next session from wasting context on already-processed material -->
- `[file path]` — already summarized in `[summary file path]`
```

---

## Template 5: Project Brief (Initial Setup)

**Use when:** Creating the 00-project-brief.md at project start

**Write to:** `./docs/summaries/00-project-brief.md`

```markdown
# Project Brief: [Project Name]
**Created:** [YYYY-MM-DD]
**Last Updated:** [YYYY-MM-DD]

## Project Owner
- **Name:** [your name / team name]
- **Domain:** [industry, field, or tech stack]
- **Context:** [solo / team / client work — any relevant background]
- **Key Contacts:** [collaborators, stakeholders, or reviewers if any]

## Project
- **Type:** [software development / research / writing / consulting / agent development / other]
- **Scope:** [one paragraph description]
- **Target Deliverable:** [specific output expected]
- **Timeline:** [deadline if known]
- **Resource Context:** [budget, compute, team size — if relevant]

## Input Documents
| Document | Path | Processed? | Summary At |
|----------|------|-----------|------------|
| [name]   | `[path]` | Yes/No | `[summary path]` |

## Success Criteria
- [criterion 1]
- [criterion 2]

## Known Constraints
- [constraint 1]
- [constraint 2]

## Project Phase Tracker
| Phase | Status | Summary File | Date |
|-------|--------|-------------|------|
| Discovery | Not Started / In Progress / Complete | `[path]` | |
| Strategy | Not Started / In Progress / Complete | `[path]` | |
| Deliverable Draft | Not Started / In Progress / Complete | `[path]` | |
| Review & Polish | Not Started / In Progress / Complete | `[path]` | |
```

---

## Subagent Output Contracts

**CRITICAL: When subagents return results to the main agent, unstructured prose causes information loss. These output contracts define the EXACT format subagents must return.**

### Contract for Document Analysis Subagent

```
=== DOCUMENT ANALYSIS OUTPUT ===
SOURCE: [file path]
TYPE: [document type]
CONFIDENCE: [high/medium/low]

NUMBERS:
- [metric]: [exact value]
[repeat for all numbers found]

REQUIREMENTS:
- REQ: [requirement] | CONDITION: [if any] | PRIORITY: [level] | CONSTRAINT: [if any]
[repeat]

DECISIONS_REFERENCED:
- DEC: [what] | WHY: [rationale] | BY: [who]
[repeat]

CONTRADICTIONS:
- [this document says X] CONTRADICTS [other known fact Y]
[repeat or NONE]

OPEN:
- [unresolved item] | NEEDS: [who/what to resolve]
[repeat or NONE]

QUOTES:
- "[verbatim]" — [speaker], [context]
[repeat, max 5]

=== END OUTPUT ===
```

### Contract for Research/Analysis Subagent

```
=== RESEARCH OUTPUT ===
QUERY: [what was researched]
SOURCES: [list]
CONFIDENCE: [high/medium/low] BECAUSE [reason]

CORE_FINDING: [one sentence]

EVIDENCE:
- [data point]: [exact value] | SOURCE: [where] | DATE: [when]
[repeat]

CONCLUSIONS:
- IF [condition] THEN [conclusion] | EVIDENCE: [reference]
[repeat]

GAPS:
- [what was not found or not covered]
[repeat or NONE]

NEXT_STEPS:
- [recommended action] | PRIORITY: [level]
[repeat]

=== END OUTPUT ===
```

### Contract for Review/QA Subagent

```
=== REVIEW OUTPUT ===
REVIEWED: [file path or deliverable name]
AGAINST: [what standard — spec, requirements, style guide]

PASS: [yes/no/partial]

ISSUES:
- SEVERITY: [critical/major/minor] | ITEM: [description] | LOCATION: [where in document] | FIX: [suggested resolution]
[repeat]

MISSING:
- [expected content/section not found] | REQUIRED_BY: [which requirement]
[repeat or NONE]

INCONSISTENCIES:
- [item A says X] BUT [item B says Y] | RESOLUTION: [suggested]
[repeat or NONE]

STRENGTHS:
- [what works well — for positive reinforcement in iteration]
[max 3]

=== END OUTPUT ===
```

---

## Phase-Based Workflow Templates

### Template A: Software Development

```
Phase 1: Discovery & Design
├── Process requirements, specs, existing code docs → Source Summaries
├── Identify unknowns and technical constraints → flag as OPEN items
├── Create Decision Records for architecture/tool choices
├── Write: ./docs/summaries/01-design-complete.md (Handoff Template)
├── → Suggest new session for Phase 2

Phase 2: Core Implementation
├── Read summaries + design decisions only (NOT raw specs)
├── Implement core logic and data layer
├── Write tests for critical paths
├── Write: ./docs/summaries/02-core-complete.md (Handoff Template)
├── → Suggest new session for Phase 3

Phase 3: Integration & Edge Cases
├── Read implementation handoff + decision records only
├── Wire up remaining components, APIs, UI
├── Handle error cases, validation, edge cases
├── Write: ./docs/summaries/03-integration-complete.md (Handoff Template)
├── → Suggest new session for Phase 4

Phase 4: Testing & Polish
├── Read integration handoff + original requirements summary
├── Comprehensive test coverage
├── Performance validation against requirements
├── Documentation, cleanup, deployment setup
├── QA check against original spec using Review/QA Output Contract
```

### Template B: Enterprise Sales / Consulting Deliverable

```
Phase 1: Discovery & Input Processing
├── Process all client documents → Source Document Summaries
├── Identify gaps in information → flag as OPEN items
├── Create Decision Records for any choices made
├── Write: ./docs/summaries/01-discovery-complete.md (Handoff Template)
├── → Suggest new session for Phase 2

Phase 2: Strategy & Positioning
├── Read summaries only (NOT source documents)
├── Competitive positioning analysis → Analysis Summary
├── Value proposition development
├── ROI framework construction with EXACT numbers
├── Write: ./docs/summaries/02-strategy-complete.md (Handoff Template)
├── → Suggest new session for Phase 3

Phase 3: Deliverable Creation
├── Read strategy summary + project brief only
├── Draft deliverable (proposal / deck / workshop plan)
├── Output to: ./output/deliverables/
├── Write: ./docs/summaries/03-deliverable-draft.md (Handoff Template)
├── → Suggest new session for Phase 4

Phase 4: Review & Polish
├── Read draft deliverable + strategy summary
├── Quality review using Review/QA Output Contract
├── Final edits and formatting
├── Output final version to: ./output/deliverables/
```

### Template C: Agent/Application Development

```
Phase 1: Requirements → Spec
├── Process all input documents → Source Document Summaries
├── Generate structured specification
├── Output: ./output/SPEC.md
├── Write: ./docs/summaries/01-spec-complete.md (Handoff Template)
├── → Suggest new session for Phase 2

Phase 2: Architecture → Schema
├── Read SPEC.md + summaries only
├── Design data model
├── Define agent behaviors and workflows
├── Output: ./output/schemas/data-model.yaml
├── Output: ./output/schemas/agent-definitions.yaml
├── Write: ./docs/summaries/02-architecture-complete.md (Handoff Template)
├── → Suggest new session for Phase 3

Phase 3: Prompts → Integration
├── Read schemas + spec only
├── Write system prompts for each agent
├── Map API integrations and data flows
├── Output: ./output/prompts/[agent-name].md (one per agent)
├── Output: ./output/schemas/integration-map.yaml
├── Write: ./docs/summaries/03-prompts-complete.md (Handoff Template)
├── → Suggest new session for Phase 4

Phase 4: Assembly → Package
├── Read all output files
├── Assemble complete application package
├── Generate deployment/setup instructions
├── Output: ./output/deliverables/[project]-complete-package/
├── QA check against original spec using Review/QA Output Contract
```

### Template D: Hybrid (Sales + Agent Development)

```
Phase 1: Client Discovery → Summaries
Phase 2: Solution Design → Architecture + Schema
Phase 3a: Client-Facing Deliverable (proposal/deck)
Phase 3b: Internal Technical Package (schemas/prompts)
Phase 4: Review both tracks against each other for consistency
```

---

## End of Templates

**Return to your task after reading the template(s) you need. Do not keep this file in active context.**
