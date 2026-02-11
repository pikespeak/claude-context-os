# Changelog

## v1.1.0 (2026-02-11) — Community Improvements

### Fixed
- **Step 1 vs Rule 1 contradiction**: Session startup no longer bulk-reads all summary files. Now reads only `00-project-brief.md` + latest handoff, loading others on-demand.

### Added
- **Lightweight templates**: 3-field versions of Source Summary, Session Handoff, and Decision Record for projects under 5 sessions. Start light, upgrade when needed.
- **Worked example**: `examples/software-dev-project/` — a realistic 2-session Go project showing filled-in project brief, source summary, and session handoff.
- **Slash commands**: `/handoff`, `/process-doc`, `/status` — automate the most common protocols instead of following manual instructions.
- **Software Development workflow**: New phase-based workflow template (Template A) for dev projects alongside the existing consulting and agent development workflows.

### Changed
- **De-biased language**: Replaced consulting-specific terms throughout. "Client" → "Project Owner", "Engagement" → "Project". Project Brief template now works for any domain.
- **Template lettering**: Workflow templates re-ordered — Software Dev (A), Enterprise Sales (B), Agent Development (C), Hybrid (D).
- **`.gitignore`**: `.claude/commands/` now tracked so slash commands ship with the repo.

## v1.0.0 (2026-02-09) — Initial Release

- Core CLAUDE.md operating system (~3.5K tokens)
  - 4-step session startup protocol
  - 6 context management rules
  - Session discipline and handoff protocol
  - Document processing protocol
  - Archive protocol with summary lifecycle rules
  - Subagent deployment rules
  - Quality gates checklist
  - Error recovery procedures
- Structured templates (`templates/claude-templates.md`)
  - 5 core templates: Source Summary, Analysis Summary, Decision Record, Session Handoff, Project Brief
  - 3 subagent output contracts: Document Analysis, Research/Analysis, Review/QA
  - 3 phase-based workflow templates: Enterprise Sales, Agent Development, Hybrid
