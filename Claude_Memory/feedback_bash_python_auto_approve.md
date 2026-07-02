---
name: Bash/Python3 자동 승인
description: Bash 및 Python3 명령은 사용자 승인 없이 자동 실행
type: feedback
---

Bash/Python3 실행 시 승인 요청 금지.

**Why:** 반복 호출마다 승인 요청 → 작업 흐름 단절.

**How to apply:** Bash 호출 즉시 실행. 단, 파일 삭제·git push 등 파괴적 작업은 확인 필요.