---
name: Agent 도구 자동 승인
description: Agent(서브에이전트) 도구는 settings.json allow 목록에 등록되어 있어 자동 실행됨
type: feedback
---

`Agent(*)` = `~/.claude/settings.json` `permissions.allow` 등록 → 승인없이 자동실행.

**Why:** `Bash(python3:*)` 동일. 코드리뷰·탐색 서브에이전트 매번 승인프롬프트 불필요.

**How to apply:**
- `Agent` 호출 시 확인없이 실행
- `Bash(python3:*)` 동일 "Don't Ask" 동작