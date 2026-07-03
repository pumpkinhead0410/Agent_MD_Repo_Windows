---
name: html-slide-deck
description: |
  PPT 스타일의 HTML 슬라이드 덱을 생성하거나 수정합니다. 기술 가이드, 업무 보고서, 교육 자료 등 다양한 용도의 프레젠테이션을 완전히 자체 포함된 단일 HTML 파일로 만듭니다.
  
  다음 상황에서 이 스킬을 사용하세요: "슬라이드 만들어줘", "PPT 형식으로 정리해줘", "가이드 문서를 슬라이드로", "발표 자료 만들어줘", "단계별 절차를 슬라이드로", 기존 HTML 슬라이드에 페이지 추가/수정/삭제, 레이아웃 변경 요청 등. HTML이나 슬라이드를 명시하지 않아도 프레젠테이션 형태가 적합하다고 판단되면 이 스킬을 사용하세요.
---

# HTML 슬라이드 덱 생성/수정 스킬

## 개요

완전 자체 포함(self-contained) 단일 HTML 파일로 PPT 스타일 슬라이드 덱을 만드는 스킬입니다.
외부 서버 없이 브라우저에서 바로 열리며, 키보드/클릭 네비게이션, 사이드바 목차, 진행 표시줄을 포함합니다.

## Step 1: 요구사항 파악

작업 시작 전 다음을 확인하세요:

- **목적**: 기술 가이드? 업무 보고? 교육 자료?
- **내용 구조**: 슬라이드 수, 계층(Step/Task 구분 여부), 코드 포함 여부
- **신규 생성 vs 기존 수정**: 파일이 이미 있는지 확인
- **출력 경로**: 저장할 폴더

## Step 2: 템플릿 선택

### 🔷 기술 가이드 (Technical Guide) — 이번 세션 검증됨
절차적 기술 문서에 최적. 단계(Step) + 태스크(Task) 계층, 코드 블록, 다이어그램 지원.
- 다크 제목 슬라이드 + 라이트 콘텐츠 슬라이드
- Step 헤더 슬라이드 (강조색 배경) + Task 콘텐츠 슬라이드
- Highlight.js (C++/Python/JS 등 코드 강조) + Mermaid.js (플로우차트)
→ `references/engine.md`의 **Technical Guide 보일러플레이트** 사용

### 🔶 업무 보고서 (Business Report) — 기본 엔진 사용
데이터, 표, 불릿포인트 중심. 밝은 배경, 헤더/푸터 포함.
→ `references/engine.md`의 **기본 엔진**을 기반으로 커스텀 CSS 적용

### 🟢 교육 자료 (Training Material) — 기본 엔진 사용
퀴즈, 예시, 체크리스트 위주. 색상 풍부, 시각적 강조.
→ `references/engine.md`의 **기본 엔진**을 기반으로 커스텀 CSS 적용

## Step 3-A: 신규 생성 워크플로우

1. **콘텐츠 아웃라인 작성**: 슬라이드 목록과 각 슬라이드의 내용을 먼저 정리
2. **SLIDE_META 설계**: 사이드바에 표시될 슬라이드 메타데이터 배열 설계
   ```javascript
   const SLIDE_META = [
     { idx: 0, label: '제목 · 목차',    dot: '🏠', color: '#475569', step: false },
     { idx: 1, label: 'Step 1 · 개요', dot: '1',  color: '#1D4ED8', step: true  },
     { idx: 2, label: 'Task 1-1 · ...',dot: '1',  color: '#1D4ED8', step: false },
     // idx는 DOM 순서(0부터 시작)와 정확히 일치해야 함
   ];
   ```
3. **보일러플레이트 삽입**: `references/engine.md`에서 전체 HTML 구조 복사
4. **슬라이드 추가**: `references/slide-types.md`의 타입별 템플릿 참조
5. **레이아웃 적용**: `references/layout-patterns.md`의 패턴 참조
6. **제목 슬라이드 뱃지 업데이트**: "N 슬라이드" 카운트를 실제 슬라이드 수로

## Step 3-B: 기존 수정 워크플로우

수정 전 반드시:
1. **전체 SLIDE_META 파악**: 현재 슬라이드 목록과 idx 값 확인
2. **DOM 슬라이드 총 수 확인**: `class="slide ` 패턴 grep으로 총 개수 파악
3. **data-idx는 장식용**: JS 엔진은 DOM 순서(querySelectorAll 결과)를 사용함.
   SLIDE_META의 `idx` 값이 DOM 순서와 일치해야 사이드바 네비게이션이 동작함.

**슬라이드 추가 시**:
- 원하는 위치에 HTML 삽입
- 해당 위치 이후의 모든 SLIDE_META idx 값을 +1
- "N 슬라이드" 뱃지 +1

**슬라이드 삭제/병합 시**:
- HTML에서 슬라이드 div 제거
- SLIDE_META에서 해당 항목 제거, 이후 idx -1
- "N 슬라이드" 뱃지 -1

**레이아웃 수정 시**:
- 변경 전 `grep`으로 현재 CSS 값 정확히 확인 후 수정
- 추측으로 old_string 작성하지 말 것

## Step 4: 품질 체크

완성 후 확인:
- [ ] SLIDE_META idx가 DOM 순서와 정확히 일치하는가
- [ ] The End 슬라이드가 SLIDE_META에 포함되지 않는가 (네비게이션 전용)
- [ ] 제목 슬라이드의 "N 슬라이드" 뱃지가 실제 수와 맞는가
- [ ] 모든 코드 블록이 `hljs.highlightAll()` 범위 내에 있는가
- [ ] Mermaid 다이어그램에 `class="mermaid"`가 있는가
- [ ] 모바일/좁은 화면에서 overflow가 적절히 처리되는가

## 레퍼런스 파일 안내

| 파일 | 내용 | 언제 읽기 |
|------|------|-----------|
| `references/engine.md` | HTML 보일러플레이트 (head, CSS, JS 엔진 전체) | 신규 생성 시, 엔진 동작 이해 필요 시 |
| `references/slide-types.md` | 슬라이드 타입별 HTML 템플릿 | 새 슬라이드 추가 시 |
| `references/layout-patterns.md` | 콘텐츠 레이아웃 패턴 (코드, 그리드, 테이블 등) | 슬라이드 내부 레이아웃 구성 시 |
