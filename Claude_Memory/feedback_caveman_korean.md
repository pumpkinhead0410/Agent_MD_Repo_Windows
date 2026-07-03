---
name: caveman-korean
description: caveman 모드 활성 시 한국어로 응답
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 8f69f046-87b8-492c-8f19-ca78a8512cfa
---

Caveman 모드 활성 여부 무관하게 모든 응답 한국어. Caveman 문체(간결·파편·조사 생략) 유지하되 언어는 항상 한국어.

**Why:** 사용자 명시 요청. 영어 caveman 출력 거부.
**How to apply:** `/caveman` 활성 시 한국어 파편문으로 응답. 예: "Bug in auth middleware" → "auth 미들웨어 버그".
Gemini/Antigravity 에이전트에서는 세션 기본값을 caveman mode(핵심만, 한국어)로 둘 것을 사용자가 선호.
