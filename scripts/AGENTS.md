<!-- Parent: ../AGENTS.md -->

# Scripts Directory

## Purpose

This directory contains validation and utility scripts for handoff document quality assurance. All scripts are designed for cross-platform compatibility (macOS and Linux) and serve as automated checkers in development workflows.

## Overview

| Script | Language | Purpose |
|--------|----------|---------|
| `validate.sh` | Bash | Validates handoff markdown quality and calculates quality score (0-100) |

---

## validate.sh

### Purpose

Automated handoff document validator that checks documentation quality against established standards. Provides actionable feedback through a scoring system and detailed issue reporting.

### Usage

```bash
./validate.sh <handoff-file.md>
```

**Arguments:**
- `<handoff-file.md>` (required): Path to the handoff markdown file to validate

**Examples:**
```bash
# Validate a handoff document
./validate.sh handoff-20260201-130000.md

# Validate with absolute path
./validate.sh /Users/ahnjundaram_g/dev/tools/handoff/.claude/handoffs/session-1.md

# Validate in subdirectory
./validate.sh ../examples/example-handoff.md
```

### Exit Codes

| Code | Meaning | Condition |
|------|---------|-----------|
| `0` | PASS | Score >= 70 (all major quality checks passed) |
| `1` | FAIL | Score < 70 (handoff quality below acceptable threshold) |
| `2` | ERROR | Invalid usage (wrong arguments, file not found) |

### Quality Checks (5 Checks, 20 Points Each)

All checks are weighted equally at 20 points. Total score ranges from 0-100.

#### 1. No Incomplete Placeholders (+20)
**Check:** Validates that no incomplete `[TODO: ...]` placeholders remain in the document.

**Why it matters:** Incomplete placeholders indicate unfinished work. A handoff should have actionable items in the "Pending" section, not scattered TODOs throughout the text.

**Passes when:**
- Zero instances of `[TODO:` pattern found

**Fails when:**
- One or more `[TODO: ...]` patterns detected anywhere in the document

**Example:**
```markdown
# FAIL - Contains placeholder
Next, we need to [TODO: implement error handling]

# PASS - No placeholders
- [ ] Implement error handling (Next Steps section)
```

#### 2. No Secrets Detected (+20)
**Check:** Scans for sensitive information patterns that should never appear in handoff documents.

**Why it matters:** Handoff documents are preserved in git history and shared context. Accidental exposure of API keys, tokens, or credentials is a critical security risk.

**Detected Patterns:**
- API keys: `API_KEY`, `API-KEY`, `APIKEY`
- Secrets: `SECRET`
- Passwords: `PASSWORD`
- Tokens: `TOKEN`
- Private keys: `PRIVATE_KEY`, `PRIVATE-KEY`
- Bearer tokens: `Bearer [long alphanumeric string]`
- Long random strings: `[a-zA-Z0-9_-]{32,}` (32+ characters)

**Passes when:**
- No secret patterns detected in uncommented lines

**Fails when:**
- Any secret pattern found in code/content (patterns in comments are ignored)

**Example:**
```markdown
# FAIL - Contains secret
API_KEY=sk-1234567890abcdefghij

# PASS - Redacted or documented
API_KEY=***REDACTED***
Authentication: Use environment variables for secrets
```

#### 3. All Required Sections Present (+20)
**Check:** Validates that handoff document contains all mandatory structural sections.

**Why it matters:** Consistent structure ensures future sessions can quickly locate critical information. Missing sections indicate incomplete documentation.

**Required Sections (Case-Insensitive Header Match):**
1. **Summary** - Overview of session work
2. **Completed** - List of finished tasks
3. **Pending** - List of incomplete work
4. **Next Steps** - Explicit action items for next session

**Passes when:**
- All 4 required sections found as markdown headers (`#`)

**Fails when:**
- One or more required sections missing

**Example:**
```markdown
# PASS - All sections present
# Handoff Document
## Summary
...
## Completed
...
## Pending
...
## Next Steps
...

# FAIL - Missing "Pending" section
# Handoff Document
## Summary
...
## Completed
...
## Next Steps
...
```

#### 4. Referenced Files Exist (+20, Partial Credit +10)
**Check:** Validates that all files referenced in "Files Modified" section actually exist on disk.

**Why it matters:** Broken file references indicate the handoff may be outdated or paths may have changed since the document was created.

**Behavior:**
- Extracts file paths from markdown list items under "Files Modified" section
- Resolves relative paths from the handoff file's directory location
- Supports absolute paths
- Checks existence on the file system

**Scoring:**
- All files exist: +20 points (full credit)
- 1-2 missing files: +10 points (partial credit)
- 3+ missing files: +0 points (fail)

**Example:**
```markdown
## Files Modified
- `src/auth.ts`
- `./src/config.ts`
- `/Users/ahnjundaram_g/dev/tools/handoff/docs/API.md`

# PASS if all three files exist
# PARTIAL if one is missing (10 pts)
# FAIL if two or more missing (0 pts)
```

#### 5. Next Steps Has At Least 2 Items (+20)
**Check:** Validates that the "Next Steps" section contains at least 2 action items.

