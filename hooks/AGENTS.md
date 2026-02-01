# hooks/ - Auto-Handoff Hook System

<!-- Parent: ../AGENTS.md -->

## Directory Purpose

The hooks directory contains the core automation functionality for Claude Code integration. It implements a **PostToolUse hook system** that monitors context usage and suggests context-preserving handoffs when approaching capacity limits.

This is the critical infrastructure that makes the handoff plugin work seamlessly with Claude Code. The hook runs automatically on every tool execution and makes intelligent decisions about when to prompt the user.

## Core Responsibilities

| Component | Responsibility |
|-----------|-----------------|
| **auto-handoff.mjs** | Main PostToolUse hook - monitors context, estimates tokens, suggests handoff |
| **constants.mjs** | Configuration and messaging - centralized thresholds and user-facing text |
| **recover.mjs** | Recovery CLI - finds interrupted sessions and draft files for recovery |
| **lockfile.mjs** | Lock file management - tracks generation state and interrupted operations |
| **install.sh** | Installation script - adds hook to ~/.claude/settings.json for Claude Code integration |

## File Specifications

### auto-handoff.mjs

**Entry Point:** Main hook executable - called by Claude Code on every tool use

**Lifecycle:**
```
Tool executes (Read, Grep, Glob, Bash, WebFetch)
         ↓
auto-handoff.mjs is invoked with JSON on stdin
         ↓
Hook estimates tokens from tool_response
         ↓
Accumulates tokens per session in /tmp/auto-handoff-state.json
         ↓
Checks thresholds (70%, 80%, 90%)
         ↓
Saves draft file + outputs JSON to stdout with message
```

**Input Schema (stdin):**
```json
{
  "tool_name": "Read|Grep|Glob|Bash|WebFetch",
  "session_id": "string",
  "tool_response": "large text output"
}
```

**Output Schema (stdout):**
```json
{
  "decision": "approve",
  "additionalContext": "User-facing message with handoff suggestion"
}
```

**Key Functions:**

| Function | Purpose |
|----------|---------|
| `loadState()` / `saveState()` | Persists session token counts to /tmp/auto-handoff-state.json |
| `getSessionState()` | Gets or creates per-session tracking object |
| `estimateTokens()` | Converts response text length to token count (chars / 4) |
| `saveDraft()` | Creates .draft-{timestamp}.json with session metadata |
| `checkRecentHandoff()` | Prevents repeated suggestions if handoff just happened |
| `getGitInfo()` | Captures branch/status for draft context |
| `main()` | Orchestrates the hook logic |

**Thresholds & Actions:**
- **< 70%**: No action, no message
- **70% (HANDOFF_THRESHOLD)**: Save draft, show suggestion message, 3min cooldown
- **80% (WARNING_THRESHOLD)**: Show urgent warning message
- **90% (CRITICAL_THRESHOLD)**: Show critical message
- Max 2 suggestions per session to avoid spam

**State File:** `/tmp/auto-handoff-state.json`
```json
{
  "sessions": {
    "{session-id}": {
      "lastSuggestionTime": 1234567890000,
      "suggestionCount": 0,
      "estimatedTokens": 45000,
      "handoffCreated": false,
      "draftSaved": true
    }
  }
}
```

**Draft File Location:** `./.claude/handoffs/.draft-{timestamp}.json`
```json
{
  "sessionId": "session-id",
  "timestamp": "2025-02-01T12:30:45Z",
  "estimatedTokens": 45000,
  "cwd": "/path/to/project",
  "gitBranch": "main",
  "gitStatus": "output of git status --short"
}
```

---

### constants.mjs

**Type:** Configuration & constants module - imported by all other scripts

**Exports:**

