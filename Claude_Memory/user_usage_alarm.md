---
name: user-usage-alarm
description: Claude Code usage 리셋 알람 패턴 — 사용자가 usage 꽉 찼다고 하면 1시간 후 PushNotification 예약
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 246c0ef0-1a76-42ef-bcc3-483d58b9fc21
---

사용자 "usage 꽉 찼다"/"usage full" 전달 시:
1. 즉시 `ScheduleWakeup(delaySeconds=18000)` 호출 — 5시간 후 예약
2. 5시간 후 `PushNotification` 발송 — "Claude Code usage가 리셋됐을 것 같습니다"

**Why:** Claude Code usage % 조회 API 없음 — 직접 감지 불가. 사용자 수동 알림 기준 5시간 타이머.

**How to apply:** 사용자 usage 한도 도달 메시지 수신 즉시 — 확인 없이 ScheduleWakeup 실행.