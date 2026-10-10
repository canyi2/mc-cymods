https://canyi2.github.io/mc-cymods/
全文由大肥鱼编写
# Mod 更新检查器

一个部署在 GitHub Pages 上的纯前端工具，用来批量检查 Modrinth 项目的更新情况，按 MC 版本 / 加载器批量下载，并导出 `.mrpack` 整合包。

无需服务器、无需数据库，把 `index.html` 和 `pack.json` 上传到仓库即可使用。

---

## 功能一览

### 支持 4 种 Modrinth 项目类型

| 类型 | 版本分组方式 | 下载到 |
|---|---|---|
| **Mod** | 按 MC 版本 | `mods/` |
| **材质包** | 按 MC 版本 | `resourcepacks/` |
| **光影** | 按加载器（iris / optifine 等） | `shaderpacks/` |
| **整合包** | 按 MC 版本 | `mods/`（仅作展示，不能嵌套进 mrpack） |

添加项目时只需粘贴 Modrinth 链接，类型自动识别。例如：

```
https://modrinth.com/mod/sodium
https://modrinth.com/shader/complementary-reimagined
https://modrinth.com/resourcepack/xk-redstone-display
https://modrinth.com/modpack/zombie-invade-100-days
```

也可以直接输入 slug，如 `sodium`。

### 更新检查

- 打开页面自动检查所有项目是否有新版本
- 有更新的项目会显示「有更新 ← 旧版本号」标记
- 检测结果保存在浏览器 localStorage，用来对比下次打开时的版本

### 版本筛选

- **MC 版本面板**：列出所有 mod / 材质包 / 整合包涉及的 MC 版本，可多选
- **光影加载器面板**：列出所有光影涉及的加载器，可多选
- 两个面板独立工作，互不干扰

### 双布局

- **卡片模式**：每个项目一张卡片，展示最新 3 个版本（光影展示 10 个加载器）
- **列表模式**：一行一个项目，展示最新 3 个版本 + 最多 2 个「其他版本」输入
- 「其他版本」输入框（如 `1.20.1`、`iris`）会在每个项目卡片里高亮显示对应版本，方便切换
- 布局偏好保存在 localStorage

### 搜索与类型筛选

- 搜索框支持按项目名或 slug 过滤
- 类型面板可分别勾选 Mod / 材质包 / 光影 / 整合包
- 搜索、类型筛选、分类筛选三者**叠加生效**

### 自定义分类

- 使用工具栏的「+ 加入分类」按钮，把勾选的项目批量加入分类
- 分类总数上限 **5 个**
- 分类 chip 显示在类型面板右侧，点击即可只显示该分类下的项目
- 每个分类 chip 带 `×`，可一键删除整个分类
- 每个项目卡片底部有标签行，显示所属分类，标签的 `×` 可从该项目移除分类
- 导出 `pack.json` 时自动生成 `collections` 字段

### 固定分类（primary）

- 在 `pack.json` 里用 `"primary": "分类名"` 指定一个固定分类
- 固定分类始终排在最前，**名称和内容都只能通过编辑 pack.json 修改**
- UI 上不显示删除按钮，也不能通过「+ 加入分类」修改

### 快捷链接

每个项目卡片都提供：

- **Modrinth**：跳转到项目的 Modrinth 页面
- **MC百科搜索**：跳转到 MC百科搜索页并填充项目名，一键找到对应详情

### 批量下载

- 勾选 MC 版本 / 加载器 + 勾选项目 → 点击「下载所选」
- 按项目实际类型分别匹配版本，自动跳过不匹配的组合
- 下载间隔 450ms，避免浏览器拦截多文件下载
- 完成后统计成功 / 失败 / 跳过数量

### 导出 .mrpack

点击「导出 .mrpack」按钮：

- 填写整合包名称、版本号、MC 版本、加载器及版本
- 生成标准 `.mrpack` 文件（ZIP，内含 `modrinth.index.json`）
- 支持导入 Modrinth App、Prism Launcher、ATLauncher、HMCL 等启动器
- 整合包类型会自动跳过（不能嵌套）

### 导出 pack.json

点击「导出 pack.json」按钮：

- 导出当前所有项目（按类型分组，按当前排序）
- 如果创建了分类，同时包含 `collections` 字段
- 如果指定了固定分类，包含 `primary` 字段
- 下载后上传到仓库根目录覆盖旧文件即可

---

## 部署步骤

### 1. 创建仓库

在 GitHub 上创建一个仓库，例如 `yourname.github.io`（如果想要 `https://yourname.github.io` 这样的根域名，仓库名必须和用户名一致）。

### 2. 上传两个文件

| 文件 | 说明 |
|---|---|
| `index.html` | 完整的页面代码 |
| `pack.json` | 项目清单，格式见下方 |