| Constant | Type | Value | Purpose |
|----------|------|-------|---------|
| `HANDOFF_THRESHOLD` | number | 0.70 | Primary threshold for handoff suggestion |
| `WARNING_THRESHOLD` | number | 0.80 | Secondary warning threshold |
| `CRITICAL_THRESHOLD` | number | 0.90 | Emergency threshold |
| `HANDOFF_COOLDOWN_MS` | number | 180_000 | 3-minute wait between suggestions |
| `MAX_SUGGESTIONS` | number | 2 | Max suggestions per session |
| `CLAUDE_CONTEXT_LIMIT` | number | 200_000 or 1_000_000 | Token budget (reads from env) |
| `CHARS_PER_TOKEN` | number | 4 | Average estimate |
| `HANDOFF_SUGGESTION_MESSAGE` | string | User message | 70% threshold message |
| `HANDOFF_WARNING_MESSAGE` | string | User message | 80% threshold message |
| `HANDOFF_CRITICAL_MESSAGE` | string | User message | 90% threshold message |
| `AUTO_DRAFT_ENABLED` | boolean | true | Enable auto-draft saving |
| `DRAFT_FILE_PREFIX` | string | ".draft-" | Prefix for draft files |
| `LOCK_FILE_NAME` | string | ".generating.lock" | Lock file name |

**Key Features:**

- **Dynamic Context Limit**: Detects 1M context via `ANTHROPIC_1M_CONTEXT` or `VERTEX_ANTHROPIC_1M_CONTEXT` env vars
- **Centralized Messages**: All user-facing messages defined here for consistency
- **Safe Defaults**: Works with standard 200K context if env vars not set

**When to Update:**
- Changing thresholds: Update threshold constants
- Improving messages: Update MESSAGE constants
- Adding new features: Add new export and document here

---

### recover.mjs

**Type:** CLI utility - run standalone for recovery operations

**Command:** `node recover.mjs`

**Output:** Formatted report of drafts and lock state

**Key Functions:**

| Function | Purpose |
|----------|---------|
| `findDraftFiles()` | Recursively searches .claude/handoffs for .draft-*.json files |
| `formatBytes()` | Formats file size for display |
| `formatTime()` | Formats timestamps in Korean locale (YYYY-MM-DD HH:mm:ss) |
| `main()` | Displays recovery information |

**Search Locations:**
1. `./.claude/handoffs/` (local project)
2. `~/.claude/handoffs/` (global user)

**Output Format:**
```
🔍 Handoff 복구 정보 확인 중...

🔒 중단된 생성 작업 발견:
   세션 ID: {session-id}
   주제: current-topic
   시작 시간: 2025-02-01 12:30:45

📋 2개의 초안 발견:

1. .draft-1707046245000.json
   경로: /path/to/.claude/handoffs/.draft-1707046245000.json
   생성 시간: 2025-02-01 12:30:45
   세션 ID: {session-id}
   추정 토큰: 45,000
   작업 디렉토리: /path/to/project
   Git 브랜치: main
```

**Use Cases:**
- Check for draft files after interrupted sessions
- Recover session metadata (branch, tokens, cwd)
- Determine if a new session is needed or if recovery is possible

---

### lockfile.mjs

**Type:** Utility module - provides lock file management API

**Exports:**

| Export | Type | Purpose |
|--------|------|---------|
| `createLock(sessionId, topic)` | function | Create .generating.lock during handoff generation |
| `removeLock()` | function | Delete lock file when generation completes |
| `checkLock()` | function | Read lock file contents or return null |

**Lock File Location:** `./.claude/handoffs/.generating.lock`

**Lock File Schema:**
```json
{
  "sessionId": "session-id",
  "startTime": "2025-02-01T12:30:45Z",
  "topic": "refactor-auth"
}
```

**Behavior:**
- Creates handoff dir if missing: `.claude/handoffs/`
- Checks local first, falls back to global
- All functions return boolean success (except checkLock)

**Error Handling:**
- Returns false on creation/removal failure
- Returns null if lock doesn't exist
- Prints error messages to stderr on failure

**Usage Pattern:**
```javascript
import { createLock, removeLock, checkLock } from './lockfile.mjs';

// At handoff start
createLock(sessionId, topic);

// During recovery
const lock = checkLock();
if (lock) {
  console.log(`Interrupted at: ${lock.startTime}`);
}

// At handoff completion
removeLock();
```

