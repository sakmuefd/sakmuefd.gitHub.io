<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>四分屏查看器 · 保存与依次打开</title>
<style>
  *{box-sizing:border-box}
  html,body{height:100%;margin:0}
  body{
    display:flex;flex-direction:column;overflow:hidden;
    background:#0b0e13;color:#e6edf3;
    font:14px/1.45 -apple-system,BlinkMacSystemFont,"Segoe UI","Microsoft YaHei",sans-serif;
    -webkit-font-smoothing:antialiased;
  }

  /* 隐藏顶栏 */
  body.top-hidden .topbar{display:none}

  /* 顶栏 */
  .topbar{
    display:flex;align-items:center;gap:8px;flex-wrap:wrap;
    padding:8px 12px;background:#11161d;border-bottom:1px solid #1f262f;flex:0 0 auto;
  }
  .brand{font-weight:700;color:#58a6ff;letter-spacing:.5px;white-space:nowrap}
  .brand small{color:#6e7681;font-weight:400;font-size:12px;margin-left:6px}
  .group{display:flex;gap:4px;padding:3px;border-radius:8px;background:#0b0e13;border:1px solid #1f262f}
  .spacer{flex:1}
  button{
    font:inherit;font-size:12.5px;color:#c9d1d9;background:#1c232c;border:1px solid #2a323d;
    padding:5px 11px;border-radius:6px;cursor:pointer;transition:.15s;white-space:nowrap;
  }
  button:hover{background:#253040;border-color:#3d4a5c;color:#fff}
  button.active{background:#1f6feb;border-color:#1f6feb;color:#fff}

  /* 保存/依次打开按钮强调色 */
  #btnSave{background:#1c2b3a;border-color:#2a4a66}
  #btnSave:hover{background:#1f6feb;border-color:#1f6feb;color:#fff}
  #btnOpenSeq{background:#1f2b1c;border-color:#2e4a2a}
  #btnOpenSeq:hover{background:#238636;border-color:#2ea043;color:#fff}

  /* 同步按钮 */
  #btnSync{min-width:104px;font-weight:600}
  #btnSync[data-level="0"]{background:#1c232c}
  #btnSync[data-level="1"]{background:#1f6feb;border-color:#1f6feb;color:#fff}
  #btnSync[data-level="2"]{background:#238636;border-color:#2ea043;color:#fff;box-shadow:0 0 10px rgba(46,160,67,.35)}

  /* 恢复顶栏的悬浮按钮 */
  .reveal-btn{
    position:fixed;top:10px;right:10px;z-index:300;
    width:34px;height:34px;border-radius:50%;
    background:rgba(22,28,36,.5);border:1px solid rgba(255,255,255,.12);
    color:#c9d1d9;font-size:15px;line-height:1;cursor:pointer;
    backdrop-filter:blur(8px);opacity:.28;transition:.2s;display:none;
  }
  body.top-hidden .reveal-btn{display:block}
  .reveal-btn:hover{opacity:1;transform:scale(1.08);background:#1f6feb;color:#fff}

  /* 舞台 */
  .stage{position:relative;flex:1;min-height:0;display:grid;gap:8px;padding:8px}

  .stage > .panel:nth-child(1){grid-area:p1}
  .stage > .panel:nth-child(2){grid-area:p2}
  .stage > .panel:nth-child(3){grid-area:p3}
  .stage > .panel:nth-child(4){grid-area:p4}

  .stage.layout-a{grid-template-columns:1fr 1fr;grid-template-rows:1fr 1fr;grid-template-areas:"p1 p2" "p3 p4"}
  .stage.layout-b{grid-template-columns:2fr 1fr;grid-template-rows:1fr 1fr 1fr;grid-template-areas:"p1 p2" "p1 p3" "p1 p4"}
  .stage.layout-c{grid-template-columns:repeat(3,1fr);grid-template-rows:1fr 1fr;grid-template-areas:"p1 p1 p1" "p2 p3 p4"}
  .stage.layout-d{grid-template-columns:repeat(4,1fr);grid-template-rows:1fr;grid-template-areas:"p1 p2 p3 p4"}

  @media (max-width:820px){
    .stage.layout-d{grid-template-columns:1fr 1fr;grid-template-rows:1fr 1fr;grid-template-areas:"p1 p2" "p3 p4"}
  }

  .stage.sync-deep .panel{border-color:rgba(46,160,67,.4)}

  /* 面板 */
  .panel{
    display:flex;flex-direction:column;min-width:0;min-height:0;overflow:hidden;
    background:#11161d;border:1px solid #1f262f;border-radius:10px;transition:border-color .15s;
  }
  .panel .bar{
    display:flex;align-items:center;gap:6px;flex:0 0 auto;
    padding:5px 6px;background:#161c24;border-bottom:1px solid #1f262f;
  }
  .dot{width:8px;height:8px;border-radius:50%;background:var(--c);flex:0 0 auto;box-shadow:0 0 8px var(--c)}
  .panel input{
    flex:1;min-width:0;font:inherit;font-size:12px;color:#e6edf3;background:#0b0e13;
    border:1px solid #2a323d;border-radius:6px;padding:4px 8px;outline:none;
  }
  .panel input::placeholder{color:#556072}
  .panel input:focus{border-color:#58a6ff;box-shadow:0 0 0 2px rgba(88,166,255,.15)}
  .panel .bar button{font-size:12px;padding:4px 8px}

  .sync-tag{
    font-size:10.5px;padding:1px 6px;border-radius:4px;white-space:nowrap;flex:0 0 auto;line-height:1.6;
  }
  .sync-tag[data-state="none"]{display:none}
  .sync-tag[data-state="linked"]{background:rgba(46,160,67,.18);color:#3fb950;border:1px solid rgba(46,160,67,.4)}
  .sync-tag[data-state="blocked"]{background:rgba(248,81,73,.12);color:#f85149;border:1px solid rgba(248,81,73,.3)}
  .sync-tag[data-state="url"]{background:rgba(88,166,255,.12);color:#58a6ff;border:1px solid rgba(88,166,255,.3)}

  .panel .body{
    position:relative;flex:1;min-height:0;display:flex;align-items:center;justify-content:center;
    background:#000;overflow:hidden;
  }
  .panel .body iframe{width:100%;height:100%;border:0;display:block;background:#fff}
  .panel .body img,.panel .body video{max-width:100%;max-height:100%;display:block}
  .panel .body audio{width:85%}
  .panel .body.dragover{outline:2px dashed #58a6ff;outline-offset:-6px;background:#0a1626}

  .stage.hide-bars .panel .bar{display:none}

  .panel.max{position:absolute;inset:8px;z-index:30;box-shadow:0 24px 70px rgba(0,0,0,.7)}

  .hint{text-align:center;color:#6e7681;padding:16px;pointer-events:none;user-select:none}
  .hint .num{font-size:13px;font-weight:600;color:#8b949e;margin-bottom:6px}
  .hint p{margin:4px 0;font-size:12px}
  .hint .small{color:#4d5560;font-size:11.5px}
  .hint.err .num{color:#f85149}

  /* 弹窗 */
  dialog{
    background:#161c24;color:#e6edf3;border:1px solid #2a323d;border-radius:12px;
    padding:20px;max-width:580px;width:calc(100% - 40px);
  }
  dialog::backdrop{background:rgba(0,0,0,.65)}
  dialog h3{margin:0 0 10px;color:#58a6ff;font-size:16px}
  dialog p,dialog li{font-size:13px;line-height:1.65;color:#c9d1d9}
  dialog ul{padding-left:20px;margin:8px 0}
  dialog textarea{
    width:100%;font-family:ui-monospace,SFMono-Regular,Menlo,monospace;font-size:11px;
    background:#0b0e13;color:#8b949e;border:1px solid #2a323d;border-radius:8px;
    padding:10px;resize:vertical;margin-top:6px;
  }
  .dlg-actions{display:flex;gap:8px;justify-content:flex-end;margin-top:14px}

  /* 临时提示 toast */
  .toast{
    position:fixed;left:50%;bottom:28px;transform:translateX(-50%) translateY(14px);
    background:rgba(31,111,235,.92);color:#fff;font-size:13px;padding:8px 18px;border-radius:20px;
    opacity:0;pointer-events:none;transition:.25s;z-index:400;box-shadow:0 8px 30px rgba(0,0,0,.4);
  }
  .toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
</style>
</head>
<body>

<header class="topbar">
  <div class="brand">四分屏<small>Split × 4</small></div>

  <div class="group">
    <button data-layout="a" class="active">2×2</button>
    <button data-layout="b">1大3小</button>
    <button data-layout="c">上1下3</button>
    <button data-layout="d">横排4</button>
  </div>

  <div class="spacer"></div>

  <button id="btnSave" title="把当前四个窗口的网址保存到本地">保存网址</button>
  <button id="btnOpenSeq" title="按保存的顺序依次打开到四个窗口">依次打开</button>
  <button id="btnSync" data-level="0" title="切换同步级别：关 / 地址同步 / 深度同步">同步 · 关</button>
  <button id="btnHelp" title="关于操作同步">?</button>
  <button id="btnBar">隐藏地址栏</button>
  <button id="btnClear">清空</button>
  <button id="btnFull">全屏</button>
  <button id="btnHideTop" title="隐藏上方菜单（按 H 或点右上角恢复）">隐藏顶栏</button>
</header>

<button id="btnReveal" class="reveal-btn" title="显示顶栏 (H)">☰</button>
<div id="toast" class="toast"></div>

<main class="stage layout-a" id="stage"></main>

<dialog id="helpDlg">
  <h3>操作同步说明</h3>
  <p>浏览器出于安全限制，<b>跨域页面</b>无法被父页面直接监听滚动、点击等操作。因此同步分为三档：</p>
  <ul>
    <li><b>关</b>：各窗口完全独立。</li>
    <li><b>地址同步</b>：在任意窗口加载地址 / 拖入文件，其他窗口同步加载同一地址。不涉及页面内操作。</li>
    <li><b>深度同步</b>：除地址同步外，还会同步<b>滚动</b>、<b>点击</b>和<b>页面内跳转</b>。仅对<b>同源页面</b>自动生效。</li>
  </ul>
  <p>跨域页面若想参与深度同步，需要在其控制台执行下方脚本（开发者工具中需切换到对应 iframe 上下文）：</p>
  <textarea id="scriptBox" rows="9" readonly></textarea>
  <div class="dlg-actions">
    <button id="btnCopyScript">复制脚本</button>
    <button id="btnCloseDlg">关闭</button>
  </div>
</dialog>

<script>
(function () {
  'use strict';

  /* ===================== 常量 ===================== */
  var COUNT = 4;
  var STORAGE_KEY = 'split4.sync.v2';
  var SAVED_KEY = 'split4.saved.v1';
  var COLORS = ['#58a6ff', '#3fb950', '#d29922', '#f778ba'];
  var LAYOUTS = ['a', 'b', 'c', 'd'];
  var ALL = [0, 1, 2, 3];

  var SYNC_OFF = 0;
  var SYNC_URL = 1;
  var SYNC_DEEP = 2;

  /* ===================== 状态 ===================== */
  var state = { layout: 'a', sync: SYNC_OFF, urls: ['', '', '', ''] };
  var expectedUrl = ['', '', '', ''];

  var stage = document.getElementById('stage');
  var btnSync = document.getElementById('btnSync');
  var toastEl = document.getElementById('toast');
  var toastTimer = null;

  function panelAt(i) {
    return stage.querySelector('.panel[data-index="' + i + '"]');
  }

  /* ===================== 提示 ===================== */
  function toast(msg) {
    toastEl.textContent = msg;
    toastEl.classList.add('show');
    clearTimeout(toastTimer);
    toastTimer = setTimeout(function () {
      toastEl.classList.remove('show');
    }, 2000);
  }

  /* ===================== 持久化 ===================== */
  function loadState() {
    try {
      var s = JSON.parse(localStorage.getItem(STORAGE_KEY) || '{}');
      if (LAYOUTS.indexOf(s.layout) !== -1) state.layout = s.layout;
      if (typeof s.sync === 'number') state.sync = s.sync;
      if (Array.isArray(s.urls)) {
        for (var i = 0; i < COUNT; i++) {
          if (typeof s.urls[i] === 'string') state.urls[i] = s.urls[i];
        }
      }
    } catch (e) {}
  }
  function saveState() {
    try { localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); } catch (e) {}
  }

  /* ===================== 工具 ===================== */
  function guessKind(url) {
    if (/^blob:/.test(url)) return 'frame';
    var path = url.split(/[?#]/)[0].toLowerCase();
    if (/\.(png|jpe?g|gif|webp|bmp|svg|avif|ico)$/.test(path)) return 'image';
    if (/\.(mp4|webm|ogv|mov|m4v)$/.test(path)) return 'video';
    if (/\.(mp3|wav|m4a|aac|ogg)$/.test(path)) return 'audio';
    return 'frame';
  }
  function escapeHtml(s) {
    return String(s).replace(/[&<>"']/g, function (c) {
      return { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[c];
    });
  }
  function targetsFor(i) {
    return state.sync === SYNC_OFF ? [i] : ALL;
  }

  /* ===================== 注入脚本（同源 iframe 内执行） ===================== */
  function injectedSyncScript() {
    if (window.__splitSyncInstalled) return;
    window.__splitSyncInstalled = true;
    if (window.parent === window) return;

    var ignoreScroll = false;
    var scrollTimer = null;

    function post(msg) {
      msg.__splitSync = true;
      try { window.parent.postMessage(msg, '*'); } catch (e) {}
    }

    window.addEventListener('scroll', function () {
      if (ignoreScroll) return;
      if (scrollTimer) return;
      scrollTimer = setTimeout(function () {
        scrollTimer = null;
        post({ action: 'scroll', x: window.scrollX, y: window.scrollY });
      }, 40);
    }, { passive: true });

    var ignoreClick = false;
    window.addEventListener('click', function (e) {
      if (ignoreClick) return;
      post({ action: 'click', x: e.pageX, y: e.pageY });
    }, true);

    window.addEventListener('message', function (e) {
      var d = e.data;
      if (!d || d.__splitSync !== true) return;

      if (d.action === 'scroll') {
        ignoreScroll = true;
        window.scrollTo(d.x, d.y);
        setTimeout(function () { ignoreScroll = false; }, 100);
      } else if (d.action === 'click') {
        var vx = d.x - window.scrollX;
        var vy = d.y - window.scrollY;
        var el = document.elementFromPoint(vx, vy);
        if (el) {
          ignoreClick = true;
          try {
            el.dispatchEvent(new MouseEvent('click', {
              bubbles: true, cancelable: true, view: window,
              clientX: vx, clientY: vy
            }));
          } catch (err) {}
          ignoreClick = false;
        }
      }
    });
  }

  function injectSyncScript(iframe) {
    if (state.sync !== SYNC_DEEP) return;
    var win;
    try { win = iframe.contentWindow; } catch (e) { return; }
    if (!win) return;
    if (win.__splitSyncInstalled) return;

    var doc;
    try { doc = win.document; } catch (e) { return; }
    if (!doc) return;

    try {
      var s = doc.createElement('script');
      s.textContent = '(' + injectedSyncScript.toString() + ')();';
      (doc.head || doc.documentElement).appendChild(s);
    } catch (e) {}
  }

  /* ===================== 同步状态标签 ===================== */
  function updateSyncTag(i) {
    var panel = panelAt(i);
    if (!panel) return;
    var tag = panel.querySelector('.sync-tag');
    var iframe = panel.querySelector('iframe');

    if (state.sync === SYNC_OFF || !iframe) {
      tag.dataset.state = 'none';
      return;
    }

    var src = iframe.src || '';
    if (!src || src === 'about:blank') {
      tag.dataset.state = 'none';
      return;
    }

    var sameOrigin = false;
    try { sameOrigin = !!iframe.contentDocument; } catch (e) { sameOrigin = false; }

    if (state.sync === SYNC_URL) {
      tag.dataset.state = 'url';
      tag.textContent = '仅地址';
    } else if (state.sync === SYNC_DEEP) {
      if (sameOrigin) {
        tag.dataset.state = 'linked';
        tag.textContent = '已连接';
      } else {
        tag.dataset.state = 'blocked';
        tag.textContent = '跨域受限';
      }
    }
  }

  /* ===================== 渲染 ===================== */
  function makeHint(i) {
    var d = document.createElement('div');
    d.className = 'hint';
    d.innerHTML =
      '<div class="num">画面 ' + (i + 1) + '</div>' +
      '<p>在上方输入地址后按回车加载</p>' +
      '<p class="small">支持网页 / 图片 / 视频，也可把本地文件拖进来</p>';
    return d;
  }

  function makeError(url) {
    var d = document.createElement('div');
    d.className = 'hint err';
    d.innerHTML =
      '<div class="num">加载失败</div>' +
      '<p class="small">' + escapeHtml(url) + '</p>' +
      '<p class="small">该站点可能禁止被嵌入（X-Frame-Options）</p>';
    return d;
  }

  function renderContent(i, url) {
    var panel = panelAt(i);
    if (!panel) return;
    var body = panel.querySelector('.body');
    body.classList.remove('dragover');
    body.innerHTML = '';

    if (!url) {
      body.appendChild(makeHint(i));
      updateSyncTag(i);
      return;
    }

    var kind = guessKind(url);
    var el;

    if (kind === 'image') {
      el = new Image();
      el.src = url;
      el.alt = '';
      el.onerror = function () { body.innerHTML = ''; body.appendChild(makeError(url)); };
    } else if (kind === 'video') {
      el = document.createElement('video');
      el.src = url;
      el.controls = true;
      el.loop = true;
      el.playsInline = true;
      el.onerror = function () { body.innerHTML = ''; body.appendChild(makeError(url)); };
    } else if (kind === 'audio') {
      el = document.createElement('audio');
      el.src = url;
      el.controls = true;
    } else {
      el = document.createElement('iframe');
      el.src = url;
      el.allow = 'autoplay; encrypted-media; picture-in-picture; fullscreen';
      el.allowFullscreen = true;
      el.referrerPolicy = 'no-referrer';
      expectedUrl[i] = url;

      (function (idx, frame) {
        frame.addEventListener('load', function () {
          onIframeLoad(idx, frame);
        });
      })(i, el);
    }

    body.appendChild(el);
    updateSyncTag(i);
  }

  function onIframeLoad(i, iframe) {
    if (state.sync === SYNC_DEEP) {
      injectSyncScript(iframe);
    }
    updateSyncTag(i);

    if (state.sync === SYNC_DEEP) {
      var href = '';
      try { href = iframe.contentWindow.location.href; } catch (e) { href = ''; }

      if (href && href !== 'about:blank' &&
          href.indexOf('blob:') !== 0 &&
          href !== expectedUrl[i]) {
        expectedUrl[i] = href;
        var srcPanel = panelAt(i);
        if (srcPanel) srcPanel.querySelector('.url').value = href;
        state.urls[i] = href;

        ALL.forEach(function (j) {
          if (j === i) return;
          expectedUrl[j] = href;
          state.urls[j] = href;
          var p = panelAt(j);
          if (!p) return;
          p.querySelector('.url').value = href;
          renderContent(j, href);
        });
        saveState();
      }
    }
  }

  /* ===================== 加载（同步感知） ===================== */
  function applyUrl(i, raw) {
    var url = (raw || '').trim();
    targetsFor(i).forEach(function (idx) {
      state.urls[idx] = url;
      var panel = panelAt(idx);
      if (!panel) return;
      panel.querySelector('.url').value = url;
      renderContent(idx, url);
    });
    saveState();
  }

  /** 只在指定窗口打开，不受同步影响（用于"依次打开"） */
  function openInPanel(i, url) {
    state.urls[i] = url;
    expectedUrl[i] = url;
    var panel = panelAt(i);
    if (!panel) return;
    panel.querySelector('.url').value = url;
    renderContent(i, url);
    saveState();
  }

  function showLocalFile(i, file) {
    var blobUrl = URL.createObjectURL(file);

    targetsFor(i).forEach(function (idx) {
      var panel = panelAt(idx);
      if (!panel) return;

      state.urls[idx] = '';
      expectedUrl[idx] = blobUrl;
      panel.querySelector('.url').value = '📁 ' + file.name;

      var body = panel.querySelector('.body');
      body.classList.remove('dragover');
      body.innerHTML = '';

      var el;
      if (file.type.indexOf('image/') === 0) {
        el = new Image(); el.src = blobUrl;
      } else if (file.type.indexOf('video/') === 0) {
        el = document.createElement('video');
        el.src = blobUrl; el.controls = true; el.loop = true; el.playsInline = true;
      } else if (file.type.indexOf('audio/') === 0) {
        el = document.createElement('audio'); el.src = blobUrl; el.controls = true;
      } else {
        el = document.createElement('iframe'); el.src = blobUrl;
        (function (idx2, frame) {
          frame.addEventListener('load', function () { onIframeLoad(idx2, frame); });
        })(idx, el);
      }
      body.appendChild(el);
      updateSyncTag(idx);
    });

    saveState();
  }

  /* ===================== 保存 / 依次打开 ===================== */
  function getCurrentUrls() {
    return ALL.map(function (i) {
      var p = panelAt(i);
      if (!p) return '';
      var v = p.querySelector('.url').value.trim();
      // 本地文件（blob）无法跨会话保存，存为空
      if (v.indexOf('📁 ') === 0) return '';
      return v;
    });
  }

  var btnSave = document.getElementById('btnSave');
  var btnOpenSeq = document.getElementById('btnOpenSeq');

  btnSave.addEventListener('click', function () {
    var urls = getCurrentUrls();
    if (!urls.some(function (u) { return u; })) {
      toast('没有可保存的网址');
      return;
    }
    try {
      localStorage.setItem(SAVED_KEY, JSON.stringify({ urls: urls, time: Date.now() }));
      toast('已保存 4 个窗口的网址');
    } catch (e) {
      toast('保存失败：本地存储不可用');
    }
  });

  btnOpenSeq.addEventListener('click', function () {
    var saved = null;
    try {
      var raw = localStorage.getItem(SAVED_KEY);
      if (raw) {
        var obj = JSON.parse(raw);
        if (obj && Array.isArray(obj.urls)) saved = obj.urls;
      }
    } catch (e) { saved = null; }

    if (!saved || !saved.some(function (u) { return u; })) {
      toast('请先保存网址');
      return;
    }

    toast('依次打开中…');

    var delay = 0;
    var opened = 0;
    saved.forEach(function (url, i) {
      if (!url || i >= COUNT) return;
      // 按顺序排队，即使有空项也不打乱已排定的时间顺序
      setTimeout(function () {
        openInPanel(i, url);
      }, delay);
      delay += 550;
      opened++;
    });

    if (opened === 0) toast('保存的网址为空');
  });

  /* ===================== 放大 ===================== */
  function updateMaxButtons() {
    stage.querySelectorAll('.panel').forEach(function (p) {
      p.querySelector('.max').textContent = p.classList.contains('max') ? '还原' : '⤢';
    });
  }
  function toggleMax(i) {
    var p = panelAt(i);
    var willMax = !p.classList.contains('max');
    stage.querySelectorAll('.panel.max').forEach(function (x) { x.classList.remove('max'); });
    if (willMax) p.classList.add('max');
    updateMaxButtons();
  }
  function clearMax() {
    stage.querySelectorAll('.panel.max').forEach(function (x) { x.classList.remove('max'); });
    updateMaxButtons();
  }

  /* ===================== 面板创建 ===================== */
  function makePanel(i) {
    var p = document.createElement('section');
    p.className = 'panel';
    p.dataset.index = i;
    p.innerHTML =
      '<div class="bar">' +
        '<span class="dot" style="--c:' + COLORS[i] + '"></span>' +
        '<input class="url" type="text" spellcheck="false" autocomplete="off" ' +
               'placeholder="画面 ' + (i + 1) + ' · 网址 / 图片 / 视频地址">' +
        '<span class="sync-tag" data-state="none"></span>' +
        '<button class="go" title="加载">↵</button>' +
        '<button class="max" title="放大 / 还原">⤢</button>' +
      '</div>' +
      '<div class="body"></div>';

    var input = p.querySelector('.url');
    input.addEventListener('keydown', function (e) {
      if (e.key === 'Enter') { e.preventDefault(); applyUrl(i, input.value); }
    });
    p.querySelector('.go').addEventListener('click', function () { applyUrl(i, input.value); });
    p.querySelector('.max').addEventListener('click', function () { toggleMax(i); });

    var body = p.querySelector('.body');
    body.addEventListener('dragover', function (e) { e.preventDefault(); body.classList.add('dragover'); });
    body.addEventListener('dragleave', function () { body.classList.remove('dragover'); });
    body.addEventListener('drop', function (e) {
      e.preventDefault();
      body.classList.remove('dragover');

      var file = e.dataTransfer.files && e.dataTransfer.files[0];
      if (file) { showLocalFile(i, file); return; }

      var text = e.dataTransfer.getData('text/uri-list') || e.dataTransfer.getData('text/plain');
      if (text && text.trim()) applyUrl(i, text.trim());
    });

    body.appendChild(makeHint(i));
    return p;
  }

  /* ===================== 布局 ===================== */
  function setLayout(layout) {
    if (LAYOUTS.indexOf(layout) === -1) layout = 'a';
    state.layout = layout;
    saveState();
    LAYOUTS.forEach(function (l) {
      stage.classList.toggle('layout-' + l, l === layout);
    });
    document.querySelectorAll('[data-layout]').forEach(function (btn) {
      btn.classList.toggle('active', btn.dataset.layout === layout);
    });
    clearMax();
  }

  /* ===================== 同步 UI ===================== */
  function updateSyncUI() {
    btnSync.dataset.level = state.sync;
    btnSync.textContent = ['同步 · 关', '同步 · 地址', '同步 · 深度'][state.sync];
    stage.classList.toggle('sync-deep', state.sync === SYNC_DEEP);
  }

  btnSync.addEventListener('click', function () {
    state.sync = (state.sync + 1) % 3;
    saveState();
    updateSyncUI();

    ALL.forEach(function (i) {
      if (state.sync === SYNC_DEEP) {
        var iframe = panelAt(i).querySelector('iframe');
        if (iframe) injectSyncScript(iframe);
      }
      updateSyncTag(i);
    });
  });

  /* ===================== 消息转发 ===================== */
  window.addEventListener('message', function (e) {
    var d = e.data;
    if (!d || d.__splitSync !== true) return;
    if (state.sync !== SYNC_DEEP) return;

    var source = null;
    ALL.forEach(function (i) {
      var f = panelAt(i).querySelector('iframe');
      if (f && f.contentWindow === e.source) source = f;
    });
    if (!source) return;

    ALL.forEach(function (i) {
      var f = panelAt(i).querySelector('iframe');
      if (!f || f === source) return;
      try { f.contentWindow.postMessage(d, '*'); } catch (err) {}
    });
  });

  /* ===================== 隐藏顶栏 ===================== */
  var btnHideTop = document.getElementById('btnHideTop');
  var btnReveal = document.getElementById('btnReveal');

  btnHideTop.addEventListener('click', function () {
    document.body.classList.add('top-hidden');
  });
  btnReveal.addEventListener('click', function () {
    document.body.classList.remove('top-hidden');
  });

  /* ===================== 帮助弹窗 ===================== */
  var helpDlg = document.getElementById('helpDlg');
  var scriptBox = document.getElementById('scriptBox');

  document.getElementById('btnHelp').addEventListener('click', function () {
    scriptBox.value = '(' + injectedSyncScript.toString() + ')();';
    if (typeof helpDlg.showModal === 'function') helpDlg.showModal();
    else helpDlg.setAttribute('open', '');
  });

  document.getElementById('btnCloseDlg').addEventListener('click', function () {
    if (typeof helpDlg.close === 'function') helpDlg.close();
    else helpDlg.removeAttribute('open');
  });

  document.getElementById('btnCopyScript').addEventListener('click', function () {
    scriptBox.select();
    try {
      document.execCommand('copy');
      toast('脚本已复制');
    } catch (e) {
      if (navigator.clipboard) {
        navigator.clipboard.writeText(scriptBox.value).then(function () {
          toast('脚本已复制');
        }).catch(function () {});
      }
    }
  });

  /* ===================== 初始化 ===================== */
  function init() {
    loadState();

    for (var i = 0; i < COUNT; i++) stage.appendChild(makePanel(i));
    setLayout(state.layout);

    for (var j = 0; j < COUNT; j++) {
      var url = state.urls[j];
      if (url) {
        panelAt(j).querySelector('.url').value = url;
        renderContent(j, url);
      }
    }

    updateSyncUI();

    document.querySelectorAll('[data-layout]').forEach(function (btn) {
      btn.addEventListener('click', function () { setLayout(btn.dataset.layout); });
    });

    var btnBar = document.getElementById('btnBar');
    btnBar.addEventListener('click', function () {
      var hidden = stage.classList.toggle('hide-bars');
      btnBar.textContent = hidden ? '显示地址栏' : '隐藏地址栏';
    });

    document.getElementById('btnClear').addEventListener('click', function () {
      ALL.forEach(function (i) {
        state.urls[i] = '';
        expectedUrl[i] = '';
        panelAt(i).querySelector('.url').value = '';
        renderContent(i, '');
      });
      saveState();
    });

    document.getElementById('btnFull').addEventListener('click', function () {
      if (!document.fullscreenElement) {
        document.documentElement.requestFullscreen && document.documentElement.requestFullscreen().catch(function () {});
      } else {
        document.exitFullscreen && document.exitFullscreen();
      }
    });

    document.addEventListener('keydown', function (e) {
      var tag = (document.activeElement && document.activeElement.tagName) || '';
      var typing = /^(INPUT|TEXTAREA|SELECT)$/.test(tag);

      if (e.key === 'Escape') {
        clearMax();
      } else if (!typing && (e.key === 'h' || e.key === 'H')) {
        // H：切换顶栏显示/隐藏
        document.body.classList.toggle('top-hidden');
      }
    });
  }

  init();
})();
</script>
</body>
</html>