**Why it matters:** A single action item suggests incomplete planning for the next session. Two or more items indicate the work has been properly decomposed into sequential actions.

**Behavior:**
- Counts markdown list items (`-`, `*`, `+`) in the "Next Steps" section
- Items must be between "## Next Steps" header and next section header

**Passes when:**
- 2 or more list items found in Next Steps section

**Fails when:**
- 0 or 1 items found

**Example:**
```markdown
## Next Steps
- [ ] Set up database schema
- [ ] Implement user registration endpoint

# PASS - Has 2 items

## Next Steps
- [ ] Deploy to production

# FAIL - Has only 1 item
```

### Output Format

The validator provides detailed console output with color support (when terminal detected):

```
Validating: handoff-example.md

Checking for incomplete TODOs... PASS
Checking for secrets... PASS
Checking required sections... PASS
Checking referenced files... PASS
Checking Next Steps... PASS

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Quality Score: 100/100

✓ PASS
```

For failing validation:

```
Validating: handoff-incomplete.md

Checking for incomplete TODOs... FAIL
Checking for secrets... PASS
Checking required sections... WARNING
Checking referenced files... PASS
Checking Next Steps... FAIL

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Issues Found:
  • Found incomplete [TODO: ...] placeholders
  • Missing required sections: Pending
  • Next Steps section has fewer than 2 items (found: 1)

Quality Score: 40/100

✗ FAIL (minimum score: 70)
```

### Color Output

When running in a terminal:
- **RED** (`\033[0;31m`): FAIL status and critical issues
- **GREEN** (`\033[0;32m`): PASS status and success
- **YELLOW** (`\033[1;33m`): WARNING status and partial credit
- **BLUE** (`\033[0;34m`): Score and informational text
- **BOLD** (`\033[1m`): Headers and emphasized text
- **NC** (`\033[0m`): No Color (reset)

When piped (non-terminal output), colors are automatically disabled.

### Implementation Details

**Error Handling:**
- Uses `set -euo pipefail` for strict bash mode
  - `-e`: Exit on any command failure
  - `-u`: Error on undefined variables
  - `-o pipefail`: Return status of pipeline's last command

**Path Resolution:**
- Handoff directory determined from input file location
- Relative paths resolved from handoff directory (not current working directory)
- Supports both relative (`./path/to/file`) and absolute (`/full/path/to/file`) paths

**Regex Patterns:**
- Extended regex mode (`-E` flag) for complex patterns
- Case-insensitive matching for secret detection
- Comment-aware secret detection (patterns in comments ignored)

**Pattern Matching:**
- Uses `sed` for section extraction and text processing
- Uses `grep` for pattern detection and counting
- Handles multiline sections gracefully

### Common Patterns

#### Extracting Sections
All section extraction uses the pattern:
```bash
sed -n '/^##[^#].*SectionName/,/^##[^#]/p' file.md
```

This matches:
- Lines between "## SectionName" (level 2 header)
- Stops at the next level 2 header or higher

#### Counting List Items
```bash
sed -n '/^##[^#].*SectionName/,/^##[^#]/p' file.md | grep -c '^[[:space:]]*[-*+][[:space:]]'
```

Counts valid list items:
- Leading whitespace
- One of: `-` (hyphen), `*` (asterisk), `+` (plus)
- Followed by space

#### Pattern Detection
```bash
grep -iE "PATTERN" file.md | grep -v '^\s*#' | grep -q .
```

Three-stage pattern detection:
1. Case-insensitive extended regex search
2. Exclude commented lines (start with optional whitespace then `#`)
3. Verify non-empty match

### Testing Requirements

When implementing changes or validating behavior:

#### Test Case 1: Valid Handoff (Should Pass)
```bash
./validate.sh examples/example-handoff.md
# Expected: Exit 0, Score >= 70
```

Verify it checks:
- No TODOs present
- No secrets visible
- All 4 sections exist
- Files referenced actually exist
- Next Steps has 2+ items

#### Test Case 2: Missing Sections (Should Fail)
Create test file with missing "Pending" section:
```bash
cat > /tmp/test-missing.md << 'EOF'
## Summary
Work summary here

## Completed
- [x] Task 1

## Next Steps
- [ ] Task 2
- [ ] Task 3
EOF

./validate.sh /tmp/test-missing.md
# Expected: Exit 1, Score 60 (missing 1 section = -20)
```

#### Test Case 3: TODOs Present (Should Fail)
Create test file with incomplete placeholders:
```bash
cat > /tmp/test-todos.md << 'EOF'
## Summary
Work summary here [TODO: expand this]

## Completed
- [x] Task 1

## Pending
- [ ] Task 2

## Next Steps
- [ ] Task 3
- [ ] Task 4
EOF

./validate.sh /tmp/test-todos.md
# Expected: Exit 1, Score 80 (has TODO = -20)
```

