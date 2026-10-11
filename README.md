# 🎮 Achievements

> [!NOTE]
> **个人自用 Fork —— 非官方发布版本。**
>
> 本仓库是 [PSerban93/Achievements](https://github.com/PSerban93/Achievements) 的 Fork，仅作个人自用维护，与原作者无任何关联或背书，此处不提供任何支持与担保。
>
> 本 Fork 中的所有修改均由 AI 完成。原项目仍为 MIT 许可，版权归原作者所有 —— 问题反馈与功能建议请前往上游仓库。
>
> 本说明文档由英文原文翻译而来。
>
> 📖 **完整使用文档（中文）**：<https://saiyajiang.github.io/Achievements-custom/> （源码见本仓库 [`docs/`](docs/index.html) 目录）

## 📌 遗留项与待改进

以下内容仍保留上游实现，后续按需改进：

| 项 | 现状 | 后续计划 |
|---|---|---|
| Uplay 游戏 ID 映射数据源 | `utils/match-uplay-steam.js` 仍从上游 `PSerban93/UPLAY_GAME_ID` 拉取映射表 | 数据仓库未 fork，暂作第三方数据源依赖；如上游停更可自维护一份 |
| 文档站链接 | `docs/` 内部分页面保留上游项目链接 | 属正常文档引用，无需处理 |
| 代码签名 | 已改用本机自签证书（`CN=saiyajiang`），指纹已写入 `windows-signing-policy.json`，不再依赖上游证书 | 自签证书有效期至 **2031-10-12**，到期需重新生成并同步更新策略文件 |
| 管理员权限 | `build.win.requestedExecutionLevel` 仍为 `requireAdministrator` | 功能跑通后尝试降为 `asInvoker` 并验证浮层/快捷键 |
| 赞助入口 | `.github/FUNDING.yml` 已移除；原作者信息与版权仅在 README 保留 | 无 |

一个基于 Electron 的桌面应用，用于监控正在运行的游戏并显示成就通知，支持：

- ✅ 成就解锁
- ⏱️ 游戏时长统计（正在游玩 / 已游玩 X 分钟）
- 📈 进度更新
- 🖼️ 游戏图片浮层
- 📊 实时成就面板
- Steam / Uplay / GOG / Epic 模拟器、Xbox PC、RetroAchievements、XLiveLessNess、MarkerPatch、MadnessPatch、《最终幻想 VII》`achievement.dat` 以及官方启动器成就表支持（尽可能自动识别）

**运行平台：** Windows（使用任务计划程序 + Windows 路径）。

## ☕ 支持作者

如果你想进一步支持原项目，可以通过 Ko-fi 请作者喝杯咖啡：

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/V7V81U42NF)

## ✨ 功能特性

- **成就追踪**
  - 通过进程名检测正在运行的游戏
  - 使用自定义 HTML/CSS 动画发送通知
  - 实时监控并更新进度
  - 支持来自 Online-Fix `Stats.ini` 和 Tenoke `user_stats.ini` 的进度统计
  - 成就解锁时可截图（可选）
  - 可选录制成就视频片段，支持 10–30 秒时长、30/60 FPS 及系统音频
  - 可选 GPU HDR 转 SDR 转换，使录制的 H.264/MP4 片段兼容标准 SDR 播放器
- **智能面板**
  - 所有已配置游戏的网格视图
  - 每个游戏的实时进度追踪
  - 多种排序方式：
    - 字母顺序（A-Z / Z-A）
    - 进度（低-高 / 高-低）
    - 最近更新（新-旧 / 旧-新）
  - 快速游戏搜索与筛选
  - 按平台筛选，并支持多选进行忽略/删除操作
  - 自定义游戏合集以面板卡片形式展示，支持"仅配置"、"仅合集"或合并视图；打开合集可查看其游戏、多选添加成员，并选择颜色、图标或自定义图片
  - 点击即可加载配置
  - 游戏启动按钮（需配置可执行文件及可选参数）
  - 当配置或存档文件变化时自动刷新
- **通知系统**
  - 多种通知类型：
    - 成就解锁
    - 进度更新
    - 游戏时长追踪（正在游玩 / 会话结束）
  - 动画预设、Windows 原生预设，以及通过可选的 Game Bar 小组件 [Achievements Overlay by JokerVerse] 发送的原生 Xbox Game Bar 通知
  - Game Bar 推送支持成就解锁、稀有度/白金元数据、点击跳转以及文本进度（`当前 / 最大值 • 百分比`）
  - Game Bar 伴生组件为可选项；未安装或未连接时，Electron 应用仍可独立运行并使用已配置的回退方案
  - 可选的奖杯模式：稀有度低于 20% 判定为金杯，20% 至低于 50% 为银杯，50% 及以上为铜杯；全部成就完成时保留专属的白金通知样式
  - 奖杯模式会在通知、主成就表格和浮层中的成就图标周围显示对应等级颜色；铜/银/金通知使用 Rare 样式
  - 可自定义音效与视觉预设
  - 可调位置、持续时间和缩放比例（预设最高支持 200%）
  - 非侵入式浮层系统
  - 可选点击跳转到浮层或主应用中对应的成就
  - 游戏时长头部图片本地缓存，加快重复通知的显示
  - 按游戏静音进度通知（当配置处于激活状态时）
- **游戏时长统计**
  - 通过进程监控检测已配置游戏的启动与退出
  - 将每个配置的总时长存储在 `%APPDATA%/Achievements/playtime-totals.json`
  - 在成就面板中显示总游戏时长
  - 触发由 `playtime.html` 渲染的专属通知
- **个性化**
  - 带标签页的现代化设置界面
  - 内置 GitHub 发布更新日志，支持缓存离线回退
  - 实时应用日志查看器，支持按来源和严重级别筛选
  - 多种视觉主题/预设
  - 启动选项（最大化/最小化）
  - 界面缩放（75% 至 200%）
  - 成就持续时间滑块（自动或自定义）
  - 成就音量（0% 至 200%）
  - 显示隐藏描述（若可用）
  - 关闭到托盘选项
  - 当数据源支持本地化时，可选择生成哪些成就表语言
  - 浮层可选支持手柄（`设置 -> 高级 -> 渲染`）
  - 成就多语言支持
- **档案备份与恢复**
  - 从 **设置 -> 备份与恢复** 导出完整的便携式档案
  - 通过 Zstandard 压缩的 TAR 容器流式备份，无需将整个档案载入内存
  - 一次操作即可恢复配置、自定义合集及其图片、成就缓存、封面、图片、预设、音效、主题、游戏时长、偏好设置、账号导入元数据和面板摘要
  - 替换当前档案前可预览备份版本与内容
  - 在监视器启动前应用恢复，迁移期间抑制成就通知
  - 将档案移动到另一个 Windows 账户或电脑时，会重写内部档案路径并刷新面板指纹
  - 认证令牌文件被有意排除；已连接的服务可能需要重新登录
  - 备份压缩包未加密，可能包含 Steam Web API 密钥等敏感设置

## 📁 项目结构

