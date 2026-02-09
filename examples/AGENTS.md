<!-- Parent: ../AGENTS.md -->

# Examples Directory - AGENTS.md

## Purpose

This directory contains canonical example handoff documents demonstrating proper structure and content organization for multi-session project handoffs. These examples serve as reference implementations for both human developers and AI agents creating or working with handoff documents.

The examples directory teaches:
- **Structure**: How to organize project information for seamless session continuity
- **Clarity**: What constitutes an effective handoff document
- **Patterns**: Best practices for documenting decisions, failures, and next steps

---

## Directory Contents

### Files

| File | Description | Use Case |
|------|-------------|----------|
| `example-handoff.md` | Complete JWT authentication session handoff | Production-quality reference for complex feature work |

---

## Reference Document: example-handoff.md

### Overview

**Project:** JWT User Authentication System
**Duration:** 4.5 hours

The example document demonstrates a complete authentication feature implementation, including production-ready JWT token management with refresh rotation, role-based access control (RBAC), and security considerations.

### New v2.2.0 Handoff Format

The v2.2.0 smart handoff uses a streamlined 7-section format:

#### 1. Summary
3-5 sentence overview of session outcomes. Focuses on what was accomplished and the current state.

**Quality indicators:**
- Specific outcomes, not vague promises
- Clear scope boundary
- Realistic assessment of completion state

#### 2. Completed
Checkbox list with detailed descriptions of finished work. Each item explains what was done and how.

**Quality indicators:**
- Specific technical details, not just task names
- Sub-bullets explain implementation approach
- Actionable for next session to understand what's available

#### 3. Pending
Checkbox list of remaining work with status and blockers identified.

**Quality indicators:**
- Clear description of what's left
- Blockers or dependencies called out
- Realistic scope assessment

#### 4. Key Decisions
Record important trade-offs and their rationale. Each decision includes why it was chosen and its impact.

**Quality indicators:**
- Explains reasoning behind architectural choices
- Lists consequences and trade-offs
- Helps future sessions understand constraints

#### 5. Failed Approaches
Document what didn't work to prevent repeating mistakes. Includes what was tried, why it failed, and lessons learned.

**Quality indicators:**
- Concrete approach with clear attempt
- Root cause analysis
- Learning extracted for future reference
- No blame, just facts

#### 6. Files Modified
Track all code changes with context. Shows file paths, status, and description of changes.

**Quality indicators:**
- Absolute file paths
- Clear change descriptions
- Helps next session locate modified code

#### 7. Next Step (singular)
The single most important action for the next session. One clear, actionable item with context.

**Quality indicators:**
- Specific and actionable
- Explains why this is the priority
- No time estimates or multiple priorities
- Clear starting point

---

## Key Patterns from Example

### Pattern 1: Decision Documentation
```
Decision Name
  → Rationale (why this was chosen)
  → Impact (consequences and trade-offs)
```

Use this pattern for all architectural decisions.

### Pattern 2: Failed Approach Structure
```
What We Tried
  → Why It Failed (root cause analysis)
  → Resolution (what we did instead)
  → Lesson Learned (principle for future work)
```

This pattern makes handoffs educational and prevents repeated mistakes.

### Pattern 3: Specific vs. Vague

Bad: "Added authentication"
Good: "Implemented JWT token generation with HS256 signing and 15-minute expiration"

Bad: "Fix bugs"
Good: "OAuth2 Social Login Integration - Google and GitHub providers not yet implemented, requires OAuth app credentials"

---

## For AI Agents Working in This Directory

### Creating Handoff Documents

1. **Study Structure First**: Read example-handoff.md to understand the flow
2. **Match Specificity**: Use concrete details, not abstract descriptions
3. **Document Failures**: Include Failed Approaches to prevent repeat attempts
4. **Track Decisions**: Every architectural choice needs rationale and impact
5. **Single Next Step**: Focus on one clear priority, not a laundry list

### Quality Standards

| Aspect | Standard | How to Verify |
|--------|----------|---------------|
| **Completed Items** | Specific with sub-bullets explaining implementation | Can someone understand what's available without reading code? |
| **Pending Items** | Includes blockers and dependencies | Would next session know what's blocking progress? |
| **Failed Approaches** | Root cause analysis + lesson learned | Does it prevent repeating the mistake? |
| **Next Step** | Single, specific action with rationale | Can next session start immediately? |
| **Files Modified** | Shows status and change descriptions | Can next session find the modified files? |

---

## References

- **Parent Directory:** `../` (handoff project root)
- **Skill Documentation:** `../SKILL.md` (how the handoff skill works)
- **Project README:** `../README.md` (usage instructions)

---

**Last Updated:** 2026-02-10
