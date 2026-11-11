# 鹏云智码 · 官网

> 西安市未央区鹏云智码软件开发部的官方网站：一家只做**本地化 AI 交付**的小团队，把模型、智能体、技能库和多模态识别装进客户自己的机房。

一个单页站点，**没有后台、没有数据库、没有构建步骤、没有一个第三方依赖**。整站由 `index.html`（视图）+ `content.json`（内容）两个文件撑起来，改内容只动 JSON，推上去就发布。

![首屏](docs/preview.jpg)

![案例列表](docs/work.jpg)

| | |
|---|---|
| **线上地址** | https://edrin09.github.io/edrin/ |
| **托管** | GitHub Pages（`main` 分支根目录，`.nojekyll` 关闭 Jekyll） |
| **技术栈** | 原生 HTML / CSS / JavaScript，零依赖零构建 |
| **体量** | 1 个 HTML（约 100 KB，样式与脚本全内联）+ 1 个 JSON + 媒体目录 |
| **浏览器** | Chrome / Edge / Safari / Firefox 近两年版本；不支持 IE |

---

## 目录

- [这个仓库是什么](#这个仓库是什么)
- [页面结构](#页面结构)
- [为什么这么写](#为什么这么写)
- [目录结构](#目录结构)
- [改内容](#改内容)
- [本地预览](#本地预览)
- [部署](#部署)
- [提交前检查清单](#提交前检查清单)
- [推广自动化](#推广自动化)
- [常见问题](#常见问题)

---

## 这个仓库是什么

两件事合一：

1. **对外门面**——服务能力介绍 + 交付案例作品集，给潜在客户看的。
2. **案例的唯一数据源**——八个作品（BOKI 财务软件、朗读者 TTS、OfflineAI 本地大模型、AI 漫剧、数据采集、校园墙、慢病监测、自动签到）的标题、简介、技术栈、角色分工、截图、仓库链接，全部写在 `content.json` 里。想加案例，加一段 JSON。

内容与页面彻底分离，所以**改文案不需要碰 HTML，不会改坏样式**。

## 页面结构

单页八个区块，导航锚点直达：

| # | 区块 | 锚点 | 内容来源 |
|---|---|---|---|
| — | 首屏 | `#home` | `index.html` 写死（标题、四个指标数字、终端演示） |
| 01 | 能力 | `#capabilities` | `index.html` 写死 |
| 02 | 技能库 | `#skills` | `index.html` 内联数组（可切换的技能卡片） |
| 03 | 多模态识别 | `#vision` | `index.html` 内联（SVG 动画 + JSON 输出演示） |
| 04 | 本地化与安全 | `#security` | `index.html` 写死（深色区块） |
| 05 | 案例 | `#work` | **`content.json` → `works`** |
| 06 | 流程 | `#process` | `index.html` 写死 |
| 07 | 联系 | `#contact` | **`content.json` → `site`** |

也就是说：**日常只需要改 `content.json`**；只有改「能力/技能/多模态/流程」这类叙述性内容才需要动 `index.html`。

## 为什么这么写

几个刻意的选择，改之前先看一眼，别踩回去：

- **单文件、零依赖**：没有 npm、没有打包、没有 CDN 外链。断网、内网、老浏览器都不会白屏，也不会有供应链风险。代价是 `index.html` 大一点——但它 gzip 后不到 30 KB。
- **数据与视图分离**：运行时 `fetch("content.json")` 读取，不用重新构建页面。非技术人员也能改案例。
- **不做在线表单**：纯静态站没有接收端，表单提交必然收不到数据。联系区做成三个**直达入口**——一键拨号、写邮件、复制微信号（点一下进剪贴板，带 toast 提示）。
- **相对路径**：所有媒体写 `uploads/xxx.jpg`，项目部署在 `/edrin/` 子路径下也能正常显示，换域名不用改数据。
- **读不到数据也不瞎**：`content.json` 加载有 2.5 秒超时和 `catch` 兜底，失败时回落到 HTML 内联的联系方式，案例区显示「案例正在整理中」而不是一片空白或报错。
- **图片挂了有兜底**：`onerror` 会把裂图换成占位块，不会出现破图标。
- **尊重系统偏好**：`prefers-reduced-motion` 下所有动画降到 0.001ms；导航、折叠、灯箱都有 `aria-*` 与键盘操作（`Esc` 关灯箱、`Enter/Space` 复制微信号）。

## 目录结构

```
pengyun-site/
├─ index.html            页面本体（样式与脚本全部内联，单文件）
├─ content.json          内容数据：site 联系方式 + works 作品集
├─ favicon.png           浏览器标签图标
├─ uploads/              图片与视频素材（含 logo）
│  ├─ logo-128.png         页头页脚用 128×128，透明底
│  ├─ reel-*               能力演示区视频与海报帧
│  └─ work-*.jpg/.mp4      各案例的封面、图集、视频
├─ docs/                 README 用的截图（preview.jpg / work.jpg），与站点无关
├─ pengyun2026indexnow.txt  IndexNow 校验文件，必须能被站点根路径访问
├─ .nojekyll            告诉 GitHub Pages 别走 Jekyll
├─ .gitignore
└─ promote/             推广自动化脚本（本地工具，**不发布**，见下文）
```

## 改内容

### 联系方式

`content.json` 顶层 `site`：

| 字段 | 说明 |
|---|---|
| `name` | 全称，显示在页脚 |
| `shortName` | 简称（当前页面品牌名写死在 HTML 里，改品牌名需同时改 `index.html`） |
| `phone` / `phoneNote` | 电话与接听时段 |
| `email` | 邮箱 |
| `wechat` | 微信号，点击即复制 |

> 页面里 HTML 也留了一份同样的联系方式作为兜底（`index.html` 中 `FALLBACK.site`）。改电话/邮箱时**两处一起改**，否则 JSON 加载失败时会显示旧号码。

### 作品

`works` 是数组，**数组顺序 = 页面顺序**。每个作品：

| 字段 | 必填 | 说明 |
|---|---|---|
| `id` | ✅ | 唯一标识，随便起，不重复即可（如 `w_boki`） |
| `title` | ✅ | 作品名称 |
| `summary` | ✅ | 一句话简介，列表里显示这段。**桌面端只显示一行**（移动端两行），超出即省略号——控制在 50 字内最稳 |
| `detail` | | 展开后的项目概述，留空则用 `summary` |
| `tags` | | 技术栈。列表只显示前 3 个，展开后全部显示 |
| `role` | | 承担的工作，数组，渲染成列表 |
| `badge` | | 标题后的小标签（`PRODUCT` / `DESKTOP` / `MINIAPP`…），留空不显示 |
| `year` | | 年份，留空不显示 |
| `cover` | | 列表缩略图 + 展开后的封面 |
| `gallery` | | 图集，`["uploads/a.jpg", "uploads/b.jpg"]` |
| `video` | | mp4/webm/ogg/mov 直链，或 B 站链接（自动转播放器），或任意 iframe 地址 |
| `poster` | | 视频封面帧，不填用 `cover` |
| `videoPortrait` | | 竖屏（9:16）视频填 `true` |
| `links` | | 项目链接数组，`platform` 显示成小标签，可写多个平台 |

最小可用样例：

```json
{
  "id": "w_demo",
  "title": "作品名称",
  "badge": "PRODUCT",
  "year": "2026",
  "summary": "一句话说清它解决什么问题。",
  "tags": ["Vue 3", "NestJS", "SQLite"],
  "detail": "展开后看到的项目概述，可以写长一些。",
  "role": ["架构设计", "核心模块开发"],
  "cover": "uploads/work-demo.jpg",
  "gallery": ["uploads/work-demo-1.jpg", "uploads/work-demo-2.jpg"],
  "video": "",
  "links": [
    { "platform": "GitHub", "label": "源码仓库", "url": "https://github.com/edrin09/xxx" },
    { "platform": "Gitee", "label": "源码仓库", "url": "https://gitee.com/edrin/xxx" }
  ]
}
```

几个容易忽略的点：

- 旧写法 `"link": "https://..."` 仍兼容，等价于一个「访问项目」链接；同时存在时以 `links` 为准。
- `links` 里 `url` 为空的那条会被自动过滤，可以留半截占位。
- 缩写版字段别写错：是 `cover` 不是 `thumb`，是 `gallery` 不是 `images`——写错了不报错，只是不显示。
- JSON **不能有注释、不能有尾随逗号**。改完在 VS Code 里看一眼有没有红线，或用 `python -m json.tool content.json` 校验。

### 图片与视频规范

| 用途 | 建议尺寸 | 体积 | 备注 |
|---|---|---|---|
| 作品封面 `cover` | 1600×1000（16:10） | ≤ 300 KB | 列表缩略图按约 3:2 居中裁切，主体别贴边 |
| 图集 `gallery` | 4:3 | ≤ 300 KB / 张 | 有几张铺几张，自动占满一行 |
| 视频 `video` | 竖屏或 16:9 | ≤ 5 MB | 竖屏必须加 `"videoPortrait": true`，否则按 16:9 压扁 |

图片统一放 `uploads/`，路径写相对路径。**中文文件名可用，但别带空格和 `#`、`?`**——它们会被当成 URL 分隔符，导致 404。

## 本地预览

`content.json` 是运行时 `fetch` 的，直接双击 `index.html`（`file://` 协议）会被浏览器拦截，此时页面只显示联系方式，案例区为空——这是正常的，不是坏了。

用任意静态服务打开：

```bash
# 在仓库根目录
python -m http.server 8080
# 浏览器打开 http://localhost:8080
```

或者：

```bash
npx serve .          # Node
php -S localhost:8080  # PHP
```

## 部署

### 日常发布

本仓库 `main` 分支就是发布源，推上去即上线：

```bash
git add .
git commit -m "案例：更新 BOKI 截图"
git push
```

GitHub Pages 会在 30–60 秒后自动重建。**没变化通常就是没推上去**，先去 Actions 看构建记录。

### 从零部署到新仓库

1. 新建仓库，把整个目录推到**根目录**（`index.html`、`content.json`、`uploads/` 必须在仓库根）
2. Settings → Pages → Source 选 `Deploy from a branch`，分支 `main`、目录 `/ (root)`
3. 等 1 分钟，访问 `https://<用户名>.github.io/<仓库名>/`

### 换自定义域名（可选）

1. 仓库根目录放 `CNAME` 文件，内容是裸域名（如 `pengyun.example.com`）
2. 域名 DNS 加一条 `CNAME` 记录指向 `<用户名>.github.io`
3. Settings → Pages 勾选 `Enforce HTTPS`，等证书签发（通常几分钟）

因为全站用相对路径，**从子路径迁到独立域名不需要改任何数据**。

### 关于推送失败

若 `git push` 报 `schannel: failed to receive handshake`，是本机 Git 没走代理。带代理推：

```bash
git -c http.proxy="$http_proxy" -c https.proxy="$http_proxy" push
```

若 `push` 长时间没有任何输出、最后返回 `401 Unauthorized`，是系统凭据管理器里没有 GitHub 凭据、又不能弹窗输入。用令牌临时推（不写进 `.git/config`）：

```bash
git push "https://x-access-token:${GITHUB_TOKEN}@github.com/edrin09/edrin.git" main
```

## 提交前检查清单

发布前扫一眼，这四件事能挡掉九成的线上事故：

- [ ] `python -m json.tool content.json > /dev/null` 通过（JSON 语法没坏）
- [ ] 新加的图片/视频**确实存在**于 `uploads/`，大小写一致（Linux 服务器区分大小写）
- [ ] 本地 `python -m http.server 8080` 打开过，案例能展开、灯箱能翻页、视频能播
- [ ] 手机宽度（375px）下没有横向滚动条

## 推广自动化

`promote/` 是与站点**无关**的本地工具：每天定时对齐八个项目的 GitHub / Gitee 仓库门面、体检各项目 README、把站点 URL 提交给 IndexNow（Bing / Yandex）、记录 star 增长、生成推广日报。

它已写入 `.gitignore`，**不随站点发布**（所以 GitHub 上看不到 `promote/`，只有本地工作副本里有）。用法看 `promote/README.md`，渠道打法看 `promote/推广方案.md`。

## 常见问题

**改了 `content.json` 线上没变化？**
三查：`git status` 有没有提交；`git log origin/main -1` 远端是不是最新；浏览器强刷（`Ctrl+F5`）。`fetch` 带 `cache: "no-store"`，一般不是缓存问题。

**案例区一直显示「案例正在整理中」？**
说明 `content.json` 没被读到：要么用 `file://` 直接打开了，要么 JSON 语法坏了（`fetch` 解析失败会走兜底），要么文件名拼错（必须是根目录的 `content.json`）。

**封面图裂了 / 列表里出现灰色占位块？**
`cover` 路径大小写与实际文件不一致，或图片没提交（`.gitignore` 挡住了？）。`uploads/` 不在忽略列表里。所有图片（缩略图、封面、图集）加载失败都会自动换成占位块，不会出现破图标——看到占位块就是路径有问题。

**加了第九个案例，`promote/` 要不要同步？**
要。`promote/config.json` 的 `repos` 数组是另一份清单，加一条记录（含本地路径、仓库名、描述）才会纳入每日体检与门面同步。

**页面能加在线留言表单吗？**
不能只靠静态页。要表单就得接第三方（Formspree 之类）或自建后端——当前设计刻意避开，改用电话 / 邮件 / 微信三个直达入口。

**想加一个「博客」或「新闻」栏目？**
不建议在这个仓库里做。单页站点加内容型栏目会破坏现有结构，另起一个仓库或接静态站生成器更合适。

---

**联系**：15389232340（工作日 9:00 – 19:00）· y_p09@163.com · 微信 `PengYunZhiMa`
