# 个人网站项目 — 记忆文档

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
