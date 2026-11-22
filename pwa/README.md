# 反查背词 · PWA 发布包

单文件背单词应用的可安装版本（PWA）。所有功能都在 `index.html` 里，词库已内置，不依赖后端。

## 目录

```
pwa/
├── index.html              # 应用本体（与 ../vocab-recall-search.html 同步）
├── manifest.webmanifest    # 应用名、图标、主题色、独立窗口
├── sw.js                   # Service Worker：离线缓存
├── icons/icon-192.png
├── icons/icon-512.png
└── README.md
```

## 本机预览（手机同一 WiFi 就能访问）

```powershell
cd E:\word\pwa
python -m http.server 8080
```

电脑上打开 `http://localhost:8080`；手机浏览器打开 `http://<电脑局域网IP>:8080`（IP 用 `ipconfig` 看）。

注意：Service Worker 需要**安全上下文**（`https://` 或 `localhost`）。用局域网 IP（`http://192.168.x.x:8080`）在手机上打开时，应用能正常使用，但**不能安装、不能离线**；想真机安装就部署到 HTTPS（Vercel、Netlify、GitHub Pages 都自带）。双击 `file://` 打开同样只是普通网页。

## 正式部署

- **Vercel / Netlify**：把 `pwa` 文件夹拖进去即可，自动 HTTPS。建议把 `start_url` 保持 `./`。
- **GitHub Pages**：把 `pwa` 内容推到 `gh-pages` 分支根目录，访问 `https://<用户名>.github.io/<仓库>/`。
- 部署后手机打开：Android Chrome 菜单「添加到主屏幕 / 安装应用」；iOS Safari 分享 →「添加到主屏幕」。

## 数据在哪

学习进度、错词本、自建词库、打卡记录、日志都存在**本机 localStorage**，不上传。换设备或清缓存前，去「学习统计 → 数据备份 → 导出备份」，在新设备「导入备份」即可接着背。

## 更新流程

1. 改 `E:\word\vocab-recall-search.html`；
2. 重新复制成 `pwa/index.html`（保留其中的 manifest / Service Worker 注册片段）；
3. 把 `sw.js` 里的 `CACHE = "vocab-v1"` 改成下一个版本号，让老缓存失效；
4. 重新上传。

## 已知边界

- 词库为第三方整理的顺序词表，**仅建议个人学习与内测使用**；商用前必须确认词表与释义的授权。
- 目前是单机数据，没有账号与云同步；跨设备依赖手动备份。
- iOS 对 PWA 通知支持有限，打卡提醒只在打开 App 时出现（这本来就是设计取向：不打扰）。
- 首屏需要下载约 3MB 的 HTML（含 7 个词库），首次打开偏慢，后续走缓存。
