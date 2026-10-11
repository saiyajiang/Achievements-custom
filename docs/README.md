# Achievements 使用文档（中文版）

本目录是 Achievements 桌面应用的中文化使用文档，为纯静态站点，可通过 GitHub Pages 直接发布。

## 页面

| 页面 | 内容 |
| --- | --- |
| [`index.html`](index.html) | 总览与入口导航 |
| [`setup.html`](setup.html) | 快速上手、首次运行引导、手动配置、监视文件夹、自动配置 |
| [`platforms.html`](platforms.html) | Steam / Epic / Xbox PC / RetroAchievements / GOG Galaxy / Ubisoft Connect / EA Desktop / XLiveLessNess / Xenia / RPCS3 / ShadPS4 等平台与模拟器设置流程 |
| [`customization.html`](customization.html) | 面板、浮层、偏好设置、运行时路径、键盘与手柄 |
| [`troubleshooting.html`](troubleshooting.html) | 游戏兼容性、已知限制与解决办法 |

## 本地预览

无需构建，用任意静态服务器打开本目录即可，例如：

```bash
cd docs
python -m http.server 8000
# 浏览器访问 http://localhost:8000/index.html
```

## 来源与许可

翻译自 [PSerban93/Achievements-Docs](https://github.com/PSerban93/Achievements-Docs)
（原站：<https://pserban93.github.io/Achievements-Docs/>）。

原项目 **Achievements** 由 **JokerVerse** 开发，采用 MIT 许可。
本文档仅作中文翻译，界面截图与演示 GIF 仍引用原仓库的 releases 资源。

翻译与页面改动由 AI 完成。

## 与原版的差异

- 五个页面的正文、标题、导航与提示文案全部中文化。
- 保留原有锚点 id、外部链接、代码块、命令行与文件路径，跳转不会失效。
- `assets/style.css` 的字体栈追加了 `PingFang SC` / `Microsoft YaHei` / `Noto Sans CJK SC`，改善中文显示。
- 原版背景图 `Achievements.png`（约 4 MB）未随仓库迁移，已改为纯渐变背景，视觉几乎无差别。
