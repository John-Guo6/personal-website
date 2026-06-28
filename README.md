# guozikang 个人导航网站

> 开发者 · 终身学习者

🔗 **在线地址：** https://objectstorage.ap-singapore-1.oraclecloud.com/n/ax9xub0hkknv/b/JohnG-personal/o/index.html

## 功能

- 🎓💼🌈🤖 四大分类（学校 / 工作 / 生活 / AI）
- 📁 自定义子分类 + ↑↓ 排序
- 🔗 外部链接 + 🐙 我的项目
- 🔒 名称/URL 全局去重
- 👁️ 名称显隐切换
- 💾 localStorage 持久化
- 🚀 GitHub Actions → Oracle Cloud 自动部署

## 本地开发

```bash
# 直接浏览器打开
open index.html

# 或启动本地服务器
python3 -m http.server 8765
```

## 部署

Push 到 `main` 分支即自动部署到 OCI Object Storage。

## 技术栈

纯静态 HTML + CSS + JS，零依赖。