### 3. 开启 GitHub Pages

仓库 → **Settings** → **Pages** → Source 选 `Deploy from a branch` → Branch 选 `main`，Folder 选 `/ (root)` → **Save**。

等待 1–3 分钟后访问：

- 如果仓库名是 `yourname.github.io`：`https://yourname.github.io`
- 其他仓库名：`https://yourname.github.io/仓库名/`

---

## pack.json 格式

```json
{
  "mod": [
    "sodium",
    "iris",
    "lithium",
    "fabric-api",
    "modmenu",
    "cloth-config"
  ],
  "shader": [
    "complementary-reimagined"
  ],
  "resourcepack": [
    "xk-redstone-display"
  ],
  "modpack": [
    "zombie-invade-100-days"
  ],

  "primary": "inf",

  "collections": {
    "inf": {
      "mod": ["sodium", "lithium"],
      "shader": ["complementary-reimagined"]
    },
    "pvp": {
      "mod": ["sodium"],
      "resourcepack": ["xk-redstone-display"]
    }
  }
}
```

### 字段说明

| 字段 | 必需 | 说明 |
|---|---|---|
| `mod` | 否 | Mod 的 slug 数组 |
| `shader` | 否 | 光影的 slug 数组 |
| `resourcepack` | 否 | 材质包的 slug 数组 |
| `modpack` | 否 | 整合包的 slug 数组 |
| `primary` | 否 | 固定分类名称，字符串。指定后该分类排最前，UI 无法修改 |
| `collections` | 否 | 自定义分类。每个 key 是分类名，value 和顶层一样按类型分组 |

四个类型字段都是可选的，缺省即为空。

### 兼容旧格式

如果 `pack.json` 直接是一个数组：

```json
["sodium", "iris", "lithium"]
```

页面也能读取，只是没有类型分组和分类。

---

## 日常使用流程

### 首次配置

1. 编辑 `pack.json`，填入你的项目 slug 列表
2. 上传到仓库根目录
3. 打开 `https://yourname.github.io`，页面会自动加载并检查更新

### 添加新项目

1. 在顶部输入框粘贴 Modrinth 链接或 slug
2. 点「添加」，会自动加载项目信息

### 整理分类

1. 勾选要归类的项目（可以配合类型筛选 + 搜索快速定位）
2. 点「+ 加入分类」，输入分类名（如 `pvp`、`优化`）或选择已有分类
3. 完成后点「导出 pack.json」，把下载的 `pack.json` 上传覆盖仓库里的旧文件

### 检查并下载

1. 打开页面等待更新检查完成
2. 勾选需要的 MC 版本（或光影加载器）
3. 勾选需要的项目
4. 点「下载所选」，浏览器会依次下载所有文件

### 生成整合包

1. 勾选要打包的项目
2. 点「导出 .mrpack」
3. 填写整合包信息，生成 `.mrpack` 文件
4. 用启动器导入即可

---

## 缓存说明

页面使用 `localStorage` 保存项目列表和分类，所以：

- 修改 `pack.json` 后刷新页面，**如果已经打开过页面**，列表不会变（读取的是本地缓存）
- 要强制重新读取 `pack.json`，在控制台执行：

```javascript
localStorage.removeItem('mod-checker-v11')
```

然后 `Ctrl + F5` 强制刷新。

- 每次修改缓存的键名（如升级到 `mod-checker-v12`）也会自动使旧缓存失效

---

## 常见问题

### 页面显示的项目列表不是 pack.json 里的？

浏览器缓存了旧列表。按上面的「缓存说明」清掉再刷新。

### 访问 pack.json 显示 404？

- 确认 `pack.json` 和 `index.html` 在同一目录（仓库根目录）
- 确认文件名精确为 `pack.json`（GitHub Pages 大小写敏感）
- 等 1–3 分钟让 Pages 重新部署

### 某个项目加载失败？

- 检查 slug 是否正确，或者链接是否能打开
- 部分项目可能被 Modrinth 下架或隐藏

### 26.x 版本不显示？

确保 `MC_RE` 正则为 `/^\d+\.\d+(\.\d+)?$/`，它会匹配 `1.21.4`、`26.3` 等格式。

### 下载没反应？

浏览器可能拦截了多文件下载。出现提示时选择「允许」。

### 整合包导出的 .mrpack 里没有某些项目？

- 整合包类型不能嵌套进 mrpack，会自动跳过并提示
- 如果某个项目在你选的 MC 版本下没有版本，也会跳过

---

## 技术栈

- 纯 HTML / CSS / JavaScript，无框架
- Modrinth API v2
- JSZip（从 CDN 加载，用于生成 .mrpack）
- localStorage 用于本地持久化

---

## 许可证

MIT。仅供个人使用，请遵守 Modrinth 和 MC百科的服务条款。
