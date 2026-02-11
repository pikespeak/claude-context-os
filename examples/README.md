# Examples

Worked examples showing Claude Context OS in action.

## Software Dev Project

**Scenario:** A solo developer building a task queue API in Go over ~2 weeks.

Shows what `docs/summaries/` looks like after 2 sessions of real use:

```
examples/software-dev-project/
└── docs/summaries/
    ├── 00-project-brief.md              ← Project setup (Template 5)
    ├── source-api-notes.md              ← Processed design doc (Template 1)
    └── handoff-2026-02-09-core-impl.md  ← Session 1 → Session 2 handoff (Template 4)
```

**What to notice:**
- The **project brief** uses generic terms (Project Owner, not Client) — works for any domain
- The **source summary** has exact numbers (10ms p99, 256KB max, 30s timeout) that would get rounded in prose summarization
- The **handoff** lists every file created with exact paths, so the next session doesn't have to search
- The **"What NOT to Re-Read"** section prevents the next session from wasting context on already-processed docs
- **Conditional logic** is preserved in IF/THEN format, not flattened to simple statements

**How Session 3 would start:**
1. Claude reads `CLAUDE.md`
2. Reads `00-project-brief.md` — gets project context
3. Reads `handoff-2026-02-09-core-impl.md` — gets current state and next steps
4. Starts working on dead-letter logic (first item in "What the NEXT Session Should Do")
5. Does NOT re-read the original design doc — the source summary has everything needed

Total context consumed at session start: ~2K tokens (vs. re-reading everything from scratch).
