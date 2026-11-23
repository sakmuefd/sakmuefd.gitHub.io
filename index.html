<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>URL ck 管理器</title>
<style>
  * { box-sizing: border-box; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    max-width: 960px;
    margin: 0 auto;
    padding: 24px 16px 80px;
    background: #f5f5f7;
    color: #1d1d1f;
    line-height: 1.5;
  }
  h1 { font-size: 26px; margin: 0 0 4px; }
  .subtitle { color: #86868b; font-size: 14px; margin-bottom: 24px; }
  .subtitle code, .hint code, label code {
    background: #e8e8ed;
    padding: 1px 6px;
    border-radius: 4px;
    font-size: 0.92em;
    font-family: ui-monospace, "SF Mono", Menlo, monospace;
  }
  .panel {
    background: #fff;
    border-radius: 14px;
    padding: 20px;
    margin-bottom: 16px;
    box-shadow: 0 1px 4px rgba(0,0,0,0.06);
  }
  .panel h2 { font-size: 16px; margin: 0 0 14px; }
  label { display: block; font-size: 13px; color: #6e6e73; margin-bottom: 5px; font-weight: 500; }
  input[type="text"], textarea {
    width: 100%;
    padding: 9px 12px;
    border: 1px solid #d2d2d7;
    border-radius: 8px;
    font-size: 14px;
    font-family: inherit;
    background: #fafafa;
    transition: border-color .2s, background .2s, box-shadow .2s;
  }
  input[type="text"]:focus, textarea:focus {
    outline: none;
    border-color: #0071e3;
    background: #fff;
    box-shadow: 0 0 0 3px rgba(0,113,227,0.12);
  }
  textarea { resize: vertical; min-height: 100px; font-family: ui-monospace, Menlo, monospace; font-size: 13px; }
  .field { margin-bottom: 12px; }
  .actions { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 16px; }
  button {
    padding: 9px 16px;
    border: none;
    border-radius: 8px;
    font-size: 14px;
    font-weight: 500;
    cursor: pointer;
    font-family: inherit;
    transition: opacity .15s, transform .05s, background .15s;
  }
  button:active { transform: scale(0.98); }
  .btn-primary { background: #0071e3; color: #fff; }
  .btn-primary:hover { opacity: .9; }
  .btn-secondary { background: #e8e8ed; color: #1d1d1f; }
  .btn-secondary:hover { background: #dcdce1; }
  .btn-danger { background: #ffe5e5; color: #d70015; }
  .btn-danger:hover { background: #ffd0d0; }
  .btn-small { padding: 6px 12px; font-size: 13px; }
  .toolbar {
    display: flex;
    align-items: center;
    gap: 10px;
    flex-wrap: wrap;
    background: #fff;
    border-radius: 14px;
    padding: 12px 16px;
    margin-bottom: 12px;
    box-shadow: 0 1px 4px rgba(0,0,0,0.06);
  }
  .toolbar .sel-info { font-size: 13px; color: #6e6e73; white-space: nowrap; }
  .toolbar input[type="text"] { flex: 1; min-width: 160px; }
  .item {
    background: #fff;
    border-radius: 14px;
    padding: 14px 18px;
    margin-bottom: 10px;
    box-shadow: 0 1px 4px rgba(0,0,0,0.06);
    display: flex;
    align-items: center;
    gap: 12px;
    flex-wrap: wrap;
    transition: box-shadow .2s;
  }
  .item.selected { box-shadow: 0 0 0 2px #0071e3, 0 1px 4px rgba(0,0,0,0.06); }
  .item.editing { display: block; border: 1px solid #0071e3; box-shadow: 0 0 0 3px rgba(0,113,227,0.08); }
  .item input[type="checkbox"] {
    width: 18px;
    height: 18px;
    accent-color: #0071e3;
    cursor: pointer;
    flex-shrink: 0;
  }
  .item-name {
    font-weight: 600;
    font-size: 15px;
    flex: 1;
    min-width: 100px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
  .badge {
    font-size: 11px;
    padding: 2px 9px;
    border-radius: 999px;
    background: #e8f5e9;
    color: #1b7a34;
    font-weight: 500;
    white-space: nowrap;
  }
  .badge.warn { background: #fff3e0; color: #b35c00; }
  .item-actions { display: flex; gap: 6px; flex-wrap: wrap; }
  .edit-fields .field { margin-bottom: 12px; }
  .ck-inline { display: flex; gap: 8px; align-items: center; flex-basis: 100%; }
  .ck-inline input { flex: 1; min-width: 120px; }
  .empty {
    text-align: center;
    padding: 50px 20px;
    color: #86868b;
    font-size: 14px;
    background: #fff;
    border-radius: 14px;
    box-shadow: 0 1px 4px rgba(0,0,0,0.06);
  }
  .hint { font-size: 12px; color: #86868b; margin-top: 10px; }
  .hidden { display: none !important; }
  .toast {
    position: fixed;
    bottom: 28px;
    left: 50%;
    transform: translateX(-50%) translateY(20px);
    background: #1d1d1f;
    color: #fff;
    padding: 10px 22px;
    border-radius: 10px;
    font-size: 13px;
    opacity: 0;
    pointer-events: none;
    transition: opacity .25s, transform .25s;
    z-index: 100;
    max-width: 90vw;
    text-align: center;
  }
  .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }
  @media (max-width: 600px) {
    body { padding: 16px 12px 80px; }
    h1 { font-size: 22px; }
    .panel { padding: 16px; }
    .item { padding: 12px 14px; }
  }
</style>
</head>
<body>

  <h1>🔗 URL ck 管理器</h1>
  <div class="subtitle">用 <code>{ck}</code> 作为占位符管理 URL，支持单条更新或勾选批量更新 ck。</div>

  <!-- 添加 -->
  <div class="panel">
    <h2>➕ 添加 URL</h2>
    <div class="field">
      <label>名称</label>
      <input type="text" id="newName" placeholder="例如：订单接口">
    </div>
    <div class="field">
      <label>URL 模板（用 <code>{ck}</code> 作为 ck 占位符）</label>
      <input type="text" id="newTemplate" placeholder="例如：https://api.example.com/data?token={ck}&id=123">
    </div>
    <div class="field">
      <label>当前 ck 值（可留空）</label>
      <input type="text" id="newCk" placeholder="粘贴 ck / token">
    </div>
    <div class="actions" style="margin-bottom:0">
      <button class="btn-primary" id="addBtn">添加</button>
      <button class="btn-secondary" id="toggleBatchBtn">批量添加</button>
    </div>
    <div id="batchArea" class="hidden" style="margin-top:14px">
      <label>每行一个：<code>名称 | URL模板</code>（名称可省略）</label>
      <textarea id="batchInput" placeholder="订单接口 | https://api.example.com/data?token={ck}
https://api.example.com/user?ck={ck}"></textarea>
      <div style="margin-top:10px">
        <button class="btn-primary btn-small" id="batchAddBtn">批量添加</button>
      </div>
    </div>
    <div class="hint">如果 URL 里没有 <code>{ck}</code>，工具会尝试把 <code>ck=xxx</code> 自动替换为 <code>ck={ck}</code>。</div>
  </div>

  <!-- 勾选批量更新 ck -->
  <div class="toolbar">
    <span class="sel-info" id="selInfo">已选 0 条</span>
    <input type="text" id="batchCkInput" placeholder="输入 ck 值，应用到勾选的 URL">
    <button class="btn-primary btn-small" id="applySelectedBtn">应用到勾选</button>
    <button class="btn-secondary btn-small" id="selectAllBtn">全选</button>
    <button class="btn-secondary btn-small" id="selectNoneBtn">取消全选</button>
  </div>

  <!-- 工具栏 -->
  <div class="actions">
    <button class="btn-secondary btn-small" id="copyAllBtn">📋 复制全部完整 URL</button>
    <button class="btn-secondary btn-small" id="exportBtn">⬇️ 导出 JSON</button>
    <button class="btn-secondary btn-small" id="importBtn">⬆️ 导入 JSON</button>
    <input type="file" id="importFile" accept=".json,application/json" class="hidden">
    <button class="btn-danger btn-small" id="clearBtn">🗑 清空全部</button>
  </div>

  <!-- 列表 -->
  <div id="list"></div>

  <div class="toast" id="toast"></div>

<script>
(function () {
  'use strict';

  const STORAGE_KEY = 'urlCkManager.items.v1';
  let items = [];
  let editingId = null;
  let selectedIds = new Set();
  let saveTimer = null;

  function uid() {
    return Date.now().toString(36) + Math.random().toString(36).slice(2, 8);
  }

  function escapeHtml(s) {
    return String(s == null ? '' : s).replace(/[&<>"']/g, function (c) {
      return { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#39;' }[c];
    });
  }

  function load() {
    try {
      const raw = localStorage.getItem(STORAGE_KEY);
      items = raw ? JSON.parse(raw) : [];
      if (!Array.isArray(items)) items = [];
      items = items
        .filter(function (x) { return x && typeof x.template === 'string'; })
        .map(function (x) {
          return {
            id: x.id || uid(),
            name: x.name || '未命名',
            template: x.template,
            ck: x.ck || ''
          };
        });
    } catch (e) {
      items = [];
    }
    selectedIds = new Set();
  }

  function save() {
    try {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(items));
    } catch (e) {
      toast('保存失败：' + e.message);
    }
  }

  function saveDebounced() {
    clearTimeout(saveTimer);
    saveTimer = setTimeout(save, 300);
  }

  function normalizeTemplate(raw) {
    let t = String(raw || '').trim();
    if (!t) return '';
    if (t.indexOf('{ck}') !== -1) return t;
    return t.replace(/([?&])ck=([^&#]*)/i, '$1ck={ck}');
  }

  function buildFullUrl(item) {
    if (!item.template) return '';
    return item.template.replace(/\{ck\}/g, item.ck || '');
  }

  function toast(msg) {
    const el = document.getElementById('toast');
    el.textContent = msg;
    el.classList.add('show');
    clearTimeout(toast._t);
    toast._t = setTimeout(function () { el.classList.remove('show'); }, 1800);
  }

  async function copyText(text) {
    if (navigator.clipboard && window.isSecureContext) {
      try {
        await navigator.clipboard.writeText(text);
        return true;
      } catch (e) {}
    }
    const ta = document.createElement('textarea');
    ta.value = text;
    ta.style.position = 'fixed';
    ta.style.opacity = '0';
    document.body.appendChild(ta);
    ta.select();
    let ok = false;
    try { ok = document.execCommand('copy'); } catch (e) {}
    document.body.removeChild(ta);
    return ok;
  }

  function updateSelInfo() {
    document.getElementById('selInfo').textContent = '已选 ' + selectedIds.size + ' 条';
  }

  function render() {
    const list = document.getElementById('list');
    updateSelInfo();

    if (!items.length) {
      list.innerHTML = '<div class="empty">还没有 URL，先在上方添加一个吧 👆</div>';
      return;
    }
    list.innerHTML = items.map(function (item) {
      return item.id === editingId ? renderEdit(item) : renderView(item);
    }).join('');
  }

  // 卡片：勾选框 + 名称 + 状态 + 独立 ck 输入 + 操作按钮
  function renderView(item) {
    const hasCk = !!(item.ck && item.ck.trim());
    const checked = selectedIds.has(item.id);
    return '' +
      '<div class="item' + (checked ? ' selected' : '') + '" data-id="' + escapeHtml(item.id) + '">' +
        '<input type="checkbox" class="sel-box" data-id="' + escapeHtml(item.id) + '"' + (checked ? ' checked' : '') + '>' +
        '<div class="item-name" title="' + escapeHtml(item.name) + '">' + escapeHtml(item.name) + '</div>' +
        '<span class="badge ' + (hasCk ? '' : 'warn') + '">' + (hasCk ? '已设置 ck' : '缺 ck') + '</span>' +
        '<div class="item-actions">' +
          '<button class="btn-primary btn-small copy-one" data-id="' + escapeHtml(item.id) + '">📋 复制</button>' +
          '<button class="btn-secondary btn-small edit-one" data-id="' + escapeHtml(item.id) + '">✏️ 编辑</button>' +
          '<button class="btn-danger btn-small del-one" data-id="' + escapeHtml(item.id) + '">删除</button>' +
        '</div>' +
        '<div class="ck-inline">' +
          '<input type="text" class="ck-input" value="' + escapeHtml(item.ck) + '" placeholder="单独输入此条 URL 的 ck，实时生效" data-id="' + escapeHtml(item.id) + '">' +
        '</div>' +
      '</div>';
  }

  function renderEdit(item) {
    return '' +
      '<div class="item editing" data-id="' + escapeHtml(item.id) + '">' +
        '<div class="edit-fields">' +
          '<div class="field">' +
            '<label>名称</label>' +
            '<input type="text" class="edit-name" value="' + escapeHtml(item.name) + '">' +
          '</div>' +
          '<div class="field">' +
            '<label>URL 模板（用 <code>{ck}</code> 占位）</label>' +
            '<input type="text" class="edit-template" value="' + escapeHtml(item.template) + '">' +
          '</div>' +
          '<div class="field">' +
            '<label>当前 ck 值</label>' +
            '<input type="text" class="edit-ck" value="' + escapeHtml(item.ck) + '" placeholder="粘贴 ck / token">' +
          '</div>' +
        '</div>' +
        '<div class="item-actions">' +
          '<button class="btn-primary btn-small save-edit" data-id="' + escapeHtml(item.id) + '">保存</button>' +
          '<button class="btn-secondary btn-small cancel-edit">取消</button>' +
        '</div>' +
      '</div>';
  }

  function bindEvents() {
    document.getElementById('addBtn').addEventListener('click', addItem);

    document.getElementById('toggleBatchBtn').addEventListener('click', function () {
      document.getElementById('batchArea').classList.toggle('hidden');
    });

    document.getElementById('batchAddBtn').addEventListener('click', batchAdd);

    document.getElementById('copyAllBtn').addEventListener('click', async function () {
      if (!items.length) return toast('列表为空');
      const text = items.map(buildFullUrl).filter(Boolean).join('\n');
      if (!text) return toast('没有可复制的 URL');
      const ok = await copyText(text);
      toast(ok ? '已复制 ' + items.length + ' 条 URL' : '复制失败');
    });

    document.getElementById('exportBtn').addEventListener('click', exportJson);
    document.getElementById('importBtn').addEventListener('click', function () {
      document.getElementById('importFile').click();
    });
    document.getElementById('importFile').addEventListener('change', importJson);

    document.getElementById('clearBtn').addEventListener('click', function () {
      if (!items.length) return toast('列表为空');
      if (confirm('确定清空全部 ' + items.length + ' 条数据？此操作不可撤销。')) {
        items = [];
        selectedIds.clear();
        save();
        render();
        toast('已清空');
      }
    });

    // 应用到勾选
    document.getElementById('applySelectedBtn').addEventListener('click', function () {
      const val = document.getElementById('batchCkInput').value.trim();
      if (!val) return toast('请输入 ck 值');
      if (!selectedIds.size) return toast('请先勾选要更新的 URL');
      let count = 0;
      items.forEach(function (x) {
        if (selectedIds.has(x.id)) { x.ck = val; count++; }
      });
      save();
      document.getElementById('batchCkInput').value = '';
      render();
      toast('已更新 ' + count + ' 条');
    });

    document.getElementById('selectAllBtn').addEventListener('click', function () {
      if (!items.length) return toast('列表为空');
      items.forEach(function (x) { selectedIds.add(x.id); });
      render();
    });

    document.getElementById('selectNoneBtn').addEventListener('click', function () {
      selectedIds.clear();
      render();
    });

    const listEl = document.getElementById('list');

    // 勾选 / 取消勾选
    listEl.addEventListener('change', function (e) {
      if (!e.target.classList.contains('sel-box')) return;
      const id = e.target.dataset.id;
      if (e.target.checked) selectedIds.add(id);
      else selectedIds.delete(id);
      const card = e.target.closest('.item');
      if (card) card.classList.toggle('selected', e.target.checked);
      updateSelInfo();
    });

    // 单条 ck 实时输入
    listEl.addEventListener('input', function (e) {
      if (!e.target.classList.contains('ck-input')) return;
      const id = e.target.dataset.id;
      const item = items.find(function (x) { return x.id === id; });
      if (!item) return;
      item.ck = e.target.value;

      const card = e.target.closest('.item');
      const badge = card.querySelector('.badge');
      const hasCk = !!item.ck.trim();
      badge.textContent = hasCk ? '已设置 ck' : '缺 ck';
      badge.className = 'badge' + (hasCk ? '' : ' warn');

      saveDebounced();
    });

    listEl.addEventListener('click', async function (e) {
      const btn = e.target.closest('button');
      if (!btn) return;
      const id = btn.dataset.id;

      if (btn.classList.contains('copy-one')) {
        const item = items.find(function (x) { return x.id === id; });
        if (!item) return;
        const full = buildFullUrl(item);
        if (!full) return toast('URL 模板为空');
        const ok = await copyText(full);
        toast(ok ? '已复制完整 URL' : '复制失败');
        return;
      }

      if (btn.classList.contains('edit-one')) {
        editingId = id;
        render();
        return;
      }

      if (btn.classList.contains('del-one')) {
        const item = items.find(function (x) { return x.id === id; });
        if (!item) return;
        if (confirm('删除「' + (item.name || '未命名') + '」？')) {
          items = items.filter(function (x) { return x.id !== id; });
          selectedIds.delete(id);
          save();
          render();
          toast('已删除');
        }
        return;
      }

      if (btn.classList.contains('save-edit')) {
        const card = btn.closest('.item');
        const name = card.querySelector('.edit-name').value.trim() || '未命名';
        const template = normalizeTemplate(card.querySelector('.edit-template').value);
        const ck = card.querySelector('.edit-ck').value.trim();
        if (!template) return toast('URL 模板不能为空');
        const item = items.find(function (x) { return x.id === id; });
        if (!item) return;
        item.name = name;
        item.template = template;
        item.ck = ck;
        editingId = null;
        save();
        render();
        toast('已保存');
        return;
      }

      if (btn.classList.contains('cancel-edit')) {
        editingId = null;
        render();
      }
    });

    window.addEventListener('beforeunload', function () {
      clearTimeout(saveTimer);
      save();
    });
  }

  function addItem() {
    const nameInput = document.getElementById('newName');
    const templateInput = document.getElementById('newTemplate');
    const ckInput = document.getElementById('newCk');

    const template = normalizeTemplate(templateInput.value);
    if (!template) return toast('请填写 URL 模板');
    if (template.indexOf('{ck}') === -1) {
      if (!confirm('模板中没有 {ck} 占位符，完整 URL 将等于模板本身。仍要添加吗？')) return;
    }

    items.push({
      id: uid(),
      name: nameInput.value.trim() || ('URL ' + (items.length + 1)),
      template: template,
      ck: ckInput.value.trim()
    });
    save();
    nameInput.value = '';
    templateInput.value = '';
    ckInput.value = '';
    render();
    toast('已添加');
  }

  function batchAdd() {
    const textarea = document.getElementById('batchInput');
    const lines = textarea.value.split('\n').map(function (l) { return l.trim(); }).filter(Boolean);
    if (!lines.length) return toast('请粘贴内容');

    let added = 0;
    lines.forEach(function (line) {
      let name = '';
      let tpl = line;
      const sep = line.indexOf('|');
      if (sep !== -1) {
        name = line.slice(0, sep).trim();
        tpl = line.slice(sep + 1).trim();
      }
      const template = normalizeTemplate(tpl);
      if (!template) return;
      items.push({
        id: uid(),
        name: name || ('URL ' + (items.length + 1)),
        template: template,
        ck: ''
      });
      added++;
    });

    if (!added) return toast('没有有效内容');
    save();
    textarea.value = '';
    render();
    toast('已添加 ' + added + ' 条');
  }

  function exportJson() {
    if (!items.length) return toast('列表为空');
    const blob = new Blob([JSON.stringify(items, null, 2)], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'url-ck-manager-' + new Date().toISOString().slice(0, 10) + '.json';
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
    toast('已导出');
  }

  function importJson(e) {
    const file = e.target.files && e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = function () {
      try {
        const data = JSON.parse(reader.result);
        if (!Array.isArray(data)) throw new Error('JSON 格式应为数组');
        const valid = data
          .filter(function (x) { return x && typeof x.template === 'string'; })
          .map(function (x) {
            return {
              id: x.id || uid(),
              name: x.name || '未命名',
              template: normalizeTemplate(x.template),
              ck: x.ck || ''
            };
          });
        if (!valid.length) throw new Error('没有有效数据');

        let mode = 'merge';
        if (items.length) {
          mode = confirm('检测到已有数据。\n\n点「确定」= 合并导入\n点「取消」= 替换现有数据') ? 'merge' : 'replace';
        }
        items = mode === 'replace' ? valid : items.concat(valid);
        selectedIds.clear();
        save();
        render();
        toast('已导入 ' + valid.length + ' 条');
      } catch (err) {
        toast('导入失败：' + err.message);
      } finally {
        e.target.value = '';
      }
    };
    reader.readAsText(file);
  }

  load();
  bindEvents();
  render();
})();
</script>
</body>
</html>