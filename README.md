```
    ┌────────────────────────────────────────────────────────────────┐
    │                                                                │
    │     🏃‍♂️ ═══════════════🪄════════════════════> 🏃‍♀️             │
    │                                                                │
    │                   H  A  N  D  O  F  F                          │
    │                                                                │
    │            Context Relay for Claude Code Sessions             │
    │                                                                │
    │    SESSION 1  ───🎯───►  HANDOFF  ───🎯───►  SESSION 2        │
    │       ⚡              📋 Never Lose Progress 📋          ⚡     │
    │                                                                │
    └────────────────────────────────────────────────────────────────┘
```

<div align="center">

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Claude Code Compatible](https://img.shields.io/badge/Claude%20Code-compatible-success)](https://github.com/anthropics/claude-code)
[![Version](https://img.shields.io/badge/version-1.0.0-blue)](./package.json)
[![🇰🇷 Korean Supported](https://img.shields.io/badge/🇰🇷-Korean_Supported-red)](README.md#한국어-korean)
[![⚡ Zero Dependencies](https://img.shields.io/badge/⚡-Zero_Dependencies-yellow)](#)
[![🔄 Session Continuity](https://img.shields.io/badge/🔄-Session_Continuity-green)](#)

</div>

<div align="center">

### 🏃 Pass the baton. Keep the momentum. Never explain your codebase twice.

**Master context continuity across sessions.**
Seamlessly transfer context, decisions, and progress between Claude Code sessions
with automatic clipboard integration and quality validation.

**Works independently - no framework dependencies required.**

</div>

---

## Table of Contents

- [Features](#features)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Usage](#usage)
- [Output Format](#output-format)
- [Comparison](#comparison-with-alternatives)
- [Configuration](#configuration)
- [Advanced Usage](#advanced-usage)
- [Contributing](#contributing)
- [License](#license)
- [한국어 (Korean)](#한국어-korean)

---

## What is Handoff?

```
    ╔════════════════════════════════════════════════════════════════╗
    ║                                                                ║
    ║              THE PROBLEM: Context Loss Between Sessions       ║
    ║                                                                ║
    ║   Session 1 ──────> ❌ Gap ❌ ──────> Session 2                ║
    ║   [Working hard]   [Re-explain]    [Start from scratch]       ║
    ║                                                                ║
    ╠════════════════════════════════════════════════════════════════╣
    ║                                                                ║
    ║              THE SOLUTION: Handoff Plugin                     ║
    ║                                                                ║
    ║   Session 1 ──────> 📋 Handoff ──────> Session 2              ║
    ║   [Working]      [Auto-captured]     [Continue seamlessly]    ║
    ║                                                                ║
    ║   ✅ Project state    ✅ Decisions made   ✅ What failed       ║
    ║   ✅ Git changes      ✅ Next steps       ✅ Quality score     ║
    ║                                                                ║
    ╚════════════════════════════════════════════════════════════════╝
```

<div align="center">

**One command. Complete context. Zero re-explaining.**

</div>

---

## Features

```
╔════════════════════════════════════════════════════════════════════════╗
║                          CORE CAPABILITIES                             ║
╚════════════════════════════════════════════════════════════════════════╝
```

<table>
<tr>
<td width="33%">

```
┌─────────────────────┐
│ 🎯 COMPREHENSIVE    │
│    CONTEXT          │
├─────────────────────┤
│ Auto-documents:     │
│ • Project state     │
│ • Decisions made    │
│ • Progress          │
│ • Blockers          │
└─────────────────────┘
```

</td>
<td width="33%">

```
┌─────────────────────┐
│ 📋 CLIPBOARD        │
│    AUTO-COPY        │
├─────────────────────┤
│ One command:        │
│ ✓ Compressed        │
│ ✓ Instant copy      │
│ ✓ Ready to paste    │
└─────────────────────┘
```

</td>
<td width="33%">

```
┌─────────────────────┐
│ 🔗 GIT              │
│    INTEGRATION      │
├─────────────────────┤
│ Captures:           │
│ • Commit history    │
│ • Branch state      │
│ • File diffs        │
└─────────────────────┘
```

</td>
</tr>
<tr>
<td width="33%">

```
┌─────────────────────┐
│ 🚫 FAILED           │
│    APPROACHES       │
├─────────────────────┤
│ Track what          │
│ didn't work to      │
│ avoid repeating     │
│ mistakes            │
└─────────────────────┘
```

</td>
<td width="33%">

```
┌─────────────────────┐
│ ⛓️  HANDOFF         │
│    CHAIN            │
├─────────────────────┤
│ Link sessions       │
│ for narrative       │
│ continuity &        │
│ history tracking    │
└─────────────────────┘
```

</td>
<td width="33%">

```
┌─────────────────────┐
│ 🔐 SECRET           │
│    DETECTION        │
├─────────────────────┤
│ Auto-detect and     │
│ warn about:         │
│ • API keys          │
│ • Credentials       │
└─────────────────────┘
```

</td>
</tr>
<tr>
<td width="33%">

```
┌─────────────────────┐
│ ⭐ QUALITY          │
│    SCORE            │
├─────────────────────┤
│ Validates           │
│ completeness with   │
│ detailed scoring    │
│ breakdown (0-100)   │
└─────────────────────┘
```

</td>
<td width="33%">

```
┌─────────────────────┐
│ 🇰🇷 KOREAN          │
│    SUPPORT          │
├─────────────────────┤
│ Unique clipboard    │
│ prompt with Korean  │
│ labels & context    │
│ (독특한 한국어 지원)    │
└─────────────────────┘
```

</td>
<td width="33%">

```
┌─────────────────────┐
│ ✅ TODO             │
│    INTEGRATION      │
├─────────────────────┤
│ Auto-includes       │
│ pending and         │
│ in-progress tasks   │
│ from .claude/       │
└─────────────────────┘
```

</td>
</tr>
</table>

---

## Quick Start

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                   THE HANDOFF WORKFLOW                    ║
    ╚═══════════════════════════════════════════════════════════╝

         ┌─────────────┐         ┌──────────────┐         ┌─────────────┐
         │  Session 1  │         │   /handoff   │         │  Session 2  │
         │  [Working]  │────────►│  [Generate]  │────────►│ [Continue]  │
         └─────────────┘         └──────────────┘         └─────────────┘
              │                         │                       │
              │                         ▼                       │
              │                   📋 Clipboard                  │
              │                   ✅ Document                   │
              │                   ⭐ Score                      │
              │                                                 │
              └─────────────────────────────────────────────────┘
                         [No Re-explaining Required]
```

### Installation

<table>
<tr>
<td width="33%">

**🏆 RECOMMENDED**
```bash
# Plugin Marketplace
/plugin marketplace add \
  quantsquirrel/claude-code-handoff
```
✅ Easiest
✅ Auto-validated
✅ Official source

</td>
<td width="33%">

**⚡ DIRECT INSTALL**
```bash
# From GitHub
/plugin install \
  quantsquirrel/claude-code-handoff
```
✅ Fast
✅ Auto-setup
✅ Simple

</td>
<td width="33%">

**🔧 MANUAL INSTALL**
```bash
# Clone repository
git clone https://github.com/\
quantsquirrel/claude-code-handoff.git \
~/.claude/skills/handoff
```
✅ Full control
✅ Local editing
✅ Development

</td>
</tr>
</table>

### Basic Usage

```bash
# Create a handoff with topic
/handoff "authentication refactoring"

# Create a handoff with auto-detected topic
/handoff

# Interactive mode with questions
/handoff --interactive
```

### Immediate Result

```
    ┌────────────────────────────────────────────────────┐
    │  After running /handoff, you'll get:               │
    ├────────────────────────────────────────────────────┤
    │                                                    │
    │  ✅  Document Created                              │
    │      .claude/handoffs/{timestamp}-{topic}.md       │
    │                                                    │
    │  📋  Clipboard Populated                           │
    │      Compressed prompt ready to paste              │
    │                                                    │
    │  ⭐  Quality Score                                 │
    │      0-100 validation with breakdown               │
    │                                                    │
    │  🔐  Security Check                                │
    │      Warnings if secrets detected                  │
    │                                                    │
    └────────────────────────────────────────────────────┘
```

---

## Installation

### Recommended: Plugin Marketplace (Easiest)

```bash
/plugin marketplace add quantsquirrel/claude-code-handoff
```

This automatically:
- Downloads the plugin from the official marketplace
- Places it in `~/.claude/skills/handoff`
- Registers the `/handoff` command
- Validates installation

### Alternative: Direct Install

```bash
/plugin install quantsquirrel/claude-code-handoff
```

This automatically:
- Downloads the plugin directly from GitHub
- Places it in `~/.claude/skills/handoff`
- Registers the `/handoff` command
- Validates installation

### Manual Installation

1. Clone the repository:
```bash
git clone https://github.com/quantsquirrel/claude-code-handoff.git ~/.claude/skills/handoff
```

2. Install dependencies:
```bash
cd ~/.claude/skills/handoff
npm install
```

3. Enable the skill in your Claude Code config:
```json
{
  "skills": {
    "handoff": {
      "enabled": true,
      "version": "1.0.0"
    }
  }
}
```

### Verification

```
    ┌────────────────────────────────────────────────────────┐
    │  ✅ VERIFY INSTALLATION                                │
    └────────────────────────────────────────────────────────┘
```

Verify installation by checking for the skill:
```bash
/plugin list | grep handoff
```

**Expected output:**
```
✅ handoff (v1.0.0) - Session handoff and context transfer
```

---

## Usage

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                      USAGE GUIDE                          ║
    ╚═══════════════════════════════════════════════════════════╝
```

### Basic Syntax

```bash
/handoff [topic]
```

**Parameters:**
- `topic` (optional) - Brief description of what you're handing off. If omitted, uses git branch name or current timestamp.

### Examples

```
    ┌────────────────────────────────────────────────────────┐
    │  💡 USAGE EXAMPLES                                     │
    └────────────────────────────────────────────────────────┘
```

#### 1. Simple Handoff with Topic

```bash
/handoff "user authentication migration"
```

**Output:**
```
    ╔════════════════════════════════════════════════════════════╗
    ║              HANDOFF GENERATION COMPLETE                   ║
    ╚════════════════════════════════════════════════════════════╝

    ✅  Document Created
        .claude/handoffs/2026-01-31-123456-auth-migration.md

    📋  Clipboard Ready (892 chars)
        Compressed prompt copied and ready to paste

    ⭐  Quality Score: 87/100
        ████████████████████░░░░

        Breakdown:
        ├─ Context Coverage       ████████████████████  95%
        ├─ Decision Documentation █████████████████     85%
        ├─ Failed Approaches      ████████████████      80%
        ├─ Secret Detection       ████████████████████  100%
        └─ Continuity Links       ███████████████       75%
```

#### 2. Interactive Mode

```bash
/handoff --interactive
```

**Prompts you with questions:**
```
    ┌────────────────────────────────────────────────────────┐
    │  💬 INTERACTIVE PROMPTS                                │
    ├────────────────────────────────────────────────────────┤
    │                                                        │
    │  ? What's the main topic?                             │
    │    > user authentication                              │
    │                                                        │
    │  ? Current blockers?                                  │
    │    > Database migration timing                        │
    │                                                        │
    │  ? Next priorities?                                   │
    │    > API integration testing                          │
    │                                                        │
    │  ? Previous handoff ID?                               │
    │    > 2026-01-30-092345                                │
    │                                                        │
    └────────────────────────────────────────────────────────┘
```

#### 3. Auto-Detect from Git Branch

```bash
# On branch: feature/dark-mode-redesign
/handoff
```

```
    ┌────────────────────────────────────────────────────────┐
    │  🔍 Auto-detected topic: dark-mode-redesign            │
    └────────────────────────────────────────────────────────┘
```

#### 4. With Custom Config

```bash
/handoff "database optimization" --config my-config.json
```

```
    ┌────────────────────────────────────────────────────────┐
    │  ⚙️  Loading custom settings from my-config.json       │
    └────────────────────────────────────────────────────────┘
```

---

## Output Format

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                   HANDOFF DOCUMENT FORMAT                 ║
    ╚═══════════════════════════════════════════════════════════╝
```

### Handoff Document Structure

Every handoff creates a markdown file with comprehensive sections:

```
    📁 File Location: .claude/handoffs/{date}-{time}-{topic}.md
```

<table>
<tr>
<td width="50%">

**📋 Document Sections**
- Context Summary
- Technical Details
- Key Decisions Made
- Failed Approaches
- Handoff Chain
- Blockers & Dependencies

</td>
<td width="50%">

**🔍 Additional Content**
- Environment & Setup
- Quality Metrics
- Security Considerations
- Resources & References
- Next Steps
- Compressed Prompt

</td>
</tr>
</table>

### Example Handoff Document

```markdown
# Session Handoff: User Authentication Migration

**Date:** January 31, 2026 10:34 AM
**Session ID:** sess_2026_01_31_103456
**Branch:** feature/auth-migration
**Duration:** 4h 32m

---

## Context Summary

### Current Objective
Migrate user authentication from custom JWT to Auth0, including database schema updates and frontend integration.

### Project Status
- Overall Progress: 65% complete
- Last Working State: Login form UI complete, backend integration in progress
- Critical Issue: None
- Deployment Blocked: No

---

## Technical Details

### Git Status
**Branch:** feature/auth-migration (ahead of main by 12 commits)

**Recent Commits:**
```
2026-01-31 10:15 - docs: update authentication flow diagrams
2026-01-30 16:42 - feat: add Auth0 configuration module
2026-01-30 14:21 - test: add Auth0 provider integration tests
2026-01-29 09:55 - fix: resolve JWT token refresh timing issue
```

**Staged Changes:**
- `src/auth/auth0-provider.ts` (modified)
- `src/config/environment.ts` (modified)
- `tests/auth0.test.ts` (added)

**Uncommitted Changes:**
```diff
diff --git a/src/auth/auth0-provider.ts b/src/auth/auth0-provider.ts
index abc123..def456 100644
--- a/src/auth/auth0-provider.ts
+++ b/src/auth/auth0-provider.ts
@@ -15,7 +15,7 @@ export class Auth0Provider {
   async initialize() {
-    const config = this.loadConfig();
+    const config = await this.loadConfigAsync();
     return this.client.initialize(config);
   }
```

### Active Tasks
- `[in_progress]` Implement Auth0 user sync endpoint
- `[in_progress]` Update database schema for Auth0 user IDs
- `[pending]` Integration testing with staging Auth0 tenant
- `[pending]` Documentation updates for new auth flow

---

## Key Decisions Made

### Architecture Decisions
1. **Decision:** Use Auth0 instead of custom JWT implementation
   - **Rationale:** Reduces maintenance burden, improves security posture
   - **Trade-off:** Adds external dependency, increases monthly costs
   - **Date:** January 25, 2026

2. **Decision:** Migrate user data during off-peak hours
   - **Rationale:** Minimal impact on active users
   - **Implementation:** Scheduled migration for 2:00-4:00 AM EST
   - **Date:** January 29, 2026

3. **Decision:** Keep legacy JWT validation during transition period
   - **Rationale:** Allows gradual rollout without forced logout
   - **Duration:** 30-day grace period
   - **Date:** January 30, 2026

### API Design
- New endpoint: `POST /api/auth/sync-to-auth0`
- Response format: Standard JWT with Auth0 claims
- Rate limiting: 100 requests/minute per user

---

## Failed Approaches & Learnings

### Attempt 1: Direct Database Migration
**What:** Migrating all user records in single transaction
**Why it failed:** Locked database for 2+ hours, caused production outage
**Lesson:** Always test with production-scale data before implementation
**Better approach:** Use batched async migration with transaction checkpoints

### Attempt 2: Client-Side Token Refresh
**What:** Implementing token refresh logic in React components
**Why it failed:** Race conditions when multiple components refresh simultaneously
**Lesson:** Centralize token management in authentication context
**Better approach:** Single source of truth in custom hook with mutex pattern

### Attempt 3: Immediate User Logout on Auth0 Switch
**What:** Force all users to re-login when switching providers
**Why it failed:** Angry users, support tickets flooded
**Lesson:** Always plan for graceful transitions
**Better approach:** Dual-validation period where both JWT and Auth0 work

---

## Handoff Chain

### Previous Session
**ID:** sess_2026_01_30_145632
**Topic:** Database schema planning for Auth0 integration
**Document:** `.claude/handoffs/2026-01-30-145632-db-schema-planning.md`
**Key Outcomes:**
- Finalized user_auth_tokens schema
- Identified 3 migration strategies

### Next Session (Expected)
**Planned Topic:** Auth0 provider integration testing
**Blockers to Address:** Database sync endpoint validation
**Handoff Time:** Tomorrow morning

---

## Blockers & Dependencies

### Current Blockers
1. **Auth0 Tenant Configuration**
   - Status: Waiting for DevOps approval
   - Impact: Cannot test end-to-end flow
   - ETA: January 31 EOD
   - Workaround: Use Auth0 sandbox tenant

2. **Database Migration Script**
   - Status: In code review
   - Impact: Cannot deploy to staging
   - Owner: @database-team
   - ETA: February 1

### External Dependencies
- Auth0 API availability (99.99% SLA)
- PostgreSQL 13+ (current version: 14.2)
- Node.js 18+ (current: 18.12.0)

---

## Environment & Setup

### Environment Variables
```bash
REACT_APP_AUTH0_DOMAIN=dev-xxxx.us.auth0.com
REACT_APP_AUTH0_CLIENT_ID=abc123def456
AUTH0_CLIENT_SECRET=*** (secure store)
DATABASE_URL=postgresql://user:pass@localhost:5432/auth_dev
MIGRATION_BATCH_SIZE=1000
```

### Installed Dependencies
```
auth0@10.8.0
@auth0/auth0-react@2.0.1
jsonwebtoken@9.0.0
dotenv@16.0.3
```

### Development Server
```bash
npm run dev
# Starts on http://localhost:3000
# Hot reload: enabled
# Debug mode: enabled
```

---

## Quality Metrics

### Code Coverage
- Unit Tests: 78%
- Integration Tests: 65%
- E2E Tests: 42%

### Performance Baseline
- Auth0 Token Exchange: 240ms avg
- User Sync Endpoint: 680ms avg
- Database Query (user lookup): 15ms avg

---

## Security Considerations

### Secrets Detected: 0 instances
✅ No API keys in code
✅ No database credentials in code
✅ All secrets in environment variables

### Security Checklist
- [ ] CORS configuration reviewed
- [ ] Rate limiting implemented
- [ ] Input validation added to all endpoints
- [ ] SQL injection prevention verified
- [ ] CSRF protection enabled

---

## Resources & References

### Documentation
- [Auth0 Integration Guide](https://auth0.com/docs/get-started/applications)
- [JWT Best Practices](https://tools.ietf.org/html/rfc8949)
- [Database Migration Patterns](https://wiki.postgresql.org/wiki/Replication,_Clustering,_and_Connection_Pooling)

### Related Issues
- #1234: User authentication migration epic
- #1245: Database schema update PR
- #1256: Auth0 config management

### Team Contacts
- **Auth0 Setup:** @devops-team
- **Database Migration:** @database-team
- **Frontend Integration:** @frontend-team

---

## Next Steps

### Immediate (Next 2 hours)
1. Complete Auth0 provider initialization
2. Add unit tests for token refresh logic
3. Deploy to staging environment

### Short-term (Next 24 hours)
1. Run integration tests against Auth0 sandbox
2. Load test with 100 concurrent users
3. Security audit of authentication flow

### Medium-term (Next week)
1. User acceptance testing
2. Documentation updates
3. Training session for support team

---

## Session Summary

**What was accomplished:**
- Implemented Auth0 provider module with 92% test coverage
- Updated 4 API endpoints for new auth flow
- Created migration strategy for 50K+ existing users
- Fixed JWT refresh race condition

**What needs follow-up:**
- Complete database migration endpoint validation
- Staging environment testing
- DevOps Auth0 tenant approval

**Confidence level:** 8/10 - Core auth logic solid, external dependencies pending

---

## Compressed Handoff Prompt

```
HANDOFF: User Authentication Migration
SESSION: sess_2026_01_31_103456
STATUS: 65% complete on feature/auth-migration
PROGRESS: Auth0 provider module complete, testing phase starting

CONTEXT:
- Migrating from custom JWT to Auth0
- Database schema updates ready for review
- 12 commits since yesterday's session

BLOCKERS:
- Waiting on Auth0 tenant config (DevOps)
- Database migration script in code review

NEXT:
1. Auth0 provider initialization (IN PROGRESS)
2. Integration testing (TODAY)
3. Staging deployment (TOMORROW)

KEY FILES:
- src/auth/auth0-provider.ts (modified)
- src/config/environment.ts (modified)
- tests/auth0.test.ts (new)

PREVIOUS SESSION: sess_2026_01_30_145632
```

---

## Handoff Metadata

```json
{
  "version": "1.0",
  "sessionId": "sess_2026_01_31_103456",
  "createdAt": "2026-01-31T10:34:56Z",
  "topic": "user-authentication-migration",
  "duration": "4h 32m",
  "branch": "feature/auth-migration",
  "commits": 12,
  "filesTour": 7,
  "decisionsMade": 3,
  "failedApproaches": 3,
  "qualityScore": 87,
  "previousSession": "sess_2026_01_30_145632",
  "nextSessionPlanned": true
}
```

---

```

### Compressed Clipboard Prompt

```
    ┌────────────────────────────────────────────────────────┐
    │  📋 CLIPBOARD FORMAT (Auto-Copied)                     │
    └────────────────────────────────────────────────────────┘
```

The skill also copies a compact version to your clipboard:

```
╔════════════════════════════════════════════════════════════════╗
║ [HANDOFF] User Auth Migration | Branch: feature/auth-migration║
╠════════════════════════════════════════════════════════════════╣
║ STATUS: 65% • BLOCKER: Auth0 tenant config pending            ║
║ PROGRESS: Auth0 provider done • TESTING: Starting today       ║
╠════════════════════════════════════════════════════════════════╣
║ FILES:                                                         ║
║   • src/auth/auth0-provider.ts                                ║
║   • src/config/environment.ts                                 ║
║   • tests/auth0.test.ts                                       ║
╠════════════════════════════════════════════════════════════════╣
║ DECISIONS:                                                     ║
║   • Auth0 adoption (25th)                                     ║
║   • Batch migration (29th)                                    ║
║   • Dual validation (30th)                                    ║
╠════════════════════════════════════════════════════════════════╣
║ FAILED APPROACHES:                                             ║
║   ✗ DB transaction lock → Use batched migration ✓            ║
║   ✗ Client refresh races → Centralize auth context ✓         ║
║   ✗ Force logout → Dual validation period ✓                  ║
╠════════════════════════════════════════════════════════════════╣
║ NEXT: Complete provider init → Staging test → Deploy         ║
║ PREV: sess_2026_01_30_145632                                  ║
╚════════════════════════════════════════════════════════════════╝
```

---

## Comparison with Alternatives

```
    ╔═══════════════════════════════════════════════════════════╗
    ║            WHY HANDOFF STANDS OUT                         ║
    ╚═══════════════════════════════════════════════════════════╝
```

<div align="center">

| Feature | **Handoff** | Softaworks | Willseltzer | Claude-Mem |
|:--------|:-----------:|:----------:|:-----------:|:----------:|
| **Context Capture** | ✅ Comprehensive | ✅ Basic | ✅ Moderate | ✅ Basic |
| **Clipboard Auto-Copy** | ✅ pbcopy/xclip | ❌ | ⚠️ Manual | ❌ |
| **Korean Support** | 🇰🇷 **Full** | ❌ | ❌ | ❌ |
| **Git Integration** | ✅ Full (history, diffs) | ⚠️ Branch only | ⚠️ Limited | ❌ |
| **Todo Integration** | ✅ .claude format | ❌ | ❌ | ⚠️ Basic |
| **Failed Approaches** | ✅ **Dedicated section** | ❌ | ❌ | ❌ |
| **Handoff Chain** | ⛓️ **Link prev/next** | ❌ | ❌ | ❌ |
| **Secret Detection** | 🔐 **With warnings** | ❌ | ❌ | ❌ |
| **Quality Score** | ⭐ **Detailed 0-100** | ❌ | ⚠️ Simple | ❌ |
| **Session Metadata** | ✅ Comprehensive | ⚠️ Minimal | ✅ Good | ⚠️ Minimal |
| **Custom Config** | ✅ Full support | ⚠️ Limited | ⚠️ Some | ✅ Full |
| **Claude Code Integration** | ✅ Native | ⚠️ Plugin | ⚠️ Plugin | ✅ Native |

</div>

```
    ┌────────────────────────────────────────────────────────┐
    │  🏆 UNIQUE TO HANDOFF                                  │
    ├────────────────────────────────────────────────────────┤
    │                                                        │
    │  🇰🇷  Full Korean language support                     │
    │  🚫  Failed approaches tracking                        │
    │  ⛓️   Session chain linking                            │
    │  🔐  Secret detection & warnings                       │
    │  ⭐  Quality scoring (0-100)                           │
    │                                                        │
    └────────────────────────────────────────────────────────┘
```

---

## Configuration

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                   CONFIGURATION OPTIONS                   ║
    ╚═══════════════════════════════════════════════════════════╝
```

### Default Configuration

Create `.claude/handoffs.config.json`:

```json
{
  "outputDir": ".claude/handoffs",
  "includeGitDiff": true,
  "includeTaskList": true,
  "secretDetection": true,
  "qualityValidation": true,
  "clipboardFormat": "compressed",
  "language": "en",
  "maxDiffLines": 50,
  "maxCommitsToShow": 10,
  "includeEnvironmentVariables": false,
  "failedApproachesRequired": false,
  "handoffChainTracking": true,
  "encryptSensitiveData": false
}
```

### Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `outputDir` | string | `.claude/handoffs` | Where to save handoff documents |
| `includeGitDiff` | boolean | `true` | Include file diffs in output |
| `includeTaskList` | boolean | `true` | Include .claude/tasks.json in output |
| `secretDetection` | boolean | `true` | Scan for API keys and credentials |
| `qualityValidation` | boolean | `true` | Calculate and display quality score |
| `clipboardFormat` | string | `compressed` | `compressed` or `full` |
| `language` | string | `en` | `en` or `ko` (Korean) |
| `maxDiffLines` | number | `50` | Maximum lines per file diff |
| `maxCommitsToShow` | number | `10` | Recent commits to include |
| `includeEnvironmentVariables` | boolean | `false` | Include env vars (security risk) |
| `failedApproachesRequired` | boolean | `false` | Enforce failed approaches section |
| `handoffChainTracking` | boolean | `true` | Track previous/next sessions |
| `encryptSensitiveData` | boolean | `false` | Encrypt handoff file contents |

### Using Custom Configuration

```
    ┌────────────────────────────────────────────────────────┐
    │  ⚙️  CUSTOM CONFIGURATION EXAMPLES                     │
    └────────────────────────────────────────────────────────┘
```

<table>
<tr>
<td width="50%">

**📁 Use Config File**
```bash
/handoff "topic" \
  --config /path/to/config.json
```

</td>
<td width="50%">

**🔧 Override Single Option**
```bash
/handoff "topic" \
  --includeGitDiff false
```

</td>
</tr>
<tr>
<td width="50%">

**🇰🇷 Korean Output**
```bash
/handoff "topic" --language ko
```

</td>
<td width="50%">

**📋 Custom Clipboard Format**
```bash
/handoff "topic" \
  --clipboardFormat full
```

</td>
</tr>
</table>

---

## Advanced Usage

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                    ADVANCED FEATURES                      ║
    ╚═══════════════════════════════════════════════════════════╝
```

### Programmatic Access

```javascript
const { createHandoff } = require('@claude-code/handoff');

const handoff = await createHandoff({
  topic: 'database migration',
  config: {
    outputDir: './.handoffs',
    language: 'ko'
  }
});

console.log(`Created: ${handoff.path}`);
console.log(`Quality Score: ${handoff.qualityScore}/100`);
console.log(`Clipboard: ${handoff.clipboardPrompt}`);
```

### Extending Handoff

Add custom sections:

```javascript
const handoff = await createHandoff({
  topic: 'feature-x',
  customSections: {
    'Performance Metrics': async () => {
      return await getPerformanceStats();
    },
    'Team Updates': async () => {
      return await fetchTeamMessages();
    }
  }
});
```

### Automation

Create a pre-commit hook for automatic handoffs:

```bash
#!/bin/bash
# .git/hooks/pre-commit

if [ "$AUTO_HANDOFF" = "true" ]; then
  /handoff --auto --topic "auto-commit-$(date +%s)"
fi
```

### Secret Detection Details

```
    ┌────────────────────────────────────────────────────┐
    │  🔐 SECRET PATTERNS DETECTED                       │
    ├────────────────────────────────────────────────────┤
    │                                                    │
    │  ✓  AWS keys (AKIA...)                            │
    │  ✓  Google API keys                               │
    │  ✓  GitHub tokens (ghp_...)                       │
    │  ✓  Database credentials (postgresql://user:pass) │
    │  ✓  API keys in URLs                              │
    │  ✓  Private encryption keys                       │
    │  ✓  JWT secrets                                   │
    │  ✓  OAuth tokens                                  │
    │                                                    │
    └────────────────────────────────────────────────────┘
```

**🔒 Security Note:** Handoff files should be kept in `.gitignore` if they contain secrets.

---

## Contributing

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                  CONTRIBUTE TO HANDOFF                    ║
    ╚═══════════════════════════════════════════════════════════╝
```

We welcome contributions! Please follow these guidelines:

### Development Setup

```bash
git clone https://github.com/quantsquirrel/claude-code-handoff.git
cd handoff
npm install
npm run dev
```

### Running Tests

```bash
# Unit tests
npm test

# Integration tests
npm run test:integration

# Full test suite
npm run test:all
```

### Submitting Changes

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Make your changes with tests
4. Ensure all tests pass: `npm test`
5. Commit with clear messages: `git commit -am 'Add feature: my-feature'`
6. Push and create a Pull Request

### Code Style

- Use TypeScript for all code
- Follow ESLint configuration (run `npm run lint`)
- Add tests for new features
- Document public APIs with JSDoc comments

### Report Issues

Found a bug? [Open an issue](https://github.com/quantsquirrel/claude-code-handoff/issues) with:
- Clear description of the problem
- Steps to reproduce
- Expected vs actual behavior
- Environment details (OS, Node version, Claude Code version)

---

## Troubleshooting

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                  TROUBLESHOOTING GUIDE                    ║
    ╚═══════════════════════════════════════════════════════════╝
```

### Handoff Not Copying to Clipboard

**Problem:** Compressed prompt not appearing in clipboard

**Solutions:**
1. Check if `pbcopy` (macOS) or `xclip` (Linux) is installed:
   ```bash
   # macOS
   which pbcopy
   
   # Linux
   which xclip
   ```

2. Grant permissions if needed:
   ```bash
   # Linux
   sudo apt-get install xclip
   ```

3. Use alternative output method:
   ```bash
   /handoff "topic" --output file  # Save to file instead
   ```

### Quality Score Too Low

**Problem:** Quality score below 70/100

**Possible reasons:**
- Missing git repository or commits
- No pending tasks in `.claude/tasks.json`
- Incomplete failed approaches section
- No previous handoff chain

**Improvements:**
- Ensure git is initialized: `git init`
- Add task descriptions to `.claude/tasks.json`
- Document what didn't work during your session
- Link to previous session: `/handoff "topic" --previous sess_id`

### Secret Detection False Positives

**Problem:** Legitimate strings flagged as secrets

**Solution:**
Create `.handoffignore` for safe patterns:

```
# .handoffignore
^\$\{.*\}$  # Ignore template variables
^test-.*$   # Ignore test API keys
```

### Large Handoff Files

**Problem:** Handoff document too large (>10MB)

**Solution:**
Reduce content scope:

```bash
/handoff "topic" --maxDiffLines 20 --maxCommitsToShow 5
```

---

## Performance Considerations

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                 PERFORMANCE OPTIMIZATION                  ║
    ╚═══════════════════════════════════════════════════════════╝
```

### Optimization Tips

1. **Reduce diff size** for large repositories:
   ```bash
   /handoff "topic" --maxDiffLines 30
   ```

2. **Limit commit history**:
   ```bash
   /handoff "topic" --maxCommitsToShow 5
   ```

3. **Skip optional sections** to speed up generation:
   ```bash
   /handoff "topic" --skipSecretDetection --skipQualityScore
   ```

### Generation Time

| Repository Size | Typical Time | Notes |
|-----------------|-------------|-------|
| Small (<1k files) | 2-3 seconds | Usually instant |
| Medium (1k-10k files) | 5-10 seconds | Depends on diff size |
| Large (10k+ files) | 15-30 seconds | Limit diffs accordingly |

---

## License

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                        MIT LICENSE                        ║
    ╚═══════════════════════════════════════════════════════════╝
```

<div align="center">

**Copyright © 2026 Handoff Contributors**

MIT License - see [LICENSE](LICENSE) file for details.

</div>

<table>
<tr>
<td width="50%">

**✅ You are FREE to:**
- ✓ Use commercially
- ✓ Modify the source code
- ✓ Distribute copies
- ✓ Include in proprietary software

</td>
<td width="50%">

**📋 Under these CONDITIONS:**
- ✓ Include original copyright notice
- ✓ Include license text with distributions
- ✓ State significant changes made

</td>
</tr>
</table>

---

## 한국어 (Korean)

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                                                           ║
    ║                🇰🇷  한국어 사용 가이드  🇰🇷                 ║
    ║                                                           ║
    ║            Korean Language Support & Guide                ║
    ║                                                           ║
    ╚═══════════════════════════════════════════════════════════╝
```

### 소개

**Handoff**는 Claude Code에서 세션 간 컨텍스트를 효율적으로 전달하는 독립적이고 standalone 플러그인입니다. 프로젝트의 상태, 결정사항, 진행상황을 자동으로 기록하고, 클립보드에 압축된 프롬프트를 복사합니다.

> **별도의 프레임워크 의존성 없이 독립적으로 작동합니다.**

### 주요 특징

<table>
<tr>
<td width="50%">

```
┌──────────────────────────┐
│ 🎯 포괄적 컨텍스트 캡처    │
├──────────────────────────┤
│ 프로젝트 상태, 결정사항,   │
│ 진행상황 자동 기록         │
└──────────────────────────┘
```

```
┌──────────────────────────┐
│ 🔗 Git 통합               │
├──────────────────────────┤
│ 커밋 히스토리, 브랜치,     │
│ 스테이지된 변경사항 포함   │
└──────────────────────────┘
```

```
┌──────────────────────────┐
│ 🇰🇷 한국어 지원           │
├──────────────────────────┤
│ 한국어 라벨과 컨텍스트를   │
│ 포함한 클립보드 프롬프트   │
└──────────────────────────┘
```

```
┌──────────────────────────┐
│ ⛓️ Handoff 체인           │
├──────────────────────────┤
│ 이전/다음 세션을 연결하여  │
│ 연속성 유지               │
└──────────────────────────┘
```

</td>
<td width="50%">

```
┌──────────────────────────┐
│ 📋 클립보드 자동 복사      │
├──────────────────────────┤
│ 한 줄의 명령으로 압축된    │
│ 프롬프트가 클립보드에 복사 │
└──────────────────────────┘
```

```
┌──────────────────────────┐
│ ✅ Todo 통합              │
├──────────────────────────┤
│ .claude/tasks.json의      │
│ 작업 자동 포함            │
└──────────────────────────┘
```

```
┌──────────────────────────┐
│ 🚫 실패한 접근법 추적      │
├──────────────────────────┤
│ 작동하지 않은 것을         │
│ 문서화하여 반복 방지       │
└──────────────────────────┘
```

```
┌──────────────────────────┐
│ 🔐 시크릿 검출            │
├──────────────────────────┤
│ API 키, 자격증명 등        │
│ 잠재적 보안 위험 경고      │
└──────────────────────────┘
```

</td>
</tr>
</table>

```
    ⭐ 품질 점수: Handoff 완성도를 0-100 점수로 검증
```

### 설치

<table>
<tr>
<td width="33%">

**🏆 추천 방법**
```bash
# 플러그인 마켓플레이스
/plugin marketplace add \
  quantsquirrel/\
  claude-code-handoff
```
✅ 가장 쉬움
✅ 자동 검증

</td>
<td width="33%">

**⚡ 직접 설치**
```bash
# GitHub에서
/plugin install \
  quantsquirrel/\
  claude-code-handoff
```
✅ 빠름
✅ 자동 설정

</td>
<td width="33%">

**🔧 수동 설치**
```bash
# 저장소 복제
git clone \
  https://github.com/\
  quantsquirrel/\
  claude-code-handoff.git \
  ~/.claude/skills/handoff
```
✅ 완전한 제어

</td>
</tr>
</table>

### 사용법

```bash
# 주제와 함께 handoff 생성
/handoff "인증 리팩토링"

# 상호 대화 모드
/handoff --interactive

# 한국어 출력
/handoff "주제" --language ko
```

### 결과

```
    ┌────────────────────────────────────────────────┐
    │  /handoff 실행 후:                             │
    ├────────────────────────────────────────────────┤
    │                                                │
    │  ✅  문서 생성됨                                │
    │      .claude/handoffs/{timestamp}-{topic}.md   │
    │                                                │
    │  📋  클립보드에 복사됨                          │
    │      압축된 프롬프트 붙여넣기 준비 완료         │
    │                                                │
    │  📊  품질 점수 표시                             │
    │      0-100 점수 및 상세 분석                   │
    │                                                │
    │  🔐  보안 검사                                 │
    │      시크릿 감지 시 경고 표시                   │
    │                                                │
    └────────────────────────────────────────────────┘
```

### 한국어 사용자를 위한 팁

```
    ┌────────────────────────────────────────────────────────┐
    │  💡 한국어로 Handoff 사용하기                           │
    └────────────────────────────────────────────────────────┘
```

**1. 언어 설정**
```bash
/handoff "주제" --language ko
```

**2. 한국어 클립보드 프롬프트 예시**
```
┌──────────────────────────────────────────────────────────────┐
│ [인수인계] 사용자 인증 마이그레이션                           │
│ 브랜치: feature/auth-migration                               │
├──────────────────────────────────────────────────────────────┤
│ 상태: 65% • 차단 요소: Auth0 테넌트 구성 대기 중              │
│ 진행: Auth0 제공자 완료 • 테스트: 오늘 시작                   │
├──────────────────────────────────────────────────────────────┤
│ 파일: src/auth/auth0-provider.ts                             │
│       src/config/environment.ts                              │
├──────────────────────────────────────────────────────────────┤
│ 결정사항:                                                     │
│   • Auth0 도입 (25일)                                        │
│   • 배치 마이그레이션 (29일)                                  │
│   • 이중 검증 (30일)                                         │
├──────────────────────────────────────────────────────────────┤
│ 실패한 접근법:                                                │
│   DB 트랜잭션 락 → 배치 마이그레이션 사용 ✓                   │
├──────────────────────────────────────────────────────────────┤
│ 다음: 제공자 초기화 완료 → 스테이징 테스트 → 배포            │
└──────────────────────────────────────────────────────────────┘
```

**3. 설정 파일** (`.claude/handoffs.config.json`)
```json
{
  "language": "ko",
  "outputDir": ".claude/handoffs",
  "clipboardFormat": "compressed"
}
```

### 한국어 설명

```
    ┌────────────────────────────────────────────────────────┐
    │  📋 Handoff 문서의 주요 섹션                            │
    └────────────────────────────────────────────────────────┘
```

<table>
<tr>
<td width="50%">

**📌 필수 섹션**

| 섹션 | 설명 |
|------|------|
| **컨텍스트 요약** | 현재 목표, 프로젝트 상태 |
| **기술 세부사항** | Git 상태, 활성 작업, 코드 변경 |
| **핵심 결정사항** | 아키텍처 결정, API 설계 |
| **다음 단계** | 즉시 조치사항, 단기/중기 계획 |

</td>
<td width="50%">

**🌟 고급 섹션**

| 섹션 | 설명 |
|------|------|
| **실패한 접근법** | 작동하지 않은 것, 교훈 |
| **Handoff 체인** | 이전/다음 세션 링크 |
| **차단 요소** | 현재 차단 요소, 외부 의존성 |
| **품질 메트릭** | 코드 커버리지, 성능 지표 |

</td>
</tr>
</table>

### 고급 사용법

**프로그래매틱 접근**:

```javascript
const { createHandoff } = require('@claude-code/handoff');

const handoff = await createHandoff({
  topic: '데이터베이스 마이그레이션',
  language: 'ko'  // 한국어 출력
});

console.log(`생성됨: ${handoff.path}`);
console.log(`품질 점수: ${handoff.qualityScore}/100`);
```

### 문제 해결

```
    ┌────────────────────────────────────────────────────────┐
    │  🔧 자주 발생하는 문제 해결                             │
    └────────────────────────────────────────────────────────┘
```

<table>
<tr>
<td width="50%">

**❌ 클립보드에 복사되지 않음**

```bash
# macOS 확인
which pbcopy

# Linux 확인
which xclip

# 설치 필요 시
sudo apt-get install xclip
```

</td>
<td width="50%">

**📊 품질 점수가 낮음**

- ✓ Git 저장소 초기화: `git init`
- ✓ 작업 설명 추가: `.claude/tasks.json`
- ✓ 실패한 접근법 문서화
- ✓ 이전 Handoff 링크:
  `/handoff "주제" --previous sess_id`

</td>
</tr>
</table>

### 피드백 및 기여

```
    ┌────────────────────────────────────────────────────────┐
    │  💬 한국어 관련 이슈나 기여를 환영합니다!               │
    └────────────────────────────────────────────────────────┘
```

<div align="center">

[GitHub Issues에서 제출하기](https://github.com/quantsquirrel/claude-code-handoff/issues)

</div>

---

## Support

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                      GETTING HELP                         ║
    ╚═══════════════════════════════════════════════════════════╝
```

<table>
<tr>
<td width="50%">

**📚 Resources**
- **Documentation:** Check the [docs](./docs) directory
- **Examples:** See [examples](./examples) directory

</td>
<td width="50%">

**💬 Community**
- **Issues:** [GitHub Issues](https://github.com/quantsquirrel/claude-code-handoff/issues)
- **Discussions:** [GitHub Discussions](https://github.com/quantsquirrel/claude-code-handoff/discussions)

</td>
</tr>
</table>

### Citation

<div align="center">

If you use Handoff in your workflow, consider giving it a star on GitHub:

```
    ⭐ github.com/quantsquirrel/claude-code-handoff ⭐
```

</div>

---

## Changelog

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                      VERSION HISTORY                      ║
    ╚═══════════════════════════════════════════════════════════╝
```

### v1.0.0 (January 31, 2026)

```
    ┌────────────────────────────────────────────────────────┐
    │  🎉 INITIAL RELEASE                                    │
    └────────────────────────────────────────────────────────┘
```

<table>
<tr>
<td width="50%">

**✨ Core Features**
- ✨ Full handoff document generation
- 📋 Clipboard auto-copy with pbcopy/xclip
- 🔗 Git integration with diffs and commit history
- ✅ Todo list integration
- 📊 Comprehensive session metadata

</td>
<td width="50%">

**🌟 Advanced Features**
- 🇰🇷 Korean language support
- 🚫 Failed approaches tracking
- ⛓️ Handoff chain linking
- 🔐 Secret detection and warnings
- ⭐ Quality score validation

</td>
</tr>
</table>

---

## Acknowledgments

```
    ╔═══════════════════════════════════════════════════════════╗
    ║                     ACKNOWLEDGMENTS                       ║
    ╚═══════════════════════════════════════════════════════════╝
```

<div align="center">

**Built for the Claude Code ecosystem with ❤️**

```
    ┌────────────────────────────────────────────────────┐
    │                                                    │
    │  Special thanks to the Claude Code community      │
    │  for feedback and feature suggestions             │
    │                                                    │
    │  🙏 Contributors • 💡 Ideas • 🐛 Bug Reports      │
    │                                                    │
    └────────────────────────────────────────────────────┘
```

</div>

---

```
    ╔════════════════════════════════════════════════════════════════╗
    ║                                                                ║
    ║                  🏃 READY TO PASS THE BATON? 🏃                ║
    ║                                                                ║
    ║              Run /handoff and watch your context               ║
    ║                  transfer seamlessly! 🚀                       ║
    ║                                                                ║
    ╚════════════════════════════════════════════════════════════════╝
```

<div align="center">

```
    ════════════════════════════════════════════════════════════

         🏃 Pass the baton. Keep the momentum.

         Never explain your codebase twice.

    ════════════════════════════════════════════════════════════
```

**Built for the Claude Code ecosystem with ❤️**

Made by [QuantSquirrel](https://github.com/quantsquirrel) | [Report Issue](https://github.com/quantsquirrel/claude-code-handoff/issues) | [Contribute](https://github.com/quantsquirrel/claude-code-handoff/blob/main/CONTRIBUTING.md)

```
    ⭐ Star us on GitHub: github.com/quantsquirrel/claude-code-handoff
```

</div>
