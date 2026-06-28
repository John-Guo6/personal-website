# 个人导航网站 实施计划

> **For agentic workers:** REQUIRED: Use $subagent-driven-development (if subagents available) or $executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 构建单文件个人导航网站，支持三分类（学校/工作/生活）标签切换 + 外部链接/GitHub项目卡片展示 + 增删改查 + localStorage 持久化。

**Architecture:** 单个 `index.html`，内嵌 CSS 和 JS。数据分预置数据（JS 对象）和用户数据（localStorage），页面加载时合并。标签切换通过 JS 控制显隐，卡片点击跳转新标签页。

**Tech Stack:** HTML5 + CSS3 + Vanilla JavaScript，零依赖，托管 GitHub Pages。

---

## Chunk 1: 页面骨架 + 基础样式

### Task 1: HTML 结构骨架

**Files:**
- Create: `index.html`

- [ ] **Step 1: 创建 HTML 基础结构**

写出完整的 HTML 骨架，包含头部（个人介绍）、标签导航、内容区（三个面板，每个面板含外部链接和我的项目两个子区域）、页脚。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>guozikang</title>
</head>
<body>
  <header><!-- 个人介绍 --></header>
  <nav><!-- 标签导航 --></nav>
  <main>
    <section id="tab-school">
      <div class="sub-section" id="school-links"><h3>🔗 外部链接</h3><div class="card-list"></div></div>
      <div class="sub-section" id="school-projects"><h3>🐙 我的项目</h3><div class="card-list"></div></div>
    </section>
    <section id="tab-work"><!-- 同上结构 --></section>
    <section id="tab-life"><!-- 同上结构 --></section>
  </main>
  <footer><!-- © 2026 guozikang --></footer>
