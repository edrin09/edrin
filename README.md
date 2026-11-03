# 鹏云智码 · 官网

纯静态站点，没有后台、没有数据库、没有构建步骤。改内容只改 `content.json`。

## 部署到 GitHub Pages

1. 新建仓库，例如 `pengyun`
2. 把整个目录推上去（`index.html`、`content.json`、`uploads/` 必须在仓库**根目录**）
3. 仓库 Settings → Pages → Source 选 `Deploy from a branch`，分支 `main`、目录 `/ (root)`，保存
4. 等 1 分钟，访问 `https://edrin09.github.io/pengyun/`

命令行推送：

```bash
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin https://github.com/edrin09/pengyun.git
git push -u origin main
```

更新内容后同样 `git add . && git commit && git push`，Pages 会自动重建（约 30–60 秒）。

## 改内容

编辑 `content.json`，两个部分：

| 字段 | 说明 |
|---|---|
| `site` | 联系方式：名称、电话、邮箱、微信、地址 |
| `works` | 作品集，**数组顺序即页面展示顺序** |

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
uploads/       图片与视频素材
```
