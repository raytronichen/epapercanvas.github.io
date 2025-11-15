# 双栏目配置指南

## 概述

网站现在支持两个独立的栏目：
- **Tech** (`/tech/`) - 技术栏目，对应 `www.einkframe.com`
- **Product** (`/product/`) - 产品栏目，对应 `www.epapercanvas.com`

两个栏目可以共享文章，也可以有独立的文章，并且可以相互链接。

## 文章分类

### 添加 category 字段

在文章的 front matter 中添加 `category` 字段：

```yaml
---
layout: post
title: Your Post Title
date: 2025-11-13
lang: en
category: tech  # 或 product，或两者都包含
tags: [tag1, tag2]
---
```

### 共享文章

如果一篇文章要同时出现在两个栏目，可以使用数组：

```yaml
category: [tech, product]
```

或者使用 `categories` 字段：

```yaml
categories: [tech, product]
```

## 目录结构

```
.
├── tech/
│   ├── index.md          # Tech 栏目首页（英文）
│   └── index-zh.md       # Tech 栏目首页（中文）
├── product/
│   ├── index.md          # Product 栏目首页（英文）
│   └── index-zh.md       # Product 栏目首页（中文）
└── _posts/
    └── *.md              # 所有文章（通过 category 分类）
```

## 域名配置

### 重要限制

GitHub Pages 一个仓库只能有一个 CNAME 文件，因此不能同时绑定两个自定义域名到同一个仓库。

### 解决方案

#### 方案 1：使用两个 GitHub Pages 仓库（推荐）

**原理**：创建两个独立的 GitHub Pages 仓库，每个仓库只显示一个栏目的内容。

**具体步骤**（以当前仓库 `raytronichen.github.io` 为例）：

1. **主仓库（当前仓库）**：`raytronichen.github.io`
   - 用途：Tech 栏目
   - 域名：`www.einkframe.com`
   - 操作：
     - 在仓库根目录创建 `CNAME` 文件，内容为：`www.einkframe.com`
     - 修改 `_config.yml`，设置 `baseurl: ""`（因为这是主域名）
     - 只显示 `category: tech` 的文章
     - 访问地址：`https://www.einkframe.com` → 显示 Tech 栏目

2. **第二个仓库**：创建新仓库，例如 `epapercanvas.github.io`
   - 用途：Product 栏目
   - 域名：`www.epapercanvas.com`
   - 操作：
     - 在 GitHub 上创建新仓库 `epapercanvas.github.io`
     - 将当前仓库的内容复制过去
     - 修改 `_config.yml`：
       ```yaml
       url: https://www.epapercanvas.com
       baseurl: ""
       ```
     - 创建 `CNAME` 文件，内容为：`www.epapercanvas.com`
     - 修改所有页面，只显示 `category: product` 的文章
     - 访问地址：`https://www.epapercanvas.com` → 显示 Product 栏目

3. **共享文章的处理**：
   - 如果文章 `category: [tech, product]`，需要同时复制到两个仓库
   - 或者使用 GitHub Actions 自动同步

**优点**：
- 两个域名完全独立
- 每个仓库可以有不同的配置
- 管理简单清晰

**缺点**：
- 需要维护两个仓库
- 共享文章需要手动同步或使用自动化工具

---

#### 方案 2：使用同一个仓库 + DNS 重定向（最简单）

**原理**：两个域名都指向同一个 GitHub Pages 仓库，但通过 JavaScript 或服务器端重定向来显示不同栏目。

**具体步骤**：

1. **主仓库配置**：`raytronichen.github.io`
   - 在 `CNAME` 文件中只保留：`www.einkframe.com`
   - 在 GitHub Pages 设置中绑定 `www.einkframe.com`
   - 访问 `https://www.einkframe.com` → 显示完整网站（包含两个栏目）

2. **第二个域名配置**：`www.epapercanvas.com`
   - 在域名 DNS 设置中添加 CNAME 记录：
     ```
     类型: CNAME
     名称: www
     值: raytronichen.github.io
     ```
   - 在 GitHub Pages 设置中添加自定义域名 `www.epapercanvas.com`
   - 创建 `index.html` 重定向文件（见下方代码）

