# Memory Index

> 작업 시작 시 `claude_info/CHECKPOINT.md` 먼저 확인.
> 소스 코드 작업 전 `graphify-out/GRAPH_REPORT.md` 먼저 읽기.

## 세션 상태
- [CHECKPOINT](claude_info/CHECKPOINT.md) — 현재 작업 상태, 브랜치, 다음 시작 위치
- [NOTES](claude_info/NOTES.md) — 세션별 주요 발견사항·비직관적 동작
- [TIPS](claude_info/TIPS.md) — 재사용 패턴 & 해결법
- [TODO](claude_info/TODO.md) — 우선순위별 다음 작업 목록

## 작업 로그 (claude_log/)
- (현재 없음 — 프로젝트별 로그는 각 프로젝트 폴더에 기록. 포맷은 [README](claude_log/README.md) 참고)

## Feedback (행동 규칙)
- [Commit 전 Linter](feedback_linter_before_commit.md) — commit 전 변경 파일 linter 실행
- [Agent 자동 승인](feedback_agent_permission.md) — Agent(*) 자동 실행
- [Bash/Python3 자동 승인](feedback_bash_python_auto_approve.md) — Bash/Python3 즉시 실행
- [Truncated output 판단 금지](feedback_truncated_output.md) — "Output too large" 시 전체 파일 읽고 판단
- [Caveman 한국어](feedback_caveman_korean.md) — caveman 모드 활성 시 한국어 파편문
- [Caveman MD 스타일](feedback_caveman_md_style.md) — 기획/스펙 문서 caveman style 작성
- [Handoff 로드](feedback_handoff_load.md) — 핸드오프 파일 경유 세션 인계
- [Worktree 확인](feedback_worktree_check.md) — 코드 작업 전 git worktree list + .worktrees/ 확인

## User Profile
- [코드 주석 스타일](user_code_comment_style.md) — 주요 로직 영문 주석, Doxygen (모든 코드베이스 공통)
- [세션 시작 스킬 안내](user_session_start_skills.md) — 세션 시작 시 활성 스킬 목록 제시
- [Usage 리셋 알람](user_usage_alarm.md) — "usage 꽉 찼다" → 5시간 후 알람 예약
- [Developer Profile](user_developer_profile.md) — 소통/의사결정/디버깅 성향 프로필

## Reference
- [graphify 도구](reference_graphify.md) — Knowledge Graph 빌드, 출력물 구조, 명령어 (범용)

## Skills
- `claude_skills/` — archive-session, graphify, grill-me, handoff
- `../caveman/` — caveman, cavecrew, caveman-commit/compress/help/review/stats, find-skills
- `../html-slide-deck/`, `../html-slide-deck.skill`

---

## 프로젝트별 메모리는 각 프로젝트 폴더에서 관리

이 폴더(`Agent_Memories`)는 프로젝트에 무관하게 재사용 가능한 범용 지식만 보관한다.
특정 프로젝트(코드베이스·언어·툴체인에 종속된) 지식은 해당 프로젝트 폴더 안에서 관리한다.
예: Universal_AVI 프로젝트 관련 지식은 `4_Universal_AVI/.claude/claude_info/`, `claude_log/`에 있음
(2026-07-01 통합 정리 이전 자료는 `4_Universal_AVI/.claude/_imported_from_agent_memory_unification/`에 아카이브됨).
