# 云端构建 Windows 安装包（本地零环境）

本仓库已内置 GitHub Actions 构建配置。启用后，**本地完全不需要安装 Node、Visual Studio、Python 或任何 Spectre 组件**——编译全在 GitHub 的 Windows 服务器上完成，你只需下载构建好的 exe。

## 为什么需要这个

本地 `npm install` 会编译三个原生模块（`@vscode/windows-process-tree`、`uiohook-napi`、`koffi`）。其中 `windows-process-tree` 在默认的 MSVC 配置下会报：

```
error MSB8040: 此项目需要缓解了 Spectre 漏洞的库
```

要解决得手动往 Visual Studio 里补 Spectre 缓解库组件，比较折腾。而 GitHub 的 `windows-latest` 镜像**自带完整工具链**，不存在这个问题。

## 启用步骤（一次即可）

由于仓库 token 没有 `workflow` 权限，我无法直接把文件写入 `.github/workflows/`。需要你手动放一下，两种方式任选：

### 方式 A：GitHub 网页（推荐，最简单）

1. 打开仓库，切到 **zh-CN** 分支
2. 点 **Add file → Create new file**
3. 文件名填：`.github/workflows/build-windows.yml`（直接输入完整路径，GitHub 会自动建目录）
4. 打开仓库根目录已有的 [`build-windows.workflow.yml`](build-windows.workflow.yml)，全选复制，粘贴进去
5. 点 **Commit changes**

### 方式 B：本地命令行

```bash
cd Achievements-custom
git checkout zh-CN
git pull
mkdir -p .github/workflows
mv build-windows.workflow.yml .github/workflows/build-windows.yml
git add .github/workflows/build-windows.yml
git commit -m "ci: 启用 GitHub Actions 云端构建"
git push
```

## 触发构建

1. 打开仓库的 **Actions** 标签页
2. 左侧选 **Build Windows Installer**
3. 右侧点 **Run workflow**，可以选择：
   - `portable`：是否同时产出绿色免安装版（默认开）
   - `create_release`：是否自动创建 Release（默认关）
   - `release_tag`：Release 标签名，如 `v1.3.1-zh.1`
4. 点绿色 **Run workflow** 按钮

构建约需 10–20 分钟（要下载几百 MB 的 Chromium）。

## 下载产物

构建完成后，在对应的 workflow run 页面底部 **Artifacts** 区域下载 `Achievements-Windows`。

解压后你会得到：

| 文件 | 说明 |
| --- | --- |
| `Achievements-Setup-*.exe` | 标准安装版，双击安装 |
| `Achievements-*.exe`（非 Setup） | 绿色免安装版，双击直接运行，不写注册表 |

如果勾选了创建 Release，产物还会以草稿形式发布到 Releases 页面。

## 构建流程说明

| 步骤 | 作用 |
| --- | --- |
| `setup-node` 22 LTS | 使用 LTS 版本，提高原生模块预编译命中率 |
| `rust-toolchain` | 编译成就录制与 HDR 截图模块（失败不中断） |
| `npm install` | 云端完整工具链，不会撞 MSB8040 |
| `npm run dl-browsers` | 下载 Playwright Chromium（成就抓取必需） |
| `electron-builder` | 免签名打包 |

## 关于签名

构建使用免签名参数：

```
--config.win.signExecutable=false
--config.forceCodeSigning=false
```

原因是 `windows-signing-policy.json` 里绑定的是**上游作者的证书指纹**，你拿不到对应私钥。免签名打包出的 exe 在 Windows SmartScreen 上会提示"来源未知"，属正常现象，选择"仍要运行"即可。

## 额度

公开仓库的 GitHub Actions 完全免费，每月 2000 分钟额度，构建一次约 15 分钟，足够用。

## 上游归属

本项目是 [PSerban93/Achievements](https://github.com/PSerban93/Achievements) 的派生版本（MIT 许可），改动由 AI 完成。打包分发时请保留原作者版权声明。
