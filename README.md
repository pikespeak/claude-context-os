# Claude Context OS v4

> New CLAUDE.md that solves the compaction/context loss problem and mimics persistent memory through effective session handoff in token efficient manner

---

## What changed in v4

v3 got 150+ stars. Then I read 9 peer-reviewed papers on how LLMs actually process system prompts. Turns out v3 was working against itself.

**The research said three things that broke my assumptions:**

1. **LLMs track 5-10 rules before performance degrades.** v3 had 15+. Some rules were silently dropped every session. ([arXiv:2409.10715](https://arxiv.org/abs/2409.10715))

2. **Explanatory prose actively interferes with instruction compliance.** The "Why It Matters" sections in v3 weren't neutral — they competed with the actual rules for Claude's attention. Topically related filler is worse than random noise. ([Chroma Context Rot, 2025](https://research.trychroma.com/context-rot))

3. **Claude 4.6 overtriggers on emphatic language.** "CRITICAL", "NEVER", "YOU MUST" worked for Claude 3.x. The newer models are more responsive to system prompts and now overcorrect when they see aggressive language. ([Anthropic Claude 4 Best Practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices))

**The result:** v4 is 47 lines and 7 rules. Down from 327 lines and 15+ rules. Every design choice maps to a specific research finding.

| | v3 | v4 |
|---|---|---|
| Lines | 327 | 47 |
| Rules | 15+ | 7 |
| Explanatory prose | Heavy | Zero |
| Language style | "CRITICAL / NEVER / MUST" | Plain imperatives |
| Session startup | Read all summaries | Read only what handoff references |
| Reference content | Embedded in CLAUDE.md | Moved to docs/context/ (loaded on demand) |
| Templates | Same | Same + task template + handoff index |

## The problem (unchanged from v3)

Claude's default summarization loses five specific things:

1. **Precise numbers** get rounded or dropped
2. **Conditional logic** (IF/BUT/EXCEPT) collapses to simple statements
3. **Decision rationale** — the WHY evaporates, only the WHAT survives
4. **Cross-document relationships** flatten to single statements
5. **Open questions** get silently resolved as settled

The fix isn't better summarization — it's structured templates with explicit fields that mechanically prevent these five failure modes.

## What's in the box

```
CLAUDE.md                    (47 lines, ~1,200 tokens) — the operating system
templates/
  claude-templates.md        (on-demand only) — 6 templates, 3 subagent contracts,
                              3 workflow phases, task definition format
docs/
  context/                   (on-demand) — reference files loaded by work type
    processing-protocol.md   — how to handle multiple documents
    archive-rules.md         — summary lifecycle and file management
    project-structure.md     — full directory tree + scaffold commands
    subagent-rules.md        — when to use subagents vs. main agent
    client-briefs/           — per-client context (you populate these)
.claude/commands/
  handoff.md                 /handoff — end a session cleanly
  process-doc.md             /process-doc — summarize an input document
  status.md                  /status — check project state
examples/
  software-dev-project/      worked example with filled-in templates
CLAUDE-v3.md                 previous version (preserved for reference)
```

## The 7 rules

1. **Do not mix client contexts** in one session
2. **Write state to disk, not conversation** — summaries to docs/summaries/ using structured templates
3. **Before compaction or session end**, write everything to disk — numbers, decisions, questions, file paths, next actions
4. **When switching work types**, write a handoff and suggest a new session
5. **Do not silently resolve open questions** — mark them OPEN or ASSUMED
6. **Do not bulk-read documents** — process one at a time, summarize, release before reading next
7. **Sub-agent returns must be structured** — use output contracts, not free-form prose

## Why 7 rules and not 15

Research on transformer working memory ([arXiv:2409.10715](https://arxiv.org/abs/2409.10715)) shows LLMs can actively track **5-10 constraints** before performance degrades to near-random. With 15 rules, Claude silently drops the ones it considers least relevant. With 7, compliance per rule goes up significantly.

The rules that were cut didn't disappear — they moved to `docs/context/` where Claude loads them only when the task requires it. This matches Anthropic's own recommendation for ["just-in-time context"](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents).

## Why zero explanation in CLAUDE.md

v3 had sections like "Structured Summarization: Why It Matters" and "Cost of NOT Splitting" — prose that explained *why* each rule existed.

The [Chroma Context Rot study](https://research.trychroma.com/context-rot) (July 2025, 18 LLMs tested) found that **coherent filler text that's topically related to the target task interferes MORE than random noise**. Explanatory prose about context management rules actively competes with the context management rules themselves.

v4 has rules only. No justification, no persuasion, no theory. The rationale lives here in the README — for humans, not for Claude.

## Why plain language instead of "CRITICAL" and "NEVER"

Anthropic's [Claude 4 best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices) note that Claude Opus 4.5/4.6 is "significantly more proactive" and "more responsive to the system prompt." Instructions that needed emphasis for Claude 3.x now cause overtriggering in Claude 4.6.

v4 uses calm imperatives: "Do not mix client contexts" instead of "NEVER mix client contexts. This is CRITICAL."

## Why "Before Delivering Output" is last

The [Lost in the Middle](https://aclanthology.org/2024.tacl-1.9/) paper (Liu et al., TACL 2024) showed LLM performance follows a **U-shaped curve** — highest attention at the beginning and end of context, lowest in the middle. Anthropic's own testing shows queries placed at the end of prompts [improve response quality by up to 30%](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/long-context-tips).

The verification checklist is deliberately placed at the end of CLAUDE.md so it's the last instruction Claude processes before starting work.

## New in v4

### Work-type context mapping

CLAUDE.md now tells Claude which context files to load based on the type of work:

```
- Proposals/sales → client-briefs/[client].md + sales-frameworks.md
- Content/thought leadership → content-strategy.md
- Workshops → workshop-design.md + client-briefs/[client].md
- Agent development → use codebase directly
```

No more loading everything. Claude loads what the task needs.

### Task definition template

Inspired by [GSD](https://github.com/gsd-build/get-shit-done)'s atomic task structure:

```
## Task: [name]
Context files: [which files to load]
Action: [what to produce]
Verify: [how to check it's correct]
Done: [what files exist when complete]
```

### Handoff index layer

Inspired by [claude-mem](https://github.com/thedotmack/claude-mem)'s progressive disclosure pattern. Every handoff now includes a "Files to Load Next Session" field — an explicit manifest that tells the next session exactly what to read, instead of loading all summaries.

## Quick start

1. Copy `CLAUDE.md` to your project root
2. Copy the `templates/` and `docs/context/` directories
3. Copy `.claude/commands/` for slash commands
4. Edit the Identity section in CLAUDE.md for your workflow
5. Populate `docs/context/` files for your domain (sales frameworks, client briefs, etc.)
6. Start a Claude Code session

No dependencies. No install.

## Who this is for

Anyone doing multi-session work with Claude — consultants, developers, researchers, sales professionals, no-code builders. If your projects span days or weeks, not minutes.

v4 was specifically designed to work for **non-developers** too — consultants, sales professionals, researchers, and no-code builders using Claude Code for business tasks like proposals, competitive analysis, content strategy, and workshop design.

## The research behind v4

| Design Decision | Research |
|---|---|
| 7 rules max | [Working Memory Limits](https://arxiv.org/abs/2409.10715) — LLMs track 5-10 constraints |
| No explanatory prose | [Context Rot](https://research.trychroma.com/context-rot) — coherent filler interferes |
| Rules are 1-3 lines each | [Hidden in the Haystack](https://arxiv.org/abs/2505.18148) — too-short instructions harder to find |
| Verification at end | [Lost in the Middle](https://aclanthology.org/2024.tacl-1.9/) — U-shaped attention curve |
| Plain imperatives | [Claude 4 Best Practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices) — overtriggering |
| "State your understanding" step | [Anthropic](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) + [EMNLP 2025](https://arxiv.org/abs/2510.05381) — recite-before-reasoning |
| Just-in-time context loading | [Anthropic Context Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) |
| Session splitting as architecture | [NoLiMa](https://arxiv.org/html/2502.05167v1) — 32K token performance cliff |

Additional references:
- [Context Length Alone Hurts LLM Performance](https://arxiv.org/abs/2510.05381) — EMNLP 2025
- [Claude Code Best Practices](https://code.claude.com/docs/en/best-practices) — Anthropic official

## v3 → v4 migration

If you're using v3:
1. Your `templates/claude-templates.md` is mostly compatible — v4 adds Template 6 (Task Definition) and a "Files to Load Next Session" field to the handoff template
2. Copy the new `docs/context/` directory — this is where v3's displaced content now lives
3. Replace your `CLAUDE.md` with v4
4. Your existing `docs/summaries/` files work unchanged

v3 is preserved as `CLAUDE-v3.md` in this repo.

## Contributing

PRs and issues welcome. If you've adapted this for a specific workflow — academic research, legal, creative writing, sales — I'd like to see your fork.

## License

MIT

---

**Created by [Poorna Reddy](https://github.com/Arkya-AI)**. v3 was built from necessity. v4 was built from research.
