# Homebrew Launcher（HBL / hbmenu）

`.3dsx` 自制程序的运行环境：一个菜单，列出并启动 SD 卡 `3ds/` 目录下的所有 `.3dsx` 程序。boot9strap + Luma 时代，它是跑自制工具的标准入口。

## 两个组成部分

| 组成 | 是什么 | 位置 |
| --- | --- | --- |
| Homebrew Menu（hbmenu） | `.3dsx` 菜单本体，Luma 默认的 homebrew 加载器 | SD 卡根目录 `boot.3dsx` |
| hblauncher_loader | 一个 CIA"壳"，从 HOME 菜单图标进入 hbmenu（无卡/无 Rosalina 场景用） | 用 FBI 安装的普通应用 |

boot9strap 时代 Luma 直接提供 Rosalina 入口，大多数情况下 `boot.3dsx` 就够了；`hblauncher_loader.cia` 让 HBL 以应用形式出现在 HOME 菜单。两种方式互不冲突，可以都装。

## 下载与安装位置

- Homebrew Menu 官方下载：[fincs/new-hbmenu Releases](https://github.com/fincs/new-hbmenu/releases)（已核实，最新 v2.4.3；Luma v13.1+ 发布包已自带同版 `boot.3dsx`）
- hblauncher_loader 官方下载：[yellows8/hblauncher_loader Releases](https://github.com/yellows8/hblauncher_loader/releases)（已核实，最新 v1.3；**项目已于 2020-12 归档**，只读，但发布包仍可下载）
- 安装：
  - `boot.3dsx` → SD 卡根目录
  - 每个自制程序一个子目录放 `3ds/`：
    ```
    /3ds/
    ├── FBI/FBI.3dsx
    ├── TYSS/TYSS.3dsx
    └── <程序名>/<程序名>.3dsx
    ```
  - `hblauncher_loader.cia` → FBI 安装
- 本仓库留档：`tools/3ds/hblauncher_loader/`（`hblauncher_loader.cia` + 原包 `hblauncher_loader_v1.3.zip`）；`boot.3dsx` 见 `tools/3ds/Luma3DS-v13.4/boot.3dsx`

## 常用操作

- 启动 hbmenu：Rosalina 菜单（L+下+Select）→ `Switch the hb. title to the current app.` 或直接重启进 `boot.3dsx`；装了 hblauncher_loader 则直接点 HOME 菜单图标。
- 菜单内：方向键选程序，A 启动，Y 查看详情，SELECT 收藏。
- `3dslink`（devkitPro 工具）可向 hbmenu 的 NetLoader 直接推送 3dsx 调试（按 Y 开启接收）。

## 与 CIA 应用的区别

| | `.3dsx`（HBL 运行） | `.cia`（FBI 安装） |
| --- | --- | --- |
| 安装 | 拷文件到 `3ds/` 即可 | 要写系统 title 数据库 |
| 入口 | 先进 HBL 再选 | HOME 菜单直接有图标 |
| 权限 | 受 homebrew 环境限制，部分功能（改系统数据）需要 Rosalina patches | 完整 title 权限，图标/ Banner 完整 |
| 卸载 | 删文件 | FBI → Titles 卸载 |

日常装 CIA 版更顺手；3dsx 版适合轻量工具、或需要"用完即走"不留系统痕迹的场景（如 Cthulhu 作者推荐 3dsx 版，不进活动记录）。

## 注意事项 / 坑

- 部分 `.3dsx` 依赖 Rosalina 的 SM/FS patches 才能读系统信息（如 JKSM Rosalina 版）；Luma 下默认已开启。
- `boot.3dsx` 丢失不影响开机，但进不了 HBL；很多工具的"最后一道门"是它。
- hblauncher_loader 已归档不再更新，能装但不再需要更新；hbmenu 本体跟随 Luma 发布包更新即可。

## 来源

- [fincs/new-hbmenu Releases](https://github.com/fincs/new-hbmenu/releases)（下载链接已核实）
- [yellows8/hblauncher_loader Releases](https://github.com/yellows8/hblauncher_loader/releases)（下载链接已核实，已归档）
- [备份与恢复](../01-系统折腾/10-备份与恢复.md)（SD 卡目录结构）
