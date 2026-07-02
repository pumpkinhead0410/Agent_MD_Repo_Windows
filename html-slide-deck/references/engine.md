# HTML 슬라이드 엔진 레퍼런스

이 파일에는 슬라이드 덱의 뼈대(boilerplate) 전체가 담겨 있습니다.
신규 파일을 만들 때 아래 보일러플레이트를 복사한 뒤 `<!-- SLIDES HERE -->` 영역에 슬라이드를 채워 넣으세요.

---

## Technical Guide 보일러플레이트

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>{{TITLE}}</title>

  <!-- 코드 하이라이팅 (Highlight.js) -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/atom-one-dark.min.css" />
  <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/languages/cpp.min.js"></script>
  <!-- Mermaid 다이어그램 (필요 없으면 제거 가능) -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mermaid/10.6.1/mermaid.min.js"></script>

  <style>
    /* ══ Reset ══════════════════════════════════════════════════ */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html, body {
      width: 100%; height: 100%; overflow: hidden;
      font-family: 'Segoe UI', 'Apple SD Gothic Neo', sans-serif;
      background: #0F172A;
    }

    /* ══ Sidebar ════════════════════════════════════════════════ */
    #sidebar {
      position: fixed; top: 0; left: 0; bottom: 0; width: 300px;
      background: rgba(10,17,32,0.97); backdrop-filter: blur(16px);
      border-right: 1px solid rgba(255,255,255,.08);
      z-index: 600; transform: translateX(-300px);
      transition: transform .28s cubic-bezier(.4,0,.2,1);
      display: flex; flex-direction: column; overflow: hidden;
      box-shadow: 4px 0 32px rgba(0,0,0,.5);
    }
    #sidebar.open { transform: translateX(0); }

    .sb-head { padding: 18px 16px 14px; border-bottom: 1px solid rgba(255,255,255,.06); flex-shrink: 0; }
    .sb-badge {
      display: inline-block; background: #1D4ED8; color: #fff;
      font-size: 9px; font-weight: 800; letter-spacing: .1em;
      text-transform: uppercase; padding: 3px 8px; border-radius: 4px; margin-bottom: 8px;
    }
    .sb-title { color: #F8FAFC; font-size: 13px; font-weight: 700; line-height: 1.4; }
    .sb-list { flex: 1; overflow-y: auto; padding: 10px 8px; }
    .sb-list::-webkit-scrollbar { width: 4px; }
    .sb-list::-webkit-scrollbar-thumb { background: rgba(255,255,255,.15); border-radius: 2px; }

    .sb-item {
      display: flex; align-items: center; gap: 10px;
      padding: 8px 10px; border-radius: 8px; cursor: pointer;
      transition: background .15s; margin-bottom: 2px;
    }
    .sb-item:hover { background: rgba(255,255,255,.07); }
    .sb-item.active { background: rgba(255,255,255,.12); }
    .sb-item.is-step { margin-top: 6px; }
    .sb-dot {
      width: 26px; height: 26px; border-radius: 6px;
      display: flex; align-items: center; justify-content: center;
      font-size: 11px; font-weight: 800; flex-shrink: 0;
    }
    .sb-label { color: #CBD5E1; font-size: 12px; font-weight: 500; line-height: 1.3; }
    .sb-item.active .sb-label { color: #F1F5F9; font-weight: 700; }

    /* 사이드바 최하단 도움말 버튼 */
    .sb-help-btn {
      margin: 10px 12px 14px; padding: 8px 12px;
      background: rgba(255,255,255,.06); border: 1px solid rgba(255,255,255,.1);
      border-radius: 8px; color: #94A3B8; font-size: 11px;
      cursor: pointer; text-align: center; flex-shrink: 0;
      transition: background .15s;
    }
    .sb-help-btn:hover { background: rgba(255,255,255,.1); }

    /* ══ Deck ═══════════════════════════════════════════════════ */
    #deck {
      position: fixed; top: 0; left: 0; right: 0; bottom: 48px;
      transition: left .28s cubic-bezier(.4,0,.2,1),
                  right .28s cubic-bezier(.4,0,.2,1);
      overflow: hidden;
    }
    body.sb-open #deck { left: 300px; }

    /* ══ Slides ═════════════════════════════════════════════════ */
    .slide {
      position: absolute; inset: 0;
      transform: translateX(100%);
      transition: transform .32s cubic-bezier(.4,0,.2,1);
      overflow: hidden;
    }
    .slide.active  { transform: translateX(0); }
    .slide.exit-left  { transform: translateX(-100%); }
    .slide.exit-right { transform: translateX(100%); }

    /* ── 제목/목차 슬라이드 ─── */
    .slide-title {
      background: linear-gradient(135deg, #0F172A 0%, #1E1B4B 50%, #0F172A 100%);
      display: flex; flex-direction: row;
      justify-content: flex-start; align-items: stretch;
      padding: 0; overflow: hidden;
    }
    .tc-left {
      flex: 0 0 50%; display: flex; flex-direction: column; justify-content: center;
      padding: 52px 44px 52px 72px;
      border-right: 1px solid rgba(255,255,255,.1);
      position: relative; z-index: 1;
    }
    .tc-right {
      flex: 0 0 50%; display: flex; flex-direction: column;
      padding: 32px 52px 24px 44px;
    }
    .t-eyebrow { font-size: 11px; font-weight: 700; letter-spacing: .15em; text-transform: uppercase; color: #818CF8; margin-bottom: 16px; }
    .t-title { font-size: 36px; font-weight: 800; color: #F8FAFC; line-height: 1.2; margin-bottom: 16px; }
    .t-subtitle { font-size: 15px; color: #94A3B8; line-height: 1.6; margin-bottom: 28px; }
    .t-meta { display: flex; gap: 16px; flex-wrap: wrap; }
    .t-meta-item { font-size: 12px; color: #64748B; }
    .t-meta-item strong { color: #94A3B8; }

    /* TOC 오른쪽 패널 */
    .toc-label { font-size: 10px; font-weight: 700; letter-spacing: .12em; text-transform: uppercase; color: #6366F1; margin-bottom: 14px; flex-shrink: 0; }
    .toc-grid {
      display: grid; grid-template-columns: 1fr;
      gap: 8px; flex: 1; align-content: stretch; overflow: hidden;
    }
    .toc-item {
      display: flex; align-items: center; gap: 10px;
      background: rgba(255,255,255,.05); border: 1px solid rgba(255,255,255,.08);
      border-radius: 8px; padding: 0 14px; cursor: pointer;
      transition: background .15s; min-height: 0;
    }
    .toc-item:hover { background: rgba(255,255,255,.1); }
    .toc-num { font-size: 11px; font-weight: 800; color: #818CF8; min-width: 20px; }
    .toc-text { font-size: 12px; color: #CBD5E1; font-weight: 500; }

    /* ── Step 헤더 슬라이드 ─── */
    .slide-step {
      display: flex; flex-direction: column;
      justify-content: center; align-items: flex-start;
      padding: 64px 80px;
    }
    /* Step 배경색 변형 */
    .bg-s1 { background: linear-gradient(135deg, #1e3a8a 0%, #1d4ed8 100%); }
    .bg-s2 { background: linear-gradient(135deg, #3b0764 0%, #7c3aed 100%); }
    .bg-s3 { background: linear-gradient(135deg, #831843 0%, #db2777 100%); }
    .bg-s4 { background: linear-gradient(135deg, #064e3b 0%, #059669 100%); }
    .bg-s5 { background: linear-gradient(135deg, #7c2d12 0%, #ea580c 100%); }

    .sh-big-num {
      font-size: 120px; font-weight: 900; color: rgba(255,255,255,.12);
      line-height: 1; margin-bottom: -20px; user-select: none;
    }
    .sh-eyebrow { font-size: 11px; font-weight: 700; letter-spacing: .15em; text-transform: uppercase; color: rgba(255,255,255,.6); margin-bottom: 16px; }
    .sh-title { font-size: 42px; font-weight: 800; color: #fff; line-height: 1.15; margin-bottom: 20px; }
    .sh-desc { font-size: 15px; color: rgba(255,255,255,.75); line-height: 1.7; max-width: 560px; margin-bottom: 28px; }
    .sh-pills { display: flex; gap: 8px; flex-wrap: wrap; }
    .sh-pill { background: rgba(255,255,255,.15); color: #fff; font-size: 11px; font-weight: 600; padding: 5px 12px; border-radius: 20px; }

    /* ── 콘텐츠 슬라이드 ─── */
    .slide-content { background: #F8FAFC; display: flex; flex-direction: column; }
    .sc-header {
      display: flex; align-items: center; gap: 12px;
      padding: 20px 52px 16px; border-bottom: 2px solid #E2E8F0;
      flex-shrink: 0;
    }
    .sc-header h2 { font-size: 20px; font-weight: 800; color: #0F172A; }
    .sc-body { flex: 1; overflow-y: auto; padding: 22px 52px 36px; }
    .sc-body-flex { display: flex; flex-direction: column; }

    /* Task 뱃지 색상 */
    .task-badge {
      font-size: 10px; font-weight: 800; letter-spacing: .08em;
      text-transform: uppercase; padding: 4px 10px; border-radius: 6px; flex-shrink: 0;
    }
    .b1 { background: #DBEAFE; color: #1D4ED8; }
    .b2 { background: #EDE9FE; color: #7C3AED; }
    .b3 { background: #FCE7F3; color: #DB2777; }
    .b4 { background: #D1FAE5; color: #059669; }
    .b5 { background: #FFEDD5; color: #EA580C; }

    /* ── 엔딩 슬라이드 ─── */
    .slide-end {
      background: linear-gradient(135deg, #0F172A 0%, #1E1B4B 100%);
      display: flex; flex-direction: column;
      justify-content: center; align-items: center; text-align: center;
      position: relative; overflow: hidden;
    }
    .end-deco1 {
      position: absolute; width: 400px; height: 400px; border-radius: 50%;
      background: radial-gradient(circle, rgba(99,102,241,.2) 0%, transparent 70%);
      top: -100px; right: -100px;
    }
    .end-deco2 {
      position: absolute; width: 300px; height: 300px; border-radius: 50%;
      background: radial-gradient(circle, rgba(139,92,246,.15) 0%, transparent 70%);
      bottom: -80px; left: -80px;
    }
    .end-label { font-size: 11px; font-weight: 700; letter-spacing: .2em; text-transform: uppercase; color: #6366F1; margin-bottom: 20px; position: relative; z-index: 1; }
    .end-title { font-size: 72px; font-weight: 900; color: #F8FAFC; margin-bottom: 24px; position: relative; z-index: 1; }
    .end-divider { width: 60px; height: 3px; background: #6366F1; border-radius: 2px; margin-bottom: 24px; position: relative; z-index: 1; }
    .end-subtitle { font-size: 18px; color: #94A3B8; margin-bottom: 12px; position: relative; z-index: 1; }
    .end-meta { font-size: 13px; color: #64748B; position: relative; z-index: 1; }

    /* ══ 공용 컴포넌트 ═══════════════════════════════════════════ */
    .desc { font-size: 13.5px; color: #475569; line-height: 1.7; }
    code { font-family: 'Consolas', 'Courier New', monospace; background: #EEF2FF; color: #4338CA; padding: 2px 6px; border-radius: 4px; font-size: .9em; }

    /* 파일 경로 뱃지 */
    .file-row { margin-bottom: 12px; }
    .file-pill { display: inline-block; background: #1E293B; color: #94A3B8; font-family: 'Consolas', monospace; font-size: 11px; padding: 4px 10px; border-radius: 6px; }

    /* 코드 블록 */
    .code-wrap { border-radius: 10px; overflow: hidden; border: 1px solid #E2E8F0; }
    .code-hdr { display: flex; justify-content: space-between; align-items: center; padding: 8px 14px; background: #1E293B; }
    .code-fn { color: #94A3B8; font-size: 11px; font-family: 'Consolas', monospace; }
    .code-lang { color: #64748B; font-size: 10px; font-weight: 700; letter-spacing: .08em; text-transform: uppercase; }
    .code-wrap pre { margin: 0; max-height: 320px; overflow-y: auto; }
    .code-wrap pre code { font-size: 12.5px; line-height: 1.6; }

    /* Callout */
    .callout { display: flex; gap: 12px; padding: 12px 16px; border-radius: 10px; margin-top: 14px; }
    .callout.note { background: #EFF6FF; border-left: 3px solid #3B82F6; }
    .callout.warn { background: #FFFBEB; border-left: 3px solid #F59E0B; }
    .callout.tip  { background: #F0FDF4; border-left: 3px solid #22C55E; }
    .callout.danger { background: #FEF2F2; border-left: 3px solid #EF4444; }
    .callout .ci { font-size: 16px; flex-shrink: 0; margin-top: 1px; }
    .callout div { font-size: 12.5px; color: #334155; line-height: 1.6; }
    .callout strong { display: block; margin-bottom: 4px; color: #0F172A; font-size: 13px; }

    /* 체크리스트 */
    .checklist { display: flex; flex-direction: column; gap: 14px; margin-top: 8px; }
    .check-item { display: flex; gap: 14px; align-items: flex-start; }
    .check-circle {
      width: 32px; height: 32px; border-radius: 50%; flex-shrink: 0;
      background: #EEF2FF; color: #4F46E5; font-size: 13px; font-weight: 800;
      display: flex; align-items: center; justify-content: center;
    }
    .check-item div { font-size: 13px; color: #334155; line-height: 1.65; }
    .check-item strong { display: block; color: #0F172A; margin-bottom: 2px; }

    /* 테이블 */
    .ini-table { width: 100%; border-collapse: collapse; font-size: 13px; }
    .ini-table th { background: #1E293B; color: #94A3B8; padding: 10px 14px; text-align: left; font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: .06em; }
    .ini-table td { padding: 10px 14px; border-bottom: 1px solid #E2E8F0; color: #334155; vertical-align: top; }
    .ini-table tr:last-child td { border-bottom: none; }
    .ini-table td:first-child { font-family: 'Consolas', monospace; color: #6D28D9; font-weight: 600; }
    .ini-table.compact td, .ini-table.compact th { padding: 6px 10px; font-size: 12px; }

    /* ══ 진행 표시줄 & 네비게이션 ════════════════════════════════ */
    #progress-bar {
      position: fixed; top: 0; left: 0; right: 0; height: 3px;
      background: rgba(255,255,255,.08); z-index: 500;
      transition: left .28s cubic-bezier(.4,0,.2,1), right .28s cubic-bezier(.4,0,.2,1);
    }
    body.sb-open #progress-bar { left: 300px; }
    #progress-fill { height: 100%; background: #6366F1; transform: scaleX(0); transform-origin: left; transition: transform .32s ease; }

    #nav {
      position: fixed; bottom: 0; left: 0; right: 0; height: 48px;
      background: rgba(15,23,42,.95); backdrop-filter: blur(12px);
      border-top: 1px solid rgba(255,255,255,.06);
      display: flex; align-items: center; justify-content: center; gap: 16px;
      z-index: 500;
      transition: left .28s cubic-bezier(.4,0,.2,1), right .28s cubic-bezier(.4,0,.2,1);
    }
    body.sb-open #nav { left: 300px; }
    #nav button {
      background: rgba(255,255,255,.08); border: 1px solid rgba(255,255,255,.1);
      color: #94A3B8; width: 32px; height: 32px; border-radius: 8px;
      font-size: 16px; cursor: pointer; transition: all .15s;
      display: flex; align-items: center; justify-content: center;
    }
    #nav button:hover:not(:disabled) { background: rgba(255,255,255,.15); color: #F1F5F9; }
    #nav button:disabled { opacity: 0.3; cursor: default; }
    #slide-counter { font-size: 11px; color: #64748B; min-width: 44px; text-align: center; }
    #dot-nav { display: flex; gap: 6px; align-items: center; }
    .dot { width: 6px; height: 6px; border-radius: 50%; background: rgba(255,255,255,.2); cursor: pointer; transition: all .2s; }
    .dot.active { background: #6366F1; transform: scale(1.4); }

    /* 사이드바 토글 버튼 */
    #sb-toggle {
      position: fixed; top: 12px; left: 12px; z-index: 700;
      width: 36px; height: 36px; border-radius: 10px;
      background: rgba(15,23,42,.9); border: 1px solid rgba(255,255,255,.12);
      color: #94A3B8; font-size: 18px; cursor: pointer;
      display: flex; align-items: center; justify-content: center;
      transition: all .15s;
    }
    #sb-toggle:hover { background: rgba(255,255,255,.15); color: #F1F5F9; }

    /* 도움말 모달 */
    #help-modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,.6); z-index: 900; align-items: center; justify-content: center; }
    #help-modal.open { display: flex; }
    .hm-box { background: #1E293B; border-radius: 16px; padding: 28px 32px; max-width: 480px; width: 90%; border: 1px solid rgba(255,255,255,.1); }
    .hm-title { color: #F8FAFC; font-size: 16px; font-weight: 700; margin-bottom: 20px; }
    .kb-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
    .kb-item { display: flex; gap: 10px; align-items: center; }
    .kb-key { background: #334155; color: #F1F5F9; font-size: 10px; font-family: monospace; padding: 3px 7px; border-radius: 4px; white-space: nowrap; flex-shrink: 0; }
    .kb-desc { color: #94A3B8; font-size: 12px; }
    .hm-close { margin-top: 20px; width: 100%; padding: 10px; background: #334155; border: none; color: #F1F5F9; border-radius: 8px; cursor: pointer; font-size: 13px; }
  </style>
</head>
<body>

<!-- 사이드바 토글 -->
<button id="sb-toggle" onclick="toggleSidebar()" title="목차 (T)">☰</button>

<!-- 사이드바 -->
<div id="sidebar">
  <div class="sb-head">
    <div class="sb-badge">{{SHORT_TITLE}}</div>
    <div class="sb-title">{{FULL_TITLE}}</div>
  </div>
  <div class="sb-list" id="sb-list"></div>
  <div class="sb-help-btn" onclick="document.getElementById('help-modal').classList.toggle('open')">⌨ 단축키 도움말</div>
</div>

<!-- 진행 표시줄 -->
<div id="progress-bar"><div id="progress-fill"></div></div>

<!-- 슬라이드 덱 -->
<div id="deck">

  <!-- SLIDES HERE -->

</div><!-- #deck -->

<!-- 네비게이션 -->
<div id="nav">
  <button id="btn-prev" onclick="prevSlide()" disabled title="이전 (← PgUp)">&#8592;</button>
  <div id="dot-nav"></div>
  <span id="slide-counter">1 / 1</span>
  <button id="btn-next" onclick="nextSlide()" title="다음 (→ PgDn)">&#8594;</button>
</div>

<!-- 도움말 모달 -->
<div id="help-modal">
  <div class="hm-box">
    <div class="hm-title">⌨ 키보드 단축키</div>
    <div class="kb-grid">
      <div class="kb-item"><span class="kb-key">→ / PgDn</span><div class="kb-desc">다음 슬라이드</div></div>
      <div class="kb-item"><span class="kb-key">← / PgUp</span><div class="kb-desc">이전 슬라이드</div></div>
      <div class="kb-item"><span class="kb-key">Home</span><div class="kb-desc">첫 슬라이드로</div></div>
      <div class="kb-item"><span class="kb-key">End</span><div class="kb-desc">마지막 슬라이드로</div></div>
      <div class="kb-item"><span class="kb-key">T</span><div class="kb-desc">목차 사이드바 토글</div></div>
      <div class="kb-item"><span class="kb-key">Esc</span><div class="kb-desc">모달/사이드바 닫기</div></div>
    </div>
    <button class="hm-close" onclick="document.getElementById('help-modal').classList.remove('open')">닫기</button>
  </div>
</div>

<script>
/* ─── Mermaid (다이어그램 필요 시) ──────────────────────────── */
mermaid.initialize({
  startOnLoad: true, securityLevel: 'loose', theme: 'base',
  themeVariables: { fontFamily: "'Segoe UI','Apple SD Gothic Neo',sans-serif", fontSize: '14px' },
  flowchart: { curve: 'basis', htmlLabels: true, nodeSpacing: 60, rankSpacing: 80 }
});

/* ─── Slide Engine ──────────────────────────────────────────── */
const slides   = Array.from(document.querySelectorAll('.slide'));
const TOTAL    = slides.length;
let current    = 0;
let animating  = false;
const DURATION = 320; // CSS transition 길이와 일치해야 함

function goToSlide(idx) {
  if (idx < 0 || idx >= TOTAL || idx === current || animating) return;
  animating = true;
  const isForward = idx > current;
  const prevIdx   = current;
  current         = idx;
  const prevEl    = slides[prevIdx];
  const nextEl    = slides[current];

  // 다음 슬라이드를 트랜지션 없이 오프스크린에 배치
  nextEl.style.transition = 'none';
  nextEl.style.transform  = isForward ? 'translateX(100%)' : 'translateX(-100%)';
  void nextEl.offsetWidth; // reflow 강제

  // 트랜지션 복원 후 동시 이동
  nextEl.style.transition = '';
  prevEl.style.transform  = isForward ? 'translateX(-100%)' : 'translateX(100%)';
  nextEl.style.transform  = 'translateX(0)';
  nextEl.classList.add('active');
  prevEl.classList.remove('active');

  setTimeout(() => { animating = false; }, DURATION);
  updateUI();
}

function nextSlide() { goToSlide(current + 1); }
function prevSlide() { goToSlide(current - 1); }

/* ─── Sidebar ───────────────────────────────────────────────── */
const SB_WIDTH = 300;
function toggleSidebar() { document.body.classList.toggle('sb-open'); }

/* ─── SLIDE_META → 사이드바 + 도트 네비 생성 ──────────────── */
// ⚠️ idx 값이 DOM 순서와 반드시 일치해야 합니다 (0부터 시작)
// The End 슬라이드는 SLIDE_META에 포함하지 않습니다
const SLIDE_META = [
  // { idx: 0, label: '제목 · 목차', dot: '🏠', color: '#475569', step: false },
  // 여기에 슬라이드 메타데이터 추가
];

const sbList = document.getElementById('sb-list');
SLIDE_META.forEach(m => {
  const el    = document.createElement('div');
  el.className = 'sb-item' + (m.step ? ' is-step' : '') + (m.idx === 0 ? ' active' : '');
  el.dataset.idx = m.idx;
  const dot   = document.createElement('div');
  dot.className = 'sb-dot';
  dot.style.background = m.color + '22';
  dot.style.color      = m.color;
  dot.textContent      = m.dot;
  const label = document.createElement('div');
  label.className = 'sb-label';
  label.textContent   = m.label;
  el.appendChild(dot);
  el.appendChild(label);
  el.addEventListener('click', () => goToSlide(+el.dataset.idx));
  sbList.appendChild(el);
});

/* ─── 도트 네비 생성 ────────────────────────────────────────── */
const dotNav = document.getElementById('dot-nav');
slides.forEach((_, i) => {
  const d = document.createElement('div');
  d.className = 'dot' + (i === 0 ? ' active' : '');
  d.addEventListener('click', () => goToSlide(i));
  dotNav.appendChild(d);
});

/* ─── UI 업데이트 ───────────────────────────────────────────── */
const counter     = document.getElementById('slide-counter');
const progressFill = document.getElementById('progress-fill');
const btnPrev     = document.getElementById('btn-prev');
const btnNext     = document.getElementById('btn-next');
const dots        = Array.from(dotNav.children);

function updateUI() {
  counter.textContent = `${current + 1} / ${TOTAL}`;
  progressFill.style.transform = `scaleX(${(current + 1) / TOTAL})`;
  btnPrev.disabled = current === 0;
  btnNext.disabled = current === TOTAL - 1;
  dots.forEach((d, i) => d.classList.toggle('active', i === current));
  document.querySelectorAll('.sb-item').forEach(el => {
    el.classList.toggle('active', +el.dataset.idx === current);
  });
}

/* ─── 키보드 단축키 ─────────────────────────────────────────── */
document.addEventListener('keydown', e => {
  const modal  = document.getElementById('help-modal');
  const isOpen = modal.classList.contains('open');
  switch (e.key) {
    case 'ArrowRight': case 'PageDown': e.preventDefault(); nextSlide(); break;
    case 'ArrowLeft':  case 'PageUp':   e.preventDefault(); prevSlide(); break;
    case 'Home':       e.preventDefault(); goToSlide(0); break;
    case 'End':        e.preventDefault(); goToSlide(TOTAL - 1); break;
    case 't': case 'T': if (!isOpen) toggleSidebar(); break;
    case 'Escape':
      if (isOpen) modal.classList.remove('open');
      else document.body.classList.remove('sb-open');
      break;
  }
});

/* ─── 초기화 ────────────────────────────────────────────────── */
hljs.highlightAll();
updateUI();
</script>
</body>
</html>
```

---

## 핵심 동작 원리

### 슬라이드 트랜지션
모든 슬라이드는 `position: absolute; inset: 0`으로 겹쳐 있고, `transform: translateX(±100%)`로 오프스크린에 숨겨집니다. `goToSlide()`는 `void el.offsetWidth`(reflow 강제)를 호출해 position 재설정 후 트랜지션을 시작하므로, 방향성 있는 슬라이드-인/아웃 애니메이션이 구현됩니다.

### 사이드바 Push
`body.sb-open` 클래스가 토글되면 `#deck`, `#nav`, `#progress-bar`가 CSS `left: 300px`으로 밀려납니다. 사이드바 자체는 `translateX(-300px) → translateX(0)` 전환입니다.

### SLIDE_META vs DOM 순서
- JS 엔진은 `querySelectorAll('.slide')` 결과의 **배열 인덱스**를 사용
- `SLIDE_META[i].idx`가 그 배열 인덱스와 일치해야 사이드바 클릭이 올바른 슬라이드로 이동
- 슬라이드 div의 `data-idx` 속성은 **시각적 레퍼런스용**일 뿐, JS가 읽지 않음
