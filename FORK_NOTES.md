# Fork 说明（saiyajiang/Achievements）

- 上游：`https://github.com/PSerban93/Achievements`（MIT 许可证）
- 本 fork 基线：上游 `main` @ `a43eaf3`（版本 `1.3.1-beta.2`，2026-10-04）
- Fork 时间：2026-10-12
- 上游同步状态：已与上游 `main` @ `a43eaf3` 拉齐（GitHub merge-upstream 返回 "This branch is not behind the upstream"）
- 改动来源：本 fork 中的修改由 AI 完成

## 已完成的改动

| 文件 | 改动 | 原因 |
|---|---|---|
| `package.json` → `build.publish.owner` | `PSerban93` → `saiyajiang` | 不改的话客户端会去**上游仓库**拉更新，等于把更新通道交给别人 |
| `package.json` → `build.publish.repo` | `Achievements` → `Achievements-custom` | 更新通道指向本 fork 仓库，GitHub Releases 版本控制生效 |
| `package.json` → `version` | `1.3.1-beta.2` → `0.0.1` | 版本号从零开始计数，脱离上游版本序列 |
| `package.json` → `build.appId` | `com.achievements.app` → `com.saiyajiang.achievements` | 避免与上游安装包冲突 |
| `package.json` → `build.copyright` | 保留 `Copyright © 2025 JokerVerse`，追加 fork 维护者说明 | MIT 要求保留原版权声明 |
| `README.md` | 顶部新增声明：个人自用 fork、非官方发布、改动由 AI 完成 | 避免他人误以为是官方版本 |
| `main.js` | changelog 服务 `owner/repo` → `saiyajiang/Achievements-custom` | 否则设置页更新日志会去**上游仓库**拉 GitHub Releases |
| `index.html` | 设置页 GitHub 链接、文档站链接 → 本 fork 仓库及中文文档站 | 避免外链跳转到上游 |
| `utils/github-changelog-service.js` | 默认 `owner/repo` → `saiyajiang/Achievements-custom` | 兜底默认值，防止未传参时指向上游 |
| `windows-signing-policy.json` | 上游证书指纹 → 本机自签证书（`CN=saiyajiang`） | 彻底脱离上游证书依赖，签名/校验走自己的证书 |
| `.github/FUNDING.yml` | 已删除 | 移除赞助入口，原作者信息仅在 README 保留 |
| `README.md` | 新增「遗留项与待改进」章节 | 记录仍保留上游实现的部分，后续按需改进 |
| `build-windows.workflow.yml` | 转 UTF-8 BOM | 解决 Windows 本地 PowerShell 读取中文乱码（文件本身 UTF-8 正常） |

## 待办（需要时再处理）

**1. ~~Windows 签名策略~~（已解决）**

`windows-signing-policy.json` 已改用本机自签证书（`CN=saiyajiang`）：
- `certificateStoreSha1: 8291402EE03B9112F38E8DC85A5EF5ABCDB614B7`
- `allowedCertificateSha256: ["03E4493A912A6EAF8ECB4A53C1E367D15BB073C97311EDC8C640913EEAFEC6BD"]`
- 打包分发用 `npm run dist:self-signed`（会校验证书匹配策略文件）
- ⚠️ 自签证书有效期至 **2031-10-12**，到期需重新生成 `New-SelfSignedCertificate -Type CodeSigningCert -Subject "CN=saiyajiang"` 并同步更新策略文件；换机器打包也需把证书导出导入
- 对外分发仍会提示"未知发布者"，属自签正常现象

**2. `build.win.requestedExecutionLevel: "requireAdministrator"`**

应用每次启动都以管理员权限运行。建议功能跑通后尝试改为 `asInvoker`，再验证浮层、快捷键是否仍正常。

## Windows 本地跑通

```bash
git clone https://github.com/saiyajiang/Achievements.git
cd Achievements
npm install
npm run dl-browsers        # 下载 Playwright Chromium（数百 MB），必需，否则无法抓取成就数据
npm start
```

需要成就录制 / HDR 截图时（需 Rust 工具链）：

```bash
npm run prepare:native:windows   # 编译 achievements-recorder.exe 与 achievements-hdr-screenshot.exe
```

## 已知坑

- **仓库里没有 `tests/` 目录**，`package.json` 里的 `npm run test:*` 脚本全部会失败，不要依赖现有测试。
- `tools/schema_parse.zip`（21 MB）是随仓库分发的第三方 PyInstaller 打包工具，运行时解压并调用其中的 `generate_emu_config.exe`，是杀软误报高发点；不用模拟器成就功能时可不启用。
- `main.js` 单文件 3.7 万行、`utils/watched-folders.js` 1.7 万行，改动前建议先按功能关键字定位，避免大范围重构。
- 部分 UI 字体走 Google Fonts（`fonts.googleapis.com`），离线环境会回退。

## 同步上游更新

```bash
git remote add upstream https://github.com/PSerban93/Achievements.git
git fetch upstream
git merge upstream/main        # 或用 git rebase upstream/main
```

合并后注意检查 `package.json` 的 `publish` / `appId` 是否被上游改动覆盖——这两项是 fork 的关键差异点。
