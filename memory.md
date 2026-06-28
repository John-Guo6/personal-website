# 个人网站项目 — 记忆文档

## 目标

搭建个人导航网站，集中管理常用网站书签和 GitHub 项目链接。

## 关键决策

| 项目 | 决策 |
|------|------|
| 技术 | 纯静态 HTML/CSS/JS，单文件 `index.html` |
| 设计 | 暗色主题、极简程序员风、等宽字体 |
| 结构 | 三大分类（学校/工作/生活），顶部标签页切换 |
| 子分类 | 每个标签页下分 🔗外部链接 + 🐙我的项目 |
| 交互 | 浮动 + 按钮添加、卡片悬浮显示编辑/删除按钮 |
| 持久化 | localStorage，预置数据 + 用户数据合并 |
| 托管 | GitHub Pages |

## 文件清单

| 文件 | 说明 |
|------|------|
| `index.html` | 网站主文件（634行，HTML+CSS+JS） |
| `memory.md` | 本文档，记录目标和计划 |
| `docs/superpowers/specs/2026-06-28-personal-website-design.md` | 设计文档 |
| `docs/superpowers/plans/2026-06-28-personal-website.md` | 实施计划 |

## 待办

- [ ] 用户填充实际链接数据（替换示例数据）
- [ ] 用户提供个人介绍标签内容
- [ ] 将本地脚本上传至 GitHub 后添加到项目区
- [ ] 部署到 GitHub Pages

## 数据编辑指南

修改 `index.html` 中 `defaultData` 对象即可更新预置链接：

```javascript
const defaultData = {
  school: {
    links: [
      { id: 's1', name: '名称', url: 'https://...', addedByUser: false },
    ],
    projects: [
      { id: 'sp1', name: '项目名', url: 'https://github.com/...', addedByUser: false },
    ],
  },
  work: { links: [], projects: [] },
  life: { links: [], projects: [] },
};
```

用户通过网页添加的链接自动存入浏览器 localStorage。
