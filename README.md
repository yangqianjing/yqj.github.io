# 我的个人博客

使用 Jekyll 和 GitHub Pages 搭建的个人博客。

## 快速开始

### 本地运行

1. 安装 Ruby 和 Jekyll：
```bash
# Windows 用户推荐使用 RubyInstaller
# 下载地址：https://rubyinstaller.org/

# macOS/Linux 用户
gem install bundler jekyll
```

2. 安装依赖：
```bash
bundle install
```

3. 启动本地服务器：
```bash
bundle exec jekyll serve
```

4. 访问 `http://localhost:4000`

## 部署到 GitHub Pages

### 方法 1：使用 GitHub Actions（推荐）

1. 在 GitHub 创建新仓库，命名为 `yourusername.github.io`
2. 将代码推送到仓库
3. 在仓库设置中启用 GitHub Pages，选择 `main` 分支
4. 等待几分钟后访问 `https://yourusername.github.io`

### 方法 2：手动部署

```bash
# 初始化 Git 仓库
git init
git add .
git commit -m "Initial commit"

# 添加远程仓库并推送
git remote add origin https://github.com/yourusername/yourusername.github.io.git
git branch -M main
git push -u origin main
```

## 配置

编辑 `_config.yml` 文件，修改以下内容：

- `title`: 博客标题
- `email`: 你的邮箱
- `description`: 博客描述
- `url`: 你的 GitHub Pages 网址
- `github_username`: 你的 GitHub 用户名

## 写作

在 `_posts` 目录下创建新文件，文件名格式：`YYYY-MM-DD-title.md`

文章开头需要包含以下信息：

```markdown
---
layout: post
title: "文章标题"
date: 2026-06-07 12:00:00 +0800
categories: 分类
tags: [标签1, 标签2]
---

文章内容...
```

## 自定义主题

默认使用 Minima 主题。更换主题：

1. 在 `_config.yml` 中修改 `theme` 字段
2. 在 `Gemfile` 中添加对应的主题 gem
3. 运行 `bundle install`

更多主题：
- [Jekyll Themes](http://jekyllthemes.org/)
- [GitHub Pages Themes](https://pages.github.com/themes/)

## 目录结构

```
my-blog/
├── _config.yml      # 配置文件
├── _posts/          # 博客文章
├── _layouts/        # 页面布局（可选）
├── _includes/       # 可复用组件（可选）
├── assets/          # 静态资源（可选）
├── about.md         # 关于页面
├── index.html       # 首页
├── Gemfile          # Ruby 依赖
└── README.md        # 说明文档
```

## 常见问题

### 1. 本地预览时样式不正常
确保 `_config.yml` 中 `baseurl` 为空：
```yaml
baseurl: ""
```

### 2. GitHub Pages 部署后 404
- 确认仓库名为 `yourusername.github.io`
- 检查 GitHub Pages 设置中的分支选择

### 3. 文章不显示
- 检查文件名格式是否正确
- 确认文章日期不是未来日期

## 参考资源

- [Jekyll 官方文档](https://jekyllrb.com/)
- [GitHub Pages 文档](https://docs.github.com/pages)
- [Markdown 语法](https://www.markdownguide.org/)

---

祝你写作愉快！🎉
