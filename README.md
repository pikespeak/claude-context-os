# Claude Context OS

> A CLAUDE.md that keeps Claude from forgetting everything when conversations get long.

---

## The problem

If you've used Claude for more than an afternoon, you know the drill. You're twenty messages into a session, Claude auto-compacts, and suddenly it's forgotten your file paths, your decisions, the numbers you spent an hour working out. You start over. Again.

People on Reddit have figured out pieces of this — "use a plan file," "summarize manually," "start new chats." These help. But they're individual fixes for a systemic problem.

I needed something that worked across multi-week projects without me babysitting context. So I built this.

## The core insight

Claude's default summarization loses five specific things:

1. **Precise numbers** get rounded or dropped
2. **Conditional logic** (IF/BUT/EXCEPT) collapses to simple statements
3. **Decision rationale** — the WHY evaporates, only the WHAT survives
4. **Cross-document relationships** flatten to single statements
5. **Open questions** get silently resolved as settled

If you ask Claude to "summarize what we've done," you get prose. And prose triggers the exact same compression that caused the problem. So the fix isn't better summarization — it's structured templates with explicit fields that mechanically prevent these five failure modes.

**Don't fight compaction. Design around it.**

## What it actually does

| Problem | What this does about it |
|---|---|
| Auto-compaction destroys context | You compact manually at 60-70%, and always write state to disk first |
| Claude forgets mid-conversation | State lives in files, not in conversation memory — survives any compaction |
| Numbers and rationale vanish | Templates have explicit fields for exact metrics, IF/THEN constraints, decision rationale + rejected alternatives |
| "Maximum length" after a few messages | One concern per session + handoffs between sessions |
| Context feels smaller than advertised | Explicit budget math: 200K total minus CLAUDE.md minus summaries = what you actually have left |
| Claude re-reads files it already processed | "What NOT to Re-Read" field in every handoff |

## What's in the box

```
CLAUDE.md                    (~3.5K tokens) — the operating system
templates/
  claude-templates.md        (on-demand only) — light + full templates,
                              3 subagent contracts, 4 workflow phases
.claude/commands/
  handoff.md                 /handoff — end a session cleanly
  process-doc.md             /process-doc — summarize an input document
  status.md                  /status — check project state
examples/
  software-dev-project/      worked example with filled-in templates
```

The core file is ~3.5K tokens. The templates are explicitly marked "do NOT read at session start" — they only get loaded when you need one. So they don't eat your window.

## The six rules

1. **Never bulk-read documents** — process one at a time, summarize it, move on
2. **Write state to disk, not conversation** — if it's important, it goes in a file
3. **Compact manually at 60-70%** — not when the system forces it. And always write state first.
4. **One concern per session** — research OR writing OR review, not all three
5. **CLAUDE.md is an index, not an encyclopedia** — project context goes in summary files, not here
6. **Monitor context continuously** — check every 3-4 exchanges, split proactively

## Templates

Two tiers — use **light** for quick projects (under 5 sessions), **full** for longer engagements:

| Template | Light (3 fields) | Full (8 fields) |
|---|---|---|
| **Source Document Summary** | Key facts, numbers, open items | + requirements as IF/THEN/BUT/EXCEPT, decisions with rationale, cross-doc relationships, verbatim quotes |
| **Session Handoff** | Done, next steps, don't re-read | + exact numbers, conditional logic, assumptions, open questions |
| **Decision Record** | Chose X because Y, rejected Z | + quantified impact, conditions, stakeholder input, downstream effects |
| **Analysis/Research Summary** | Full version only | Evidence tables, confidence levels, conditional conclusions, scope boundaries |
| **Project Brief** | Full version only | Project setup, success criteria, input tracking, phase status |

## Session handoffs

When a session ends:
1. Write a structured handoff to `docs/summaries/`
2. Next session reads that file and picks up where you left off
3. Old handoffs get archived automatically

Here's why this matters: by message 30 in a long conversation, each exchange carries ~50K tokens of history. A fresh session with a handoff summary starts at ~5K. That's 10x less context per message.

## Subagent contracts

When subagents return results as free-form prose, you get the same compression problem. So there are structured output contracts for document analysis, research, and review subagents. They return data in explicit fields, not paragraphs.

## Error recovery

Things go wrong. The CLAUDE.md has procedures for:
- **Context corruption** — save what you have, start fresh
- **Surprise auto-compaction** — re-read summaries, report what's missing
- **Task too big for context** — warn upfront, suggest phases

## Quick start

1. Copy `CLAUDE.md` to your project root
2. Copy the `templates/` directory
3. Copy the `.claude/commands/` directory (for `/handoff`, `/process-doc`, `/status` commands)
4. Edit the Identity section at the top of `CLAUDE.md` for your workflow
5. Start a Claude Code session

That's it. No dependencies, no install.

Check `examples/software-dev-project/` to see what filled-in templates look like in practice.

## Who this is for

People doing real work across multiple sessions — consultants, developers, researchers. Anyone whose projects span days or weeks, not minutes.

If you're just asking Claude a question, you don't need any of this.

## Architecture

```
project-root/
├── CLAUDE.md                          ← the OS (~3.5K tokens)
├── templates/
│   └── claude-templates.md            ← loaded on-demand only
├── .claude/
│   └── commands/                      ← slash commands (/handoff, /process-doc, /status)
├── docs/
│   ├── discovery/                     ← raw inputs
│   ├── research/                      ← research sources
│   ├── archive/                       ← processed files (don't re-read)
│   │   └── handoffs/                  ← old handoffs
│   └── summaries/                     ← this is your persistent brain
│       ├── 00-project-brief.md
│       ├── source-[filename].md       ← per-document summaries
│       ├── analysis-[topic].md        ← research outputs
│       ├── decision-[num]-[topic].md  ← decision records (permanent)
│       └── handoff-[date]-[topic].md  ← latest handoff only
└── output/
    ├── schemas/
    ├── prompts/
    └── deliverables/
```

The `docs/summaries/` directory is the whole point. Everything else is either raw input (archived after processing) or final output. The summaries are the project state.

## FAQ

**Doesn't the CLAUDE.md itself eat context?**
~3.5K tokens. You spend 3.5K on structure but save 50-100K by not re-reading source documents every session. You come out way ahead.

**Does this work on claude.ai web or just Claude Code?**
Designed for Claude Code and desktop, which have filesystem access. The ideas — structured summaries, session splitting, manual compaction timing — work anywhere, but the file-based state management needs a filesystem.

**Can I use this with my existing CLAUDE.md?**
Yes. This manages sessions and context. Your project-level CLAUDE.md handles project-specific rules. They coexist fine.

**This seems like overkill.**
For a single conversation, it is. For 2-5 session projects, use the lightweight templates — three fields each, minimal overhead. For multi-week projects where a lost session costs you two hours of re-work, the full templates pay for themselves immediately.

**Why not just tell Claude to "summarize better"?**
Because that triggers the exact same compression that caused the problem. Structured fields — CONFIRMED vs. OPEN vs. ASSUMED, exact numbers with sources, IF/THEN/BUT/EXCEPT — mechanically prevent it. It's an engineering fix, not a prompt.

## Contributing

PRs and issues welcome. If you adapt this for a specific workflow — academic research, software dev, creative writing — I'd like to see your fork.

## License

MIT

---

**Created by [Poorna Reddy](https://github.com/Arkya-AI)**. Built out of necessity, not theory — after too many sessions lost to compaction.