3. **创建重定向页面**（可选）：
   在仓库根目录创建 `index.html`：
   ```html
   <!DOCTYPE html>
   <html>
   <head>
     <script>
       // 检测域名并重定向
       if (window.location.hostname === 'www.epapercanvas.com') {
         window.location.href = '/product/';
       } else {
         window.location.href = '/tech/';
       }
     </script>
   </head>
   <body>
     <p>Redirecting...</p>
   </body>
   </html>
   ```

**优点**：
- 只需要一个仓库
- 配置简单
- 共享文章自动同步

**缺点**：
- 两个域名实际上访问的是同一个网站
- 需要额外的重定向逻辑
- GitHub Pages 可能不支持多个自定义域名同时绑定

---

#### 方案 3：使用 GitHub Actions 构建两个独立站点（最灵活）

**原理**：使用 GitHub Actions 自动构建两个不同配置的站点，分别部署到不同的分支或路径。

**具体步骤**：

1. **创建 GitHub Actions 工作流**：
   在 `.github/workflows/deploy.yml` 中创建：

   ```yaml
   name: Deploy Dual Sites
   
   on:
     push:
       branches: [ main ]
   
   jobs:
     deploy-tech:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - name: Setup Ruby
           uses: ruby/setup-ruby@v1
         - name: Build Tech Site
           run: |
             # 修改 _config.yml 只显示 tech 栏目
             echo "baseurl: ''" >> _config.yml
             echo "url: https://www.einkframe.com" >> _config.yml
         - name: Deploy to Tech Branch
           # 部署到 gh-pages-tech 分支
   
     deploy-product:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - name: Setup Ruby
           uses: ruby/setup-ruby@v1
         - name: Build Product Site
           run: |
             # 修改 _config.yml 只显示 product 栏目
             echo "baseurl: ''" >> _config.yml
             echo "url: https://www.epapercanvas.com" >> _config.yml
         - name: Deploy to Product Branch
           # 部署到 gh-pages-product 分支
   ```

2. **创建两个 GitHub Pages 仓库**：
   - 仓库1：绑定 `www.einkframe.com`，从 `gh-pages-tech` 分支部署
   - 仓库2：绑定 `www.epapercanvas.com`，从 `gh-pages-product` 分支部署

**优点**：
- 自动化程度高
- 可以灵活配置每个站点
- 共享文章自动同步

**缺点**：
- 配置复杂
- 需要了解 GitHub Actions
- 需要维护工作流脚本

---

### 推荐方案对比

| 方案 | 难度 | 维护成本 | 灵活性 | 推荐度 |
|------|------|----------|--------|--------|
| 方案1：两个仓库 | ⭐⭐ | 中 | 高 | ⭐⭐⭐⭐⭐ |
| 方案2：DNS重定向 | ⭐ | 低 | 低 | ⭐⭐⭐ |
| 方案3：GitHub Actions | ⭐⭐⭐⭐ | 中 | 极高 | ⭐⭐⭐⭐ |

**建议**：如果刚开始，推荐使用**方案1（两个仓库）**，简单直接，容易理解和管理。

## 当前配置

- **Tech 栏目**：`/tech/` 和 `/tech/zh/`
- **Product 栏目**：`/product/` 和 `/product/zh/`
- **导航栏**：已添加 Tech 和 Product 链接
- **跨栏目链接**：文章页面底部会显示另一个栏目的链接

## 使用示例

### 创建 Tech 文章

```yaml
---
layout: post
title: Technical Deep Dive
date: 2025-11-13
lang: en
category: tech
tags: [technology, e-ink]
---
```

### 创建 Product 文章

```yaml
---
layout: post
title: Product Update
date: 2025-11-13
lang: en
category: product
tags: [product, update]
---
```

### 创建共享文章

```yaml
---
layout: post
title: Shared Article
date: 2025-11-13
lang: en
category: [tech, product]
tags: [shared]
---
```

## 注意事项

1. 所有现有文章已默认设置为 `category: tech`
2. 文章导航（上一页/下一页）只显示同一栏目内的文章
3. 文章页面底部会显示另一个栏目的链接
4. 首页 (`/`) 现在重定向到 `/tech/`