---

### install.sh

**Type:** Installation script - bash, runs once to configure Claude Code

**Command:** `bash install.sh`

**Steps:**
1. Creates `~/.claude/settings.json` if missing
2. Checks if hook already installed (grep for "auto-handoff.mjs")
3. Backs up existing settings to `~/.claude/settings.json.backup`
4. Uses Node.js to safely merge hook configuration
5. Outputs installation summary

**Generated Configuration:**
Adds to `~/.claude/settings.json`:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Read|Grep|Glob|Bash|WebFetch",
        "hooks": [
          {
            "type": "command",
            "command": "node /path/to/hooks/auto-handoff.mjs"
          }
        ]
      }
    ]
  }
}
```

**Idempotency:**
- Safe to run multiple times
- Detects existing installation and skips if present
- Always creates backup before modification

**Output:**
```
📋 Auto-Handoff Hook Installer
==============================

📁 Backed up settings to ~/.claude/settings.json.backup
✅ Hook configuration added successfully!

🎉 Installation complete!

The auto-handoff hook will now suggest running /handoff when:
  • Context usage reaches 70% - Suggestion
  • Context usage reaches 80% - Warning
  • Context usage reaches 90% - Urgent

To test, use Claude Code until context fills up.

Debug mode: AUTO_HANDOFF_DEBUG=1
Logs: /tmp/auto-handoff-debug.log
```

---

## Agent Guidance

### When Working in This Directory

**File Modification Rules:**
- **constants.mjs**: Safe to modify - update thresholds or messages
- **auto-handoff.mjs**: Modify core logic with extreme care - must verify hook still works
- **recover.mjs**: Safe to enhance - add new recovery features
- **lockfile.mjs**: Stable API - only enhance exports if needed
- **install.sh**: Safe to improve - test thoroughly

**Testing Requirements (MANDATORY):**

Before committing changes to any hook file:

1. **Test hook execution with debug mode:**
   ```bash
   AUTO_HANDOFF_DEBUG=1 node auto-handoff.mjs <<< '{"tool_name":"Read","session_id":"test-123","tool_response":"..."}'
   cat /tmp/auto-handoff-debug.log
   ```

2. **Test state persistence:**
   ```bash
   # Run multiple hook invocations
   # Verify /tmp/auto-handoff-state.json accumulates tokens
   cat /tmp/auto-handoff-state.json | jq .
   ```

3. **Test installation:**
   ```bash
   bash install.sh
   # Verify ~/.claude/settings.json modified correctly
   jq '.hooks.PostToolUse' ~/.claude/settings.json
   ```

4. **Test recovery:**
   ```bash
   node recover.mjs
   # Should show draft files if any exist
   ```

5. **Test draft saving:**
   - Run hook at 70%+ threshold
   - Verify draft file created in `./.claude/handoffs/`
   - Check draft contains correct metadata

**Common Patterns:**

| Pattern | Example | Notes |
|---------|---------|-------|
| State persistence | `loadState()`, `saveState()` | Always check fs.existsSync before read |
| Token estimation | `text.length / CHARS_PER_TOKEN` | Conservative estimate |
| File cleanup | Remove old drafts per session | Prevents .claude/handoffs bloat |
| Environment detection | Check `process.env.ANTHROPIC_1M_CONTEXT` | For context limit determination |
| Directory creation | `fs.mkdirSync(dir, { recursive: true })` | Always use recursive: true |

**Import Pattern:**
```javascript
// Always import constants first
import {
  HANDOFF_THRESHOLD,
  CHARS_PER_TOKEN,
  // ... other constants needed
} from './constants.mjs';

