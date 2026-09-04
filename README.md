# Jiuxiao Hexo Blog

这是 九霄巡航小队 的 Hexo 博客源码仓库，主要用于编写、预览、构建和发布个人博客文章。

博客地址：<https://flightsquadyeah.github.io>

GitHub 仓库：<https://github.com/Flightsquadyeah/flightsquadyeah.github.io>

## 分支说明

本项目使用两个分支分别保存源码和生成后的网页：

| 分支 | 用途 |
| --- | --- |
| `source` | Hexo 源码、文章、主题配置和依赖配置。日常写文章应在此分支进行。 |
| `main` | Hexo 生成后的静态网页，由 `hexo deploy` 自动更新，用于 GitHub Pages 展示。 |

平时不要直接修改 `main` 分支中的网页文件。正确的流程是在 `source` 分支编写文章，然后生成并部署网站。

## 环境要求

建议安装以下工具：

- Node.js，建议使用长期支持版本；
- npm，随 Node.js 一起安装；
- Git；
- 一个具有该仓库读写权限的 GitHub 账号。

查看工具是否安装成功：

```powershell
node --version
npm --version
git --version
```

## 第一次在新电脑上使用

克隆源码分支：

```powershell
git clone --branch source https://github.com/Flightsquadyeah/flightsquadyeah.github.io.git
Set-Location flightsquadyeah.github.io
```

安装项目依赖：

```powershell
npm install
```

安装完成后，可以确认 Hexo 是否可用：

```powershell
npx hexo version
```

如果本机已经配置了 GitHub SSH，也可以使用 SSH 地址克隆：

```powershell
git clone --branch source git@github.com:Flightsquadyeah/flightsquadyeah.github.io.git
```

## 日常多端同步流程

### 开始写作前

每次打开项目后，先进入源码目录并同步远程最新内容：

```powershell
Set-Location D:\Lab\Blog\jiuxiao
git switch source
git pull --rebase
```

仓库已经配置了自动变基和自动暂存本地改动：

```text
pull.rebase=true
rebase.autostash=true
```

这样多台电脑先后写作时，Git 会优先整理提交历史，不会反复要求选择合并策略。

如果本地有尚未完成的文章修改，建议先保存为一次本地提交，再执行同步：

```powershell
git add source/_posts
git commit -m "WIP: save article draft"
git pull --rebase
```

### 写作完成后

提交文章并推送到源码分支：

```powershell
git add source/_posts
git commit -m "Add new article"
git push
```

如果修改了配置、主题或依赖，则使用：

```powershell
git add .
git commit -m "Update blog configuration"
git push
```

### 在另一台电脑继续写

在另一台电脑上，进入项目目录后执行：

```powershell
git pull --rebase
```

然后再编辑文章即可。

## 新建文章

使用 Hexo 命令新建文章：

```powershell
npx hexo new post "文章标题"
```

文章会生成在：

```text
source/_posts/
```

也可以直接创建 Markdown 文件，例如：

```text
source/_posts/DroneMotor.md
```

推荐使用以下 Front Matter 格式：

```markdown
---
title: 文章标题
date: 2026/9/4 18:30:00
tags:
  - drone
categories:
  - drone
excerpt: 文章摘要，会显示在文章列表或页面摘要位置。
---

正文从这里开始。
```

注意事项：

- `title` 是文章标题；
- `date` 决定文章时间和排序；
- `tags` 和 `categories` 建议使用列表格式；
- 文件名建议使用英文、数字或简短拼音，避免使用特殊字符；
- 图片等文章资源可以根据主题需求放在 `source` 目录中；
- 草稿可以暂时放在 `source/_drafts/`，不会默认发布。

## 本地预览

启动本地开发服务器：

```powershell
npm run server
```

或者：

```powershell
npx hexo server
```

默认访问地址：

<http://localhost:4000>

启动后修改文章，浏览器通常会自动刷新。按 `Ctrl+C` 可以停止本地服务器。

## 构建网站

清理 Hexo 缓存和旧的生成文件：

```powershell
npm run clean
```

生成静态网页：

```powershell
npm run build
```

