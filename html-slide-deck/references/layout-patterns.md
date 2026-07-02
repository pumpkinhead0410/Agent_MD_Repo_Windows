# 콘텐츠 레이아웃 패턴

`sc-body` 안에서 사용하는 레이아웃 패턴 모음입니다.

---

## 1. 단일 코드 블록

```html
<div class="file-row"><span class="file-pill">src/MyFile.cpp</span></div>
<p class="desc" style="margin-bottom:12px;">{{설명 텍스트}}</p>
<div class="code-wrap">
  <div class="code-hdr">
    <span class="code-fn">MyFile.cpp — functionName()</span>
    <span class="code-lang">C++</span>
  </div>
  <pre><code class="language-cpp">// 코드 여기에
void MyFunction() {
    // ...
}</code></pre>
</div>
```

**지원 언어**: `language-cpp`, `language-javascript`, `language-python`, `language-plaintext` 등 Highlight.js 지원 언어 모두 사용 가능.

---

## 2. 화면을 채우는 코드 블록 (sc-body-flex 필요)

```html
<!-- sc-body에 sc-body-flex 클래스 추가 필요 -->
<div class="file-row"><span class="file-pill">src/MyFile.cpp</span></div>
<div style="display:flex; flex-direction:column; flex:1; min-height:0; gap:10px; margin-top:8px;">
  <div class="code-wrap" style="flex:1; display:flex; flex-direction:column; min-height:0;">
    <div class="code-hdr">
      <span class="code-fn">MyFile.cpp</span>
      <span class="code-lang">C++</span>
    </div>
    <pre style="flex:1; overflow-y:auto; margin:0;">
      <code class="language-cpp">// 긴 코드</code>
    </pre>
  </div>
</div>
```

---

## 3. 2×2 스텝 그리드 (단계별 절차)

4단계 절차를 카드로 표현할 때 사용합니다. `sc-body-flex` 필요.

```html
<!-- sc-body에 sc-body-flex 클래스 추가 필요 -->
<p class="desc" style="margin-bottom:6px;">{{전체 설명}}</p>
<div style="display:grid; grid-template-columns:1fr 1fr; grid-template-rows:1fr 1fr; gap:12px; flex:1; min-height:0; margin-top:10px;">

  <!-- 카드 ① -->
  <div style="background:#F5F3FF; border-radius:10px; padding:14px 16px 12px; border-left:3px solid #7C3AED; display:flex; flex-direction:column; gap:8px; overflow:hidden;">
    <div style="display:flex; align-items:center; gap:8px; font-size:13px; font-weight:800; color:#3730A3; flex-shrink:0;">
      <span style="background:#7C3AED; color:#fff; width:22px; height:22px; border-radius:50%; display:flex; align-items:center; justify-content:center; font-size:11px;">①</span>
      {{단계 제목}}
    </div>
    <pre style="background:rgba(67,56,202,.1); border-radius:6px; padding:8px 12px; flex:1; overflow:auto; font-family:'Consolas','Courier New',monospace; font-size:10.5px; color:#312E81; line-height:1.65; white-space:pre; margin:0;">{{코드 또는 내용}}</pre>
  </div>

  <!-- 카드 ② ③ ④ — 동일 구조 반복 -->

</div>
```

**카드 수 조정**: 3개는 `grid-template-columns: 1fr 1fr 1fr; grid-template-rows: 1fr;`, 6개는 `1fr 1fr 1fr / 1fr 1fr`.

---

## 4. 2컬럼 레이아웃 (테이블 + 코드)

```html
<!-- sc-body에 sc-body-flex 클래스 추가 필요 -->
<div class="file-row"><span class="file-pill">config.ini</span></div>
<div style="display:flex; gap:18px; flex:1; min-height:0; margin-top:8px;">

  <!-- 좌: 필드 설명 테이블 -->
  <div style="flex:0 0 44%; display:flex; flex-direction:column; min-width:0;">
    <div style="font-size:10px; font-weight:700; text-transform:uppercase; letter-spacing:.1em; color:#EA580C; margin-bottom:7px;">📋 필드 구조</div>
    <table class="ini-table compact">
      <thead><tr><th>필드</th><th>설명</th><th>예시</th></tr></thead>
      <tbody>
        <tr><td>Field1</td><td>설명</td><td>값</td></tr>
      </tbody>
    </table>
  </div>

  <!-- 우: 코드 + callout -->
  <div style="flex:1; display:flex; flex-direction:column; gap:10px; overflow-y:auto; min-width:0;">
    <div style="font-size:10px; font-weight:700; text-transform:uppercase; letter-spacing:.1em; color:#EA580C; margin-bottom:7px;">✏️ 등록 방법</div>
    <div class="code-wrap" style="flex:1; min-height:0;">
      <div class="code-hdr"><span class="code-fn">config.ini</span><span class="code-lang">INI</span></div>
      <pre style="flex:1; overflow-y:auto;"><code class="language-plaintext">; 코드</code></pre>
    </div>
    <div class="callout note" style="flex-shrink:0;">
      <span class="ci">💡</span>
      <div><strong>팁 제목</strong>팁 내용</div>
    </div>
  </div>

</div>
```