// Then import internal modules
import { checkLock } from './lockfile.mjs';
```

### Debug Mode

Enable verbose logging:
```bash
AUTO_HANDOFF_DEBUG=1 node auto-handoff.mjs
```

Outputs to `/tmp/auto-handoff-debug.log`:
```
[2025-02-01T12:30:45.123Z] [auto-handoff] Tracking output {
  "tool": "read",
  "tokens": 1250,
  "cumulative": 45000
}
[2025-02-01T12:30:45.456Z] [auto-handoff] Draft saved: ./.claude/handoffs/.draft-1707046245000.json
```

### Threshold Configuration

All thresholds are in `constants.mjs`. To adjust:

**For different contexts:**
- 200K context (default): 140K (70%), 160K (80%), 180K (90%)
- 1M context: 700K (70%), 800K (80%), 900K (90%)

**To add new threshold:**
```javascript
// In constants.mjs
export const MY_NEW_THRESHOLD = 0.85;

// In auto-handoff.mjs
if (usageRatio >= MY_NEW_THRESHOLD) {
  // handle new threshold
}
```

## File Dependencies

```
auto-handoff.mjs
├── constants.mjs (imports all thresholds & messages)
├── fs, path, os (Node.js built-ins)
└── process.env (for debug flag)

recover.mjs
├── constants.mjs (DRAFT_FILE_PREFIX)
├── lockfile.mjs (checkLock function)
├── fs, path, os (Node.js built-ins)
└── formatTime for Korean locale

lockfile.mjs
├── constants.mjs (LOCK_FILE_NAME)
├── fs, path, os (Node.js built-ins)
└── homedir for global fallback

install.sh
├── $HOME/.claude/settings.json (reads/modifies)
├── Node.js (for JSON merge)
└── auto-handoff.mjs (for path)

constants.mjs
└── No dependencies (standalone)
```

## State Files & Locations

| File | Location | Purpose | Cleanup |
|------|----------|---------|---------|
| Session state | `/tmp/auto-handoff-state.json` | Tracks tokens per session | Cleared on system restart |
| Debug log | `/tmp/auto-handoff-debug.log` | Hook execution logs | Manual deletion |
| Lock file | `./.claude/handoffs/.generating.lock` | Marks ongoing generation | Auto-removed on completion |
| Draft files | `./.claude/handoffs/.draft-*.json` | Session recovery data | Manual deletion safe |
| Settings | `~/.claude/settings.json` | Hook configuration | Never delete manually |
| Backup | `~/.claude/settings.json.backup` | Pre-installation backup | Keep for safety |

## Integration Points

**Claude Code Integration:**
- Hook is called via PostToolUse matcher on: Read, Grep, Glob, Bash, WebFetch
- Hook receives JSON on stdin, outputs JSON on stdout
- Claude Code displays additionalContext message to user

**Handoff Plugin Integration:**
- Plugin reads draft files to recover interrupted sessions
- Plugin removes lock file on successful handoff creation
- Plugin updates session state when handoff completes

**File System Integration:**
- Checks both `./.claude/handoffs/` (local) and `~/.claude/handoffs/` (global)
- Prefers local if both exist
- Creates directories as needed with recursive: true

## Verification Checklist

When modifying hooks files, verify:

- [ ] All imports are resolvable (no missing modules)
- [ ] Constants are imported from constants.mjs, not hard-coded
- [ ] fs.existsSync checks before all file reads
- [ ] JSON parse/stringify wrapped in try-catch
- [ ] Directory creation uses { recursive: true }
- [ ] State file is JSON valid before write
- [ ] Lock file operations return boolean/null consistently
- [ ] Debug logging uses debugLog() function
- [ ] Hook outputs valid JSON to stdout
- [ ] All thresholds use CONSTANTS from constants.mjs
- [ ] Installation script idempotent (safe to run twice)
- [ ] Recovery script finds draft files correctly

## Related Documentation

- **Parent directory**: `/Users/ahnjundaram_g/dev/tools/handoff/` - Main plugin structure
- **Installation**: `install.sh` - How to integrate with Claude Code
- **Configuration**: `constants.mjs` - All adjustable parameters
- **Recovery**: `recover.mjs` - How to resume interrupted sessions
