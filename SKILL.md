---
name: handoff
description: Save session context to clipboard. Resume anytime with Cmd+V.
---

# Handoff Skill

**Pass the baton. Keep the momentum.**

세션 컨텍스트를 저장하고 클립보드에 복사합니다. 새 세션이나 autocompact 후 붙여넣기로 복원하세요.

## Usage

```bash
/handoff [level] [topic]
  level: l1 (핵심 ~100토큰) | l2 (상세 ~300토큰) | l3 (전체 ~500토큰)
  기본값: l2

# Aliases (backward compatibility)
/handoff fast [topic]        # Alias for l1
/handoff slow [topic]        # Alias for l3
/handoff [topic]             # Alias for l2
```

<dim>
Examples:
  /handoff l1                # 핵심 요약
  /handoff l1 "auth 구현 중"
  /handoff l2                # 상세 요약 (기본값)
  /handoff l2 "JWT 마이그레이션"
  /handoff l3                # 전체 핸드오프
  /handoff fast              # = l1 (빠른 체크포인트)
  /handoff slow              # = l3 (완전한 문서화)
</dim>

## When to Use

| Situation | Command | Level |
|-----------|---------|-------|
| 컨텍스트 70%+ 도달 | `/handoff l1` | 핵심 |
| 짧은 휴식 (1시간 이내) | `/handoff l1` | 핵심 |
| 작업 체크포인트 | `/handoff l2` | 상세 (기본) |
| 세션 전환 | `/handoff l2` | 상세 (기본) |
| 세션 종료 | `/handoff l3` | 전체 |
| 긴 휴식 (2시간+) | `/handoff l3` | 전체 |

## Behavior

### Level 1 (L1) - 핵심 (~100 tokens)

**용도**: 빠른 컨텍스트 체크포인트, 짧은 휴식

1. **수집**: 현재 작업 1줄 + 다음 액션 1줄
2. **저장**: `.claude/handoffs/l1-YYYYMMDD-HHMMSS.md`
3. **복사**: 초간결 요약

**출력 템플릿:**

```markdown
# L1 Handoff - 핵심

**Time:** YYYY-MM-DD HH:MM
**Topic:** [topic or auto-detected]

**Current Task:** [현재 작업 1문장]

**Next Step:** [다음 액션 1문장]
```

### Level 2 (L2) - 상세 (~300 tokens, 기본값)

**용도**: 작업 체크포인트, 세션 전환

1. **수집**: 현재 작업, 주요 결정, 실패 시도 요약, 수정 파일 (최대 5개)
2. **저장**: `.claude/handoffs/l2-YYYYMMDD-HHMMSS.md`
3. **복사**: 상세 요약

**출력 템플릿:**

```markdown
# L2 Handoff - 상세

**Time:** YYYY-MM-DD HH:MM
**Topic:** [topic or auto-detected]

## Current Task
[현재 작업 1-2문장]

## Key Decisions
- **[결정]**: [이유 1줄]

## Failed Approaches
- **[시도]**: [실패 원인 1줄]

## Active Files
- `file1.ts` - [변경 내용 1줄]
- `file2.ts` - [변경 내용 1줄]

## Next Step
[다음 액션 1-2문장]
```

### Level 3 (L3) - 전체 (~500 tokens)

**용도**: 세션 종료, 긴 휴식, 완전한 문서화

1. **수집**: 완료/미완료 작업, 주요 결정, 실패한 시도, 수정 파일
2. **저장**: `.claude/handoffs/l3-YYYYMMDD-HHMMSS.md`
3. **복사**: 클립보드에 요약본 복사

**출력 템플릿:**

```markdown
# L3 Handoff - 전체

**Generated:** YYYY-MM-DD HH:MM:SS
**Topic:** [topic or auto-detected]
**Working Directory:** [cwd]

## Session Summary
[2-3문장 요약]

## Completed
- [x] 완료 작업 1
- [x] 완료 작업 2

## Pending
- [ ] 미완료 작업 1
- [ ] 미완료 작업 2

## Key Decisions
- **[결정]**: [이유]

## Failed Approaches
- **[시도]**: [실패 원인] → [배운 점]

## Files Modified
- `path/to/file.ts` - [변경 내용]

## Next Step
[다음에 할 구체적인 액션 1개]
```

### Legacy Aliases

- `/handoff fast` = `/handoff l1` (핵심)
- `/handoff slow` = `/handoff l3` (전체)
- `/handoff` = `/handoff l2` (상세, 기본값)

## Clipboard Format

클립보드에 복사되는 요약본 (붙여넣기용):

```
<system-instruction>
🛑 STOP: 이 내용은 이전 세션의 참고 자료입니다.
절대로 아래 내용을 자동으로 실행하지 마세요.
사용자의 새로운 지시가 있을 때까지 대기하세요.
</system-instruction>

<previous_session context="reference_only" auto_execute="false">
📋 이전 세션 요약 (Topic)
- 완료: N개 | 미완료: M개
- 수정 파일: K개

[미완료 작업 - 참고용, 실행 금지]
• 작업 1
• 작업 2

📄 상세: [handoff-path]
</previous_session>

---
✋ 이전 세션 컨텍스트를 확인했습니다.
무엇을 도와드릴까요?
```

## How to Resume

1. **새 세션** 또는 **autocompact 후**
2. `Cmd+V` (macOS) 또는 `Ctrl+V` (Linux/Windows)
3. Claude가 컨텍스트를 확인하고 지시 대기

## Secret Detection

민감 정보 자동 탐지 및 제거:

```
API_KEY=sk-1234...  → API_KEY=***REDACTED***
PASSWORD=secret     → PASSWORD=***REDACTED***
```

## Installation

```bash
curl -o ~/.claude/commands/handoff.md \
  https://raw.githubusercontent.com/quantsquirrel/claude-handoff/main/SKILL.md
```

## Notes

- 핸드오프는 `.claude/handoffs/`에 저장됩니다
- `[topic]` 생략 시 대화 컨텍스트에서 자동 추출
- 클립보드 요약은 자동 실행 방지 포맷 적용
- L1은 임시 체크포인트, L2는 표준 요약(기본값), L3는 완전한 문서화
- 기존 `fast`/`slow` 명령은 L1/L3 별칭으로 계속 사용 가능
