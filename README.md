# 个人主页 · 部署到 Cloudflare Pages

纯静态站点，无需构建。**Cloudflare Pages 免费套餐**即可托管，并支持绑定自有域名 + 免费 HTTPS 证书。

## 目录结构

```
index.html    首页
404.html      自定义 404 页面
README.md     本文件
```

## 部署方式一：Git 集成（推荐，后续推送自动部署）

1. 在 GitHub / GitLab 新建一个仓库（例如 `my-homepage`）。
2. 在本目录执行：

```bash
git init
git add .
git commit -m "init: 个人主页"
git branch -M main
git remote add origin https://github.com/<你的用户名>/my-homepage.git
git push -u origin main
```

3. 打开 [Cloudflare Dashboard](https://dash.cloudflare.com) → **Workers 和 Pages** → **创建** → **Pages** → **连接到 Git**。
4. 选中该仓库，构建配置填：

| 配置项 | 值 |
| --- | --- |
| 框架预设 | 无（None） |
| 构建命令 | 留空 |
| 输出目录 | `/` 或留空（填 `.` 也可以） |
| 构建分支 | `main` |

5. 点击**保存并部署**，约 30 秒后得到 `xxx.pages.dev` 访问地址。

> 之后每次 `git push` 都会自动重新部署。

## 部署方式二：Wrangler 命令行（直接上传，无需 Git）

```bash
npx wrangler login            # 浏览器授权
npx wrangler pages deploy . --project-name=my-homepage
```

首次会提示创建项目，完成后返回 `https://my-homepage.pages.dev`。

> 也可以用 API Token 免登录：`export CLOUDFLARE_API_TOKEN=xxxx` 后再执行 deploy。

## 绑定自有域名

在 Pages 项目的 **自定义域** → **设置自定义域**，输入你的域名（如 `www.example.com` 或 `example.com`）。

**情况 A：域名的 DNS 由 Cloudflare 托管**
- 无需手动操作，Cloudflare 会自动添加 DNS 记录和证书，等待约 1 分钟生效。

**情况 B：DNS 在阿里云 / 腾讯云等其他服务商**
1. 先按提示把域名 NS 接入 Cloudflare（推荐，可享 CDN 与免费证书），或
2. 保持原 DNS，手动添加一条记录：

| 类型 | 名称 | 目标 |
| --- | --- | --- |
| CNAME | `www` | `my-homepage.pages.dev` |

根域名（`example.com`）不支持 CNAME，需使用 Cloudflare 的 **CNAME Flattening**（即方案 1）才能绑定 apex 域名。

推荐做法：先绑定 `www.example.com`，再把根域 301 跳转到 `www`（Pages 的**重定向规则**或 Cloudflare 的 **Rules → Redirect Rules** 里配置）。

## 上线前检查

- [ ] 把 `index.html` 里的姓名、简介、项目经历、技能替换为真实内容
- [ ] 把邮箱 / GitHub / 博客链接改成自己的
- [ ] 把 `<title>` 与页面底部版权信息改成自己的
- [ ] 用手机打开确认排版正常
