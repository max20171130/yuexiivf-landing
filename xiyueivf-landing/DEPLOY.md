# 部署到 xiyueivf.com — 完整步骤

预计耗时：**10 分钟**（不算 DNS 生效等待时间）

---

## 第 1 步：推到 GitHub

打开终端，cd 到 `xiyueivf-landing` 这个文件夹，依次执行：

```bash
cd xiyueivf-landing

# 初始化 git
git init
git add .
git commit -m "Initial: chat-style landing page"

# 在 GitHub 上新建空仓库（不要勾选 README），假设叫 xiyueivf-landing
# 然后回到终端，添加远程并推送
git branch -M main
git remote add origin git@github.com:<你的用户名>/xiyueivf-landing.git
git push -u origin main
```

如果用 HTTPS 而不是 SSH，把最后的 `git remote add` 换成：
```bash
git remote add origin https://github.com/<你的用户名>/xiyueivf-landing.git
```

---

## 第 2 步：连接 Vercel

1. 打开 https://vercel.com/new
2. 点 **Import Git Repository**
3. 选刚推的 `xiyueivf-landing` 仓库
4. 全部用默认配置（Framework Preset 选 **Other**，Root Directory 留空）
5. 点 **Deploy**

等 30 秒，Vercel 会给你一个临时域名比如 `xiyueivf-landing.vercel.app`，**打开看看页面正不正常**。

---

## 第 3 步：绑定 xiyueivf.com

### 3.1 在 Vercel 添加域名

1. 进刚才那个项目 → **Settings** → **Domains**
2. 输入 `xiyueivf.com`，点 **Add**
3. 再输入 `www.xiyueivf.com`，点 **Add**（推荐都加上）
4. Vercel 会显示需要配置的 DNS 记录，**记下来**，类似：

```
A 记录:
  Name: @ (or 留空)
  Value: 76.76.21.21

CNAME 记录:
  Name: www
  Value: cname.vercel-dns.com
```

### 3.2 在域名注册商配置 DNS

去你买域名的地方（Cloudflare/NameSilo/GoDaddy/阿里云/腾讯云等），找到 **DNS 解析** 设置：

- 添加 A 记录：`@ → 76.76.21.21`
- 添加 CNAME 记录：`www → cname.vercel-dns.com`

⚠️ 如果之前有指向其他地方的记录，先删掉再加。

### 3.3 等待 DNS 生效

- 一般 **5-30 分钟**，最长 24 小时
- 在 Vercel Domains 页面会自动检测，绿色对勾就生效了
- Vercel 会自动签 HTTPS 证书（Let's Encrypt），不用手动配

---

## 第 4 步：验证部署

打开 https://xiyueivf.com ，应该能看到：
- ✅ 顶部黑条 + 绿色"加微信"按钮
- ✅ 两段欢迎消息打字播放
- ✅ 6 题问卷渐入显示
- ✅ HTTPS 安全锁

---

## 第 5 步：把它接到 Google Ads

部署成功后，回 Google Ads → 找一个广告组（建议先用「找机构组」做 A/B 测试）→ 改它的「最终到达网址」为：

```
https://xiyueivf.com/?utm_source=google&utm_medium=cpc&utm_campaign=src-brand-global&utm_content=agency&utm_term={keyword}
```

跑 3-5 天后对比转化率：
- yuexihui.org/consult.html → 旧版长落地页
- xiyueivf.com → 新版聊天落地页

哪个转化高用哪个。

---

## 后续维护

每次改完 `index.html`：

```bash
git add index.html
git commit -m "update copy"
git push
```

Vercel 自动重新部署，**1-2 分钟后线上就更新了**。

---

## 已知 TODO

| 项 | 现状 | 怎么改 |
|---|---|---|
| 表单提交后端 | 只 console.log | 接 Google 表单 / 邮箱 / 飞书机器人 |
| gtag 转化 ID | 占位 `YOUR_CONVERSION_ID` | 替换成你 Google Ads 转化 ID |
| 预算第 3 选项 | 占位"12 万美金以上" | 你定一个具体数 |

---

## 出问题怎么办

| 症状 | 排查 |
|---|---|
| Vercel 部署失败 | 看 build log，多半是 vercel.json 格式问题 |
| 域名打不开 | DNS 没生效，等等再试。或 `nslookup xiyueivf.com` 看指向哪 |
| 页面打开但 CSS 乱 | 浏览器强刷 Cmd+Shift+R |
| HTTPS 红色不安全 | Vercel 还在签证书，等 10 分钟 |
