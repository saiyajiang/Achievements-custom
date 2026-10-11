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
| `package.json` → `build.appId` | `com.achievements.app` → `com.saiyajiang.achievements` | 避免与上游安装包冲突 |
| `package.json` → `build.copyright` | 保留 `Copyright © 2025 JokerVerse`，追加 fork 维护者说明 | MIT 要求保留原版权声明 |
| `README.md` | 顶部新增声明：个人自用 fork、非官方发布、改动由 AI 完成 | 避免他人误以为是官方版本 |

## 待办（需要时再处理）

**1. Windows 签名策略：`windows-signing-policy.json`**

该文件仍绑定上游作者的证书指纹：

- `certificateStoreSha1: 101E5C39...`
- `allowedCertificateSha256: ["503FA527..."]`

- **开发/自用（`npm start`）**：不受影响，签名校验只在 `app.isPackaged` 为 true 时启用。
- **若要打包分发**：原作者私钥你拿不到，必须用下面两种方式之一：
  - `npm run dist:unsigned`（关闭签名，最省事）
  - `npm run dist:self-signed`（生成本机自签证书），随后同步把新证书指纹写回 `windows-signing-policy.json`
- ⚠️ 注意：不要清空 `allowedCertificateSha256`。运行时校验会直接抛 `No valid Windows signing certificate pins configured`。

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
