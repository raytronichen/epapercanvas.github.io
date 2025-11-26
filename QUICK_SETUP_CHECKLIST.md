# 快速配置检查清单

## ✅ 第一个仓库：Tech 栏目 (www.einkframe.com)

### GitHub Pages 配置
- [ ] 访问：https://github.com/raytronichen/raytronichen.github.io/settings/pages
- [ ] Source: `main` 分支
- [ ] Custom domain: `www.einkframe.com`
- [ ] Enforce HTTPS: ✅ 已勾选

### DNS 配置
- [ ] CNAME 记录：`www` → `raytronichen.github.io`
- [ ] 验证：访问 `https://www.einkframe.com`

---

## 🔧 第二个仓库：Product 栏目 (www.epapercanvas.com)

### 步骤 1：GitHub Pages 配置（立即执行）

1. **打开仓库设置**
   ```
   直接访问：https://github.com/raytronichen/epapercanvas.github.io/settings/pages
   ```

2. **配置 Pages 源**
   - 找到 "Source" 部分
   - 选择：`Deploy from a branch`
   - Branch: 选择 `main`
   - Folder: 选择 `/ (root)`
   - 点击 **Save** 按钮

3. **配置自定义域名**
   - 找到 "Custom domain" 部分
   - 在输入框中输入：`www.epapercanvas.com`
   - 勾选 **Enforce HTTPS**（如果可用）
   - 点击 **Save** 按钮

4. **等待构建**
   - 页面会显示 "Your site is live at..." 
   - 等待 1-5 分钟让 GitHub 构建完成
   - 可以点击 "Visit site" 查看效果

### 步骤 2：DNS 配置（在你的域名注册商操作）

#### 如果你使用 Cloudflare：

1. **登录 Cloudflare**
   - 访问：https://dash.cloudflare.com
   - 登录你的账户

2. **选择域名**
   - 点击域名 `epapercanvas.com`（或你的域名）

3. **添加 CNAME 记录**
   - 进入 **DNS** → **Records**
   - 点击 **Add record**
   - 配置如下：
     ```
     类型: CNAME
     名称: www
     目标: raytronichen.github.io
     Proxy status: DNS only (灰色云 ☁️) ← 重要！
     TTL: Auto
     ```
   - ⚠️ **重要**：Proxy status 必须选择 "DNS only"（灰色云），不要开启代理（橙色云）
   - 点击 **Save**

#### 如果你使用其他域名注册商（GoDaddy、Namecheap 等）：

1. **登录域名管理面板**
   - 访问你的域名注册商网站
   - 登录账户

2. **找到 DNS 设置**
   - 通常叫 "DNS Management"、"DNS 设置" 或 "域名解析"

3. **添加 CNAME 记录**
   ```
   类型: CNAME
   主机记录/名称: www
   记录值/指向: raytronichen.github.io
   TTL: 3600 (或默认值)
   ```

4. **保存设置**

### 步骤 3：验证配置

#### 验证 GitHub Pages
1. 访问：`https://raytronichen.github.io`
2. 应该能看到 Product 栏目的内容 ✅

#### 验证 DNS（等待 5-60 分钟）
1. 访问：https://dnschecker.org/
2. 输入：`www.epapercanvas.com`
3. 选择：`CNAME`
4. 点击 "Search"
5. 查看全球 DNS 服务器是否都已指向 `raytronichen.github.io`

#### 验证网站访问（DNS 生效后）
1. 访问：`https://www.epapercanvas.com`
2. 应该能看到 Product 栏目的内容 ✅
3. 导航栏中的 "Tech" 链接应指向 `https://www.einkframe.com` ✅

---

## 🚨 常见问题快速解决

### 问题：GitHub Pages 显示 404
**解决**：
1. 确认仓库是 Public
2. 确认 Pages 源设置为 `main` 分支
3. 等待 5 分钟重新构建

### 问题：自定义域名无法访问
**解决**：
1. 确认 DNS 记录已添加
2. 等待 DNS 传播（最多 48 小时）
3. 在 GitHub Pages 设置中重新保存域名

### 问题：HTTPS 证书未生效
**解决**：
1. 在 GitHub Pages 设置中勾选 "Enforce HTTPS"
2. 等待证书自动生成（几分钟到几小时）
3. 确认 DNS 的 Proxy 已关闭（如果使用 Cloudflare）

---

## 📝 配置完成后

两个网站应该都能正常访问：

| 网站 | URL | 状态 |
|------|-----|------|
| Tech 栏目 | https://www.einkframe.com | ✅ |
| Product 栏目 | https://www.epapercanvas.com | ✅ |

两个网站可以相互链接，独立管理文章！