#### Test Case 4: Secrets Detected (Should Fail)
```bash
cat > /tmp/test-secrets.md << 'EOF'
## Summary
Set up authentication with TOKEN=sk-abc123xyz789

## Completed
- [x] Task 1

## Pending
- [ ] Task 2

## Next Steps
- [ ] Task 3
- [ ] Task 4
EOF

./validate.sh /tmp/test-secrets.md
# Expected: Exit 1, Score 80 (has secret = -20)
```

#### Test Case 5: One Missing File (Partial Credit)
Create test file referencing missing files:
```bash
cat > /tmp/test-files.md << 'EOF'
## Summary
Modified several files

## Files Modified
- `src/exists.ts`
- `src/missing.ts`

## Completed
- [x] Task 1

## Pending
- [ ] Task 2

## Next Steps
- [ ] Task 3
- [ ] Task 4
EOF

touch /tmp/src-exists.ts
./validate.sh /tmp/test-files.md
# Expected: Exit 0, Score 90 (1 missing = -10, partial credit)
```

### Dependencies

**External:**
- `bash` (any modern version)
- `grep` (with `-E` for extended regex)
- `sed` (GNU sed or BSD sed with POSIX extensions)

**Platform Support:**
- macOS: Tested with BSD sed and grep
- Linux: Tested with GNU sed and grep
- Windows: Works via WSL (Windows Subsystem for Linux)

All dependencies are standard Unix tools included in macOS and Linux distributions.

### Performance

- **Execution Time:** < 100ms for typical handoff documents
- **Scalability:** Proportional to file size; tested with documents up to 100KB
- **Memory:** Minimal memory usage; uses streaming grep/sed where possible

### Integration Examples

#### GitHub Actions
```yaml
- name: Validate handoff
  run: ./scripts/validate.sh handoff.md
```

#### Pre-commit Hook
```bash
#!/bin/bash
# .git/hooks/pre-commit
for file in $(git diff --cached --name-only | grep '\.md$'); do
  if [[ "$file" == *"handoff"* ]]; then
    ./scripts/validate.sh "$file" || exit 1
  fi
done
```

#### Development Workflow
```bash
# After creating handoff
/handoff .claude/handoffs/session-handoff.md

# Validate before commit
./scripts/validate.sh .claude/handoffs/session-handoff.md

# Then commit
git add .claude/handoffs/session-handoff.md
git commit -m "docs: add session handoff"
```

---

## For AI Agents - Working in This Directory

### Agent Capabilities

When working with validation scripts in this directory:

| Task | Capability | Notes |
|------|-----------|-------|
| Run validation | Yes | Use direct bash execution |
| Analyze validation output | Yes | Parse exit codes and colors |
| Test scripts | Yes | Create temporary test files |
| Modify validation logic | Yes | Improve check patterns or add features |
| Create new validation scripts | Yes | Follow the same bash strict mode pattern |

### Script Development Guidelines

When creating or modifying scripts in this directory:

1. **Use strict mode**: Always start with `set -euo pipefail`
2. **Add colors**: Support colored output when running in terminal
3. **Document sections**: Include comments explaining each check
4. **Test compatibility**: Verify on both macOS (BSD) and Linux (GNU) tools
5. **Handle edge cases**: Test with empty sections, missing files, etc.
6. **Provide examples**: Include usage examples in documentation

### Common Patterns for Script Work

**Pattern 1: Validate Before Operations**
```bash
# Always validate before using handoff in workflows
if ! ./scripts/validate.sh "$handoff_file"; then
  echo "Handoff validation failed"
  exit 1
fi
```

**Pattern 2: Parse Validation Results**
```bash
# Capture exit code for conditional logic
./scripts/validate.sh "$file"
case $? in
  0) echo "Validation passed" ;;
  1) echo "Validation failed - quality issues found" ;;
  2) echo "Invalid usage" ;;
esac
```

**Pattern 3: Batch Validation**
```bash
# Validate all handoffs in a directory
for handoff in .claude/handoffs/*.md; do
  echo "Validating: $handoff"
  ./scripts/validate.sh "$handoff" || failed=true
done
[[ -z "${failed:-}" ]] && echo "All handoffs valid"
```

### Debugging Scripts

When debugging validation failures:

1. **Check file exists**: `ls -la <file>` before running validator
2. **Check syntax**: `bash -n scripts/validate.sh` to verify bash syntax
3. **Run with verbose**: Can add `set -x` to debug execution flow
4. **Test patterns in isolation**: Use `grep` directly to test regex patterns
5. **Check file encoding**: Ensure no hidden characters with `file <handoff>`

---

## Directory Structure

```
scripts/
├── AGENTS.md              # This file - agent documentation
└── validate.sh            # Main validation script
```

---

## Version History

| Version | Changes |
|---------|---------|
| 1.0 | Initial validation script with 5 quality checks |
| 1.1 | Added color output support for terminal detection |
| 1.2 | Improved secret detection patterns and comments |

---

## Future Enhancements

Potential improvements for this directory:

- `generate.sh` - Scaffold new handoff documents with templates
- `compare.sh` - Compare handoffs across sessions to track progress
- `migrate.sh` - Upgrade legacy handoff formats to current standard
- `analyze.sh` - Extract statistics and trends from handoff history
- `lint.sh` - Additional checks for markdown formatting, spelling