| 文件/文件夹                                         | 说明                                                             |
| --------------------------------------------------- | ---------------------------------------------------------------- |
| `main.js`                                           | Electron 主进程：窗口处理、核心逻辑                             |
| `preload.js`                                        | IPC 桥接与渲染进程 API                                           |
| `utils/playtime-log-watcher.js`                     | 追踪游戏启动/退出并计算总时长                                    |
| `index.html`                                        | 主界面，含面板与配置管理                                         |
| `overlay.html`                                      | 成就通知浮层                                                     |
| `san-notification.html`                             | 动画成就通知渲染器                                               |
| `playtime.html`                                     | 游戏时长通知模板                                                 |
| `progress.html`                                     | 进度通知模板                                                     |
| `tray-menu.html/js/css`                             | 托盘菜单界面与逻辑                                               |
| `playtime-totals.json`                              | 运行时生成的时长统计（`%APPDATA%/Achievements/`）                |
| `preferences.json`                                  | 运行时设置（`%APPDATA%/Achievements/`）                          |
| `LICENSE`                                           | 项目许可证文件                                                   |
| `package.json`                                      | Node.js 依赖与脚本                                               |
| `README.md`                                         | 本说明文档                                                       |
| `style.css`                                         | 所有界面组件的全局样式                                           |
| `assets/`                                           | 静态资源：                                                       |
| `assets/steamdb.json`                               | Steam 数据库缓存                                                 |
| `assets/uplay-steam.json`                           | Uplay 到 Steam 的映射表                                          |
| `assets/locales/`                                   | 界面翻译文件                                                     |
| `assets/san-runtime/`                               | 动画通知所需的打包运行时资源                                     |
| `assets/vendor/fontawesome/`                        | Font Awesome 图标                                                |
| `build/`                                            | 构建脚本与清单文件                                               |
| `build/prepare-playwright-browsers.js`              | 安装并校验完整的 Playwright Chromium 运行时                      |
| `fonts/`                                            | 字体文件与许可证                                                 |
| `presets/`                                          | `Default Presets` 与 `Users Presets` 主题                        |
| `sounds/`                                           | 通知音效资源                                                     |
| `utils/`                                            | 辅助模块与工具：                                                 |
| `utils/auto-config-generator.js`                    | 从存档目录自动生成游戏配置                                       |
| `utils/generate_achievements_schema.js`             | 生成多平台成就表                                                 |
| `utils/watched-folders.js`                          | 监视器 + 自动选择 + 自动配置                                     |
| `utils/steam-appcache*.js`                          | Steam 官方 appcache 解析 + 成就表构建                            |
| `utils/exophase-scraper.js`                         | 从 Exophase 多语言抓取                                           |
| `utils/xenia-*`                                     | Xenia 解析 + 成就表生成                                          |
| `utils/rpcs3-*`                                     | RPCS3 解析 + 成就表生成                                          |
| `utils/shadps4-*`                                   | PS4 奖杯解析 + 成就表生成                                        |
| `utils/achievement-data.js`                         | 成就数据处理                                                     |
| `utils/achievement-rarity.js`                       | 成就稀有度计算                                                   |
| `utils/trophy-mode.js`                              | 成就通知的奖杯模式等级判定                                       |
| `utils/app-navigation.js`                           | 应用启动参数与导航路由                                           |
| `utils/atomic-json-store.js`                        | 原子化 JSON 写入与备份恢复                                       |
| `utils/blacklist-identity.js`                       | 全局/平台黑名单身份处理                                          |
| `utils/config-deletion-guard.js`                    | 防止删除期间配置被重建                                           |
| `utils/config-deletion-paths.js`                    | 校验可选的存档/成就表删除目标                                    |
| `utils/config-name.js`                              | 安全配置名与 JSON 路径解析                                       |
| `utils/config-platform-migrator.js`                 | 配置在不同平台间迁移                                             |
| `utils/content-version.js`                          | 内容版本控制工具                                                 |
| `utils/game-collections.js`                         | 自定义合集的原子化存储与游戏成员管理                             |
| `utils/game-collection-images.js`                   | 自定义合集图片的校验与本地存储                                   |
| `utils/controller-input-manager.js`                 | 手柄输入处理                                                     |
| `utils/ea-desktop-local.js`                         | EA Desktop 本地集成                                              |
| `utils/epic-api.js`                                 | Epic Games API 集成                                              |
| `utils/epic-auth.js`                                | Epic 认证                                                        |
| `utils/epic-identity.js`                            | Epic artifact/AppID 身份回退                                     |
| `utils/epic-local-installations.js`                 | Epic 本地安装检测                                                |
| `utils/epic-official.js`                            | Epic 官方成就                                                    |
| `utils/xbox-pc.js`                                  | Xbox App PC 发现与 Xbox Network 直接同步                         |
| `utils/fileCopy.js`                                 | 文件复制工具                                                     |
| `utils/game-cover.js`                               | 游戏封面图片处理                                                 |
| `utils/gog-auth.js`                                 | GOG 认证                                                         |
| `utils/gog-galaxy-local.js`                         | GOG Galaxy 本地集成                                              |
| `utils/i18n-ui.js`                                  | 界面国际化                                                       |
| `utils/local-game-name-cache.js`                    | 本地游戏名缓存                                                   |
| `utils/logger.js`                                   | 日志工具                                                         |
| `utils/log-viewer-service.js`                       | 有界实时日志追踪、轮转处理与订阅                                 |
| `utils/github-changelog-service.js`                 | GitHub 发布信息获取，带原子缓存与离线回退                        |
| `utils/lumaplay-event-watcher.js`                   | LumaPlay 原生注册表变更监视器                                    |
| `utils/lumaplay-registry.js`                        | LumaPlay 注册表处理                                              |
| `utils/markerpatch.js`                              | 《死亡空间 2》MarkerPatch 检测与位标志解析                       |
| `utils/madnesspatch.js`                             | 《爱丽丝：疯狂回归》MadnessPatch 检测、成就表与档案位标志解析    |
| `utils/ff7-achievement-dat.js`                      | 《最终幻想 VII》旧版 achievement.dat 检测与解析                  |
| `utils/xlivelessness.js`                            | XLiveLessNess 发现、配置生成与 DAT 状态解析                      |
| `utils/xlivelessness-spa.js`                        | XLiveLessNess PE SPAFILE/XDBF 元数据与图片解析                   |
| `utils/xlivelessness-worker.js`                     | XLiveLessNess 可执行文件异步发现工作线程                         |
| `utils/retroachievements.js`                        | RetroAchievements Web API 认证、导入与轮询                       |
| `utils/adaptive-path-watcher.js`                    | 延迟创建的本地成就路径监控                                       |
| `utils/match-uplay-steam.js`                        | Uplay 到 Steam 的匹配                                            |
| `utils/native-windows-notification-navigation.js`   | 原生 Toast 激活路由                                              |
| `utils/overlay-controller-service.js`               | 浮层手柄服务                                                     |
| `utils/overlay-shortcut-manager.js`                 | 浮层快捷键管理                                                   |
| `utils/parseStatsBin.js`                            | Stats 二进制解析                                                 |
| `utils/paths.js`                                    | 路径工具                                                         |
| `utils/playtime-store.js`                           | 游戏时长数据存储                                                 |
| `utils/profile-backup.js`                           | 校验式流式档案备份、暂存与恢复                                   |
| `utils/profile-backup-worker.js`                    | 脱离主线程的 TAR + Zstandard 备份创建与校验                      |
| `utils/playwright-runtime.js`                       | Playwright 运行时解析                                            |
| `utils/process-event-watcher.js`                    | 进程事件监控                                                     |
| `utils/process-config-match.js`                     | 进程到配置的匹配                                                 |
| `utils/process-native-host.js`                      | 隔离的原生进程监视宿主                                           |
| `utils/process-name-utils.js`                       | 进程名工具                                                       |
| `utils/process-poller.js`                           | 进程轮询                                                         |
| `utils/pslist-wrapper.mjs`                          | PS list 封装                                                     |
| `utils/raw-hid-controller-hub.js`                   | 原生 HID 手柄中枢                                                |
| `utils/raw-hid-controller-worker.js`                | 原生 HID 手柄工作线程                                            |
| `utils/raw-hid-profiles.js`                         | 原生 HID 配置文件                                                |
| `utils/rpcs3-config-generator.js`                   | RPCS3 配置生成                                                   |
| `utils/rpcs3-trophy.js`                             | RPCS3 奖杯处理                                                   |
| `utils/shadps4-config-generator.js`                 | ShadPS4 配置生成                                                 |
| `utils/shadps4-trophy.js`                           | ShadPS4 奖杯处理                                                 |
| `utils/startup-task.js`                             | 启动任务管理                                                     |
| `utils/steam-appcache-generator.js`                 | Steam appcache 生成                                              |
| `utils/steam-appcache.js`                           | Steam appcache 处理                                              |
| `utils/steam-local-users.js`                        | Steam 本地用户                                                   |
| `utils/steam-schema-parse.js`                       | 内置 Steam 成就表工具运行时与生成                                |
| `utils/steamdb-launch-metadata.js`                  | SteamDB 启动元数据                                               |
| `utils/ubisoft-connect-local.js`                    | Ubisoft Connect 本地集成                                         |
| `utils/windows-process-native-provider.js`          | 原生 Windows 进程快照提供者                                      |
| `utils/xenia-config-generator.js`                   | Xenia 配置生成                                                   |
| `utils/xenia-gpd.js`                                | Xenia GPD 处理                                                   |

