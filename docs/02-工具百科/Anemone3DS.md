# Anemone3DS（主题 / 启动画面 / 徽章管理）

主题、开机启动画面（Splashes）与徽章（Badges）的安装管理器，配合 Luma3DS 的 LayeredFS 工作，是 3DS 美化的主力工具。

## 下载与安装位置

- 官方发布：[Anemone3DS GitHub Releases](https://github.com/astronautlevel2/Anemone3DS/releases)（已核实，最新 v3.0.1，2024-06 发布；v3.0.0 起支持徽章）。
- 装法二选一：用 FBI 安装 `Anemone3DS.cia`，或把 `Anemone3DS.3dsx` 放 `/3ds/Anemone3DS/` 从 Homebrew Launcher 启动。
- 前置：Luma3DS 并开启 Enable game patching。仅装主题用 HBL 即可；启动画面和 QR 扫码功能需要 CFW。

## 主题格式

- Anemone3DS 只识别 SD 卡根目录 `/Themes` 下的 `.zip` 或 `.s3dz` 压缩包。
- 压缩包根目录需要直接包含主题文件，不能把外层文件夹一起压进去：
  - `body_LZ.bin`（必选，主题主体）
  - `bgm.bcstm`（可选，主题 BGM）
  - `preview.png`（可选，Anemone 里的预览图）
  - `layout.bin`（可选，部分主题自带）
- 解压后的散文件目录不会被 Anemone3DS 识别，必须先打包。

## 常用操作

### 安装主题

1. 把主题 `.zip` / `.s3dz` 放进 SD 卡根目录 `/Themes`（没有就新建；传文件方法见 [ftpd与文件传输.md](ftpd与文件传输.md)）。
2. 打开 Anemone3DS，自动扫描 `/Themes`，列表中出现所有主题包。
3. 选中后先 `Preview` 预览，满意再 `Install`。
4. 可一次安装多个主题，之后在 Anemone 里开启主题轮换，或在系统设置中手动切换。
5. 高级：按住 A+方向键可分项安装——上 = 完整安装（含音效/BGM），右 = 只装主题不覆盖现有 BGM，左 = 只装 BGM。

### 安装启动画面（Splashes）

启动画面是开机时 Luma3DS 显示的自定义图片（仅 CFW 环境可用）：

1. 把启动画面文件（`.bin`）放进 SD 卡根目录 `/Splashes`。
2. 打开 Anemone3DS，按 **L**（或触屏 S 图标）切换到 Splash 模式。
3. 选中后按 **A** 安装到 `/luma`，按 **B** 删除当前的 splash。
4. 确保 Luma3DS 配置里启用了 splash 显示（开机按住 Select 进配置页设置）。
5. 主题和启动画面可以在 Anemone 里一起预览后再装。

### 安装徽章（Badges）

徽章是 HOME 菜单里摆在图标旁的小装饰图，Anemone v3.0.0 起支持：

1. 把徽章图片（`.png`）或压缩包放进 SD 卡根目录 `/Badges`。
2. 打开 Anemone3DS，按 **X** 打开附加菜单，再按十字键 **右**（或触屏 Badge 图标）进入徽章安装。
3. 按 **R** 打开转储菜单、十字键 **左** 可把已装徽章备份到 `/3ds/Anemone3DS/BadgeBackups`。

目录组织：`/Badges` 根下的散文件进默认「Other Badges」集；在 `/Badges` 下建子文件夹即成为一个徽章集，文件夹里放 `_seticon.png`（必须 48×48）可作为集的自定义图标。

规则与限制：

- 单张徽章 PNG 最大 384×768 像素（6×12 格，单文件最多切出 72 个徽章），超限直接跳过。
- 整机徽章上限 1000 个，超出部分随机装不上。

## 打包方法（macOS）

进入主题文件夹，选中里面的 `body_LZ.bin`、`bgm.bcstm`、`preview.png` 等文件直接压缩，确保这些文件在压缩包根目录。命令行方式：

```bash
zip -j "主题名.zip" "主题文件夹/"*
```

扩展名用 `.zip` 或 `.s3dz` 均可。

## 注意事项 / 坑

- ⚠️ **装徽章会覆盖已有徽章**：安装前先按上面方法 dump 备份现有徽章，不想丢就别跳过。
- Anemone3DS 不认解压后的主题文件夹，只认 `/Themes` 下的 `.zip` / `.s3dz`，且包内文件必须在根目录。
- 主题预览与实际效果可能受系统版本影响；装完 HOME 菜单起不来时，在 Anemone3DS 里卸载当前主题，或按住 Start 进 GodMode9 删除 HOME 菜单的主题 extdata 后恢复。
- 制作自定义主题/启动画面的在线工具：Yata（主题）、splash-ds（启动画）；Theme Plaza 上有现成主题/启动画/徽章，并提供 QR 码，Anemone 扫码可直接下载安装（需 CFW）。

## 本仓库留档

| 位置 | 内容 |
| --- | --- |
| `tools/3ds/Anemone3DS/` | `Anemone3DS.cia`、`Anemone3DS.3dsx` |

## 来源

- [Anemone3DS 官方仓库与 Releases](https://github.com/astronautlevel2/Anemone3DS/releases)（已核实，最新 v3.0.1）
- [Anemone3DS Wiki - Badges](https://github.com/astronautlevel2/Anemone3DS/wiki/Badges)（徽章目录结构、限制、备份方法）
- [Theme Plaza](https://themeplaza.art/)（主题/启动画/徽章分享站，FAQ 已核实）
