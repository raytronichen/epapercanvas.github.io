# 当前仓库配置说明（Tech 栏目）

## 仓库信息

- **仓库名称**：`raytronichen.github.io`
- **用途**：Tech 栏目（技术文章）
- **绑定域名**：`www.einkframe.com`
- **GitHub Pages URL**：`https://raytronichen.github.io`

## 当前配置

### CNAME 文件
```
www.einkframe.com
```

### _config.yml 关键配置
```yaml
url: https://www.einkframe.com
baseurl: ""
default_category: tech
```

### 文章过滤
- 首页只显示 `category: tech` 的文章
- 文章导航（上一页/下一页）只显示同栏目文章
- 导航栏中的 Product 链接指向外部网站：`https://www.epapercanvas.com`

## 文件结构

```
.
├── CNAME                    # 自定义域名配置
├── _config.yml              # Jekyll 配置文件
├── index.md                 # 首页（英文，显示 Tech 文章）
├── index-zh.md              # 首页（中文，显示 Tech 文章）
├── _posts/                  # 所有文章
│   └── *.md                 # 文章文件（通过 category 分类）
├── _layouts/                # 布局文件
│   ├── default.html         # 默认布局（导航栏已配置）
│   └── post.html            # 文章布局（跨栏目链接已配置）
├── tech/                    # Tech 栏目页面（可选）
└── product/                 # Product 栏目页面（保留但不使用）
```

## 注意事项

1. **只显示 Tech 文章**：所有页面都通过 `category: tech` 过滤，只显示技术相关文章
2. **跨栏目链接**：导航栏和文章底部会链接到 Product 网站（`www.epapercanvas.com`）
3. **共享文章**：如果文章 `category: [tech, product]`，需要确保两个仓库都有这篇文章

## 创建新文章

在 `_posts/` 目录下创建新文件，front matter 示例：

```yaml
---
layout: post
title: Your Tech Article Title
date: 2025-11-13
lang: en
category: tech  # 必须设置为 tech
tags: [technology, e-ink]
---
```

## 部署

1. 提交更改到 `main` 分支
2. GitHub Pages 会自动构建和部署
3. 访问 `https://www.einkframe.com` 查看效果

## 相关文档

- `SETUP_PRODUCT_REPO.md` - 第二个仓库（Product 栏目）的配置指南
- `DUAL_SECTIONS_GUIDE.md` - 双栏目系统使用指南

