---
name: 코드 주석 스타일 규칙
description: 구현 시 주요 Logic→영문 주석, 필요시 Doxygen
type: user
---

Impl: EN comment per major logic block. Doxygen for func/class/complex logic.

**Applies to:** 모든 코드베이스 (프로젝트 무관, 2026-04-08 global 규칙으로 격상됨)

**Why:** readability/doc quality 선호, all codebases.

**How to apply:**
- Every significant logic block → short English comment describing intent
- Function/class definitions → Doxygen `/** @brief ... @param ... @return ... */`
- Complex conditionals → inline comment: condition purpose