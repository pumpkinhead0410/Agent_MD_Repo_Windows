---
name: 코드 작업 전 Worktree 확인
description: 코드 작업 전 `git worktree list` + `.worktrees/` 확인 → 기존 worktree 재사용 or 신규 생성 여부 사용자 확인
type: feedback
---

코드 작업 전 반드시 worktree 상태 확인.

**Why:** 기존 worktree 모르고 main 브랜치 작업 or 브랜치 중복 생성 방지.

**How to apply:**
1. `git worktree list`로 등록된 worktree 목록 확인.
2. `.worktrees/` 하위 폴더명 확인 (`ls .worktrees/` 또는 `Glob`).
3. 관련 worktree 존재 → 해당 worktree 사용.
4. 관련 worktree 없음 → 사용자에게 신규 생성 여부 확인.
5. 생성 시 `.worktrees/<작업명>` 경로 + 브랜치명 사용자 협의.