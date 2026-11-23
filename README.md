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
    <input type="file" id="importFile" accept=".json,ap# sakmuefd.gitHub.io