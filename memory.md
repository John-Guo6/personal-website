# 个人网站项目 — 记忆文档

## 代理规则

| # | 触发条件 | 动作 |
|---|---------|------|
| 1 | 重要变更（新功能、改配置、开始计划等） | 自动更新本文档并推送 |
| 2 | 上下文压缩 / 会话结束前 | 询问是否需要更新本文档 |
| 3 | 启动 brainstorming/plan 模式或使用 superpowers 等 skill | 自动记录到本文档 |

---

## 目标

搭建个人导航网站，集中管理常用网站书签和 GitHub 项目链接。

## 关键决策

| 项目 | 决策 |
|------|------|
| 技术 | 纯静态 HTML/CSS/JS，单文件 `index.html` |
| 设计 | 暗色主题、极简程序员风、等宽字体 |
| 结构 | 四大分类（学校/工作/生活/AI），顶部标签页切换 |
| 子分类 | 每个标签页下分 🔗外部链接 + 🐙我的项目 |
| 交互 | 浮动 + 按钮添加、卡片悬浮显示编辑/删除按钮 |
| 持久化 | localStorage，预置数据 + 用户数据合并 |
| 托管 | Oracle Cloud Object Storage（ap-singapore-1） |
| CI/CD | GitHub Actions 自动部署，每次 push 到 main 触发 |

## 文件清单

| 文件 | 说明 |
|------|------|
| `index.html` | 网站主文件 |
| `memory.md` | 本文档，记录目标和计划 |
| `.github/workflows/deploy.yml` | GitHub Actions 自动部署配置 |
| `docs/superpowers/specs/2026-06-28-personal-website-design.md` | 设计文档 |
| `docs/superpowers/plans/2026-06-28-personal-website.md` | 实施计划 |

## 当前数据

### 学校
- 🔗 教务管理系统、学校图书馆、Google Drive、IELTS、Lingnan Portal
- 🐙 course-helper

### 工作（空）

### 生活（空）

### AI（空）

## 在线地址

https://objectstorage.ap-singapore-1.oraclecloud.com/n/ax9xub0hkknv/b/JohnG-personal/o/index.html

## GitHub

https://github.com/John-Guo6/personal-website

## 已完成的里程碑
- [x] 名称URL去重 + 名称显隐 + 自定义子分类排序 (2026-06-29)

- [x] 网站搭建（HTML/CSS/JS 单文件）
- [x] 暗色主题 + 卡片式布局
- [x] 增删改查功能 + localStorage 持久化
- [x] 数据合并（用户添加的 Google Drive、IELTS、Lingnan Portal 写入 defaultData）
- [x] 添加 AI 分类（四大分类）
- [x] OCI Object Storage 部署
- [x] GitHub Actions 自动部署

## 待办

- [ ] 填写工作、生活、AI 分类的链接
- [ ] 将本地脚本上传至 GitHub 后添加到项目区

## 会话记录
| 2026-06-29 | Default | 新增三大功能：名称/URL去重 + 名称显隐 + 自定义子分类排序 |

| 日期 | 模式 / Skill | 重要变更 |
|------|-------------|---------|
| 2026-06-28 | $brainstorming + $writing-plans | 初始设计 + 实施计划 |
| 2026-06-28 | $using-superpowers | 工作流初始化 |
| 2026-06-28 | - | 搭建完整网站（Chunk 1-4） |
| 2026-06-28 | - | OCI Object Storage 部署 |
| 2026-06-28 | - | GitHub Actions 自动部署 |
| 2026-06-28 | - | 添加 AI 分类 + 记忆规则 |
| 2026-06-28 | $using-superpowers | 会话启动，检查 memory-recorder 触发 |

## 功能二 - 新增功能（2026-06-29）

### 名称 + URL 去重
- 全局跨分类去重
- 名称重复 → 自动生成编号（笔趣阁→笔趣阁1→笔趣阁2）
- 二次确认弹窗：显示自动生成名称，确认使用或自行填写
- URL 重复 → 警告"你已添加该网站"

### 名称显隐
- 新增链接/项目时可选择是否展示名称（默认展示）
- 卡片上 👁️ 眼睛图标切换显示/隐藏
- 隐藏时名称显示为 `***`

### 自定义子分类
- 每个大分类下可自建子分类（默认"默认"子分类不可删除/重命名）
- 子分类支持 ↑↓ 排序
- 链接/项目挂在子分类下

### 数据迁移
- 旧格式（V1: `{links, projects}`）自动迁移到新格式（V2: `{subcategories: [...]}`）
- localStorage key 从 `siteData` 迁移到 `siteDataV2`
