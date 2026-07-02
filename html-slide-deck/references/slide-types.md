# 슬라이드 타입별 HTML 템플릿

각 슬라이드 타입의 HTML 구조입니다. `<!-- SLIDES HERE -->` 위치에 삽입하세요.

---

## 1. 제목 + 목차 슬라이드 (slide-title)

프레젠테이션의 첫 슬라이드. 좌측 = 제목 정보, 우측 = 목차 그리드.

```html
<div class="slide slide-title active" data-idx="0">
  <!-- 좌측: 제목 영역 -->
  <div class="tc-left">
    <div class="t-eyebrow">{{CATEGORY}} · {{SUBTITLE}}</div>
    <div class="t-title">{{MAIN TITLE}}</div>
    <div class="t-subtitle">{{DESCRIPTION}}</div>
    <div class="t-meta">
      <div class="t-meta-item">🖥 <strong>N 슬라이드</strong></div>
      <div class="t-meta-item">⏱ <strong>약 X분</strong></div>
      <div class="t-meta-item">📁 <strong>X개 파일</strong></div>
    </div>
  </div>
  <!-- 우측: 목차 그리드 -->
  <div class="tc-right">
    <div class="toc-label">📋 목차</div>
    <div class="toc-grid">
      <div class="toc-item" onclick="goToSlide(N)">
        <span class="toc-num">01</span>
        <span class="toc-text">{{TOC ITEM 1}}</span>
      </div>
      <div class="toc-item" onclick="goToSlide(N)">
        <span class="toc-num">02</span>
        <span class="toc-text">{{TOC ITEM 2}}</span>
      </div>
      <!-- 항목 추가 -->
    </div>
  </div>
</div>
```

**팁**: 목차 항목이 많을수록 각 행이 작아집니다. 5개 이하일 때 가장 보기 좋습니다. `goToSlide(N)`의 N은 해당 섹션의 첫 번째 슬라이드 DOM 인덱스입니다.

---

## 2. Step 헤더 슬라이드 (slide-step)

새로운 단계(Step)를 시작할 때 사용하는 전환 슬라이드. 강조색 그라디언트 배경.

```html
<div class="slide slide-step bg-s1" data-idx="N">
  <!-- bg-s1~bg-s5 중 선택: s1=파랑, s2=보라, s3=핑크, s4=초록, s5=주황 -->
  <div class="sh-big-num">1</div>  <!-- 단계 번호 (장식용) -->
  <span class="sh-eyebrow">Step 1 · {{CATEGORY}}</span>
  <div class="sh-title">{{STEP TITLE}}</div>
  <div class="sh-desc">{{STEP 설명 — 이 단계에서 무엇을 하는지 1~2문장}}</div>
  <div class="sh-pills">
    <span class="sh-pill">Task 1-1 · {{태스크명}}</span>
    <span class="sh-pill">Task 1-2 · {{태스크명}}</span>
  </div>
</div>
```

**배경색 선택 가이드**: 단계 순서에 따라 s1→s2→s3→s4→s5 순으로 사용하면 자연스러운 색상 흐름이 생깁니다.

---

## 3. 콘텐츠 슬라이드 (slide-content)

실제 내용이 담기는 슬라이드. 헤더 + 본문 영역으로 구성됩니다.

### 기본 구조

```html
<div class="slide slide-content" data-idx="N">
  <div class="sc-header">
    <span class="task-badge b1">Task 1-1</span>  <!-- b1~b5 중 해당 Step 색상과 일치시킬 것 -->
    <h2>{{슬라이드 제목}}</h2>
  </div>
  <div class="sc-body">
    <!-- 여기에 레이아웃 패턴 삽입 (layout-patterns.md 참조) -->
  </div>
</div>
```

### 콘텐츠가 많은 경우 (flex 확장)

```html
<div class="slide slide-content" data-idx="N">
  <div class="sc-header">
    <span class="task-badge b2">Task 2-1</span>
    <h2>{{슬라이드 제목}}</h2>
  </div>
  <div class="sc-body sc-body-flex" style="padding-top:16px;">
    <!-- flex: 1로 높이를 최대한 활용하는 레이아웃 -->
  </div>
</div>
```

**`sc-body-flex` 사용 시기**: 코드 블록, 다단 레이아웃, 테이블 등이 화면 높이를 최대한 채워야 할 때.

---

## 4. 엔딩 슬라이드 (slide-end)

프레젠테이션 마지막 슬라이드. SLIDE_META에 **포함하지 않습니다**.

```html
<div class="slide slide-end" data-idx="N">
  <div class="end-deco1"></div>
  <div class="end-deco2"></div>
  <div class="end-label">End of Presentation</div>
  <div class="end-title">The End</div>
  <div class="end-divider"></div>
  <div class="end-subtitle">{{프레젠테이션 제목}}</div>
  <div class="end-meta">{{부제 — 예: "X단계 · Y개 Task · Z개 파일 수정"}}</div>
</div>
```

---

## 태스크 뱃지 색상 매핑

Step 헤더의 배경색과 Task 슬라이드의 뱃지 색상을 일치시키세요:

| Step 배경 | Task 뱃지 클래스 | 색상 |
|-----------|-----------------|------|
| `bg-s1`   | `b1`            | 파랑 (#1D4ED8) |
| `bg-s2`   | `b2`            | 보라 (#7C3AED) |
| `bg-s3`   | `b3`            | 핑크 (#DB2777) |
| `bg-s4`   | `b4`            | 초록 (#059669) |
| `bg-s5`   | `b5`            | 주황 (#EA580C) |

SLIDE_META의 `color`도 동일한 색상 코드를 사용하면 사이드바의 색상이 통일됩니다.