---

## 5. Callout 변형

```html
<!-- 정보 (파랑) -->
<div class="callout note">
  <span class="ci">💡</span>
  <div><strong>제목</strong>내용</div>
</div>

<!-- 주의 (노랑) -->
<div class="callout warn">
  <span class="ci">⚠️</span>
  <div><strong>제목</strong>내용</div>
</div>

<!-- 성공/팁 (초록) -->
<div class="callout tip">
  <span class="ci">✅</span>
  <div><strong>제목</strong>내용</div>
</div>

<!-- 위험 (빨강) -->
<div class="callout danger">
  <span class="ci">🚨</span>
  <div><strong>제목</strong>내용</div>
</div>
```

---

## 6. 체크리스트

검증 단계나 확인 사항 나열에 사용합니다.

```html
<p class="desc" style="margin-bottom:16px;">{{서두}}</p>
<div class="checklist">
  <div class="check-item">
    <div class="check-circle">①</div>
    <div><strong>항목 제목</strong>상세 설명</div>
  </div>
  <div class="check-item">
    <div class="check-circle">②</div>
    <div><strong>항목 제목</strong>상세 설명</div>
  </div>
</div>
```

---

## 7. Mermaid 다이어그램

```html
<div class="mermaid">
flowchart LR
  A[시작] --> B{조건}
  B -->|Yes| C[처리]
  B -->|No| D[종료]
  C --> D
</div>
```

**주의**: Mermaid는 `mermaid.initialize()`가 실행된 이후 DOM에 있어야 렌더링됩니다.
슬라이드가 처음부터 hidden 상태라면 첫 번째 슬라이드가 아닌 경우 렌더링이 지연될 수 있습니다.
해결: `mermaid.initialize({ startOnLoad: true })`가 보일러플레이트에 포함되어 있으므로 보통 정상 동작합니다.

---

## 8. 플로우 & 파일 목록 (2열 Overview)

전체 흐름과 관련 파일을 한 슬라이드에 나란히 보여줄 때 사용합니다.

```html
<!-- sc-body에 sc-body-flex 클래스 추가 필요 -->
<div style="display:flex; gap:28px; flex:1; min-height:0; margin-top:4px;">

  <!-- 좌: 플로우 다이어그램 -->
  <div style="flex:1; display:flex; flex-direction:column; min-width:0;">
    <div style="font-size:10px; font-weight:700; text-transform:uppercase; letter-spacing:.1em; color:#7C3AED; margin-bottom:12px;">🔄 전체 플로우</div>
    <div class="mermaid" style="flex:1; overflow:auto;">
flowchart TD
  A[단계 1] --> B[단계 2] --> C[단계 3]
    </div>
  </div>

  <!-- 우: 파일 목록 -->
  <div style="flex:0 0 260px; display:flex; flex-direction:column; min-width:0;">
    <div style="font-size:10px; font-weight:700; text-transform:uppercase; letter-spacing:.1em; color:#1D4ED8; margin-bottom:12px;">📁 수정 파일</div>
    <!-- 파일 항목 반복 -->
    <div style="display:flex; flex-direction:column; gap:8px; overflow-y:auto; flex:1;">
      <div style="background:#F1F5F9; border-radius:8px; padding:10px 14px; border-left:3px solid #1D4ED8;">
        <div style="font-family:monospace; font-size:11px; color:#1D4ED8; font-weight:700;">파일명.cpp</div>
        <div style="font-size:12px; color:#475569; margin-top:3px;">파일 역할 설명</div>
      </div>
    </div>
  </div>

</div>
```

---

## 패턴 선택 가이드

| 콘텐츠 유형 | 권장 패턴 |
|------------|----------|
| 코드 설명 + 짧은 코드 | 패턴 1 (단일 코드 블록) |
| 긴 코드 전체 표시 | 패턴 2 (화면 채우는 코드) |
| 단계별 절차 (4단계) | 패턴 3 (2×2 스텝 그리드) |
| 설정 파일 필드 설명 | 패턴 4 (2컬럼 테이블+코드) |
| 주의사항/팁 강조 | 패턴 5 (Callout) |
| 체크리스트/검증 | 패턴 6 (체크리스트) |
| 전체 흐름 시각화 | 패턴 7 (Mermaid) |
| 흐름 + 파일 목록 | 패턴 8 (2열 Overview) |