生成结果会放在 `public/` 目录。`public/` 已被加入 `.gitignore`，不需要手动提交到源码分支。

如果只想快速检查文章是否有 Front Matter 或 Markdown 错误，可以执行：

```powershell
npx hexo generate
```

看到类似下面的输出，说明生成成功：

```text
INFO  files generated in ...
```

## 发布网站

本项目的部署配置位于 `_config.yml`，当前发布目标为：

```yaml
deploy:
  type: git
  repo: https://github.com/Flightsquadyeah/flightsquadyeah.github.io.git
  branch: main
```

推荐使用以下命令发布：

```powershell
npm run clean
npm run build
npm run deploy
```

也可以直接执行：

```powershell
npx hexo clean
npx hexo generate
npx hexo deploy
```

发布命令会把 `public/` 中的静态网页推送到远程 `main` 分支。源码仍然保存在 `source` 分支中。

发布前建议先确认：

```powershell
git status --short --branch
```

文章修改已经提交并推送到 `source` 后，再执行部署，可以避免源码和线上页面状态不一致。

## 常用命令速查

| 操作 | 命令 |
| --- | --- |
| 查看当前状态 | `git status` |
| 查看当前分支 | `git branch --show-current` |
| 同步源码 | `git pull --rebase` |
| 新建文章 | `npx hexo new post "文章标题"` |
| 本地预览 | `npm run server` |
| 清理缓存 | `npm run clean` |
| 生成网站 | `npm run build` |
| 发布网站 | `npm run deploy` |
| 提交文章 | `git add source/_posts; git commit -m "Add new article"; git push` |

## 常见问题

### Git 提示需要合并分支

先确认当前位于源码分支：

```powershell
git switch source
git pull --rebase
```

如果本地有未提交修改，先提交或暂存：

```powershell
git add .
git commit -m "Save local changes"
git pull --rebase
```

不建议直接使用 `git push --force`，以免覆盖其他电脑已经推送的文章。

### 两台电脑同时修改了同一个文件

如果两台电脑修改了同一篇文章，变基时可能出现冲突。Git 会列出冲突文件。处理步骤如下：

```powershell
git status
git add <已经解决的文件>
git rebase --continue
```

如果确认不想继续这次变基，可以取消：

```powershell
git rebase --abort
```

解决冲突时要同时保留需要的内容，然后重新检查文章和构建结果。

### 部署时无法连接 GitHub

如果出现以下类型的错误：

```text
Failed to connect to github.com port 443
```

通常是当前网络无法访问 GitHub。先确认网络、代理或 VPN 正常，再重新执行：

```powershell
npm run deploy
```

### 主题目录显示为修改状态

如果 `git status` 显示：

```text
m themes/alpha-dust+
```

说明主题目录自身包含独立的 Git 状态。不要为了消除提示而随意删除主题内容。只要没有修改主题源码，日常提交文章时可以只提交：

```powershell
git add source/_posts
```

如果确实修改了主题，需要另外确认主题仓库的提交和同步方式。

## 目录说明

```text
.
├── _config.yml              # Hexo 主配置和 GitHub Pages 部署配置
├── package.json             # 项目依赖和常用命令
├── package-lock.json        # npm 依赖锁定文件
├── source/_posts/           # 博客文章
├── scaffolds/               # 新建文章时使用的模板
├── themes/alpha-dust+/      # 当前使用的博客主题
├── public/                  # Hexo 生成的网页，不提交到源码分支
└── .deploy_git/             # Hexo 部署过程使用的临时目录
```

## 推荐工作流

完整的一次写作和发布流程如下：

```powershell
# 1. 进入项目并同步最新源码
Set-Location D:\Lab\Blog\jiuxiao
git switch source
git pull --rebase

# 2. 新建或编辑文章
npx hexo new post "文章标题"

# 3. 本地预览
npm run server

# 4. 检查通过后提交源码
git add source/_posts
git commit -m "Add new article"
git push

# 5. 发布到 GitHub Pages
npm run clean
npm run build
npm run deploy
```

源码分支负责协作和保存内容，`main` 分支负责展示生成结果。只要坚持先同步、再写作、后提交和发布，就可以在多台电脑之间稳定维护博客。
