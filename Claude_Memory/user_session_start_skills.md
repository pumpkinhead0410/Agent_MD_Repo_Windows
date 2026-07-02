---
name: 세션 시작 시 활성 스킬 안내
description: 세션 시작 시 활성 skill 목록 사용자에게 알릴 것
type: user
---

세션 시작 시 활성 Claude skill 목록 사용자에게 안내.

**Why:** 사용자가 `/skills` 없이 활성 스킬 파악 가능.

**How to apply:** 새 세션 시작 시 (context 요약 후 첫 응답 포함) system-reminder 의 skills 목록 확인, 표 형식으로 제시.