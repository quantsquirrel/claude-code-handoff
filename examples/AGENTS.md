<!-- Parent: ../AGENTS.md -->

# Examples Directory - AGENTS.md

## Purpose

This directory contains canonical example handoff documents demonstrating proper structure, content organization, and quality standards for multi-session project handoffs. These examples serve as reference implementations for both human developers and AI agents creating or working with handoff documents.

The examples directory teaches:
- **Structure**: How to organize complex project information for seamless session continuity
- **Quality**: What constitutes a high-quality (70+) handoff document
- **Patterns**: Common patterns and anti-patterns in technical handoffs
- **Best Practices**: How to write clear Next Steps, document Failed Approaches, and track Key Decisions

---

## Directory Contents

### Files

| File | Description | Use Case |
|------|-------------|----------|
| `example-handoff.md` | Complete JWT authentication session handoff | Production-quality reference for complex feature work |

### Size & Metrics

```
examples/
├── example-handoff.md       (450 lines, Quality Score: 87/100)
└── AGENTS.md                (This file)
```

---

## Reference Document: example-handoff.md

### Overview

**Project:** JWT User Authentication System
**Session ID:** sess-auth-001
**Quality Score:** 87/100
**Duration:** 4.5 hours
**Lead Agent:** executor (Sonnet)

The example document demonstrates a complete authentication feature implementation across one session, including:
- Production-ready JWT token management with refresh rotation
- Role-based access control (RBAC) implementation
- Security considerations and trade-offs
- Clear tracking of completed work and pending items

### Structure Breakdown

#### 1. Session Summary (Lines 11-13)
**Purpose:** 3-5 sentence overview of session outcomes

```markdown
## 1. Session Summary

This session successfully implemented a production-ready JWT authentication
system with refresh token rotation, token blacklisting, and role-based access
control. The backend server now supports user registration, login, token refresh,
and logout flows with comprehensive validation and error handling. All core
authentication features are complete and tested with 94% code coverage.
```

**Quality Indicators:**
- Specific outcomes, not vague promises
- Measurable results (94% code coverage)
- Clear scope boundary
- Realistic assessment of completion state

#### 2. Completed (Lines 17-37)
**Purpose:** Checkbox list with detailed descriptions of completed work

```markdown
## 2. Completed ✅

- [x] **JWT Token Generation & Verification**
  - Implemented HS256 signing algorithm with configurable expiration
  - Added refresh token rotation mechanism to prevent token reuse attacks
  - Created token validation middleware with proper error handling
```

**Quality Indicators:**
- Specific technical details, not just task names
- Sub-bullets explain "what" and "how"
- Actionable for next session to understand what's available

#### 3. Pending (Lines 41-55)
**Purpose:** Checkbox list with status, blockers, and effort estimates

```markdown
## 3. Pending ⏳

- [ ] **OAuth2 Social Login Integration**
  - Google and GitHub OAuth2 providers not yet implemented
  - Plan: Use Passport.js middleware pattern (estimated 2-3 hours)
  - Dependency: Requires OAuth app credentials from user
```

**Quality Indicators:**
- Status indicators (% complete or description)
- Blockers identified (external dependencies)
- Realistic effort estimates with units

#### 4. Key Decisions (Lines 59-90)
**Purpose:** Record important trade-offs and their rationale

```markdown
### Decision 1: HTTP-Only Cookies for Refresh Tokens
**Rationale:** While access tokens are in memory (vulnerable to XSS but not
CSRF), refresh tokens are stored in HTTP-only cookies. This balances security
against both XSS and CSRF attacks. Trade-off: Requires CORS configuration.

**Impact:**
- Server-side cookie handling reduces frontend token management complexity
- Requires SameSite cookie policy configuration
- More secure than localStorage for sensitive tokens
```

**Quality Indicators:**
- Explains "why" this decision was chosen
- Lists impact/consequences
- Acknowledges trade-offs
- Helps next session understand architectural constraints

#### 5. Known Issues (Lines 92-122)
**Purpose:** Document problems discovered, workarounds, and resolution timeline

```markdown
### Issue 1: Race Condition in Token Refresh
**Description:** During concurrent refresh requests with same token, both might
succeed briefly before blacklisting.

**Workaround:**
```javascript
// Implemented idempotency key in refresh endpoint
const idempotencyKey = `${userId}:${refreshToken}`;
const cached = await redis.get(idempotencyKey);
if (cached) return JSON.parse(cached); // Return cached response
```

**Resolution Timeline:** Will implement distributed locks in next session
(low priority - race window is <100ms)
```

**Quality Indicators:**
- Specific reproduction conditions
- Concrete workaround with code example
- Risk assessment (low priority + why)
- Clear resolution path

#### 6. Files Modified (Lines 125-174)
**Purpose:** Track all code changes with context

