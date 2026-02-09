<!-- Parent: ../AGENTS.md -->
<!-- Generated: 2026-02-02 | Updated: 2026-02-02 -->

# handoff (Plugin)

## Purpose

Core handoff plugin implementation for Claude Code. Provides the `/handoff` skill with smart auto-scaling output for session context preservation. This is the primary deliverable of the Handoff project.

## Key Files

| File | Description |
|------|-------------|
| `plugin.json` | Plugin metadata: name, version, author, commands path |
| `handoff.md` | Skill definition with YAML frontmatter and usage documentation |

## For AI Agents

### Working In This Directory

- `plugin.json` defines plugin identity and entry point
- `handoff.md` is the actual skill implementation
- Changes here directly affect `/handoff` command behavior
- Test changes by running `/handoff`

### plugin.json Structure

```json
{
  "name": "handoff",
  "version": "2.2.0",
  "description": "Session context handoff with smart auto-scaling output",
  "author": { "name": "quantsquirrel" },
  "commands": "./"
}
```

- `commands: "./"` means skill files are in this directory
- Version must match `../../.claude-plugin/marketplace.json`

### handoff.md Structure

The skill file uses YAML frontmatter + markdown:

```markdown
---
name: handoff
description: Session context handoff
triggers:
  - /handoff
---

# Skill Content
[Usage documentation and templates]
```

Files are saved to `../../.claude/handoffs/`

### Testing Requirements

1. **Test handoff generation:**
   ```bash
   /handoff "test topic"
   # Verify: File created, clipboard copied, smart-scaled output
   ```

2. **Test secret redaction:**
   - Include API_KEY in context
   - Verify it's replaced with `***REDACTED***`

3. **Test clipboard format:**
   - Paste clipboard content
   - Verify proper markdown formatting

### Common Patterns

#### Updating Templates

Templates are defined in `handoff.md` as markdown code blocks. To update:

1. Find the relevant template section
2. Modify the markdown structure
3. Test that Claude generates correct output
4. Update examples in documentation

### Language Support

- Templates: English only
- Output documents: Generated in user's preferred language based on context
- Documentation: Bilingual (English + Korean)

## Dependencies

### Internal

- `../../SKILL.md` - Original detailed skill specification
- `../../hooks/auto-handoff.mjs` - Auto-suggests `/handoff` at 70%+ context
- `../../.claude/handoffs/` - Output directory for generated files

### External

- Claude Code skill system for command registration
- System clipboard (pbcopy/xclip) for copy functionality

<!-- MANUAL: -->
