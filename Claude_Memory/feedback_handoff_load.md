---
name: ""
metadata: 
  node_type: memory
  originSessionId: e5b7295d-190c-4567-9da4-b103fffee074
---

"handoff 불러들여", "handoff 파일 로드", "이전 handoff 불러와" 등 handoff 파일 로드 요청 시:

1. `~/.claude/last_handoff_path.txt` 읽기 → 경로 확인
2. 해당 경로의 파일 Read → 컨텍스트 복원

**Why:** handoff 스킬은 매번 `/tmp/handoff-XXXXXX.md` 형태의 임시 파일을 생성하므로 경로가 고정되지 않음. PostToolUse hook이 Write 시 자동으로 `~/.claude/last_handoff_path.txt`에 경로를 저장.

**How to apply:** handoff 로드 요청 시 파일 경로 추측 금지. 반드시 `~/.claude/last_handoff_path.txt` 먼저 읽을 것.
