# LumaLocaleSwitcher（区域 / 语言模拟）

按游戏单独设置区域（Region）与语言（Language）的小工具，给 Luma3DS 的区域模拟功能提供图形化界面。

## 一句话用途

配合 Luma3DS 的 game patching，为**单个游戏**生成区域/语言模拟配置（`luma/titles/<TitleID>/locale.txt`），让跨区游戏以为自己运行在指定区域/语言的机器上，避免启动报错或语言错乱。

## 下载与安装

- 官方发布：[Possum/LumaLocaleSwitcher Releases](https://github.com/Possum/LumaLocaleSwitcher/releases)（已核实，最新 v0.04）。
- ⚠️ 项目**已不再维护**，v0.04 是最终版；功能稳定，够用。
- 装法二选一：FBI 安装 `.cia`，或把 `.3dsx` 放 `/3ds/` 从 Homebrew Launcher 启动。
- 前置：Luma3DS 并开启 **Enable game patching**（开机按住 Select 进配置页勾选）。

## 常用操作

1. 启动 LumaLocaleSwitcher，选择 **Titles**，会列出本机已安装的游戏标题。
2. 找到目标游戏，设置需要的区域（如 USA、EUR、JPN）和语言（如 English、Japanese）。
3. 保存后工具会在 `luma/titles/<TitleID>/locale.txt` 写入配置。
4. 回到 HOME 菜单启动该游戏即按模拟的区域/语言运行。

补充：

- 按 **Y** 可以切换显示隐藏的标题（DS 游戏、系统标题等，一般人不用动）。
- 少数标题需要自己填路径/自定义配置（`/locales.conf`），日版机玩日版游戏绝大多数情况用不到本工具。

## 注意事项 / 坑

- 只对有跨区模拟需求的**外区**游戏有意义；日版机运行日版游戏不需要任何 locale 设置。
- 典型症状才需要它：跨区游戏启动崩溃、ErrDisp 报错，或游戏内语言/字体显示异常。
- 设置不生效时先确认 Luma 的 game patching 已开启，且 `luma/titles/<TitleID>/locale.txt` 确实存在。
- locale 模拟与 Luma3DS 版本相关，Luma 大版本升级后留意该区域功能变化。

## 本仓库留档

| 位置 | 内容 |
| --- | --- |
| `tools/3ds/LumaLocaleSwitcher/` | `LumaLocaleSwitcher-0.04.3dsx`、`LumaLocaleSwitcher-0.04.cia`、`LumaLocaleSwitcher-0.04.zip` |

## 来源

- [Possum/LumaLocaleSwitcher](https://github.com/Possum/LumaLocaleSwitcher)（已核实：仓库存在、标注不再维护、最新 v0.04）
