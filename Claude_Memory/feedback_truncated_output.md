---
name: feedback-truncated-output
description: "system-reminder에 \"Output too large / Preview\" 표시 시 전체 파일 읽기 필수"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 442b5c1a-bf31-4f49-be3a-abe955ac1516
---

system-reminder에 "Output too large... Full output saved to: <path>" 메시지가 있으면, 미리보기(Preview)만 보고 내용을 판단하지 말 것.

**Why:** 2KB 미리보기만 보고 "CHECKPOINT.md / TODO.md만 주입됐다"고 잘못 단정한 사례 발생. 실제로는 NOTES.md, TIPS.md, 로그 파일 2개까지 모두 주입됐음.

**How to apply:** "Output too large" 문구 확인 시 → 저장된 전체 파일 경로를 Read 또는 `cat`으로 읽은 후 판단.