```markdown
### `/src/middleware/auth.middleware.ts`
**Status:** ✅ Complete
**Changes:**
- Created JWT verification middleware with error handling
- Added role extraction from token claims
- Implements token expiration validation with proper HTTP 401 responses
- 한국어 코멘트: "토큰 검증 실패 시 401 반환하고 클라이언트는 refresh 시도"

**Lines Changed:** 45 lines added, 0 removed
```

**Quality Indicators:**
- File path is absolute and consistent
- Status shows completion level
- Changes section describes "why", not just "what"
- Line counts help estimate scope
- Korean comments preserved where relevant (follows CLAUDE.md language preference)

#### 7. Failed Approaches (Lines 177-209)
**Purpose:** Prevent repeating mistakes in future sessions

```markdown
### Approach 1: Storing All Tokens in JWT Claims
**What We Tried:** Encoding token version and rotation count directly in JWT
payload to avoid Redis lookups.

**Why It Failed:**
- JWTs are immutable once signed; couldn't invalidate old tokens immediately
- Client caching of tokens masked revocation
- Logout became eventual-consistent (users could use old tokens briefly)

**Resolution:** Abandoned this approach. Implemented Redis-backed blacklist
instead for instant revocation.

**Lesson Learned:** For security-critical operations, statefulness is worth
the complexity. Don't sacrifice security for theoretical performance.
```

**Quality Indicators:**
- Concrete approach with clear attempt
- Root cause analysis (immutability, caching, consistency)
- Learning extracted for future reference
- No blame/negativity, just facts

#### 8. Handoff Chain (Lines 212-227)
**Purpose:** Link to previous/next sessions and show project progression

```markdown
## 8. Handoff Chain

**Current Session:** sess-auth-001 (this document)
**Previous Session:** sess-auth-init (Session #1 - Project setup, user model, database)
**Chain Length:** 2 sessions

**Context from Previous Sessions:**
- Project initialized with Express + TypeScript + PostgreSQL
- User model with email, password, role fields
- Database migrations set up with knex.js
- Docker Compose with PostgreSQL and Redis
```

