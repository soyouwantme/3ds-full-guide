# BootNTR 与金手指

NTR CFW 插件系统的使用说明：用 BootNTR Selector 加载 NTR，配合金手指（cheats）/插件（plugins）在游戏中实时修改数值。

## BootNTR Selector

一句话用途：NTR CFW 的加载器（前端）。NTR 是 3DS 上的插件框架，负责注入游戏进程并提供金手指菜单、屏幕滤镜等功能；BootNTR Selector 负责选择并启动对应固件版本的 NTR。

### 下载与安装

- 官方发布：[Nanquitas/BootNTR Releases](https://github.com/Nanquitas/BootNTR/releases)（已核实，最新 v2.13.7）
- ⚠️ **v2.13.7 要求 Luma3DS v13.0 或更新**；旧版 Luma 上无法工作（v2.13.6 在官方 Luma 上还有崩溃问题，别用）。
- 3dsx 版：放 `sd:/3ds/BootNTRSelector/BootNTRSelector.3dsx`，从 Homebrew Launcher 启动。
- CIA 版：用 FBI 安装后在 HOME 菜单直接启动（banner 选默认或备选纯属外观）。

### 启动

1. 开机进 HOME 菜单后，启动 BootNTR Selector。
2. 首次启动会问选择 NTR 版本，按提示确认；屏幕闪三下（蓝绿黄）表示 NTR 注入成功。
3. 注入成功后回到 HOME 菜单，再进游戏。

## 金手指（Cheats）

- 金手指文本放在 SD 卡根目录 `/cheats/<游戏TitleID>.txt`（`[码名]` + 代码行的标准格式），进游戏后 NTR 自动加载对应 TitleID 的代码。
- 插件（`.3gx`/`.plg`）放在 SD 卡根目录 `/plugin/`。
- 游戏内按 **Y + X** 呼出金手指菜单，勾选要启用的代码。
- 金手指文本与插件只需按上述路径放好，无需安装；TitleID 可在 FBI 的 Titles 列表或 [3dsdb](https://3dsdb.com/) 等数据库查到。

### 注意事项 / 坑

- ⚠️ **一次只能进一款游戏**：启动金手指（NTR/BootNTR）后一次只能进一款游戏；想换游戏要先关机重启，连续切换会死机。
- ⚠️ 部分机器上 NTR 与 Luma3DS Plugin Loader 同时用会冲突；新版 Luma（v13+）自带插件加载功能，若两个系统都注入同一游戏容易崩溃，建议二选一。
- 金手指代码分系统版本，固件升级后旧代码可能失效或造成死机，来源不可靠的码慎用。

## Mode3 说明

- Mode3 是给**老三（Old 3DS/2DS）大内存游戏**用的版本：部分游戏在老三上以 80MB 扩展内存模式运行（判断方法：从 HOME 菜单关闭游戏时主机会重启），这类游戏必须用 Mode3 版 BootNTR 才能注入。
- **新三（New 3DS/2DS）不需要 Mode3**，直接用普通版。
- Mode3 的另一种用法是配合中文字库：ACG_Big_SFontT 以 80M 内存模式运行，与老三大内存游戏（如《蘑菇队长》汉化版）配合显示汉字，用法与 BootNTR-Mode3 类似。详见 [中文字库.md](中文字库.md)。
- 文件名带 **Mode3** 标签的构建就是专用版；不带即普通版。

## 本仓库留档

无（BootNTR Selector 从官方 Releases 下载安装即可，仓库未留档）。

## 来源

- [Nanquitas/BootNTR Releases](https://github.com/Nanquitas/BootNTR/releases)（已核实，最新 v2.13.7，要求 Luma3DS ≥ v13.0）
- 踩坑记录：[14-坑与技巧.md](../01-系统折腾/14-坑与技巧.md)（NTR 金手指一次只能进一款游戏、游戏内 Y+X 呼出）
