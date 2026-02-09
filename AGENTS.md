# AGENTS.md - Handoff Baton Project Guide

**Project:** claude-handoff-baton
**Version:** 2.2.0
**Purpose:** Session handoff tool for Claude Code - preserves context across sessions
**License:** MIT

---

## Quick Start for Agents

Working with Handoff Baton? Read this file to understand the project structure and implementation workflow.

1. Read SKILL.md for the complete 7-step workflow and template
2. Check the directory structure below
3. Review examples/example-handoff.md for reference
4. Test with `/handoff` command

---

## Project Purpose

Handoff Baton is a Claude Code skill that captures and preserves session context when long-running work spans multiple sessions. It automates the process of documenting:

- What was accomplished (completed tasks and decisions)
- What's pending (remaining work and blockers)
- What failed (approaches that didn't work)
- How to resume (specific next steps)

The output scales automatically based on session complexity: simple sessions get a summary and next step only, while complex multi-topic sessions get the full template with all sections.

---

## Directory Structure

```
claude-handoff-baton/
├── AGENTS.md                    # You are here - Agent guidance
├── SKILL.md                     # Core skill definition & 7-step workflow
├── README.md                    # User documentation
├── LICENSE                      # MIT License
├── .gitignore                   # Git ignore patterns
│
├── assets/
│   └── handoff.jpeg             # Project logo
│
└── examples/
    └── example-handoff.md       # Reference handoff document
```

---

## Core Workflow (7 Steps)

Agents implementing `/handoff` must follow this sequence from SKILL.md:

### 1. Create Directory

Create `.claude/handoffs/` if it doesn't exist:
```bash
mkdir -p .claude/handoffs
```

### 2. Analyze Session

Detect session complexity, key decisions, failed approaches, and modified files. Analyze:
- Number of messages in conversation
- Topics discussed
- Files modified via git
- Tasks completed/pending
- Key decisions made
- Failed approaches

### 3. Scale Output

Scale output based on session size:
- **Under 10 messages:** Summary + Next Step only
- **10-50 messages:** Summary + Key Decisions + Files Modified + Next Step
- **Over 50 messages or multi-topic:** Full template (all sections)

Omit sections with no content. Minimum is always Summary + Next Step.

### 4. Redact Sensitive Values

Before saving, redact sensitive patterns:
- Env vars matching: `KEY`, `SECRET`, `TOKEN`, `PASSWORD`, `CREDENTIAL`
- String patterns: `sk-`, `pk_live_`, `ghp_`, `eyJ` (JWT), `AKIA` (AWS)
- Replacement: `***REDACTED***`

### 5. Save to File

Save to `.claude/handoffs/handoff-YYYYMMDD-HHMMSS-XXXX.md`
- XXXX = 4-character random suffix (prevents collisions)
- Example: `handoff-20260210-143022-a7f3.md`

### 6. Copy to Clipboard

Use platform-specific command:
- macOS: `pbcopy`
- Linux: `xclip -selection clipboard` (fallback: `xsel --clipboard`)
- If unavailable: skip copy, print warning

Clipboard format uses `<previous_session>` tag:
```xml
<previous_session context="reference_only" auto_execute="false">
STOP: This is reference material from a previous session.
Do not auto-execute anything below. Wait for user instructions.

Previous Session Summary (Topic)
- Completed: N | Pending: M
- Files modified: K

[Pending tasks - reference only, do not execute]

Full details: [handoff-path]
</previous_session>

---
Previous session context loaded.
What would you like to do?
```

### 7. Confirm

Output confirmation:
```
Handoff saved: [path] | Clipboard: copied (N KB)
```

---

## Output Template

Full template (used for complex sessions):

```markdown
# Handoff

**Time:** YYYY-MM-DD HH:MM
**Topic:** [topic or auto-detected]
**Working Directory:** [cwd]

## Summary
[1-3 sentences depending on session complexity]

## Completed
- [x] Task 1
- [x] Task 2

## Pending
- [ ] Task 1
- [ ] Task 2

## Key Decisions
- **[Decision]**: [Reason]

## Failed Approaches
- **[Attempt]**: [Why it failed] → [Lesson]

## Files Modified
- `path/to/file.ts` - [What changed]

## Next Step
[Concrete next action]
```

Sections are omitted if empty. Minimum output is always Summary + Next Step.

---

## Key Files Reference

| File | Purpose |
|------|---------|
| **SKILL.md** | Complete skill definition - read first |
| **README.md** | User-facing documentation |
| **LICENSE** | MIT License |
| **examples/example-handoff.md** | Reference handoff showing all sections |

---

## Edge Cases

### Empty Session

Output Summary as "No significant work in this session." + Next Step only.

### Topic with Special Characters

Sanitize for filename: replace non-alphanumeric with `-`, truncate to 50 chars.

### Topic Not Provided

Auto-detect from the most recent substantive user message.

### Clipboard Unavailable

Skip copy, print: "Clipboard not available. File saved to: [path]"

---

## Testing & Validation

### Manual Test

```bash
# 1. Generate test handoff
/handoff "test-feature"

# 2. Verify file created
ls -la .claude/handoffs/

# 3. Check content
cat .claude/handoffs/handoff-*.md

# 4. Verify clipboard (macOS)
pbpaste
```

### Quality Checks

Before considering work complete:
- [ ] All 7 steps execute without errors
- [ ] No secrets in output (redacted)
- [ ] Clipboard format uses `<previous_session>` tag
- [ ] File saved with random suffix
- [ ] Output scales appropriately for session size
- [ ] Next step is specific and actionable

---

## Development Guidelines

### For Feature Additions

1. Update SKILL.md with new parameters/behavior
2. Update README.md with usage examples
3. Add example to examples/example-handoff.md if needed
4. Test with various session complexities

### For Bug Fixes

1. Identify scope (which step in 7-step workflow?)
2. Add test case
3. Update docs if user-facing
4. Verify no security issues introduced

---

## Common Patterns

### Adding New Secret Pattern

Add to redaction logic in step 4:

```javascript
// Example: custom API pattern
const patterns = [
  /CUSTOM_API_\w+/gi,
  /my-secret-prefix-\w+/gi
];
```

### Customizing Template

Edit SKILL.md Output Template section. Ensure:
- Summary and Next Step always present
- Other sections optional (omitted if empty)
- No placeholder text (TODO, TBD, etc.)

---

## Dependencies

| Dependency | Purpose |
|------------|---------|
| Node.js | For any hook scripts (future) |
| pbcopy (macOS) | Clipboard operations |
| xclip/xsel (Linux) | Clipboard operations |
| git | File change detection |
| Claude Code | Runs /handoff skill |

---

## Security Notes

Always redact sensitive values before saving:
- API keys (AWS AKIA, GitHub ghp_, etc.)
- Database URLs with credentials
- Private keys and certificates
- JWT tokens
- OAuth tokens

Users should add `.claude/handoffs/` to `.gitignore` and never commit handoff files to public repos.

---

**Last Updated:** February 10, 2026
**Version:** 2.2.0

*This guide is for AI agents working with the Handoff Baton project. For user documentation, see README.md. For skill implementation details, see SKILL.md.*