**Quality Indicators:**
- Links to previous session with session ID
- Summarizes prior session context (what's already done)
- Enables session jumping and context restoration

#### 9. Next Steps (Lines 230-249)
**Purpose:** Prioritized, actionable steps for next session

```markdown
### 1. **IMMEDIATE (Next 30 mins)** 🔥
   - [ ] Deploy authentication service to staging environment
   - [ ] Run production load tests (simulate 100 concurrent login requests)
   - [ ] Verify Redis connection pooling under sustained load
   - **Why:** Catch configuration issues before moving to frontend integration

### 2. **HIGH PRIORITY (Next Session, 2 hours)** 📍
   - [ ] Implement OAuth2 social login (Google + GitHub)
   - [ ] Add email verification flow with Resend email service
   - [ ] Create comprehensive API documentation with cURL examples
   - **Why:** Frontend team blocked waiting for OAuth + email verification
```

**Quality Indicators:**
- Time estimates with context (which session)
- Why section explains business/technical rationale
- Priorities clearly marked (IMMEDIATE/HIGH/MEDIUM)
- Checkboxes for tracking
- Specific, actionable items (not vague like "improve performance")

#### 10. How to Resume (Lines 252-345)
**Purpose:** Commands and context to jump back into work

```markdown
### Quick Start (5 minutes)
```bash
# 1. Pull latest changes
git pull origin main

# 2. Start development environment
docker-compose up -d

# 3. Install dependencies
npm install

# 4. Run database migrations
npm run migrate

# 5. Start development server
npm run dev
```

### Key Files to Understand First
1. **`/src/services/auth.service.ts`** - Core authentication logic
   - Token generation, verification, rotation
   - 한국어 주석으로 각 함수의 목적 설명

### Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| "Redis connection refused" | Verify `docker-compose ps` shows Redis running on port 6379 |
| "JWT_SECRET undefined" | Copy `.env.example` to `.env` and generate new secret |
```

**Quality Indicators:**
- Copy-paste ready commands
- No guesswork about environment
- Key files ranked by importance
- Common issues pre-documented
- Runnable from scratch in 5 minutes

#### 11. Quality Score (Lines 349-389)
**Purpose:** Self-assessment with breakdown and recommendations

```markdown
## 11. Quality Score

### Overall Session Quality: **87/100** ✨

#### Breakdown by Category:

| Category | Score | Notes |
|----------|-------|-------|
| **Code Quality** | 89/100 | Clean, well-commented, follows SOLID principles. Minor: Could add more JSDoc comments for public API |
| **Test Coverage** | 94/100 | 94% lines covered. Only gaps: edge cases in token expiration boundaries |
| **Security** | 91/100 | Strong fundamentals. Deferred: OAuth2 integration, email verification |
...

#### What Went Well ✅
- Zero security vulnerabilities identified in penetration testing
- Token rotation mechanism handles edge cases well

#### Improvement Opportunities 📈
- Add database query logging for debugging (APM integration)
- Implement refresh token family tracking (detect stolen tokens early)

#### Risk Assessment 🎯
- **Low Risk:** Core authentication flows are battle-tested
- **Medium Risk:** OAuth2 integration timing (frontend team blocked)
```

**Quality Indicators:**
- Honest assessment (87/100, not perfect)
- Category breakdown shows balanced evaluation
- Specific improvements, not generic feedback
- Risk assessment helps next session prioritize
- Transparent about what could be better

---

## For AI Agents Working in This Directory

### Pattern Recognition & Replication

When creating handoff documents:

1. **Study Structure First**
   - Read example-handoff.md end-to-end
   - Note the flow: Summary → Completed → Pending → Decisions → Issues → Files → Failed Approaches → Chain → Next Steps
   - Understand why this order enables efficient resumption

2. **Match Specificity Levels**
   - Don't write "Added authentication" → Write "Implemented JWT token generation with HS256 signing and 15-minute expiration"
   - Don't write "Security consideration" → Write concrete trade-offs like the HTTP-only cookie decision

3. **Replicate Failed Approaches Pattern**
   - Include what was tried, why it failed, and the lesson
   - Use example's format: concrete approach → root cause analysis → learning
   - This prevents next session from repeating mistakes

4. **Track Decisions with Impact**
   - Every architectural choice needs rationale and consequences
   - Example shows Decision 1 (HTTP-only cookies), Decision 2 (Stateful blacklist), Decision 3 (bcrypt cost)
   - Impact section explains downstream effects

5. **Make Next Steps Actionable**
   - Include time estimates
   - Explain "why" for business context
   - Order by actual priority, not arbitrary ranking
   - Example: "Frontend team blocked waiting for OAuth2" explains urgency

### Quality Benchmarks

When evaluating handoff quality, use example-handoff.md as reference:

| Aspect | Example Standard | How to Verify |
|--------|------------------|---------------|
| **Completed Items** | Specific with sub-bullets explaining "how" | Can someone understand what's available without reading code? |
| **Pending Items** | Includes status %, blockers, effort estimates | Would next session know where to start? |
| **Failed Approaches** | Root cause analysis + lesson learned | Does it prevent repeating the mistake? |
| **Next Steps** | Time estimates + "why" rationale | Can next session start immediately? |
| **Quality Score** | Honest (87/100, not inflated) | Are weaknesses acknowledged? |
| **Files Modified** | Shows status, changes, line counts | Can next session find the modified files? |

### Minimum vs. Excellent Standards

**Minimum (70/100) Handoff Should Have:**
- [x] All sections present (Summary, Completed, Pending, etc.)
- [x] No TODO/TBD placeholders
- [x] No secrets/credentials exposed
- [x] Next Steps are specific (times + details)
- [x] Files Modified listed completely

**Excellent (85+/100) Handoff Also Has:**
- [x] Failed Approaches with root cause analysis
- [x] Key Decisions with trade-offs and impact
- [x] Known Issues with workarounds
- [x] Quality Score breakdown showing honest assessment
- [x] Quick Start commands (copy-paste ready)
- [x] Architecture diagrams (ASCII or Mermaid)
- [x] Risk assessment for pending items

---

## Testing Example-Handoff.md

### Validation Script

The example document passes validation with:

```bash
# Validate structure (if you have validate.sh from parent directory)
bash ../scripts/validate.sh example-handoff.md

# Expected output:
# ✅ All required sections present
# ✅ No TODO placeholders found
# ✅ No secrets detected
# ✅ Quality Score: 87/100 (Excellent)
```

### Verification Checklist

- [x] Session Summary: 3-5 sentences, specific outcomes
- [x] Completed: 5 items with detailed descriptions
- [x] Pending: 3 items with blockers and effort estimates
- [x] Key Decisions: 3 decisions with rationale and impact
- [x] Known Issues: 2 issues with workarounds
- [x] Files Modified: 5 files with changes described
- [x] Failed Approaches: 2 approaches with lessons
- [x] Handoff Chain: Previous session linked, progression shown
- [x] Next Steps: 3 priority tiers with time estimates
- [x] How to Resume: Copy-paste commands, common issues listed
- [x] Quality Score: 87/100 with honest breakdown

### Common Issues & How Example Handles Them

| Issue | Example's Approach | Lesson |
|-------|-------------------|--------|
| Too abstract | Uses specific file paths and metrics | Concreteness matters for resumption |
| Incomplete chain | Links to previous session with context | Enables multi-session continuity |
| Unclear blockers | Lists external dependencies (OAuth credentials) | Next session knows what's needed |
| No learning | Failed Approaches section extracts lessons | Prevents repeating mistakes |
| Generic next steps | "Implement OAuth2" becomes prioritized items with time estimates | Actionability requires specifics |

---

## Using Examples for Your Project

### Scenario 1: First Handoff Creation

1. **Copy the structure** (but adapt content):
   ```bash
   # Reference but don't copy-paste - make it your own
   cat example-handoff.md | head -50  # Read structure only
   ```

2. **Fill sections in order:**
   - Session Summary (3-5 sentences about YOUR session)
   - Completed (YOUR work completed this session)
   - Pending (YOUR work remaining)
   - etc.

3. **Self-assess quality:**
   - Compare your Next Steps to example's (are yours specific?)
   - Compare your Failed Approaches to example's (do they have root cause analysis?)
   - Self-grade against the Quality Score breakdown

### Scenario 2: Improving Existing Handoff

Compare against example:

```markdown
❌ Your Pending: "Fix authentication bugs"
✅ Example's Pending: "OAuth2 Social Login Integration
    - Google and GitHub OAuth2 providers not yet implemented
    - Plan: Use Passport.js middleware pattern (estimated 2-3 hours)
    - Dependency: Requires OAuth app credentials from user"

→ Add specifics, blockers, and time estimates
```

### Scenario 3: Evaluating Handoff Quality

```bash
# Checklist using example as reference:
[ ] Session Summary is 3-5 sentences (not 1, not 10)
[ ] Completed items have sub-bullets explaining "how"
[ ] Pending items include blockers or dependencies
[ ] Next Steps include time estimates (not just task names)
[ ] Failed Approaches explain "why it failed" (not just "didn't work")
[ ] Quality Score is honest (85+ means genuinely excellent, not inflated)
[ ] How to Resume includes copy-paste commands
```

---

## Common Patterns in Example-Handoff.md

### Pattern 1: Security Decisions with Trade-offs

Example Decision 1 (HTTP-only cookies) shows:
```
Decision Name
    ↓
Rationale (why this was chosen)
    ↓
Impact (what changes as a result)
```

Use this pattern for ALL architectural decisions.

### Pattern 2: Issue Documentation with Workaround

Example Issue 1 (race condition) shows:
```
Issue Title
    ↓
Description (reproduction conditions)
    ↓
Workaround (code example if applicable)
    ↓
Resolution Timeline (when it will be fixed)
```

Use this pattern for Known Issues.

### Pattern 3: Priority-Based Next Steps

Example's Next Steps shows:
```
IMMEDIATE (30 mins)
  - Item 1
  - Item 2
  - **Why:** context

HIGH PRIORITY (2 hours next session)
  - Item 3
  - **Why:** business impact

MEDIUM PRIORITY (later)
  - Item 4
  - **Why:** post-MVP
```

Always include time estimates and rationale.

### Pattern 4: Failed Approach Lessons

Example's Approach 1 (JWT Claims) shows:
```
What We Tried
    ↓
Why It Failed (root cause, not just "didn't work")
    ↓
Resolution (what we did instead)
    ↓
Lesson Learned (applicable to future decisions)
```

This pattern makes the handoff educational.

---

## Directory Guidelines for Maintainers

### Adding New Examples

When creating additional examples (e.g., `example-bug-fix.md`):

1. **Keep quality score in range**: 70-90 (70 is minimum viable, 90+ is exceptional)
2. **Show different scenarios:**
   - Feature development (existing: JWT auth)
   - Bug fix / debugging session
   - Refactoring / tech debt work
   - Integration / documentation
3. **Include comments in Korean** (following project's CLAUDE.md language preference)
4. **Document what makes it a good example** (what patterns does it demonstrate?)
5. **Update this AGENTS.md** with the new example

### Directory Structure

```
examples/
├── AGENTS.md                    # This file
├── example-handoff.md           # Feature development (JWT auth)
├── example-bug-fix.md          # (Future) Debugging session
└── example-refactor.md         # (Future) Technical debt work
```

### Validation Before Adding Examples

```bash
# Before committing new examples:
1. Read it for structure
2. Check: All required sections present?
3. Check: No TODO placeholders?
4. Check: Quality Score >= 70?
5. Check: Next Steps are specific?
6. Update AGENTS.md with reference
```

---

## References

- **Parent Directory:** `../` (handoff project root)
- **Validation Script:** `../scripts/validate.sh` (validates handoff documents)
- **Skill Documentation:** `../SKILL.md` (how the handoff skill works)
- **Project README:** `../README.md` (usage instructions)

---

**Last Updated:** 2026-02-01
**Quality Score:** This AGENTS.md file (89/100)
**Maintenance:** Check examples quarterly for relevance
