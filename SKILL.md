---
name: handoff
description: Save session context to clipboard. Resume anytime with Cmd+V.
---

# Handoff Skill

**Pass the baton. Keep the momentum.**

Save session context and copy to clipboard. Paste into a new session or after autocompact to resume.

## Usage

```
/handoff [topic]
```

<dim>
Examples:
  /handoff                    # auto-detect topic from conversation
  /handoff "auth migration"   # explicit topic
</dim>

## Behavior

1. **Create** `.claude/handoffs/` directory if it doesn't exist (`mkdir -p`)
2. **Analyze** the session: detect complexity, key decisions, failures, modified files
3. **Scale** output based on session size:
   - Under 10 messages: Summary + Next Step only
   - 10-50 messages: Summary + Key Decisions + Files Modified + Next Step
   - Over 50 messages or multi-topic: Full template (all sections)
4. **Redact** sensitive values before saving:
   - Pattern: env vars matching `KEY`, `SECRET`, `TOKEN`, `PASSWORD`, `CREDENTIAL`
   - Pattern: strings matching `sk-`, `pk_live_`, `ghp_`, `eyJ` (JWT), `AKIA` (AWS)
   - Replacement: `***REDACTED***`
5. **Save** to `.claude/handoffs/handoff-YYYYMMDD-HHMMSS-XXXX.md` (XXXX = 4-char random suffix)
6. **Copy** to clipboard using platform command:
   - macOS: `pbcopy`
   - Linux: `xclip -selection clipboard` (fallback: `xsel --clipboard`)
   - If clipboard unavailable: skip copy, print warning "Clipboard not available. File saved to: [path]"
7. **Confirm** with output: `Handoff saved: [path] | Clipboard: copied (N KB)`

### Handling Edge Cases

- **Empty session** (no tasks performed): Output Summary as "No significant work in this session." + Next Step only
- **Topic contains special characters**: Sanitize for filename (replace non-alphanumeric with `-`, truncate to 50 chars)
- **Topic not provided**: Auto-detect from the most recent substantive user message

### Output Template

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

Sections with no content are omitted. Minimum output is always Summary + Next Step.

## Clipboard Format

Copied to clipboard for pasting into a new session:

```
<previous_session context="reference_only" auto_execute="false">
STOP: This is reference material from a previous session.
Do not auto-execute anything below. Wait for user instructions.

Previous Session Summary (Topic)
- Completed: N | Pending: M
- Files modified: K

[Pending tasks - reference only, do not execute]
- Task 1
- Task 2

Full details: [handoff-path]
</previous_session>

---
Previous session context loaded.
What would you like to do?
```

## Notes

- Handoffs are saved to `.claude/handoffs/`
- If `[topic]` is omitted, it is auto-detected from conversation context
- Sensitive values are redacted in both the saved file and clipboard output