---
name: Commit 전 Linter 수행
description: git commit 전 변경 파일에 linter 실행, 통과 후 commit
type: feedback
---

`git commit` 전 변경 파일에 linter 실행.

**Why:** 린팅 오류 커밋 이력 방지.

**How to apply:**
- `git diff --name-only HEAD` 또는 `git status`로 변경 파일 확인.
- `.clang-format` 있으면 `clang-format --dry-run`으로 C++ 검사.
- `.clang-tidy` 있으면 `clang-tidy` 정적 분석.
- 기타 linter(eslint, pylint 등) 있으면 실행.
- linter 오류 → 수정 후 commit.
- linter 설정 없어도 사용자에게 수행 여부 확인.