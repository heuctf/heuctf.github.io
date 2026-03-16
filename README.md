# HEUCTF Wiki

> 哈尔滨工程大学网络安全社团知识库

[![VuePress](https://img.shields.io/badge/VuePress-2.0.0--rc.26-4FC08D?logo=vue.js)](https://vuepress.vuejs.org/)
[![Theme](https://img.shields.io/badge/vuepress--theme--plume-1.0.0--rc.192-blue)](https://theme-plume.vuejs.press/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

## 简介

这是哈尔滨工程大学网络安全社团（HEUCTF）的官方知识库，用于存储和分享社团的学习资料、技术文档、博客文章等内容。

知识库涵盖 Web 安全、逆向工程、密码学、二进制安全、misc 等多个方向的技术文章和 Writeup。

## 技术栈

- **静态站点生成器**：[VuePress 2](https://vuepress.vuejs.org/)
- **主题**：[vuepress-theme-plume](https://theme-plume.vuejs.press/)
- **构建工具**：Vite
- **部署平台**：GitHub Pages

## 快速开始

### 环境要求

- Node.js `^20.19.0` 或 `>=22.0.0`
- pnpm / npm / yarn

### 安装依赖

```bash
npm install
```

### 本地开发

```bash
# 启动开发服务器
npm run docs:dev

# 清理缓存后启动
npm run docs:dev-clean
```

### 构建部署

```bash
# 构建生产包
npm run docs:build

# 本地预览构建结果
npm run docs:preview
```

## 项目结构

```
heuctf.github.io/
├── docs/                      # 文档源文件目录
│   ├── .vuepress/             # VuePress 配置目录
│   │   ├── config.ts          # 站点配置
│   │   ├── plume.config.ts    # 主题配置
│   │   ├── navbar.ts          # 导航栏配置
│   │   └── collections.ts     # 文档集合配置
│   ├── blog/                  # 博客文章目录
│   ├── demo/                  # 示例文档目录
│   └── README.md              # 首页
├── package.json
└── README.md
```

## 贡献指南

欢迎社团成员为本知识库贡献内容！

### 添加新文章

1. **博客文章**：在 `docs/blog/` 目录下创建 `.md` 文件

   ```markdown
   ---
   title: 文章标题
   date: 2026-03-16
   tags:
     - Web
     - CTF
   ---

   文章内容...
   ```

2. **笔记/文档**：在 `docs/` 下创建新目录，并在 `collections.ts` 中配置

### 文章规范

- 使用中文撰写，技术术语可保留英文
- 文件名使用英文小写，单词间用 `-` 连接
- 每篇文章应包含 `title`、`date`、`tags` 等 Front Matter
- 代码块标注语言类型，如 ` \`\`\`python `

### 提交代码

```bash
# 创建新分支
git checkout -b feature/new-article

# 提交更改
git add .
git commit -m "docs: 添加 XXX 文章"

# 推送分支
git push origin feature/new-article
```

然后创建 Pull Request 等待审核。

## 部署说明

本项目使用 GitHub Actions 自动部署到 GitHub Pages。

### 首次部署配置

1. **Actions 权限**：`Settings > Actions > General`，在 `Workflow permissions` 下勾选 `Read and write permissions`

2. **Pages 设置**：`Settings > Pages`，`Source` 选择 `Deploy from a branch`，`Branch` 选择 `gh-pages`

3. **Base 路径**：由于部署在 `heuctf.github.io`，`base` 已默认配置为 `/`

推送代码到 `main` 分支后，GitHub Actions 会自动构建并部署。

## 相关链接

- [VuePress 文档](https://vuepress.vuejs.org/)
- [vuepress-theme-plume 文档](https://theme-plume.vuejs.press/)
- [社团 GitHub](https://github.com/heuctf)

## License

[MIT](LICENSE)

---

<p align="center">Made with ❤️ by HEUCTF</p>
