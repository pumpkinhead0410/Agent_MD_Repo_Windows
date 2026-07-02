---
name: reference_graphify
description: graphify 도구 전체 개요 — 설치·실행 방법, 출력물 위치, 쿼리 방법
metadata:
  type: reference
---

## 개요

`graphify` (pip 패키지명: `graphifyy`)는 폴더의 코드·문서·이미지·영상을 Knowledge Graph로 변환하는 도구.
트리거: `/graphify` 슬래시 명령 → Skill tool `skill: "graphify"` 호출 필수.

> **공유 리소스 가능성**: graphify 출력물을 다른 AI Agent(예: Google Antigravity)와 공유하는 경우, 출력물 수정·재빌드 시 다른 Agent의 참조에 영향을 줄 수 있으므로 주의.

- Skill 위치: `Claude_Memory/claude_skills/graphify/SKILL.md`

---

## 출력물 구조 (`graphify-out/`)

| 파일 | 설명 |
|------|------|
| `GRAPH_REPORT.md` | 코드베이스 질문 시 **먼저 읽어야 할 파일**. God Nodes·커뮤니티·Surprising Connections 포함 |
| `graph.json` | 원본 그래프 데이터 (GraphRAG-ready JSON) |
| `graph.html` | 브라우저에서 여는 인터랙티브 시각화 |
| `manifest.json` | 이전 실행 기록 (incremental update용) |
| `cache/` | 파일별 시멘틱 추출 캐시 |
| `.graphify_labels.json` | 커뮤니티 레이블 |
| `.graphify_python` | 사용 중인 Python 인터프리터 경로 |
| `.graphify_root` | 스캔 루트 경로 |

---

## 주요 명령

```bash
# 최초 빌드
/graphify <path>

# 변경 파일만 재추출 (코드만 변경 시 LLM 불필요)
/graphify <path> --update

# 그래프 질문 (BFS: 넓은 컨텍스트)
/graphify query "<질문>"

# 그래프 질문 (DFS: 특정 경로 추적)
/graphify query "<질문>" --dfs

# 두 개념 간 최단 경로
/graphify path "<A>" "<B>"

# 단일 노드 설명
/graphify explain "<노드명>"

# 커뮤니티 재클러스터링만 (추출 생략)
/graphify <path> --cluster-only
```

---

## 관리 정책

- graphify 출력물(`graphify-out/`)은 대상 프로젝트 루트가 아니라 **이 메모리 폴더(`Agent_Memories`) 하위**에서 관리하는 것을 기본 원칙으로 한다 (프로젝트 저장소를 캐시로 오염시키지 않기 위함).
- 프로젝트별로 스캔 제외 규칙(불필요한 대용량/생성물 폴더)이 필요할 수 있음 — 대상 프로젝트의 `.gitignore` 패턴을 기본으로 따르고, 프로젝트 전용 제외 규칙은 해당 프로젝트의 자체 메모리/문서에 기록한다.

---

## 사용 규칙 (CLAUDE.md에서)

1. **소스 파일 읽기·grep/glob 검색·코드베이스 질문 전 항상 `graphify-out/GRAPH_REPORT.md` 먼저 읽기**
2. `graphify-out/wiki/index.md` 존재 시 raw 파일 대신 탐색
3. 교차 모듈 관계 질문: grep 대신 `graphify query/path/explain` 우선
4. graphify 출력물은 대상 프로젝트 루트가 아닌 이 메모리 폴더 하위에서 관리

---

## 설치 확인

```bash
# uv 기반 (권장)
uv tool install --upgrade graphifyy

# pip 기반
pip install graphifyy --break-system-packages
```

GEMINI_API_KEY 또는 GOOGLE_API_KEY 미설정 시 → Claude Code 서브에이전트로 시멘틱 추출 수행.
설정 시 → Gemini(`gemini-3-flash-preview`) 사용으로 토큰 절감.
