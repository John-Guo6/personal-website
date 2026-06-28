# AGENTS.md — 本项目的开发教训

> 每一条都是从实际 Bug 中总结的。新功能开发前请先读一遍。

---

## 1. Python 脚本替换代码的安全规则

**禁止使用：**
```python
content[:idx] + replacement + content[end_idx:]
```
这种方式容易因为 `idx` / `end_idx` 计算偏差，导致大段代码被误删。

**正确做法：**
- 用精确的字符串匹配替换 (`content.replace(old, new)`)
- `old` 必须是完整、唯一的代码片段
- 替换后立即 `grep` 验证所有函数定义都在

---

## 2. HTML ID 禁止重复

同一个 HTML 文件内，**任何两个元素不能有相同的 `id`**。

浏览器 `getElementById` 只返回第一个匹配的元素，第二个永远找不到。

本案：`<select id="modal-subcat">` 和 `<div id="modal-subcat">` 冲突，弹窗打不开。

**命名规范：**
- 下拉框等表单元素：`modal-subcat`
- 弹窗容器：`modal-subcat-editor`

---

## 3. data-* 属性与 JS 数据结构键名必须一致

如果数据用 `sc.links` / `sc.projects`（复数），那么 `data-type` 也必须是 `'links'` / `'projects'`。

**错误：** `data-type="link"` → `sourceSc['link']` → `undefined` → `Cannot read properties of undefined (reading 'findIndex')`

---

## 4. 拖拽（Drag & Drop）的可靠实现模式

| 做 | 不做 |
|----|------|
| Panel 级事件委托 (`addEventListener` on tab-panel) | Inline handler (`ondrop="..."`) |
| `e.target.closest('.card')` / `closestSubcat()` | 依赖 `this` / `e.currentTarget` |
| `e.preventDefault()` 在 `dragover` 中**总是**先调用 | 条件式 `preventDefault`（某些浏览器会拒绝 drop）|
| 只有手柄 `⠿` `draggable="true"` | 可点击的 `<a>` 标签上加 `draggable="true"` |
| 卡片 `<a>` 加 `draggable="false"` | 裸 `<a href>` 无 draggable 属性（有默认拖拽行为）|

**不要用 `dragleave` 清除高亮** — 子元素会触发误清除。用 `dragend` / `drop` 统一清除。

---

## 5. 每次代码修改后的验证清单

- [ ] `grep -n 'function ' index.html` — 确认所有函数都在
- [ ] `node -e "new Function(script)"` — JS 语法检查
- [ ] 浏览器 F12 Console — 无红色报错
- [ ] 人工测试改动的功能至少一次
- [ ] `git diff --stat` — 只改了预期的文件

---

## 6. 数据结构变更时的兼容性

- localStorage key 升级时保留旧 key 的回退读取
- `mergeData` 只合并 `addedByUser: true` 的项（预设项不覆盖用户数据）
- 预设项（`addedByUser: false`）在 `mergeData` 中每次从 `defaultData` 恢复，不可真正删除

---

## 错误记录

| # | 日期 | 错误表象 | 根因 | 修复方式 |
|---|------|---------|------|---------|
| 1 | 2026-06-29 | 子分类弹窗点击无反应 | `<select id="modal-subcat">` 和 `<div id="modal-subcat">` ID 重复，getElementById 只返回第一个 | 弹窗改名 `modal-subcat-editor` |
| 2 | 2026-06-29 | 新增/编辑/导出/导入全部失效，21个函数丢失 | Python `content[:idx]+replacement+content[end_idx:]` 替换范围过大，误删中间所有函数 | 精准字符串匹配替换，替换后立即 grep 验证 |
| 3 | 2026-06-29 | `renderAll is not defined` 页面初始化崩溃 | `renderAll` 在误删范围内，被3处调用但无定义 | 补回 renderAll 函数定义 |
| 4 | 2026-06-29 | 拖拽 hover 高亮闪烁，无法放入子分类 | `dragleave` 在进入子元素时误触发清除高亮 | 去掉 dragleave，仅用 dragend/drop 清除 |
| 5 | 2026-06-29 | 拖拽 drop 时报 `Cannot read properties of undefined (reading 'findIndex')` | `data-type` 存单数 `'link'`，但数组键名是复数 `sc.links` | `renderCard` 统一使用复数 `'links'/'projects'` |
| 6 | 2026-06-29 | `<a>` 标签默认拖拽行为干扰 HTML5 DnD | `<a href>` 有原生链接拖拽，与 `draggable="true"` 冲突 | 卡片加 `draggable="false"`，仅 `⠿` 手柄可拖 |
| 7 | 2026-06-29 | 拖拽 re-render 后 dragstart 监听器丢失 | `addEventListener` 绑在卡片元素上，innerHTML 替换后销毁 | 改用 panel 级事件委托，`e.target.closest('.card')` 查找 |
