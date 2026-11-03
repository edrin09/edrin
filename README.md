# 鹏云智码 · 官网

纯静态站点，没有后台、没有数据库、没有构建步骤。改内容只改 `content.json`。

线上地址：**https://edrin09.github.io/edrin/**
仓库：https://github.com/edrin09/edrin

## 部署

已部署完毕，本仓库 `main` 分支即发布源。改完内容推送即可：

```bash
git add .
git commit -m "更新内容"
git push
```

Pages 会在 30–60 秒后自动重建，无需额外操作。

换新仓库或迁移时的完整步骤：

1. 新建仓库，把整个目录推上去（`index.html`、`content.json`、`uploads/` 必须在仓库**根目录**）
2. Settings → Pages → Source 选 `Deploy from a branch`，分支 `main`、目录 `/ (root)`
3. 等 1 分钟，访问 `https://<用户名>.github.io/<仓库名>/`

> 若 `git push` 报 `schannel: failed to receive handshake`，是本机 Git 未走代理所致。当前网络需带代理推送：
> ```bash
> git -c http.proxy="$http_proxy" -c https.proxy="$http_proxy" push
> ```

## 改内容

编辑 `content.json`，两个部分：

| 字段 | 说明 |
|---|---|
| `site` | 联系方式：名称、电话、邮箱、微信 |
| `works` | 作品集，**数组顺序即页面展示顺序** |

> 联系区块是三个直达入口（拨号 / 写邮件 / 复制微信号），页面不放在线表单——纯静态站没有接收端，表单提交收不到数据。

每件作品的字段：

```json
{
  "id": "w_boki",              // 唯一标识，随便起，不重复即可
  "title": "作品名称",
  "badge": "PRODUCT",          // 右上角小标签，留空不显示
  "year": "",                  // 留空则不显示
  "summary": "一句话简介",      // 列表里显示这段
  "tags": ["Vue", "FastAPI"],  // 列表显示前 3 个，展开后全部显示
  "detail": "展开后的项目概述",
  "role": ["你负责的部分 1", "你负责的部分 2"],
  "cover": "",                 // 封面图，见下
  "gallery": [],               // 图集，写 ["uploads/a.jpg", "uploads/b.jpg"]
  "video": "",                 // 视频：mp4 直链或 B 站链接
  "poster": "",                // 视频封面帧，不填用 cover
  "videoPortrait": false,      // 竖屏视频填 true，按 9:16 显示
  "link": "https://..."        // 「访问项目」链接，留空不显示
}
```

**加图片**：把图片放进 `uploads/`，`cover` 写相对路径即可，例如 `"cover": "uploads/boki.png"`。

- 封面建议 1600×1000（16:10），图集建议 4:3，单张压到 300 KB 以内
- 图集有几张就铺几张，会自动占满一行
- 竖屏视频加 `"videoPortrait": true`，否则按 16:9 显示

## 本地预览

`content.json` 是运行时读取的，直接双击 `index.html`（file:// 协议）浏览器会拦截，此时只显示联系方式、作品集为空。用任意静态服务预览：

```bash
python -m http.server 8080
# 打开 http://localhost:8080
```

## 目录

```
index.html     页面（样式与脚本全部内联，单文件）
content.json   内容数据
favicon.png    浏览器标签图标
uploads/       图片与视频素材
  logo.png       品牌标识主图 480×480（透明底，供大尺寸/印刷）
  logo-128.png   页头与页脚用的 128×128（透明底）
```

## 品牌标识

标识已去白底、裁去留白，透明 PNG，可直接叠在任意底色上。页头与页脚使用 `uploads/logo-128.png`（显示尺寸 34px，覆盖 2× 屏），主图 `uploads/logo.png` 备用于更大尺寸的场景。

换 logo 时按同样规格出一张透明 PNG 覆盖这两个文件即可，不必改代码。若要调页头尺寸，改 `index.html` 里的 `.brand-mark{width:34px;height:34px}` 与 `<img>` 的 `width/height` 两处（页头与页脚）。
