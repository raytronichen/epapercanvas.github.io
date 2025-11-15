# 第二个仓库（Product 栏目）配置指南

## 概述

这是用于配置第二个 GitHub Pages 仓库的详细指南，该仓库将专门用于 Product 栏目，绑定域名 `www.epapercanvas.com`。

## 步骤 1：创建新仓库

1. 登录 GitHub
2. 点击右上角的 "+" → "New repository"
3. 仓库名称：`epapercanvas.github.io`（或你选择的其他名称）
4. 设置为 Public
5. 不要初始化 README、.gitignore 或 license
6. 点击 "Create repository"

## 步骤 2：复制文件

将当前仓库（`raytronichen.github.io`）的所有文件复制到新仓库：

```bash
# 克隆当前仓库
git clone https://github.com/raytronichen/raytronichen.github.io.git
cd raytronichen.github.io

# 添加新仓库为远程仓库
git remote add product https://github.com/YOUR_USERNAME/epapercanvas.github.io.git

# 推送到新仓库
git push product main
```

或者直接在 GitHub 上：
1. 下载当前仓库的 ZIP 文件
2. 解压后上传到新仓库

## 步骤 3：修改配置文件

### 3.1 修改 `_config.yml`

```yaml
# Site settings
title: ePaper Canvas — Product Stories and Updates
description: Discover product features, updates, and user stories about ePaper frames and canvas displays.
author: Raytronics
email: your-email@example.com
url: https://www.epapercanvas.com  # 改为新域名
baseurl: ""

# ... 其他配置保持不变 ...

# Site sections/categories
sections:
  tech:
    name: Tech
    name_zh: 技术
    url: https://www.einkframe.com  # 改为外部链接
    domain: www.einkframe.com
    description: Technology insights and technical discussions about ePaper displays
    description_zh: 关于电子纸显示技术的技术见解和讨论
  product:
    name: Product
    name_zh: 产品
    url: /product
    domain: www.epapercanvas.com
    description: Product features, updates, and user stories
    description_zh: 产品功能、更新和用户故事

# Default category (for this repository)
default_category: product  # 改为 product
```

### 3.2 创建/修改 `CNAME` 文件

创建或修改 `CNAME` 文件，内容为：

```
www.epapercanvas.com
```

### 3.3 修改首页文件

修改 `index.md`：

```yaml
---
layout: default
lang: en
permalink: /product/
redirect_from: /
---
```

修改 `index-zh.md`：

```yaml
---
layout: default
lang: zh
permalink: /product/zh/
redirect_from: /zh/
---
```

### 3.4 修改布局文件

修改 `_layouts/default.html` 中的导航栏：

```html
<nav class="site-nav">
  <div class="trigger">
    <a class="page-link" href="{{ '/' | relative_url }}">{% if page.lang == 'zh' %}首页{% else %}Home{% endif %}</a>
    <a class="page-link" href="{{ '/about' | relative_url }}">{% if page.lang == 'zh' %}关于{% else %}About{% endif %}</a>
    <a class="page-link" href="https://www.einkframe.com{% if page.lang == 'zh' %}/zh/{% endif %}" target="_blank">{% if page.lang == 'zh' %}技术{% else %}Tech{% endif %}</a>
  </div>
</nav>
```

修改 `_layouts/post.html`，在文章底部添加：

```html
{% if page.category == 'product' %}
<div class="post-category-links" style="margin-top: 30px; padding-top: 30px; border-top: 1px solid #e8e8e8; text-align: center;">
  <p style="color: #666; font-size: 14px;">
    {% if page.lang == 'zh' %}
      查看 <a href="https://www.einkframe.com/zh/" style="color: #0066cc;" target="_blank">技术栏目</a> 的更多文章
    {% else %}
      Check out more posts in the <a href="https://www.einkframe.com/" style="color: #0066cc;" target="_blank">Tech section</a>
    {% endif %}
  </p>
</div>
{% endif %}
```

### 3.5 修改栏目首页

修改 `product/index.md` 和 `product/index-zh.md`，将跨栏目链接改为外部链接：

```html
<div class="section-links" style="margin-top: 40px; padding-top: 40px; border-top: 1px solid #e8e8e8;">
  <p style="text-align: center; color: #666;">
    Also check out our <a href="https://www.einkframe.com/" style="color: #0066cc;" target="_blank">Tech section</a>
  </p>
</div>
```

### 3.6 修改文章过滤逻辑

确保所有页面只显示 `category: product` 的文章：

- `index.md` 和 `index-zh.md`
- `product/index.md` 和 `product/index-zh.md`

过滤条件改为：
```liquid
{% assign filtered_posts = site.posts | where: "lang", current_lang | where_exp: "post", "post.category == 'product' or post.category contains 'product' or post.categories contains 'product'" | sort: "date" | reverse %}
```

## 步骤 4：配置 GitHub Pages

1. 进入新仓库的 Settings
2. 点击左侧的 "Pages"
3. 在 "Source" 下选择 "Deploy from a branch"
4. 选择分支：`main`
5. 选择文件夹：`/ (root)`
6. 点击 "Save"

## 步骤 5：配置自定义域名

1. 在仓库 Settings → Pages 中
2. 在 "Custom domain" 输入框中输入：`www.epapercanvas.com`
3. 点击 "Save"
4. GitHub 会自动创建 `CNAME` 文件（如果还没有的话）

## 步骤 6：配置 DNS

在你的域名注册商（如 GoDaddy、Namecheap 等）配置 DNS：

1. 登录域名管理面板
2. 找到 DNS 设置
3. 添加 CNAME 记录：
   ```
   类型: CNAME
   名称: www
   值: YOUR_USERNAME.github.io
   TTL: 3600 (或默认值)
   ```

4. 等待 DNS 传播（通常几分钟到几小时）

## 步骤 7：验证配置

1. 等待几分钟让 GitHub Pages 构建完成
2. 访问 `https://YOUR_USERNAME.github.io` 应该能看到 Product 栏目
3. 访问 `https://www.epapercanvas.com` 应该能看到 Product 栏目
4. 检查导航栏中的 Tech 链接是否指向 `https://www.einkframe.com`

## 共享文章的处理

如果一篇文章需要同时出现在两个栏目（`category: [tech, product]`）：

1. **手动同步**：在两个仓库中都保留这篇文章
2. **自动同步**：使用 GitHub Actions 自动同步（见下方）

### GitHub Actions 自动同步（可选）

创建 `.github/workflows/sync-posts.yml`：

```yaml
name: Sync Shared Posts

on:
  push:
    paths:
      - '_posts/**'
    branches: [ main ]

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Sync to Product Repo
        run: |
          git clone https://github.com/YOUR_USERNAME/epapercanvas.github.io.git product-repo
          cd product-repo
          # 复制共享文章
          cp -r ../_posts/* _posts/
          git add .
          git commit -m "Sync shared posts" || exit 0
          git push
```

## 完成！

现在你有两个独立的仓库：
- `raytronichen.github.io` → `www.einkframe.com` (Tech 栏目)
- `epapercanvas.github.io` → `www.epapercanvas.com` (Product 栏目)

两个网站可以独立管理，也可以共享文章。

