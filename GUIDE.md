# 从零搭建一个属于自己的技术博客

> 一份面向新手的完整实操指南：域名、静态站、Git、自动部署、HTTPS、搜索引擎收录、长期备份。
>
> 全文不含任何个人信息，所有域名、邮箱、用户名均以占位符表示。

---

## 目录

1. [这份指南适合谁](#1-这份指南适合谁)
2. [为什么不直接用现成平台](#2-为什么不直接用现成平台)
3. [最终架构与成本](#3-最终架构与成本)
4. [准备工作](#4-准备工作)
5. [第一步：买域名](#5-第一步买域名)
6. [第二步：安装 Hugo](#6-第二步安装-hugo)
7. [第三步：创建站点](#7-第三步创建站点)
8. [第四步：配置站点](#8-第四步配置站点)
9. [第五步：写第一篇文章](#9-第五步写第一篇文章)
10. [第六步：推到 GitHub](#10-第六步推到-github)
11. [第七步：Cloudflare Pages 部署](#11-第七步cloudflare-pages-部署)
12. [第八步：绑定自己的域名](#12-第八步绑定自己的域名)
13. [第九步：www 跳转](#13-第九步www-跳转)
14. [第十步：搜索引擎收录](#14-第十步搜索引擎收录)
15. [第十一步：favicon 与细节](#15-第十一步favicon-与细节)
16. [第十二步：备份与存档](#16-第十二步备份与存档)
17. [日常写作流程](#17-日常写作流程)
18. [常见问题排查](#18-常见问题排查)
19. [维护日历](#19-维护日历)
20. [隐私与安全清单](#20-隐私与安全清单)
21. [可选进阶](#21-可选进阶)
22. [附录：命令速查](#22-附录命令速查)

---

## 1. 这份指南适合谁

适合你，如果你：

- 想长期写技术笔记，不希望内容被平台绑架
- 希望链接十年后还能打开
- 愿意花 1–2 小时做一次性配置
- 能接受每年一个域名钱（约 ¥60–90）

不适合你，如果你：

- 只想马上发文章，不想碰命令行
- 需要评论、点赞、关注这类社交功能
- 完全不想管理任何东西

### 最终成果

一个属于你的网站：

```
https://你的域名.com
```

- 有自己的域名和 HTTPS
- 内容是你本地的 Markdown 文件
- `git push` 后自动上线
- 全球可访问（中国大陆可访问但速度一般）
- 数据完全在你自己手里

### 前置条件

| 项目 | 要求 |
|---|---|
| 操作系统 | Windows 10/11（macOS/Linux 命令略有不同） |
| 网络 | 能访问 GitHub（可能需要代理） |
| 支付 | 支付宝/微信，或国际信用卡 |
| 时间 | 首次 1–2 小时 |

---

## 2. 为什么不直接用现成平台

常见平台的问题不是"不好用"，而是**不受你控制**：

| 平台类型 | 主要问题 |
|---|---|
| 博客托管平台 | 平台改版、停服、加付费墙，链接可能失效 |
| 国内内容平台 | 审核规则变化、导出能力弱、海外访问差 |
| 社交型平台 | 内容按算法分发，不适合长期沉淀 |
| 免费静态托管 | 中国大陆访问不稳定 |

自建静态博客解决的是**所有权**问题：

- 源文件是纯文本，存在你自己硬盘
- 版本历史在 Git，任何一次改动都可回退
- 托管商可以随时替换，内容不动
- 网址由你决定，不改就一直有效

---

## 3. 最终架构与成本

### 架构

```
本地 Markdown 文件
      │  git push
      ▼
GitHub 仓库（版本历史 + 异地副本）
      │  自动触发构建
      ▼
Cloudflare Pages（Hugo 构建 + 全球 CDN + 自动 HTTPS）
      │
      ▼
你自己的域名（永久入口）
```

### 各组件职责

| 组件 | 作用 | 费用 |
|---|---|---|
| 域名注册商 | 持有域名 | 约 ¥60–90/年 |
| Cloudflare DNS | 解析域名 | 免费 |
| Cloudflare Pages | 构建、托管、证书 | 免费 |
| GitHub | 代码托管、版本历史 | 免费 |
| Hugo | 把 Markdown 变成网页 | 开源免费 |
| PaperMod | 网页主题 | 开源免费 |

**总成本：一年一个域名钱。**

### 为什么选这套

| 选择 | 理由 |
|---|---|
| 静态站而非动态站 | 没有数据库、没有后端，不会因为程序漏洞挂掉 |
| Git 而非网盘 | 有版本历史，可回退，可迁移 |
| Cloudflare Pages 而非自建服务器 | 免运维、免证书、免续费 |
| Markdown 而非富文本 | 纯文本，二十年后的软件也能打开 |

---

## 4. 准备工作

### 4.1 需要注册的账号

| 账号 | 用途 |
|---|---|
| 域名注册商 | 买域名（阿里云/腾讯云/Cloudflare） |
| Cloudflare | DNS + 托管 |
| GitHub | 代码仓库 |

### 4.2 需要安装的软件

| 软件 | 用途 | 检查命令 |
|---|---|---|
| Git | 版本控制 | `git --version` |
| Hugo | 静态站生成 | `hugo version` |

### 4.3 术语对照

| 术语 | 含义 |
|---|---|
| Markdown | 一种纯文本写作格式，用 `#` 表示标题 |
| front matter | 文章开头的配置区，在两个 `+++` 之间 |
| slug | 网址里属于这篇文章的那一段 |
| 静态站点 | 提前生成好的 HTML 文件，不需要数据库 |
| 构建 | 把 Markdown 转成 HTML 的过程 |
| NS | 域名服务器，决定域名由谁解析 |
| 备案 | 中国大陆服务器托管网站的行政许可，海外托管不需要 |

---

## 5. 第一步：买域名

### 5.1 后缀怎么选

| 后缀 | 定位 | 首年参考 | 续费参考 | 建议 |
|---|---|---|---|---|
| `.com` | 通用、最可信 | ¥60–90 | ¥85–100 | **首选** |
| `.dev` | 开发者，强制 HTTPS | ¥100–130 | ¥110–140 | 技术博客很好的替代 |
| `.me` | 个人品牌 | ¥90–150 | ¥120–160 | 适合个人主页 |
| `.io` | 科技 | ¥300+ | ¥350+ | 贵，个人没必要 |
| `.tech` | 科技 | ¥20–40 | ¥200+ | 首年便宜续费贵，慎选 |
| `.xyz` | 便宜通用 | ¥10–30 | ¥70–100 | 信任度偏低 |
| `.cn` | 中国 | ¥30–50 | ¥40–60 | 需实名，海外访问一般 |

**核心原则：看续费价，不看首年价。** 域名是长期资产，第三年续费 200 块的便宜后缀不值得。

### 5.2 在哪买

| 渠道 | 支付方式 | 适合 |
|---|---|---|
| Cloudflare Registrar | 国际信用卡 | 有卡，图省事，按成本价 |
| 阿里云 / 腾讯云 | 支付宝、微信 | 没有国际卡 |
| Namecheap / Porkbun | 信用卡、PayPal | 有卡，界面简单 |

### 5.3 一个容易踩的坑：中国站 vs 国际站

如果使用阿里云：

| 站点 | 网址 | 面向 | 有"中国大陆"选项 |
|---|---|---|---|
| 中国站 | `aliyun.com` | 大陆个人/企业 | ✅ 有 |
| 国际站 | `alibabacloud.com` | 海外主体 | ❌ 没有 |

**务必使用中国站。** 国际站的注册地区一旦选定无法更改，后续实名、支付都会别扭。

如果访问 `aliyun.com` 被自动跳到国际站，通常是因为网络出口在境外（开了代理）。关掉代理，或换网络再试。

### 5.4 购买流程

1. 注册账号，完成**个人实名认证**（身份证 + 人脸）
2. 搜索域名，确认可注册
3. 加入清单 → 立即购买 → 选 **1 年**
4. 取消所有附加产品（云服务器、SSL、建站套餐都不需要）
5. 创建域名持有者信息模板（姓名、身份证、邮箱、地址，与实名一致）
6. 支付
7. 等待实名与注册局审核，状态从「审核中」变「正常」

### 5.5 注意事项

| 事项 | 说明 |
|---|---|
| 实名 ≠ 备案 | 备案只在大陆服务器托管时需要，海外托管不需要 |
| 隐私保护 | 开启 WHOIS 隐私，避免姓名电话公开 |
| 域名锁 | 开启禁止转移锁 |
| 注册邮箱 | 用长期可用的邮箱，转移域名靠它 |
| 自动续费 | 开启，并另设日历提醒 |

---

## 6. 第二步：安装 Hugo

### 6.1 检查是否已安装

```powershell
hugo version
```

如果提示"无法识别"，继续往下。

### 6.2 下载

打开 https://github.com/gohugoio/hugo/releases

找到最新版本的 **`hugo_extended_<版本>_windows-amd64.zip`**。

注意三点：

- 必须带 `extended`
- 必须是 `windows-amd64`（不是 arm64）
- 不要下 `withdeploy` 版本（用不到）

### 6.3 解压到固定目录

```powershell
New-Item -ItemType Directory -Force -Path "C:\tools\hugo" | Out-Null
Expand-Archive "$env:USERPROFILE\Downloads\hugo.zip" -DestinationPath "C:\tools\hugo" -Force
```

### 6.4 验证

```powershell
C:\tools\hugo\hugo.exe version
```

期望输出类似：

```
hugo v0.168.0+extended windows/amd64 BuildDate=...
```

看到 `+extended` 即成功。

### 6.5 加入 PATH

1. `Win + R` → 输入 `sysdm.cpl` → 回车
2. 「高级」→「环境变量」
3. 在「用户变量」里选中 `Path` →「编辑」
4. 「新建」→ 输入 `C:\tools\hugo` → 确定

**关掉当前终端，重新开一个**，再执行：

```powershell
hugo version
```

能出版本号即成功。

---

## 7. 第三步：创建站点

### 7.1 新建

```powershell
cd C:\Users\你的用户名\Documents
hugo new site my-blog
cd my-blog
git init
```

期望输出：

```
Congratulations! Your new Hugo site was created in ...\my-blog.
Initialized empty Git repository in .../my-blog/.git/
```

### 7.2 目录结构

```
my-blog/
├── archetypes/    新建文章的模板
├── content/       所有文章
├── static/        图片、favicon 等静态文件
├── themes/        主题
├── layouts/       自定义模板（可覆盖主题）
└── hugo.toml      全站配置
```

### 7.3 安装主题

```powershell
git clone --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
Remove-Item -Recurse -Force themes/PaperMod/.git
```

**第二步不能省。** 不删主题自带的 `.git`，推送时主题文件不会进你的仓库，线上构建会报 `theme not found`。

验证：

```powershell
Test-Path themes/PaperMod/theme.toml
```

必须返回 `True`。

---

## 8. 第四步：配置站点

把 `hugo.toml` 整体替换为下面内容，**只改三处**：`baseURL`、`title`、GitHub 链接。

```toml
baseURL = "https://你的域名.com/"
locale = "zh-cn"
defaultContentLanguage = "zh"
title = "你的博客名"
theme = "PaperMod"
enableRobotsTXT = true
hasCJKLanguage = true

[pagination]
  pagerSize = 20

[permalinks]
  posts = "/posts/:slug/"

[outputs]
  home = ["HTML", "RSS", "JSON"]

[params]
  env = "production"
  defaultTheme = "auto"
  ShowReadingTime = true
  ShowShareButtons = true
  ShowPostNavLinks = true
  ShowBreadCrumbs = true
  ShowCodeCopyButtons = true
  DateFormat = "2006-01-02"

  [params.assets]
    favicon = "/favicon.svg"
    favicon16x16 = "/favicon.svg"
    favicon32x32 = "/favicon.svg"
    apple_touch_icon = "/favicon.svg"

  [params.homeInfoParams]
    Title = "Hi, I'm 你的名字"
    Content = "这里记录我的技术笔记与长期思考。"

  [[params.socialIcons]]
    name = "github"
    url = "https://github.com/你的用户名"

[[menu.main]]
  name = "文章"
  pageRef = "/posts"
  weight = 10

[[menu.main]]
  name = "标签"
  pageRef = "/tags"
  weight = 20

[[menu.main]]
  name = "关于"
  pageRef = "/about"
  weight = 30

[markup]
  [markup.highlight]
    noClasses = false
  [markup.goldmark.renderer]
    unsafe = true
```

### 关键配置说明

| 配置 | 作用 |
|---|---|
| `baseURL` | 生成绝对链接的基准，**末尾必须带 `/`** |
| `[permalinks]` | 把文章网址固定为 `/posts/slug/`，这是"链接不变"的基础 |
| `[pagination] pagerSize` | 每页文章数（旧写法 `paginate` 已废弃） |
| `[outputs] home` | 额外输出 `index.json`，供站内搜索使用 |
| `unsafe = true` | 允许正文里写原生 HTML |
| `[params.assets]` | 指定 favicon 路径 |

---

## 9. 第五步：写第一篇文章

### 9.1 配置文章模板

编辑 `archetypes/default.md`：

```toml
+++
title = "{{ replace .File.ContentBaseName "-" " " | title }}"
date = "{{ .Date }}"
draft = true
slug = "{{ .File.ContentBaseName }}"
tags = []
categories = []
+++
```

含义：

| 占位符 | 生成时替换为 |
|---|---|
| `{{ .File.ContentBaseName }}` | 文件名（不含扩展名） |
| `{{ .Date }}` | 当前时间 |
| `{{ replace ... }}` | 文件名转成的标题 |

### 9.2 新建

```powershell
hugo new content posts/my-first-post.md
```

文件名规则：**全小写英文 + 连字符**。它会成为网址的一部分。

```
content/posts/my-first-post.md  →  /posts/my-first-post/
```

### 9.3 front matter 详解

```toml
+++
title = "文章标题"                     # 页面大标题，中文可
date = "2026-01-01T10:00:00+08:00"    # 发布时间，不能写未来
draft = false                          # true 草稿 / false 发布
slug = "my-first-post"                 # 网址，发布后不改
tags = ["标签1", "标签2"]              # 可选
categories = ["分类"]                  # 可选
+++
```

| 字段 | 规则 |
|---|---|
| `title` | 随便改，不影响链接 |
| `date` | 写未来时间会导致文章不发布 |
| `draft` | 发布前必须改 `false` |
| `slug` | 发布后不要改，改了旧链接 404 |
| `tags` | 可以留空 `[]` |

### 9.4 正文写作

正文写在第二个 `+++` 下面，用 Markdown。

**核心规则：段落之间空一行。**

```markdown
## 小标题

这是第一段。

这是第二段。

```c
printf("hello\n");
```

代码块前后也要空一行。
```

### 9.5 预览

```powershell
hugo server -D
```

打开 `http://localhost:1313`。

- `-D` 表示连草稿一起显示
- 修改文件后浏览器自动刷新
- `Ctrl + C` 停止

### 9.6 Markdown 速查

| 需求 | 写法 |
|---|---|
| 二级标题 | `## 标题` |
| 加粗 | `**文字**` |
| 斜体 | `*文字*` |
| 行内代码 | `` `code` `` |
| 代码块 | 三个反引号 + 语言名 |
| 无序列表 | `- 项目` |
| 有序列表 | `1. 项目` |
| 链接 | `[文字](https://example.com)` |
| 图片 | `![说明](/images/x.png)` |
| 引用 | `> 文字` |
| 分割线 | `---` |

---

## 10. 第六步：推到 GitHub

### 10.1 .gitignore

```powershell
@"
public/
resources/_gen/
.hugo_build.lock
"@ | Out-File -Encoding utf8 .gitignore
```

### 10.2 保护隐私：使用 noreply 邮箱

公开仓库的提交记录会暴露邮箱。GitHub 提供匿名地址：

```
数字+用户名@users.noreply.github.com
```

在 GitHub → Settings → Emails 页面可以看到自己的地址。

配置：

```powershell
git config --global user.name "你的名字"
git config --global user.email "你的noreply地址"
```

### 10.3 提交

```powershell
git add .
git commit -m "init: hugo blog with PaperMod"
```

### 10.4 建远程仓库

在 GitHub 网页上：

1. 右上角 `+` → New repository
2. 名称填 `blog`
3. 选 Public 或 Private
4. **不要**勾选 README、.gitignore、license
5. Create repository

### 10.5 推送

```powershell
git branch -M main
git remote add origin https://github.com/你的用户名/blog.git
git push -u origin main
```

首次推送会弹出浏览器授权（Git Credential Manager），点同意即可。

### 10.6 验证主题已入库

```powershell
git ls-files themes/PaperMod | Select-Object -First 5
```

必须能列出文件。**没有输出说明主题没进仓库，线上构建一定失败。**

### 10.7 网络问题

如果 `git push` 报 `Failed to connect to github.com`：

```powershell
Test-NetConnection github.com -Port 443
```

`TcpTestSucceeded: False` 说明需要代理。给 Git 单独配置：

```powershell
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

端口换成你代理软件的实际端口。

---

## 11. 第七步：Cloudflare Pages 部署

### 11.1 注册

https://dash.cloudflare.com/sign-up

可以用邮箱注册，也可以用 GitHub 登录。

### 11.2 创建项目

1. 左侧 **Workers & Pages**
2. **Create application** → **Pages** 标签
3. **Connect to Git**
4. 授权 GitHub，选择仓库
5. **Begin setup**

### 11.3 构建配置

| 字段 | 值 |
|---|---|
| Project name | 小写英文 + 连字符 |
| Production branch | `main` |
| Framework preset | `Hugo` |
| Build command | `hugo --gc --minify` |
| Build output directory | `public` |

### 11.4 环境变量（必做）

| 变量名 | 值 |
|---|---|
| `HUGO_VERSION` | 与你本地一致的版本，如 `0.168.0` |

**不设这一项，Cloudflare 会用自带的旧版 Hugo，主题可能直接构建失败。**

### 11.5 部署

点 **Save and Deploy**，等 1–2 分钟。

成功后得到临时地址：

```
https://项目名.pages.dev
```

### 11.6 构建失败排查

| 日志 | 原因 | 解决 |
|---|---|---|
| `theme "PaperMod" not found` | 主题没进仓库 | `git add themes/` 后重新推送 |
| `hugo: not found` | 版本变量没生效 | 检查 `HUGO_VERSION` 拼写 |
| `unexpected character` | front matter 语法错 | 本地跑 `hugo` 看行号 |
| `failed to resolve` | 模板或配置错误 | 看日志上下文 |

---

## 12. 第八步：绑定自己的域名

### 12.1 在 Cloudflare 添加站点

1. **Domains** → **Add a domain**
2. 输入裸域（`你的域名.com`）
3. 选 Free 套餐
4. 扫描 DNS 记录（新域名一般没有）
5. 记下两个 nameserver，形如：

```
xxx.ns.cloudflare.com
yyy.ns.cloudflare.com
```

### 12.2 去注册商改 NS

以阿里云为例：

1. 控制台 → 域名列表 → 管理
2. 左侧 **DNS修改**
3. 删除原有的 `dns1.hichina.com` 等
4. 填入 Cloudflare 的两个 NS
5. 保存，可能需要手机验证

> 页面上的「同步」按钮只是把注册局记录拉回显示，不会提交修改。要改就点「修改」。

### 12.3 等待生效

- 通常 10 分钟到 2 小时，最长 24 小时
- Cloudflare 状态从 `Pending` 变 `Active`

验证：

```powershell
nslookup -type=NS 你的域名.com 1.1.1.1
```

返回 `xxx.ns.cloudflare.com` 即生效。

### 12.4 绑定到 Pages

1. Pages 项目 → **Custom domains**
2. **Set up a custom domain**
3. 输入 `你的域名.com`
4. Continue → Activate

Cloudflare 会自动创建 CNAME 记录，无需手动加。

### 12.5 更新 baseURL

绑定成功后，把 `hugo.toml` 的 `baseURL` 改成：

```toml
baseURL = "https://你的域名.com/"
```

推送后自动重新部署。

**这一步不能漏**，否则 sitemap、RSS、分享卡片里的链接全是旧地址。

---

## 13. 第九步：www 跳转

### 13.1 先把 www 加到 Pages

Custom domains → 添加 `www.你的域名.com`，等待 Active。

如果提示需要配置 DNS，手动添加：

```
类型 CNAME | 名称 www | 内容 项目名.pages.dev | 代理开启
```

### 13.2 配置 301 跳转

1. 进入域名管理 → **Rules** → **Redirect Rules**
2. **Create rule**

**Rule name**

```
www to apex
```

**When incoming requests match**

| 字段 | 值 |
|---|---|
| Field | `Hostname` |
| Operator | `equals` |
| Value | `www.你的域名.com` |

**Then**

| 字段 | 值 |
|---|---|
| Type | `Dynamic` |
| Expression | `concat("https://你的域名.com", http.request.uri.path)` |
| Status code | `301` |
| Preserve query string | 勾选 |

点 **Deploy**，确认列表里出现规则，顶部显示 `1 out of 10`。

### 13.3 为什么用 Dynamic

Static 只能跳到固定网址，文章链接会全部丢到首页。Dynamic 会把原路径拼回去：

```
www.你的域名.com/posts/xxx/  →  你的域名.com/posts/xxx/
```

### 13.4 验证

```powershell
curl.exe -I https://www.你的域名.com
```

期望：

```
HTTP/1.1 301 Moved Permanently
Location: https://你的域名.com/
```

---

## 14. 第十步：搜索引擎收录

### 14.1 Google Search Console

1. https://search.google.com/search-console
2. **添加资源** → 选 **域名**
3. 输入 `你的域名.com`
4. 按提示添加 DNS TXT 记录
   - 如果 Cloudflare 支持 Domain Connect，会弹出一键授权，点 **Authorize** 即可
5. 回到 Search Console 点 **验证**
6. 左侧 **站点地图** → 提交：

```
https://你的域名.com/sitemap.xml
```

> 「域名」类型的资源，sitemap 必须填完整 URL，不能只写 `sitemap.xml`。

### 14.2 Bing Webmaster

1. https://www.bing.com/webmasters
2. 选 **Import from Google Search Console**
3. 授权后选择站点

如果提示"未找到站点"，通常是同步延迟，隔几小时或第二天重试；也可以手动添加，用 DNS 或 XML 文件验证。

---

## 15. 第十一步：favicon 与细节

### 15.1 用 SVG 做 favicon

不需要图片编辑软件，一个文本文件即可。

新建 `static/favicon.svg`：

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64" role="img" aria-label="H">
  <rect width="64" height="64" rx="14" fill="#111111"/>
  <rect x="16" y="16" width="8" height="32" rx="2" fill="#ffffff"/>
  <rect x="40" y="16" width="8" height="32" rx="2" fill="#ffffff"/>
  <rect x="24" y="28" width="16" height="8" fill="#ffffff"/>
</svg>
```

在 `hugo.toml` 的 `[params.assets]` 里指向它（见第 8 节配置）。

### 15.2 关于页面

新建 `content/about.md`：

```toml
+++
title = "关于"
date = "2026-01-01T10:00:00+08:00"
draft = false
slug = "about"
hidemeta = true
ShowToc = false
+++

我是谁，做什么方向，这个站点写什么。
```

`slug` 必须是 `about`，才能对应菜单里的 `/about`。

---

## 16. 第十二步：备份与存档

这一步决定十年后内容还在不在。

### 16.1 三层备份

| 层级 | 位置 | 说明 |
|---|---|---|
| 第一层 | GitHub 仓库 | 每次 push 自动更新 |
| 第二层 | 本地 bundle | 完整历史快照 |
| 第三层 | 网盘/移动硬盘 | 异地副本 |

### 16.2 打 tag

```powershell
git tag -a v1.0 -m "site online"
git push --tags
```

tag 的意义：以后想回到某个时间点的完整状态，一条命令就能切回去。

### 16.3 导出 bundle

```powershell
git bundle create "D:\backup-2026.bundle" --all
```

这个文件包含全部提交历史、分支、tag。复制到网盘。

恢复：

```powershell
git clone D:\backup-2026.bundle my-blog
```

### 16.4 三条铁律

| 规则 | 原因 |
|---|---|
| slug 永不改 | 改了旧链接 404 |
| 图片进仓库 | 图床会跑路 |
| 每年导一次 bundle | GitHub 也可能出问题 |

---

## 17. 日常写作流程

### 17.1 每次写作

```powershell
cd D:\blog
hugo new content posts/文章名.md
```

编辑 front matter，把 `draft` 改成 `false`，写正文。

```powershell
hugo server -D
```

浏览器看效果，修改后自动刷新。

```powershell
git add .
git commit -m "post: 文章标题"
git push
```

等 1–2 分钟上线。

### 17.2 修改已发布文章

| 想改 | 能不能改 |
|---|---|
| 正文 | ✅ |
| title | ✅ |
| tags / categories | ✅ |
| slug | ❌（旧链接会断） |
| date | ⚠️ 影响排序 |

确实要改 slug：

```toml
slug = "新地址"
aliases = ["/posts/旧地址/"]
```

### 17.3 删除文章

```powershell
Remove-Item content\posts\xxx.md
git add . ; git commit -m "remove: xxx" ; git push
```

只是暂时隐藏，把 `draft` 改回 `true` 更安全。

---

## 18. 常见问题排查

### 18.1 复制粘贴导致的反斜杠

**现象**：文章里出现 `\## 标题`、`\*\*加粗\*\*`、`&#x20;`。

**原因**：从某些编辑器或聊天窗口复制 Markdown 时，内容被转义了。

**解决**：

- 优先从编辑器的**源码模式**复制
- 粘完检查有没有多余的反斜杠
- 已经出现的，用脚本清理（务必先备份）

清理脚本（只处理代码块之外的转义，保留代码里的 `\n`）：

```powershell
$p='文章路径.md'
$lines=Get-Content -LiteralPath $p -Encoding utf8
$out=New-Object System.Collections.Generic.List[string]
$inFence=$false
foreach($l in $lines){
  if($l -match '^\s*```'){ $inFence=-not $inFence; $out.Add($l); continue }
  if($inFence){ $out.Add($l); continue }
  $s=$l.Replace('\\',[string][char]1)
  $s=[regex]::Replace($s,'\\([#*\[\]()_~`>!-])','$1')
  $s=$s.Replace([string][char]1,'\')
  $out.Add($s)
}
Set-Content -LiteralPath $p -Value $out -Encoding utf8
```

> ⚠️ 运行前必须备份文件，并且先用小文件测试。批量修改脚本一旦出错，可能清空正文。

### 18.2 文章不显示

| 原因 | 检查 |
|---|---|
| `draft = true` | 改成 `false` |
| 日期在未来 | 改成当前或过去 |
| 构建失败 | 看 Cloudflare 部署日志 |

### 18.3 页面没样式

`baseURL` 与实际域名不一致，或末尾少了 `/`。

### 18.4 图片不显示

- 路径必须写 `/images/xxx.png`
- 不能写 `static/images/xxx.png`
- 文件必须在 `static/images/` 下

### 18.5 代码块里的 `**` 变成加粗

正文里出现 `/**/` 这类内容时，会被 Markdown 误判。用行内代码包起来：

```markdown
(1) `/**/` 形式
```

### 18.6 本地能开、线上打不开

| 现象 | 原因 |
|---|---|
| 域名超时 | DNS 未生效或缓存，等 |
| 522 / 1016 | 自定义域名配置中，等几分钟 |
| 404 | 构建输出目录填错，应为 `public` |
| 白屏无样式 | baseURL 错误 |

### 18.7 push 失败

```powershell
Test-NetConnection github.com -Port 443
```

不通就开代理，或给 Git 配置代理。

---

## 19. 维护日历

| 频率 | 事项 |
|---|---|
| 每次写作 | commit + push |
| 每月 | 检查一次 Cloudflare 部署记录，确认没有失败 |
| 每季度 | `git tag` 打一个版本 |
| 每年 | 域名续费、导出 bundle、恢复演练一次 |

### 恢复演练

找另一台电脑：

```powershell
git clone https://github.com/你的用户名/blog.git
cd blog
hugo server -D
```

能跑起来，说明备份有效。

---

## 20. 隐私与安全清单

| 项目 | 建议 |
|---|---|
| 域名注册邮箱 | 长期可用，开启两步验证 |
| WHOIS 隐私 | 开启 |
| 域名转移锁 | 开启 |
| GitHub 提交邮箱 | 用 noreply 地址 |
| 仓库内容 | 不提交密钥、密码、内网地址 |
| 文章截图 | 发布前检查是否含个人信息 |
| 备份文件 | 不放公开仓库 |
| 公开仓库的提交历史 | 一旦推送，历史很难彻底删除 |

---

## 21. 可选进阶

| 功能 | 方案 | 成本 |
|---|---|---|
| 评论 | Giscus（基于 GitHub Discussions） | 免费 |
| 访问统计 | Cloudflare Web Analytics | 免费 |
| 站内搜索 | PaperMod 自带，需开启 JSON 输出 | 免费 |
| 域名邮箱 | Cloudflare Email Routing | 免费 |
| 文章目录 | `ShowToc = true` | 免费 |
| 多语言 | Hugo i18n | 需额外维护 |
| 国内加速 | 备案 + 国内服务器 + 分线路解析 | 约 ¥300/年 |

---

## 22. 附录：命令速查

```powershell
# 进入项目
cd D:\blog

# 新建文章
hugo new content posts/文章名.md

# 本地预览
hugo server -D

# 生产构建（本地验证）
hugo --gc --minify

# 提交与发布
git add .
git commit -m "post: 标题"
git push

# 版本标记
git tag -a v1.0 -m "说明"
git push --tags

# 导出备份
git bundle create backup.bundle --all

# 查看改了什么
git status
git diff

# 恢复某个文件
git checkout -- content/posts/xxx.md
```

### 发布前检查清单

- [ ] `draft = false`
- [ ] `slug` 是小写英文
- [ ] 日期不是未来
- [ ] 图片在 `static/images/`，路径以 `/images/` 开头
- [ ] 本地预览过，手机宽度也正常
- [ ] 没有复制带来的反斜杠

### 发布后检查清单

- [ ] 首页出现新文章
- [ ] 文章链接可访问
- [ ] Cloudflare 部署状态 Success
- [ ] 手机打开正常

---

## 结语

这套系统的价值不在于"能发文章"，而在于：

1. **内容属于你**：纯文本，在自己硬盘和仓库里
2. **链接长期有效**：只要不改 slug
3. **平台可替换**：托管商、DNS、甚至域名都可以换
4. **成本可控**：一年一个域名钱

真正需要长期维护的只有两件事：**记得续费域名**，**记得每年备份**。

其余时间，去写。

---

## 许可证

本文档可自由使用、修改、分发。建议以 MIT 或 CC BY 4.0 协议开源。