## 🛠️ 安装

1. 安装 [Node.js](https://nodejs.org) 和 [Git](https://git-scm.com)。
2. 克隆本仓库：
   ```bash
   git clone https://github.com/saiyajiang/Achievements.git
   cd Achievements
   ```
3. 安装依赖：
   ```bash
   npm install
   ```
4. 安装用于成就表抓取和本地运行的完整 Playwright Chromium 运行时：

   ```bash
   npm run dl-browsers
   ```

   该命令会先安装并校验当前的完整 Chromium 版本，然后再清除过时的 Chromium 版本和 Headless Shell 变体。抓取默认使用 Chromium 的新型无头模式，同时仍保留手动成就表生成器的 `--headed` 模式。

## 🚀 运行应用

```bash
npm start
```

### 应用启动参数

通过向已安装或已解包的可执行文件同时传入 AppID 和平台，可直接打开一个已存在的配置：

```powershell
Achievements.exe --appid=239140/steam
Achievements.exe --appid=239140 --platform=steam
```

- 两种格式都支持且等价。
- 如果 Achievements 已在运行，则会打开已有实例并跳转到匹配的配置。
- AppID 和平台必须与某个已存在的配置完全匹配。这些参数不会生成配置，也不会启动游戏。
- 支持的平台值：`steam`、`steam-official`、`uplay`、`ubisoft-official`、`ea-official`、`epic`、`epic-official`、`gog`、`gog-official`、`xbox-pc`、`retroachievements`、`xenia`、`rpcs3`、`shadps4`、`markerpatch`、`madnesspatch`、`xlivelessness`。

## 🧱 构建 Windows 可执行文件

创建未打包的 Windows 构建：

```bash
npm run pack
```

创建 Windows 安装程序：

```bash
npm run dist
```

构建产物生成在 `dist/` 文件夹中。构建脚本会校验原生进程监视器和 SAN 通知运行时。`npm run pack` 和 `npm run dist` 都会在打包前自动安装并校验完整的 Playwright Chromium。不打包 Chromium Headless Shell。

## 📦 依赖项

### 核心

- [Electron](https://electronjs.org) —— 跨平台桌面应用框架
- [@vscode/windows-process-tree](https://www.npmjs.com/package/@vscode/windows-process-tree) —— 原生 Windows 进程监控
- [Koffi](https://www.npmjs.com/package/koffi) —— 本地检测服务使用的原生 Windows API 绑定
- [ps-list](https://www.npmjs.com/package/ps-list) —— 有限的进程监控回退方案
- [crc-32](https://www.npmjs.com/package/crc-32) —— 校验和计算

### 成就处理

- [Playwright](https://playwright.dev) —— 用于成就抓取的浏览器自动化，打包完整 Chromium，默认使用其新型无头模式
- [axios](https://www.npmjs.com/package/axios) —— 用于平台 API 和元数据服务的 HTTP 客户端
- [cheerio](https://www.npmjs.com/package/cheerio) —— HTML 解析

### 功能

- [screenshot-desktop](https://www.npmjs.com/package/screenshot-desktop) —— 可选的成就截图捕获
- [windows-capture](https://github.com/NiiightmareXD/windows-capture) —— Windows Graphics Capture 与硬件加速 H.264 编码，用于可选的成就视频片段和 HDR 截图。内置录制器使用本地打过补丁的 MIT 许可 2.0.1 源码快照，以便将 GPU 色调映射表面直接传给编码器。
- [@xboxreplay/xboxlive-auth](https://www.npmjs.com/package/@xboxreplay/xboxlive-auth) —— 微软/Xbox Network 认证
- [ini](https://www.npmjs.com/package/ini) —— 配置文件解析

### 通知自定义灵感来源

通知自定义中的 SAN 风格模板与主题工作流是为 Achievements 独立实现的，设计灵感致谢 [Steam Achievement Notifier](https://github.com/SteamAchievementNotifier/SteamAchievementNotifier)。已有的 Achievements 预设仍独立受支持。

### 后台服务

- `chokidar` 持续监视配置/存档目录，以触发界面刷新
- `@vscode/windows-process-tree` 运行在隔离的 Electron 工具进程中，提供进程事件，并在需要时提供命令行数据
- `ps-list`（通过 `utils/pslist-wrapper.mjs`）校对原生快照，并作为有限的回退方案持续生效
- `achievements-recorder.exe` 仅在启用成就录制时运行，在磁盘上保留有界滚动的视频/音频分段，并通过 WASAPI 回环捕获 Windows 默认输出混音（不捕获麦克风输入；音频不可用时安全回退为仅视频）
- 原生快照约每 1 秒一次；回退检测约 2 秒一次，混合校对约 12 秒一次
- 在 **设置 -> 高级** 中禁用原生进程监视器后，仍会通过有限的 `ps-list` 回退保持可执行文件名检测；该模式下无法获取命令行参数
- 由运行中的可执行文件自动选中的配置，会在其最后一个匹配进程关闭时被清除；手动选择或由存档文件夹监视器选中的配置保持选中状态
- 设置中的日志查看器使用文件系统监视器，仅在 **日志** 标签页处于活动状态时辅以轻量轮询；标签页或设置关闭时两者都会被释放
- GitHub 发布说明按需从公开仓库 API 获取，并以原子方式缓存以供离线复用

## 🎮 设置与配置

### 快速上手（教程）

1. 打开 **设置**，设定预设和缩放比例。
2. 添加你的 **监视文件夹**（推荐），以便应用能检测到存档/模拟器目录。
3. 先启动一次游戏，使其存档文件夹出现；监视器会在可能的情况下自动创建配置。
4. 让游戏识别并自动选中配置，或手动选择配置，设置你的语言以查看成就、进度和游戏时长。
5. 可选：使用配置下拉框下方的复选框，为某个配置静音进度通知。

### 首次运行引导（自动配置开关）

- 首次运行时（或引导版本变化后），启动会暂停并在完整自动扫描开始前显示文件夹选择弹窗。
- 应用会搜索已知的成就/存档特征文件（例如 `achievements.json`、`achievements.ini`、`stats.bin`、模拟器奖杯/gpd 文件）并列出候选文件夹。
- **开始自动配置** 会保持所选文件夹为激活状态，并静音未勾选的已发现文件夹，然后继续启动与后台配置生成。
- **跳过并全部静音** 会立即继续启动，并静音已发现的/默认的自动配置根目录。
- 引导待处理期间，文件夹监视器和启动自动配置扫描会被有意推迟，以避免产生不需要的自动生成。
- 引导完成状态保存在 `%APPDATA%/Achievements/preferences.json`（`autoConfigOnboardingCompleted`、`autoConfigOnboardingVersion`、`autoConfigOnboardingCompletedAt`）。
- 若弹窗不可见但启动被阻塞，请使用托盘操作 **恢复启动（全部静音）**。

### 基础设置

#### 手动配置

1. 使用以下项创建新配置：
   - **名称**：你偏好的标识符
   - **AppID**：Steam AppID 或成就所在文件夹名
   - **配置路径**：achievements.json 与图片所在位置 / 留空则自动生成
   - **存档路径**：成就进度存储位置
   - **可执行文件**（可选）：游戏可执行文件的直接路径
   - **参数**（可选）：启动参数
   - **进程名**（可选）：要监控的具体 .exe 名称
   - _注意_：名称在保存前会被净化处理（移除非法文件名字符、压缩连续空格）；磁盘上和游戏时长统计中使用的都是净化后的名称。

**配置 JSON 字段（参考）：**

- `appid`（字符串）—— 游戏 ID
- `platform`（字符串）—— steam/uplay/gog/gog-official/epic/epic-official/xbox-pc/retroachievements/xenia/rpcs3/shadps4/markerpatch/madnesspatch/xlivelessness/steam-official/ubisoft-official/ea-official
- `config_path`（字符串）—— 包含 `achievements.json` 和 `img/` 的文件夹
- `save_path`（字符串）—— 存档/成就进度所在位置
- `process_name`（字符串）—— 用于进程追踪的可执行文件名
- `executable` / `arguments`（可选）—— 用于启动游戏

_注意_：如果 `config_path` 指向自定义位置，成就表重新生成/清理不会覆盖该文件夹。

#### 自动配置

1. 使用 **监视文件夹**（推荐）扫描你的模拟器/存档目录。
2. 应用会：
   - 检测 AppID 与平台，
   - 获取游戏名称与成就表，
   - 在可用时下载成就数据与图片，
   - 自动生成配置。
3. 默认监视文件夹包括：
   - %PUBLIC%\Documents\Steam\CODEX
   - %PUBLIC%\Documents\Steam\RUNE
   - %PUBLIC%\Documents\OnlineFix
   - %PUBLIC%\Documents\EMPRESS
   - %APPDATA%\Goldberg SteamEmu Saves
   - %APPDATA%\Goldberg UplayEmu Saves
   - %APPDATA%\R1 UplayEmu Saves
   - %APPDATA%\GSE Saves
   - %APPDATA%\EMPRESS
   - %LOCALAPPDATA%\anadius\LSX emu\achievement_watcher
   - %APPDATA%\Steam\CODEX
   - %APPDATA%\SmartSteamEmu
   - %LOCALAPPDATA%\SKIDROW
   - %LOCALAPPDATA%\UniverseLAN

**注意**：当在设置中提供了密钥时，自动配置会使用 Steam Web API。没有密钥时，会回退到 SteamDB/SteamHunters + 来自 Exophase 的语言。可用时使用的数据源：Steam Web API、SteamDB、SteamHunters、Exophase、GOG、Epic。
`Goldberg UplayEmu Saves` 和 `R1 UplayEmu Saves` 会明确地走 Uplay 到 Steam 的映射流程，同时保留原始的 Uplay AppID 和 Uplay 成就表存储。
`%LOCALAPPDATA%\UniverseLAN` 被视为严格的 GOG 根目录。每个直接的 `<ProductID>` 目录都会映射为一个 GOG 配置，运行时进度从 `<ProductID>\UniverseLANData\Achievements.ini` 监控。旧版游戏本地的 UniverseLAN 安装继续使用 `UniverseLAN.ini` 发现机制。

RUNE 的 Ubisoft Connect 存档作为可选加入的监视文件夹受支持。请在 **设置 -> 文件夹** 中手动添加 `%USERPROFILE%\Documents\RUNE\Ubisoft Connect`（或其 `achievements` 子文件夹）。应用读取 `<root>\achievements\<UserID>\<Uplay AppID>\achievements.cfg`，通过现有的 Uplay 成就表/映射流程处理，并忽略同级的 `savegames` 目录树。此路径有意未加入默认监视文件夹列表。

#### 文件夹重新扫描与黑名单

- **文件夹 -> 重新扫描** 会打开一个包含当前激活监视文件夹的选择弹窗。可用 **全选** 或 **全不选**，然后只扫描选中的根目录。
- 已忽略的文件夹和已忽略的嵌套文件夹，即使其父级监视文件夹被选中，也仍然排除在外。
- 重新扫描不会清空文件夹忽略列表或 AppID 黑名单。
- 从配置/面板忽略某个已配置游戏时，使用的是其 AppID + 平台身份，因此同一 AppID 的其他平台仍可保持激活。
- **设置 -> 高级 -> 添加黑名单 AppID** 可全局添加一个或多个 AppID，包括被错误识别的 UserID。多个值用逗号、空格或换行分隔。
- **重置黑名单** 会移除已保存的全局及平台专属黑名单条目。它不会移除监视文件夹，也不会重新扫描无关的配置。

#### Steam 模拟器进度文件

- Online-Fix 的解锁状态从 `Achievements.ini` 读取；`Stats.ini` 仅提供已映射的成就进度和进度通知。
- 空的 Online-Fix `Stats.ini` 是有效的并保持被监控。它不会解锁成就或清空已有成就缓存；后续写入的统计值会被应用。
- Tenoke 从 `user_stats.ini` 读取解锁状态和统计数据。统计值会在可用时通过成就的 `operand1` 值映射到成就表进度规则。
- 统计数据本身永远不会将成就标记为已解锁；模拟器的成就状态始终是解锁的唯一依据。

#### LumaPlay 支持

1. 启用 **设置 -> 文件夹 -> 启用 LumaPlay 监视器**。
2. 应用会扫描 `HKCU\SOFTWARE\LumaPlay` 下的成就条目，并在可能时生成带 `emu=lumaplay` 的对应 `uplay` 配置。
3. 启用期间，原生注册表变更事件会触发成就刷新和通知，无需反复轮询整个注册表树。

**重要说明：**

- LumaPlay 注册表监视器仅限 Windows，且仅在启用该选项时启动。
- 禁用此选项会停止 LumaPlay 注册表监控，但不会禁用监视文件夹监控或常规进程检测。

#### Xenia-Canary 支持

1. 打开 Xenia 并创建一个用户配置文件。
2. 使用 **监视文件夹** 添加 'Xenia 安装位置'\Content/xxxxxx/xxxx/xxxx/xxxxxx' 文件夹，该文件夹在 Xenia 中创建账号后生成。
3. 启动并游玩游戏。
4. 应用会：
   - 读取 Xenia 创建的文件，
   - 获取游戏名称、成就表和图片。
   - 自动生成配置。
   - 解锁新成就时显示通知。

#### RPCS3 支持

1. 使用 **监视文件夹** 添加 'RPCS3 安装位置\dev_hdd0\home\xxxxxxx\trophy' 文件夹，该文件夹在 RPCS3 配置完成后生成。
2. 启动并游玩游戏。
3. 应用会：
   - 读取 RPCS3 创建的文件，
   - 获取游戏名称、成就表和图片。
   - 自动生成配置。
   - 解锁新成就时显示通知。

#### ShadPS4 支持

1. 使用 **监视文件夹** 添加 ShadPS4 根文件夹：`%APPDATA%\shadPS4`。
2. 启动并游玩游戏，使 ShadPS4 创建奖杯表和用户进度文件。
3. 应用会：
   - 从 `%APPDATA%\shadPS4\trophy\<NPWR>\Xml` 读取成就表，
   - 从 `%APPDATA%\shadPS4\trophy\<NPWR>\Icons` 复制奖杯图标，
   - 从 `%APPDATA%\shadPS4\home\<userId>\trophy\<NPWR>.xml` 读取解锁进度，
   - 当本地 ShadPS4 日志或旧版数据提供映射时，将 NPWR 奖杯 ID 映射到 CUSA 游戏 ID，
   - 自动生成配置，
   - 为每个 ShadPS4 用户保留独立的成就缓存文件，
   - 通过监控所选游戏的所有本地用户进度 XML 文件来检测用户切换，
   - 解锁新成就时显示通知。

**重要说明：**

- 现代 ShadPS4 存储基于 `%APPDATA%\shadPS4\trophy\<NPWR>`（成就表/图标）和 `%APPDATA%\shadPS4\home\<userId>\trophy\<NPWR>.xml`（进度）。
- `%APPDATA%\shadPS4\game_data\<CUSA>\TrophyFiles\trophy00` 下的旧版 ShadPS4 存储仍受支持，但当两者同时存在时优先使用现代奖杯/进度布局。
- 如果存在多个 ShadPS4 用户，缓存按用户隔离，因此切换用户不会覆盖另一个用户的成就状态。

#### 《最终幻想 VII》`achievement.dat` 支持

1. 在《最终幻想 VII》游戏目录中，添加 `steam_appid.txt`，其中仅包含 `39140`。
2. 在 **设置 -> 文件夹** 中，添加该游戏目录。
3. 仅当 `steam_appid.txt`、`ff7input.cfg`、`ff7sound.cfg` 和 `ff7video.cfg` 同时存在时，检测才会激活。
4. 应用会为 AppID `39140` 生成独立的 Steam 配置和成就表，然后从相邻的 `achievement.dat` 读取解锁位。

**重要说明：**

- 仅有 `steam_appid.txt` 会被忽略，因此普通游戏文件夹和其他平台流程不受影响。
- `achievement.dat` 在配置生成期间是可选的。自适应监视器会等待游戏创建它。
- 若文件已存在，已有解锁会被静默导入。若文件是在监控开始后才首次创建的，则新设置的位会生成通知，并在缓存中记录本地检测时间。
- 该文件不包含历史解锁时间戳；因此此前已解锁但未缓存时间戳的成就不会显示虚构的历史日期。
- 已存在的 AppID `39140` 的 Steam 配置不会被修改或合并。此数据源拥有自己的配置，内部由 `ff7-achievement-dat` 提供者标识。

#### 《死亡空间 2》MarkerPatch 支持

1. 在《死亡空间 2》游戏目录中安装 MarkerPatch。
2. 在 **设置 -> 文件夹** 中，添加包含 `deadspace2.exe`、`MarkerPatch.ini` 和 `achievements` 文件夹的游戏目录。
3. 应用会从该模组的文本和图片资源创建本地 `markerpatch` 配置和成就表。
4. 成就解锁通过读取 `%LOCALAPPDATA%\EA Games\Dead Space 2\settings.txt` 中的 `Controls.AcL.X` 和 `Controls.AcL.Y` 作为一个 64 位位标志来监控。

**重要说明：**

- 所选游戏目录被视为 MarkerPatch 根目录，不会作为通用 AppID 容器扫描，即使其中包含数字子文件夹也一样。
- 仅支持解锁状态。MarkerPatch 的进度值不会被导入或显示。
- 应用只读取已安装的模组资源和外部设置文件；不会修改游戏、模组或其设置。
- 若设置目录或 `settings.txt` 尚不存在，自适应监视器会等待它并自动挂载，无需重启应用。

#### 《爱丽丝：疯狂回归》MadnessPatch 支持

1. 在游戏的 `Binaries\Win32` 目录中安装 MadnessPatch。
2. 在 **设置 -> 文件夹** 中，添加该 `Win32` 目录或包含它的游戏目录。
3. 应用会从模组的 `Achievements\txt` 和 `Achievements\img` 资源创建本地 `madnesspatch` 配置和成就表。
4. 解锁状态从 `Documents\My Games\Alice Madness Returns\AliceGame\CheckPoint\<profile>\Achievements.txt` 下的当前档案文件监控。

**重要说明：**

- Documents 基目录通过 Windows/Electron 的已知文件夹解析，因此即使 Documents 文件夹被迁移也受支持，无需硬编码 `C:\Users\...`。
- 应用不会创建 `CheckPoint` 目录树或 `Achievements.txt`。若它们尚不存在，会在 MadnessPatch 创建后自动挂载监控。
- 每个档案保留独立的内存基线。新发现的档案会被静默初始化，之后位标志的变化才会生成通知。
- 仅导入持久化的解锁位标志。模组显示的仅运行时进度不会从 `Achievements.txt` 读取。
- `MadnessPatch.ini` 中必须保持启用 `AchievementSupport`，模组才会创建并更新状态文件。
- 所选游戏目录被视为 MadnessPatch 根目录，不会作为通用 AppID 容器扫描。

#### XLiveLessNess 支持

1. 为受支持的 Games for Windows - LIVE 游戏安装并配置 XLiveLessNess，若模拟器需要创建其档案数据，则先启动一次游戏。
2. 在 **设置 -> 文件夹** 中，添加已安装的游戏目录。可以是游戏根目录，也可以是在受支持的发现深度内包含所需文件的父目录。
3. 确认某个可执行文件目录中同时包含 `xlive.dll`、`<game>.exe` 和匹配的 `<game>.exe.cfg`。若文件夹是在这些文件安装之前添加的，请使用 **文件夹 -> 重新扫描**。
4. 应用会从 `<game>.exe.cfg` 读取 Title ID，并从可执行文件内嵌的 `SPAFILE` 资源获取本地化的成就名称/描述、玩家分数（Gamerscore）和图片。
5. 生成 `xlivelessness` 本地配置和成就表。检测到的可执行文件和进程名会被保存，用于启动、进程检测、自动选择和游戏时长统计。
6. 解锁状态从 `XLiveLessNess\profile\title\<TITLEID>` 下每个匹配的 `<profile>\achievements.dat` 读取。

预期的游戏侧目录结构：

```text
<游戏文件夹>\
└─ <可执行文件目录>\
   ├─ xlive.dll
   ├─ <game>.exe
   └─ <game>.exe.cfg
```

**重要说明：**

- 发现和 SPAFILE/成就表处理均为异步执行。XLiveLessNess 的扫描和生成阶段通过与其他受支持本地平台相同的生成进度条汇报。
- 首次生成配置时不需要 `achievements.dat`。若档案/title 路径尚不存在，自适应监视器会在 XLiveLessNess 创建后挂载。
- 默认全局状态根目录 `%LOCALAPPDATA%\XLiveLessNess` 和游戏本地的 `XLiveLessNess` 根目录都会被自适应监控，包括其档案/title 目录尚不存在的情况。
- 也可以在 **设置 -> 文件夹** 中直接添加自定义的 XLiveLessNess 存储根目录。应用会将其关联到已有的 XLiveLessNess 配置，而不会把档案或 title 文件夹当作通用 AppID。
- 当进程命令行检测可用时，会解析 `-xllnconfig=<path>`，并自动将其所在目录添加为另一个状态根目录。
- 同一 Title ID 的多个可执行文件会作为一个配置中的进程名备选保留。各档案保留独立基线，因此切换档案不会产生虚假的解锁通知。
- 当内嵌的 SPAFILE 包含相应本地化时，成就表语言遵循 **设置 -> 高级 -> 成就表语言** 中的选择。
- 应用只读取已安装的可执行文件、XLiveLessNess 的 title 配置和 `achievements.dat`；不会修改游戏或模拟器数据。

#### Steam 启动器支持

1. 使用 **监视文件夹** 添加 'C:\Program Files (x86)\Steam\appcache\stats' 文件夹。
2. 通过 Steam 启动并游玩游戏。
3. 应用会：
   - 读取 Steam 创建的文件，
   - 获取游戏名称、成就表和图片。
   - 自动生成配置。
   - 解锁新成就时显示通知。

#### Epic Games 启动器支持

1. 在 **设置** 中连接 Epic 账号，并使用 **导入游戏库** 拉取已拥有的游戏。
2. 应用会自动将带成就的已拥有游戏导入为 `epic-official` 配置。
3. 当游戏已安装时，本地检测使用 Epic 清单文件解析安装位置、可执行文件路径和进程名。
4. 轮询仅针对检测到的正在运行的 Epic 游戏，或用户明确选择的 Epic Official 配置执行。
5. 面板读取本地成就缓存；不会为了渲染网格而执行一次完整的 Epic 同步。

**重要说明：**

- `epic-official` 配置是自动生成的，不应从平台下拉框手动创建。
- 导入流程依赖 Epic 登录和本地加密令牌存储。
- 商店图片优先通过 Epic 产品元数据解析，仅在 Epic 元数据无法提供可用图片时才回退到 SteamGridDB。
- 对于使用 artifact/AppID 而非命名空间的 Epic 模拟文件夹，成就表生成可以先解析相关的目录项和命名空间，再继续走正常的 Epic 成就表流程。

#### GOG Galaxy 启动器支持

1. 安装并登录 **GOG Galaxy**。
2. 使用 **监视文件夹** 添加 `%LOCALAPPDATA%\GOG.com\Galaxy\Applications` 文件夹。
3. 至少通过 GOG Galaxy 启动并游玩一次游戏。
4. 应用会：
   - 从 `%ProgramData%\GOG.com\Galaxy\storage\galaxy-2.0.db` 解析本地的 `clientId -> productId -> 游戏标题` 映射，
   - 监视 `%LOCALAPPDATA%\GOG.com\Galaxy\Applications\<clientId>\Gameplay\<userId>\gameplay.db`，
   - 自动生成 `gog-official` 配置，
   - 从 `gameplay.db` 本地构建 `achievements.json` 和 `achievementpercentages.json`，
   - 监控 `gameplay.db` 的后续变化，并在解锁新成就时显示通知。

**重要说明：**

- `gog-official` 由本地 GOG Galaxy 数据自动生成，不应从平台下拉框手动创建。
- 只有在 `gameplay.db` 存在且成就表已填充并稳定之后才会创建配置，以避免生成空的成就表。
- 若某游戏只有 `Storage\...` 数据而尚无 `Gameplay\<userId>\gameplay.db`，应用会检测到安装路径但会等待后再创建配置。
- 创建后，配置的 `save_path` 指向具体的 `Gameplay\<userId>` 文件夹，而运行时进度从 `gameplay.db` 读取。

#### Ubisoft Connect 启动器支持

1. 安装并登录 **Ubisoft Connect**。
2. 使用 **监视文件夹** 手动添加 `%LOCALAPPDATA%\Ubisoft Game Launcher\spool` 文件夹。
3. 至少通过 Ubisoft Connect 启动并游玩一次游戏，使本地 spool/缓存文件存在。
4. 应用会：
   - 检测 `%LOCALAPPDATA%\Ubisoft Game Launcher\spool\<userId>\<productId>.spool`，
   - 从 `%ProgramData%\Ubisoft\Ubisoft Game Launcher\cache\achievements` 生成 `achievements.json`、`achievementpercentages.json` 和本地图片，
   - 自动生成 `ubisoft-official` 配置，
   - 当有可用的 Steam AppID 时，使用本地 `uplay-steam` 映射以获取稀有度，
   - 监控后续的 `.spool` 变化，并在解锁新成就时显示通知。

**重要说明：**

- `ubisoft-official` 由手动监视的 Ubisoft Connect spool 根目录自动生成，不应从平台下拉框手动创建。
- 应用不会自动假定 Ubisoft spool 路径；必须在 **设置 -> 文件夹** 中手动添加 spool 根目录。
- 只有在 `.spool` 文件和本地成就存档都可用之后才会创建配置，以便先生成成就表。
- 创建后，配置的 `save_path` 指向具体的 `spool\<userId>` 文件夹，而运行时进度从 `<productId>.spool` 读取。

#### EA Desktop 启动器支持

1. 安装并登录 **EA Desktop**。
2. 使用 **监视文件夹** 手动添加 `%LOCALAPPDATA%\Electronic Arts\EA Desktop\Logs` 文件夹。
3. 至少通过 EA Desktop 启动并游玩一次游戏，使 `EADesktopVerbose.log` 中包含该游戏的本地成就查询记录。
4. 应用会：
   - 读取 `EADesktopVerbose.log`，
   - 从 EA Desktop 详细日志中解析本地的 `contentId -> achievementSet -> 游戏标题` 映射，
   - 从 EA Desktop 记录的成就集生成 `achievements.json` 和本地图片，
   - 自动生成 `ea-official` 配置，
   - 监控详细日志的后续变化，并在解锁新成就时显示通知。

**重要说明：**

- `ea-official` 由手动监视的 EA Desktop 日志根目录自动生成，不应从平台下拉框手动创建。
- 应用不会自动假定 EA Desktop 日志路径；必须在 **设置 -> 文件夹** 中手动添加日志根目录。
- 只有在 EA Desktop 已为该游戏记录完整成就集之后才会创建配置，以便先生成成就表。
- 创建后，配置的 `save_path` 指向 EA Desktop 的 `Logs` 文件夹，而运行时进度从 `EADesktopVerbose.log` 读取。
- EA Desktop 会将 `EADesktopVerbose.log` 轮转为 `EADesktopVerbose.bak`；应用会同时读取两者，因此成就事件不会因日志轮转而丢失。

### Xbox PC（微软 / Xbox Network）

1. 打开 **设置 -> 高级**，在 **Xbox PC（微软 / Xbox Network）** 下选择 **连接 Xbox**。
2. 使用 Windows Xbox 应用所用的账号，通过微软 OAuth 登录。
3. 使用 **导入 Xbox PC** 来：
   - 从 Xbox 档案中读取支持成就的 Windows/PC 游戏，
   - 扫描本地 `XboxGames` 和已打包的 GDK 安装，
   - 生成 `xbox-pc` 配置和成就表，
   - 在可用时关联本地可执行文件/AUMID 信息。
4. 所选 Xbox PC 配置的成就状态直接从 Xbox Network 刷新，带请求限流和本地缓存回退。

说明：

- 排除仅限 Xbox 主机的历史记录。只有当 Xbox 报告了 Windows/PC 设备，或其 Title ID 与本地已安装游戏匹配时，才会导入该游戏。
- 委托给 EA App、Ubisoft Connect 或其他启动器的游戏，在 PC 上可能无法提供 Xbox 成就。
- 应用以加密方式存储微软刷新令牌和 Xbox XSTS 会话；它从不索取或存储微软账户密码或 Entra 客户端密钥。
- 实验性认证路由使用的是 [OpenXbox Xbox-WebAPI 项目](https://github.com/OpenXbox/xbox-webapi-python) 公开的桌面 OAuth 身份。
- 该客户端身份不归 Achievements 项目所有。微软可以限制或撤销其使用，Xbox Network 直连端点仍可能拒绝请求。
- `xbox-pc` 配置是自动生成的，不应手动创建。

### RetroAchievements

1. 打开 **设置 -> 高级**，输入 RetroAchievements 控制面板中的用户名和 Web API 密钥。
2. 选择 **连接**，然后 **导入游戏库**，为账号历史创建独立的 `retroachievements` 配置和成就表。
3. 在面板中选择一个 RetroAchievements 配置以启动其 API 轮询器。初始缓存状态之后收到的新解锁会生成通知。

说明：

- 此集成仅使用只读的 RetroAchievements Web API。它不会扫描模拟器、ROM、进程或本地游戏文件夹。
- Web API 密钥通过 Electron safeStorage 存储，不会以明文写入偏好设置或配置文件。
- 轮询仅针对在界面中明确选择的 RetroAchievements 配置运行，当选择其他平台/配置或断开账号时停止。
- 初始导入会静默初始化已有解锁，因此历史解锁不会造成通知轰炸。

### 面板

- 按"显示面板"按钮进入游戏网格
- 使用搜索快速筛选游戏
- 按平台筛选，并按名称、进度或最近更新时间排序
- 点击任意游戏以加载其配置
- 使用 `Ctrl + 点击` 或卡片右键菜单选择多个游戏，然后从操作栏中忽略、删除或清除选择
- 当启用 **显示已拉黑的游戏** 时，可以在面板中显示已拉黑的游戏以便检查并恢复
- 对已配置可执行文件的游戏使用播放按钮（面板会关闭并将焦点返回主界面）
- 后台自动轮询会在游戏进程启动时选中该游戏
- `Esc` 或关闭按钮会恢复面板浮层，并重新启用窗口其余部分的输入

### 个性化

- 选择通知预设和屏幕位置
- 选择 **Windows 原生** 作为成就预设，以使用 Windows Toast 通知替代动画预设
- 选择通知音效和语言
- 从 **设置 -> 高级 -> 成就表语言** 中选择要生成的成就表语言
- 调整界面缩放（75% 至 200%）
- 调整成就持续时间（自动或自定义）
- 调整成就音量（0% 至 200%）
- 切换显示隐藏成就的隐藏描述
- 启用关闭到托盘（X 按钮隐藏到托盘）
- 配置浮层快捷键，或完全禁用浮层
- 配置浮层交互键（在点击穿透 ↔ 拖拽/滚动之间切换）
- 可选：在 **设置 -> 高级 -> 通知点击动作** 中设置点击动画或 Windows 原生通知时打开对应成就
- 选择通知点击后打开不可聚焦的点击穿透浮层，还是打开主应用；打开主应用会将焦点从游戏移开
- 存储在操作中心的 Windows 原生通知保留独立的导航路由，因此每条可用通知都能打开各自的成就
- 从 **设置 -> 高级 -> 渲染** 启用/禁用浮层的手柄支持
- 启用/禁用各项功能：
  - 成就截图
  - 成就录制
    - 选择 30 或 60 FPS
    - 选择 10 至 30 秒的总片段时长，在解锁前后平均分配
  - 进度通知
  - 游戏时长通知
  - 启动行为
- 当某个配置处于激活状态时，可为其静音进度通知
- 切换"随 Windows 启动"以使用当前可执行文件路径创建/移除任务计划程序条目
- 禁用原生进程监视器以仅使用可执行文件名回退检测；当已知配置的进程名时，匹配无需命令行参数
- 所有偏好设置持久化到 `%APPDATA%/Achievements/preferences.json`，并在启动时恢复

### 更新日志与实时日志

- **设置 -> 更新日志** 列出项目在 GitHub 上发布的版本，包含版本号、发布类型、发布日期和发布说明。
- 发布数据缓存 30 分钟。**刷新** 会请求当前 GitHub 数据，而当 GitHub 或网络不可用时，仍可使用最后一次有效缓存。
- 发布链接会在默认浏览器中打开对应的 GitHub 发布页。远程发布文本按纯文本渲染，不会作为 HTML 执行。
- **设置 -> 日志** 列出带有友好名称的已知应用日志来源，并会额外发现日志目录中已存在的其他 `.log` 文件。
- 所选文件会被实时跟踪，并在文件替换、截断和常规日志轮转时保持同步。
- 搜索和级别筛选（`DEBUG`、`INFO`、`WARN`、`ERROR`）只影响显示的行；不会更改或删除底层日志文件。
- **暂停** 会暂停界面更新但不停止应用日志记录。**清除视图** 只清除当前内存中的视图。**打开日志文件夹** 会在系统文件管理器中打开原始文件。
- 初始视图限制为最新的 1,000 行，渲染器最多保留 2,000 行，防止打开的日志标签页无限制增长。
- 实时文件监控仅在日志标签页处于活动状态时启动，选择其他标签页或关闭设置时会被释放。
- 日志中可能包含本地路径或账号标识符。公开分享前请先检查其内容。

### 运行时数据位置

- `%APPDATA%/Achievements/configs` —— 配置
- `%APPDATA%/Achievements/configs/schema` —— 生成的成就表 + 本地成就图片
- `%APPDATA%/Achievements/images` —— 缓存的封面
- `%USERPROFILE%/Pictures/Achievements Screenshots` —— 默认成就截图输出目录
- `%USERPROFILE%/Videos/Achievements Records` —— 默认成就视频输出目录
- `%APPDATA%/Achievements/ach_cache` —— 缓存的成就
- `%APPDATA%/Achievements/ach_cache/github-releases.json` —— 更新日志标签页使用的 GitHub 发布说明缓存
- `%APPDATA%/Achievements/ach_cache_meta.json` —— 用于避免不必要的缓存重写的缓存元数据
- `%APPDATA%/Achievements/logs` —— 应用日志
- `%APPDATA%/Achievements/playtime-totals.json` —— 游戏时长统计
- `%APPDATA%/Achievements/preferences.json` —— 应用设置、监视文件夹和黑名单状态
- `%APPDATA%/Achievements/preferences.json.bak` —— 上一次有效的偏好设置备份，用于在写入中断/损坏后恢复
- `%APPDATA%/Achievements/xbox-pc-microsoft-auth.enc` —— 加密的微软/Xbox 认证状态
- `%APPDATA%/Achievements/xbox-pc/titles` —— 每个游戏的 Xbox PC 同步数据
- `%APPDATA%/Achievements/retroachievements-auth.enc` —— 加密的 RetroAchievements Web API 凭据
- `%APPDATA%/Achievements/retroachievements/titles` —— 每个游戏的 RetroAchievements 同步数据

### 键盘与手柄导航

- **全局**
  - 设置：`F1` / `Ctrl+O`；手柄：Xbox View、PlayStation Share。
  - 面板：`F2` / `Ctrl+D`；手柄：Xbox 按钮、PlayStation 触摸板或 PS。
  - 显示/隐藏选项面板（面板）：右键菜单键或 `Shift+F10`；手柄：Xbox Y、PlayStation ⃤⃤。
  - 显示/隐藏选项面板（主界面）：`F3`；手柄：Xbox X、PlayStation ☐。
  - 返回/关闭：`Esc` / `Backspace`；手柄：Xbox B、PlayStation ◯。
  - 游玩（启动）：`P` / `Ctrl+Enter` / `Shift+Enter`；手柄：Xbox Menu、PlayStation Options。
  - 确认/激活：`Enter` / `Space`；手柄：Xbox A、PlayStation ✕。
  - 翻页滚动：`PageUp` / `PageDown`；手柄：右摇杆（RS）。
  - 移动焦点：方向键；手柄：方向键（D-pad）或左摇杆（LS）。
- **浮层（显示时）**
  - 切换显示：在设置中配置 **浮层快捷键**（分配前默认禁用）。
  - 切换交互模式（点击穿透 ↔ 交互）：在设置中配置 **交互键**（默认：`\`）。
  - 翻页滚动：`PageUp` / `PageDown`，回退为 `Ctrl+PageUp` / `Ctrl+PageDown`。
  - 吸附 5 个位置：`Ctrl+Alt+Shift+1..5`（左上、右上、居中、左下、右下）。
  - 循环吸附预设：`Ctrl+Alt+Shift+M`。
  - 微调（20 像素）：`Ctrl+Alt+Shift+方向键`。
  - 上述浮层专属快捷键仅在浮层显示时生效。
  - 从 **设置 -> 高级 -> 渲染** 启用手柄支持，可独立于主窗口导航来控制浮层。
  - 切换显示：手柄：Xbox View + Menu、PlayStation Share + Options。
  - 进入浮层控制模式：按住 Xbox LB + RB、PlayStation L1 + R1。
  - 控制模式下移动浮层：手柄：左摇杆（LS）。
  - 控制模式下翻页滚动：手柄：右摇杆（RS）。
  - 控制模式下微调：手柄：方向键（D-pad）。
  - 控制模式下循环吸附预设：手柄：Xbox Y、PlayStation △。
  - 当肩键松开、手柄断开或浮层不再可用时，浮层控制模式结束。
  - PlayStation 手柄原生支持在可用时使用微软 GameInput。若缺少 GameInput，应用会在启用该设置时发出警告，并仅回退到 XInput 兼容手柄。
- **面板**
  - 网格导航：方向键、`Home`、`End`、`PageUp`、`PageDown`；手柄：方向键或 LS。
  - 搜索：`Ctrl+F`；手柄：Xbox X、PlayStation ☐。
  - 搜索中：`Enter` / A / ✕ 打开第一个可见卡片；下方向键将焦点移到第一个卡片；`Esc` / B / ◯ 关闭面板。
  - 打开游戏（选中卡片）：`Enter`；手柄：A / ✕。
  - 显示/隐藏选项面板（面板）：右键菜单键或 `Shift+F10`；手柄：Xbox Y、PlayStation ⃤⃤。
  - 从卡片游玩：点击播放；`P` / `Ctrl+Enter` / `Shift+Enter`；手柄：Menu / Options（需已配置可执行文件）。
  - 排序循环：`Alt+1`（名称）、`Alt+2`（进度）、`Alt+3`（最近更新）；手柄：L3 / RB / LB（Xbox）、L3 / R1 / L1（PlayStation）。
- **设置面板**
  - 打开/关闭：`F1` / `Ctrl+O`；手柄：View / Share。
  - 标签页模式：上/下移动；`Enter` 选择；`Esc` 关闭；手柄：方向键或 RS 移动；A / ✕ 选择；B / ◯ 关闭。
  - 分区模式：上/下切换焦点；左/右调整；`Enter` 激活；`Esc` 返回标签页；手柄：方向键或 LS 切换焦点；A / ✕ 激活；B / ◯ 返回。
  - 循环标签页：手柄 LB/RB（Xbox）或 L1/R1（PlayStation）。
- **主界面**
  - 切换选项：`F3`；手柄：Xbox X、PlayStation ☐。
  - 新建配置：`Ctrl+N`。
  - 移动：上/下；手柄：方向键或 LS。
  - 游玩：`P` / `Ctrl+Enter` / `Shift+Enter`；手柄：Menu / Options。
- **下拉框**
  - 打开：`Enter`。
  - 打开时：上/下/左/右导航；`Enter` 确认；`Esc` / `Backspace` 取消；手柄：方向键导航；A / ✕ 确认；B / ◯ 取消。
  - 关闭时：左/右循环选项；手柄：方向键左/右。
- **说明**
  - 右摇杆滚动对当前可滚动区域使用平滑滚动。
  - 返回具有上下文：设置分区 -> 标签页；设置标签页 -> 关闭；面板 -> 关闭；配置弹窗 -> 关闭；已选中配置的主界面 -> 清除选择/返回。

### 游戏兼容性

- 在无边框窗口模式下效果最佳
  [注意：使用 DirectX 9/10/11 的游戏需要在游戏内显示设置中启用无边框/无边框窗口模式，通知才能显示在游戏窗口之上]
- 对全屏模式支持有限
  [注意：如果游戏支持并使用 DirectX 12 运行，启用全屏时通知通常也会显示在游戏窗口之上]
- 自动检测并导入已有成就
- 如果配置中包含，支持多种成就语言

## ⚠️ 已知限制与解决办法

- 让应用与游戏保持相同的权限级别（`非管理员/非管理员` 或 `管理员/管理员`），浮层和快捷键行为会更可靠。
- 当 Achievements 以管理员身份运行时，Windows 原生通知的点击激活可能受限。若跳转无效，请在游戏允许的情况下以非提权方式运行 Achievements。
- 若浮层快捷键在某个特定游戏中不触发，请改用 3 键组合，并测试 `Ctrl+PageUp` / `Ctrl+PageDown` 回退。
- 在提权/受保护的游戏上下文中，浮层拖拽可能失败；请改用吸附位置（`Ctrl+Alt+Shift+1..5`）或微调（`Ctrl+Alt+Shift+方向键`）。
- 浮层对 PlayStation 手柄的原生支持依赖微软 GameInput。缺少它时，浮层手柄功能仅支持 Xbox/XInput 兼容手柄。
- 在某些引擎/驱动组合下，浮层层级顺序可能变化（可能显示在窗口之后）；请重新切换浮层，并优先使用无边框窗口模式。
- 翻转/合成行为由 Windows 与 GPU 驱动控制；应用无法在所有系统上强制统一的翻转模式。
- 切换 **禁用硬件加速**（需重启）并保留对你的配置最稳定的模式。如果浮层热键有效但看不到任何内容，请关闭 **禁用硬件加速**（即启用硬件加速）并重启应用。
- 若出现浮层显示/呈现问题，尤其是在 **Special K** 也处于活动状态时，请启用 **强制对浮层使用 globalShortcut**。这会将浮层快捷键切换为 Electron `globalShortcut`，禁用基于钩子的拖拽，并可解决受影响系统上的兼容性/合成问题。
- 通知基于队列；较长的预设持续时间会造成明显的延迟感。如有需要，请降低 **通知持续时间**。
- 带高开销特效（模糊/背景虚化 + 分层动画）的预设在某些 GPU 上可能出现微卡顿；如有需要请使用更轻量的预设。
- 排查显示/焦点问题时，避免同时运行多个浮层/注入类工具。

### 视频

- [首次运行](https://youtu.be/c1jDfynHd-U) + [手柄支持](https://youtu.be/fsqoKiMGLkw)
- [手动配置](https://youtu.be/abdVuDB80Ow)
- [自动配置](https://youtu.be/nOoiU5lPopM)
- [多平台支持](https://youtu.be/KwRUo53VTho)
- [多平台支持 V2 + 浮层手柄支持](https://youtu.be/vDQ_4cNeIe8)

## 👤 作者

**JokerVerse**
Copyright © 2025

---

欢迎贡献、Fork 或提出改进建议！