</body>
</html>
```

- [ ] **Step 2: 在浏览器打开 index.html 验证骨架**

打开 `index.html`，确认页面结构渲染正常。

---

### Task 2: CSS 暗色主题 + 卡片样式

**Files:**
- Modify: `index.html`（在 `<style>` 中添加）

- [ ] **Step 1: 添加 CSS 变量和全局样式**

```css
:root {
  --bg-primary: #0d1117;
  --bg-secondary: #161b22;
  --border: #30363d;
  --text-primary: #c9d1d9;
  --text-secondary: #8b949e;
  --text-muted: #484f58;
  --accent-blue: #58a6ff;
  --accent-green: #3fb950;
  --accent-red: #f85149;
  --tab-active: #1f6feb;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  background: var(--bg-primary);
  color: var(--text-primary);
  font-family: 'SF Mono', 'Fira Code', 'Cascadia Code', monospace;
  min-height: 100vh;
}
```

- [ ] **Step 2: 头部、标签导航、内容区布局**

标签扁平排列居中。当前激活标签背景蓝色 `.tab.active { background: var(--tab-active); color: #fff; }`。内容面板默认 `display:none`，激活时 `display:block`。设置 `max-width: 720px; margin: 0 auto;`。

- [ ] **Step 3: 卡片样式**

```css
.card {
  display: flex; align-items: center; gap: 12px;
  background: var(--bg-secondary); border: 1px solid var(--border);
  border-radius: 8px; padding: 12px 16px; margin-bottom: 8px;
  text-decoration: none; color: inherit; transition: all 0.2s;
}
.card:hover { border-color: var(--accent-blue); box-shadow: 0 2px 8px rgba(88,166,255,0.15); }
.card-icon { font-size: 18px; flex-shrink: 0; }
.card-body { flex: 1; min-width: 0; }
.card-name { display: block; font-size: 14px; color: var(--text-primary); }
.card-url { display: block; font-size: 11px; color: var(--text-secondary); margin-top: 2px; }
.card-tag {
  font-size: 11px; padding: 2px 8px; border-radius: 4px; background: #21262d;
  color: var(--text-secondary); flex-shrink: 0;
}
.card-tag.project { color: var(--accent-green); }
.card-arrow { color: var(--text-secondary); font-size: 16px; flex-shrink: 0; }
```

关键区分：
- 外部链接卡片：`🔗` 图标 + `↗` 箭头
- 项目卡片：`🐙` 图标 + 绿色 `GitHub` 标签，无箭头

- [ ] **Step 4: 子区域标题样式**

```css
.sub-section { margin-bottom: 24px; }
.sub-section h3 { font-size: 15px; margin-bottom: 12px; color: var(--text-primary); }
.sub-section.empty { display: none; }
```

- [ ] **Step 5: 空状态提示样式**

```css
.empty-state {
  text-align: center; padding: 40px 20px; color: var(--text-muted); font-size: 14px;
}
```

- [ ] **Step 6: 浏览器验证**

确认暗色主题、卡片样式、外部链接和项目卡片的视觉差异。

---

## Chunk 2: 标签切换 + 数据渲染

### Task 3: 标签切换逻辑

**Files:**
- Modify: `index.html`（`<script>` 中）

- [ ] **Step 1: 实现标签点击切换函数**

```javascript
function switchTab(category) {
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.querySelector(`.tab[data-category="${category}"]`).classList.add('active');
  document.querySelectorAll('.tab-panel').forEach(p => p.classList.remove('active'));
  document.getElementById(`tab-${category}`).classList.add('active');
  currentTab = category; // 供添加表单默认选中
}
```

- [ ] **Step 2: 绑定标签点击事件**

- [ ] **Step 3: 浏览器验证**

点击三个标签，确认切换正常、高亮正确。

---

### Task 4: 数据模型 + 卡片渲染

**Files:**
- Modify: `index.html`（`<script>` 中）

- [ ] **Step 1: 定义预置数据对象**

```javascript
const defaultData = {
  school: { links: [], projects: [] },
  work:   { links: [], projects: [] },
  life:   { links: [], projects: [] },
};
let currentData = JSON.parse(JSON.stringify(defaultData));
let currentTab = 'school';
```

- [ ] **Step 2: 实现 `renderCard(item, type)` 函数**

根据 `type` 参数（`'link'` 或 `'project'`）渲染不同模板：

外部链接卡片：
```javascript
function renderCard(item, type) {
  const icon = type === 'project' ? '🐙' : '🔗';
  const tagHtml = type === 'project'
    ? '<span class="card-tag project">GitHub</span>'
    : '';
  const arrowHtml = type === 'link' ? '<span class="card-arrow">↗</span>' : '';
  return `
    <a class="card" href="${item.url}" target="_blank" data-id="${item.id}">
      <span class="card-icon">${icon}</span>
      <div class="card-body">
        <span class="card-name">${item.name}</span>
        <span class="card-url">${item.url}</span>
      </div>
      ${tagHtml}
      ${arrowHtml}
    </a>`;
}
```

- [ ] **Step 3: 实现 `renderCategory(category)` 函数**

遍历 `currentData[category]`，分别渲染 links 和 projects。如果数组为空，给对应 `.sub-section` 加 `.empty` 类隐藏。如果整个 category 的 links 和 projects 都为空，显示空状态提示。

```javascript
function renderCategory(category) {
  const data = currentData[category];
  // 渲染 links
  const linksSection = document.getElementById(`${category}-links`);
  const linksList = linksSection.querySelector('.card-list');
  if (data.links.length === 0) {
    linksSection.classList.add('empty');
  } else {
    linksSection.classList.remove('empty');
    linksList.innerHTML = data.links.map(item => renderCard(item, 'link')).join('');
  }
  // 渲染 projects（同理，type 传 'project'）
  // 两个都为空时显示 empty-state
}
```

- [ ] **Step 4: 页面加载时初始化渲染**

`DOMContentLoaded` 时：加载数据 → 渲染三个分类 → 默认显示"学校"标签页。

- [ ] **Step 5: 浏览器验证**

确认卡片图标/标签/箭头区分正确，空区域隐藏，全空显示提示。

---

## Chunk 3: 添加/编辑/删除功能

### Task 5: 浮动按钮 + 模态表单

**Files:**
- Modify: `index.html`（HTML + CSS + JS）

- [ ] **Step 1: 添加浮动按钮 HTML + CSS**

```html
<button id="add-btn" class="fab" title="添加链接">+</button>
```
```css
.fab {
  position: fixed; bottom: 24px; right: 24px;
  width: 52px; height: 52px; border-radius: 50%;
  background: var(--tab-active); color: #fff; font-size: 28px;
  border: none; cursor: pointer;
  box-shadow: 0 4px 16px rgba(31,111,235,0.4);
  display: flex; align-items: center; justify-content: center;
  transition: transform 0.2s;
}
.fab:hover { transform: scale(1.1); }
```

- [ ] **Step 2: 模态表单 HTML**

```html
<div id="modal-overlay" class="modal-overlay hidden">
  <div class="modal">
    <h3 id="modal-title">➕ 添加链接</h3>
    <label for="modal-category">所属分类</label>
    <select id="modal-category">
      <option value="school">🎓 学校</option>
      <option value="work">💼 工作</option>
      <option value="life">🌈 生活</option>
    </select>
    <label for="modal-type">类型</label>
    <select id="modal-type">
      <option value="links">🔗 外部链接</option>
      <option value="projects">🐙 我的项目</option>
    </select>
    <label for="modal-name">名称</label>
    <input id="modal-name" type="text" placeholder="链接名称" required>
    <label for="modal-url">URL</label>
    <input id="modal-url" type="text" placeholder="https://..." required>
    <div class="modal-actions">
      <button id="modal-cancel" type="button">取消</button>
      <button id="modal-save" type="button">保存</button>
    </div>
  </div>
</div>
```

- [ ] **Step 3: 模态 CSS**

遮罩 `position:fixed; inset:0; background:rgba(0,0,0,0.7); display:flex; align-items:center; justify-content:center`。表单卡片 `background:var(--bg-secondary); border:1px solid var(--border); border-radius:12px; padding:24px; width:400px; max-width:calc(100% - 32px)`。`.hidden { display: none; }`。输入框和下拉框沿用暗色主题样式。

- [ ] **Step 4: JS — 打开/关闭模态**

点击 `+`：移除 `.hidden`，设置 `modal-category` 为 `currentTab`，标题"➕ 添加链接"，表单清空，不设置 `editingId`。点击取消或遮罩：加 `.hidden`。

- [ ] **Step 5: JS — 保存逻辑**

点击保存：收集表单数据，生成唯一 ID（`Date.now().toString(36)`），构建条目 `{ id, name, url, addedByUser: true }`，push 到 `currentData[category][type]`，调用 `saveData()`，重新渲染当前分类，关闭模态。

- [ ] **Step 6: 浏览器验证**

添加外链 → 确认卡片带 🔗↗。添加项目 → 确认带 🐙+绿色 GitHub 标签。取消 → 确认表单关闭不保存。

---

### Task 6: 编辑 + 删除功能

**Files:**
- Modify: `index.html`（HTML + CSS + JS）

- [ ] **Step 1: 卡片渲染中嵌入操作按钮**

修改 `renderCard` 函数，在模板末尾（`</a>` 前）插入操作按钮：

```html
<div class="card-actions">
  <button class="btn-edit" data-id="${item.id}">✏️</button>
  <button class="btn-delete" data-id="${item.id}">🗑️</button>
</div>
```

- [ ] **Step 2: 操作按钮 CSS**

```css
.card-actions { opacity: 0; display: flex; gap: 4px; transition: opacity 0.2s; }
.card:hover .card-actions { opacity: 1; }
.btn-edit, .btn-delete {
  background: none; border: none; cursor: pointer; font-size: 14px; padding: 2px 4px;
}
.btn-edit { color: var(--accent-blue); }
.btn-delete { color: var(--accent-red); }
```

- [ ] **Step 3: 事件委托 — 区分卡片跳转和操作按钮**

在 `.card-list` 上绑定事件委托。如果点击目标是 `.btn-edit`：阻止默认跳转，获取 `data-id`，弹出编辑表单。如果是 `.btn-delete`：阻止默认，确认后删除。

- [ ] **Step 4: JS — 编辑逻辑**

```javascript
function editItem(id) {
  // 从 currentData 中找到对应条目
  // 设置 editingId = id
  // 弹出模态，标题"✏️ 编辑链接"，预填分类/类型/名称/URL
  // 保存时：更新条目数据（保留 addedByUser），如果编辑的是预置条目（!item.addedByUser），则设置 addedByUser = true 以确保修改持久化），重新渲染，saveData()
}
```

- [ ] **Step 5: JS — 删除逻辑**

```javascript
function deleteItem(id) {
  if (!confirm('确定删除这条链接？')) return;
  // 从 currentData 中找到并移除
  // saveData(); renderCategory(currentTab);
}
```

- [ ] **Step 6: JS — 保存时区分新增和编辑**

`handleSave()` 中检查 `editingId`：如果有则更新已有条目，否则新增条目。保存后清空 `editingId`。

- [ ] **Step 7: 浏览器验证**

添加 → 悬浮显示 ✏️🗑️ → 编辑 → 保存确认修改。删除 → 确认 → 消失。如果删除后子区域为空，自动隐藏。

---

## Chunk 4: 持久化 + 响应式 + 收尾

### Task 7: localStorage 持久化

**Files:**
- Modify: `index.html`（JS）

- [ ] **Step 1: 实现 `saveData()`**

每次增删改后调用，将 `currentData` 序列化存入 localStorage。

```javascript
function saveData() {
  localStorage.setItem('siteData', JSON.stringify(currentData));
}
```

- [ ] **Step 2: 实现 `loadData()` 和 `mergeData()`**

```javascript
function loadData() {
  const saved = localStorage.getItem('siteData');
  if (!saved) return JSON.parse(JSON.stringify(defaultData));
  const parsed = JSON.parse(saved);
  return mergeData(defaultData, parsed);
}

function mergeData(defaults, saved) {
  // 策略：以 defaults 为基础，将 saved 中各分类的 links/projects 合并进来
  const result = JSON.parse(JSON.stringify(defaults));
  for (const cat of ['school', 'work', 'life']) {
    for (const type of ['links', 'projects']) {
      const savedItems = saved[cat]?.[type] || [];
      // 只保留 addedByUser === true 的用户添加条目
      // 预置条目始终使用 defaults 中的版本（防止旧缓存覆盖更新的预置数据）
      const userItems = savedItems.filter(item => item.addedByUser === true);
      result[cat][type] = [...result[cat][type], ...userItems];
    }
  }
  return result;
}
```

- [ ] **Step 3: 页面初始化调用**

```javascript
currentData = loadData();
renderAll();
```

- [ ] **Step 4: 浏览器验证**

添加链接 → 刷新 → 确认保留。手动清除 localStorage → 刷新 → 确认回到预置数据。

---

### Task 8: 响应式适配

**Files:**
- Modify: `index.html`（CSS）

- [ ] **Step 1: 添加移动端媒体查询**

```css
@media (max-width: 640px) {
  body { font-size: 14px; }
  header { padding: 16px; }
  nav {
    overflow-x: auto; white-space: nowrap; -webkit-overflow-scrolling: touch;
    justify-content: flex-start; padding: 8px 16px;
  }
  .tab { padding: 8px 16px; font-size: 13px; }
  .tab-panel { padding: 16px; }
  .card { padding: 10px 14px; }
  .card-actions { opacity: 1; } /* 移动端始终显示操作按钮 */
  .modal { width: calc(100% - 32px); padding: 20px; }
  .fab { bottom: 16px; right: 16px; width: 48px; height: 48px; font-size: 24px; }
}
```

- [ ] **Step 2: 浏览器验证**

调整浏览器宽度到 400px，确认标签可横向滚动、卡片占满宽度、模态框适配、操作按钮始终可见。

---

### Task 9: 预置数据填充 + 最终验证

**Files:**
- Modify: `index.html`（`defaultData` 对象）

- [ ] **Step 1: 暂用示例数据填充**

在用户提供实际数据前，填入一组示例数据用于验证：

```javascript
const defaultData = {
  school: {
    links: [
      { id: 's1', name: '教务管理系统', url: 'https://example.edu.cn/jwc', addedByUser: false },
      { id: 's2', name: '学校图书馆',   url: 'https://example.edu.cn/lib', addedByUser: false },
    ],
    projects: [
      { id: 'sp1', name: 'course-helper', url: 'https://github.com/guozikang/course-helper', addedByUser: false },
    ],
  },
  work: {
    links: [],
    projects: [],
  },
  life: {
    links: [],
    projects: [],
  },
};
```

- [ ] **Step 2: 全功能验证清单**

逐项确认：
- [ ] 三个标签页正常切换，高亮正确
- [ ] 外部链接卡片显示 🔗 + ↗，点击新标签打开
- [ ] GitHub 项目卡片显示 🐙 + 绿色 GitHub 标签
- [ ] 空子区域标题自动隐藏（工作/生活标签应隐藏子区域，因数据为空）
- [ ] 全空时显示空状态提示
- [ ] 浮动 `+` 按钮固定在右下角
- [ ] 添加表单：分类默认选中当前标签页
- [ ] 添加后卡片立即出现，刷新后保留
- [ ] 编辑：预填数据正确，保存更新
- [ ] 删除：确认后移除，空区域自动隐藏
- [ ] 清除 localStorage 后回到预置数据
- [ ] 移动端（<640px）布局正常

- [ ] **Step 3: 用户替换实际数据后最终检查**
