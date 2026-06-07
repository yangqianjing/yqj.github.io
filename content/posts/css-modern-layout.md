---
title: "CSS 现代布局技术详解"
date: 2026-05-28
draft: false
tags: ["设计", "CSS"]
---

## CSS Grid 布局

Grid 是最强大的二维布局系统。

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
```

## Flexbox 布局

适合一维布局场景。

```css
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

## 容器查询

根据容器大小而非视口大小调整样式。

```css
@container (min-width: 400px) {
  .card {
    display: grid;
  }
}
```

## 瀑布流布局

使用 CSS Column 实现：

```css
.waterfall {
  column-count: 3;
  column-gap: 1.5rem;
}
```

## 总结

现代 CSS 提供了丰富的布局工具，选择合适的方案能大幅提升开发效率。
