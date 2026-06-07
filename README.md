# 我的个人博客

使用 Hugo + 自定义时间线瀑布流主题构建的个人博客。

## ✨ 特色

- 🎨 **时间线瀑布流设计**：左侧时间轴 + 右侧瀑布流卡片
- 📝 **Markdown 写作**：只需写 `.md` 文件，自动生成网站
- 🚀 **自动部署**：推送到 GitHub 自动构建并发布
- 📱 **响应式设计**：完美适配手机、平板、电脑

## 📝 如何写新文章

### 1. 创建文章文件

在 `content/posts/` 文件夹下新建 `my-article.md`：

```markdown
---
title: "文章标题"
date: 2026-06-07
draft: false
tags: ["标签1", "标签2"]
---

## 这是标题

这是正文内容，支持 **粗体**、*斜体*、[链接](url)。

### 子标题

- 列表项 1
- 列表项 2

代码块：
\`\`\`javascript
console.log('Hello World');
\`\`\`
```

### 2. 提交到 GitHub

**方式 A：网页上传**
1. 进入 GitHub 仓库
2. 进入 `content/posts/` 文件夹
3. 点击 `Add file` → `Upload files`
4. 上传你的 `.md` 文件
5. Commit changes

**方式 B：Git 命令**
```bash
git add content/posts/my-article.md
git commit -m "添加新文章"
git push
```

### 3. 等待部署

- GitHub Actions 自动构建（约 1-2 分钟）
- 访问你的博客查看新文章

## 🚀 首次部署步骤

### 1. 修改配置

编辑 `hugo.yaml`：

```yaml
baseURL: 'https://你的GitHub用户名.github.io/'
title: '你的博客标题'

params:
  github: 'https://github.com/你的用户名'
  email: 'your@email.com'
```

### 2. 创建 GitHub 仓库

- 仓库名：`你的用户名.github.io`
- 设置为 Public（公开）

### 3. 上传项目

**网页上传方式：**
1. 进入仓库页面
2. 点击 `uploading an existing file`
3. 把整个 `myblog-custom` 文件夹里的**所有文件**（不是文件夹本身）拖进去
4. Commit changes

**命令行方式：**
```bash
cd myblog-custom
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
git push -u origin main
```

### 4. 启用 GitHub Pages

1. 进入仓库 `Settings` → `Pages`
2. Source 选择 `GitHub Actions`
3. 保存

### 5. 等待部署

- 进入 `Actions` 标签查看构建状态
- 看到绿色 ✓ 表示成功
- 访问 `https://你的用户名.github.io`

## 📂 项目结构

```
myblog-custom/
├── content/              # 📝 内容文件夹
│   ├── posts/           # 博客文章（在这里写 .md）
│   │   ├── deploy-blog-on-linux.md
│   │   ├── github-pages-guide.md
│   │   └── ...
│   └── about.md         # 关于页面
├── themes/
│   └── custom/          # 🎨 自定义主题（你的 HTML 设计）
│       ├── layouts/
│       │   ├── _default/
│       │   │   ├── baseof.html    # 基础模板
│       │   │   ├── list.html      # 首页（瀑布流）
│       │   │   └── single.html    # 文章页
│       │   └── partials/
│       └── theme.toml
├── .github/
│   └── workflows/
│       └── hugo.yml     # 🚀 自动部署配置
├── hugo.yaml            # ⚙️ Hugo 配置文件
├── .gitignore
└── README.md
```

## 🎨 自定义样式

主题样式在 `themes/custom/layouts/_default/baseof.html` 的 `<style>` 标签内。

你可以修改：
- 配色方案（搜索 `#667eea`、`#764ba2` 等）
- 布局宽度（搜索 `max-width`）
- 瀑布流列数（搜索 `column-count`）

## 📊 文章示例

已包含 6 篇示例文章：

1. 如何在 Linux 上部署个人博客
2. GitHub Pages 完全指南
3. 前端开发最佳实践 2026
4. CSS 现代布局技术详解
5. 构建高性能 Web 应用
6. 我的开发工具箱推荐

可以参考它们的格式来写新文章，也可以直接删除。

## ❓ 常见问题

**Q: 部署后看不到网站？**
A: 等待 1-2 分钟，检查 Actions 是否成功（绿色 ✓）

**Q: 如何删除示例文章？**
A: 删除 `content/posts/` 里对应的 `.md` 文件，重新提交

**Q: 如何添加图片？**
A: 在文章中使用 Markdown 语法：`![描述](图片URL)`

**Q: 如何修改主题颜色？**
A: 编辑 `themes/custom/layouts/_default/baseof.html`，修改 CSS 里的颜色代码

**Q: 可以用自己的域名吗？**
A: 可以！在仓库根目录创建 `static/CNAME` 文件，内容为你的域名，然后在域名 DNS 设置 CNAME 记录指向 `你的用户名.github.io`

## 📄 许可

MIT License
