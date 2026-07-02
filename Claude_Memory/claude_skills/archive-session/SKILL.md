---
name: archive-session
description: 현재 세션의 작업 결과, 파악된 지식, 다음 단계(TODO)를 프로젝트의 공식 문서 체계(claude_log, claude_info)에 따라 자동으로 아카이브하고 요약합니다. 작업 종료 직전이나 주요 마일스톤 완료 시 사용합니다.
---

# Archive Session

이 스킬은 현재 세션에서 발생한 지식과 진행 상황을 구조화된 문서로 저장하여 다음 세션에서 문맥을 완벽하게 복원할 수 있도록 돕습니다.

## 📁 문서 체계 (Storage Locations)

모든 문서는 프로젝트 루트의 `.claude/` 하위에 저장됩니다.

- **작업 로그 (`.claude/claude_log/`)**: 세션의 상세 기술적 내역 보관.
  - 파일명: `LOG_YYYYMMDD_<Topic>.md`
- **세션 정보 (`.claude/claude_info/`)**: 지속적으로 업데이트되는 상태 정보.
  - `CHECKPOINT.md`: 현재 위치 및 다음 시작 지점.
  - `NOTES.md`: 파악된 주요 기술적 사실.
  - `TIPS.md`: 재사용 가능한 패턴.
  - `TODO.md`: 잔여 작업 목록.

## 🔄 워크플로우 (Workflow)

스킬 호출 시 다음 단계를 수행하세요:

### 1. 정보 수집 및 요약
- `git log` 및 `git diff`를 통해 이번 세션의 코드 변경 사항 확인.
- 파악된 새로운 아키텍처 규칙, 안티 패턴, 파일 구조 정리.

### 2. 상세 로그 작성 (`.claude/claude_log/`)
새로운 로그 파일을 생성합니다.
- **날짜**: YYYY-MM-DD
- **주제**: 핵심 작업 내용 (예: UI Mapping Enhancement)
- **내용**: 변경된 파일 목록, 구현된 주요 로직, 직면했던 문제와 해결 방법.

### 3. 상태 업데이트 (`.claude/claude_info/`)
기존 파일들을 **교체(Overwrite)** 하거나 **추가(Append)** 합니다.
- **CHECKPOINT**: 이전 내용을 지우고 현재의 최신 상태와 "Next Step"만 명확히 기술.
- **NOTES/TIPS**: 이번에 새로 배운 내용이 있다면 리스트에 추가.
- **TODO**: 완료된 항목은 제거하거나 체크(`[x]`)하고, 새로 발견된 과제 추가.

### 4. 프로젝트 루트 업데이트
- **CLAUDE.md**: 프로젝트 전반에 걸친 중요한 규칙(Rule) 변경이 있다면 반영.

### 5. Graphify 지식 그래프 갱신
세션 중 코드 변경이 있었다면 `graphify update .`를 실행하여 지식 그래프를 최신 상태로 유지.
```bash
if command -v graphify &> /dev/null; then
  graphify update .
fi
```
- graphify CLI가 없으면 skip.
- 완료 후 사용자에게 갱신 여부 보고.

## 💡 작성 가이드라인 (Writing Guidelines)

- **언어**: 모든 문서는 **한국어**로 작성하되, 기술 용어는 원문을 병기하거나 그대로 사용 (예: Multi-line support).
- **간결성**: Caveman 스타일을 유지하여 핵심 정보만 불릿 포인트로 기술.
- **검증**: 아카이브 완료 후 `ls` 명령으로 파일 생성을 확인하고 사용자에게 요약 보고.
