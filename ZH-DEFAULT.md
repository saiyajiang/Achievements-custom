# 默认简体中文 + 首次运行选择语言

本改动位于分支 **`feat/zh-default`**（基于 `zh-CN`）。

## 改了什么

### 1. `main.js` — 默认语言改为简体中文

```js
// DEFAULT_PREFERENCES
language: "english"  →  "schinese"     // 成就文本语言
uiLanguage: "english" → "schinese"     // 界面语言
uiLanguageSelected: false              // 新增：是否已选过语言
```

**两个字段的区别**：

| 字段 | 影响 |
| --- | --- |
| `uiLanguage` | **界面文字**（菜单、按钮、设置项、提示语） |
| `language` | **成就文本语言**（Steam 成就名/描述的抓取语言） |

两者都改成了 `schinese`，界面和成就默认都是中文。

已确认 `assets/locales/schinese.json` 覆盖完整：**1633 / 1633 条，100%**，不会大面积回退英文。

### 2. `main.js` — 首次运行弹出语言选择窗口

新增逻辑：

- 启动时检查 `preferences.json` 里的 `uiLanguageSelected`
- 为 `false`（即从未选过）→ 弹出语言选择窗口，**阻塞主窗口创建**，等选完再继续
- 选完写入 `uiLanguage` + `language` + `uiLanguageSelected: true`，后续启动不再弹
- 直接关掉窗口 → 沿用默认值（简体中文），并标记已询问，不会每次都弹

新增 IPC 通道：

| 通道 | 方向 | 作用 |
| --- | --- | --- |
| `ui-language:list` | invoke | 读取可用语言列表（扫描 `assets/locales/*.json`）与当前值 |
| `ui-language:chosen` | send | 渲染进程提交选择结果 |

### 3. `lang-select.html` — 新增语言选择页面

自包含的独立窗口页面：

- 30 种语言，按「本地名 + 英文名」双语显示
- 简体中文、English、繁體中文、日本語、Русский 排在前面
- 当前语言带高亮和「当前 / current」标记
- 点击即选中并继续启动
- `window.api` 不可用时自动回退，不会卡死启动

### 4. `preload.js` — 暴露接口并放行通道

```js
getUiLanguageOptions: () => ipcRenderer.invoke("ui-language:list"),
selectStartupUiLanguage: (language) =>
  ipcRenderer.send("ui-language:chosen", { language }),
```

`invoke` 白名单新增 `"ui-language:list"`。

## 如何测试

```bash
git fetch origin
git checkout feat/zh-default
npm start
```

第一次启动会弹出语言选择窗口。想反复测试这个弹窗，删掉标记即可：

```
%APPDATA%\Achievements\preferences.json
```

把 `"uiLanguageSelected": true` 改回 `false`，或直接删掉这一行。

## 关于"不用编译成 exe 也能运行"

**能，而且这是默认方式：**

```bash
npm start
```

这条命令就是 Electron 的开发模式运行，直接跑源码，**完全不需要打包**。之前讨论的 `npm run dist` / `npm run pack` 只是要分发给别人时才需要。

开发模式的差别：

| | `npm start` | 打包后 exe |
| --- | --- | --- |
| 运行源码 | ✅ | ❌（读 app.asar） |
| 需要打包步骤 | ❌ | ✅ |
| 改动代码后要重启 | 是 | 是 |
| 便携分发 | ❌ | ✅ |

## 已知限制

- 语言选择窗口是**新增的独立窗口**，未做国际化（它本身就是选语言的，用中英双语界面）
- 改动语言后**已打开的设置窗口文字不会实时刷新**，重启应用后完整生效（启动流程中选的语言直接生效，不存在此问题）
- 若 `preferences.json` 已存在且里面已有 `uiLanguage`，首次启动仍会弹一次选择窗口（因为 `uiLanguageSelected` 是新增字段）——这是有意为之，正好让你确认默认语言

## 合并到 zh-CN

```bash
git checkout zh-CN
git merge feat/zh-default
git push origin zh-CN
```

## 回退

```bash
git checkout zh-CN -- main.js preload.js
rm lang-select.html
```
