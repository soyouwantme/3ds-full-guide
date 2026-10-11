# 工具百科总表

本目录收录 3DS 折腾用到的核心工具：3DS 上运行的自制程序/固件，以及 PC（macOS）端配套工具。只链接工具与自制软件的官方发布页，不提供任何游戏 ROM/CIA 下载指引。

## 3DS 端：系统底层

| 工具 | 一句话用途 | 运行位置 | 官方下载 | 本仓库留档 | 详见 |
| --- | --- | --- | --- | --- | --- |
| Luma3DS | 自制固件本体，开机加载 `boot.firm`，提供 Rosalina 菜单、LayeredFS、区域模拟 | 3DS：FIRM（SD 根目录 `boot.firm`） | [LumaTeam/Luma3DS Releases](https://github.com/LumaTeam/Luma3DS/releases) | `tools/3ds/Luma3DS-v13.4/`（boot.firm + boot.3dsx） | [Luma3DS.md](Luma3DS.md) |
| GodMode9 | 万能文件管理器：NAND 备份/恢复、刷写、解密、GM9 脚本 | 3DS：FIRM payload（`luma/payloads/`，START+开机） | [d0k3/GodMode9 Releases](https://github.com/d0k3/GodMode9/releases) | `tools/3ds/GodMode9-v2.2.3/` | [GodMode9.md](GodMode9.md) |
| Homebrew Launcher（hbmenu） | `.3dsx` 自制程序运行环境 | 3DS：`boot.3dsx` + `3ds/` 目录，或 hblauncher_loader CIA | [fincs/new-hbmenu Releases](https://github.com/fincs/new-hbmenu/releases)、[yellows8/hblauncher_loader Releases](https://github.com/yellows8/hblauncher_loader/releases)（已归档） | `tools/3ds/hblauncher_loader/` | [HomebrewLauncher.md](HomebrewLauncher.md) |

## 3DS 端：安装与数据管理

| 工具 | 一句话用途 | 运行位置 | 官方下载 | 本仓库留档 | 详见 |
| --- | --- | --- | --- | --- | --- |
| FBI | `.cia` 安装/卸载、批量与 QR 安装、tickets/titles 管理 | 3DS：CIA（+ 3DSX） | [Steveice10/FBI Releases](https://github.com/Steveice10/FBI/releases) | `tools/3ds/FBI/` | [FBI.md](FBI.md) |
| TYSS | 游戏存档备份/恢复，JKSM 的继承者（中文界面、云存档、DSiWare/GBA VC 支持），已替代 Checkpoint | 3DS：CIA / 3DSX | [R-YaTian/TYSS Releases](https://github.com/R-YaTian/TYSS/releases) | `tools/3ds/TYSS/` | [TYSS.md](TYSS.md) |
| Universal-Updater | 自制程序在线更新器（Universal-DB 商店） | 3DS：CIA / 3DSX | [Universal-Team/Universal-Updater Releases](https://github.com/Universal-Team/Universal-Updater/releases) | `tools/3ds/Universal-Updater/` | [Universal-Updater.md](Universal-Updater.md) |
| Anemone3DS | 主题、启动画面、徽章管理 | 3DS：CIA / 3DSX | [astronautlevel2/Anemone3DS Releases](https://github.com/astronautlevel2/Anemone3DS/releases) | `tools/3ds/Anemone3DS/` | [Anemone3DS.md](Anemone3DS.md) |
| ftpd | 3DS 上开 FTP 服务，无线传文件 | 3DS：3DSX（新版已移除 CIA 构建） | [mtheall/ftpd Releases](https://github.com/mtheall/ftpd/releases) | `tools/3ds/ftpd/` | [ftpd与文件传输](ftpd与文件传输.md) |
| LumaLocaleSwitcher | 为单个游戏设置区域/语言模拟（配合 Luma game patching） | 3DS：CIA / 3DSX | [Possum/LumaLocaleSwitcher Releases](https://github.com/Possum/LumaLocaleSwitcher/releases)（2026-04 已归档） | `tools/3ds/LumaLocaleSwitcher/` | [LumaLocaleSwitcher](LumaLocaleSwitcher.md) |
| 3DSident | 硬件/系统信息查看（序列号、屏幕 TN/IPS、电池、NNID 等） | 3DS：CIA / 3DSX | [joel16/3DSident Releases](https://github.com/joel16/3DSident/releases) | 未留档 | — |
| Cthulhu | 清除/编辑游戏列表缓存、游玩记录、步数、设置 play coins | 3DS：3DSX / CIA | [Ryuzaki-MrL/Cthulhu Releases](https://github.com/Ryuzaki-MrL/Cthulhu/releases) | 未留档 | — |
| faketik | 重建 ticket 缺失导致 HOME 菜单不显示的游戏 | 3DS：3DSX | [ihaveamac/faketik Releases](https://github.com/ihaveamac/faketik/releases) | 未留档 | — |
| TWLFix (TWLFix-CFW) | 修复 TWL（DS 模式）系统模块，修复后需系统更新 | 3DS：3DSX | [MechanicalDragon0687/TWLFix-CFW Releases](https://github.com/MechanicalDragon0687/TWLFix-CFW/releases) | 未留档 | — |
| 中文字库 | 日版机游戏/系统显示简体中文（字库补丁类工具） | 3DS：CIA / 3DSX | 见对应页面（社区工具） | `tools/3ds/acg-font-tool/` | [中文字库](中文字库.md) |

## 3DS 端：游戏相关

| 工具 | 一句话用途 | 运行位置 | 官方下载 | 本仓库留档 | 详见 |
| --- | --- | --- | --- | --- | --- |
| BootNTR Selector | 启动 NTR 3.x（金手指、截屏、串流），游戏内 Y+X 呼出菜单 | 3DS：CIA / 3DSX | [Nanquitas/BootNTR Releases](https://github.com/Nanquitas/BootNTR/releases) | 未留档 | [BootNTR与金手指](BootNTR与金手指.md) |
| TWiLight Menu++ | NDS/GBA/复古平台游戏菜单（基于 nds-bootstrap） | 3DS：CIA / 3DSX | [DS-Homebrew/TWiLightMenu Releases](https://github.com/DS-Homebrew/TWiLightMenu/releases) | `tools/3ds/TWiLightMenu/` | [NDS与GBA](../04-功能模块/45-NDS与GBA.md) |
| mGBA | GBA 模拟器（3DS 原生移植） | 3DS：CIA / 3DSX | [mgba-emu/mgba Releases](https://github.com/mgba-emu/mgba/releases) | `tools/3ds/mGBA/`（mGBA-0.10.5-3ds.7z） | [NDS与GBA](../04-功能模块/45-NDS与GBA.md) |
| Nimbus | Pretendo 非官方联机服务客户端（NNID ↔ PNID 切换） | 3DS：CIA / 3DSX | [PretendoNetwork/Nimbus Releases](https://github.com/PretendoNetwork/Nimbus/releases) | 无 | [Pretendo联机](../04-功能模块/46-联网对战.md) |
| JKSM | 存档导入导出（老工具，已被其继承者 [TYSS](TYSS.md) 取代） | 3DS：3DSX | [J-D-K/JKSM Releases](https://github.com/J-D-K/JKSM/releases) | 未留档 | — |
| PKSM | 宝可梦存档管理与配信注入 | 3DS：CIA / 3DSX | [FlagBrew/PKSM Releases](https://github.com/FlagBrew/PKSM/releases) | 未留档 | — |

## PC 端（macOS）

| 工具 | 一句话用途 | 运行位置 | 官方下载 | 本仓库留档 | 详见 |
| --- | --- | --- | --- | --- | --- |
| 3DSort | 在电脑上整理 3DS HOME 菜单布局（拖拽图标/文件夹/排序） | PC：macOS / Windows / Linux | [SalustLab/3DSort Releases](https://github.com/SalustLab/3DSort/releases) | `tools/pc/3DSort-v1.1.0-macos-arm64.zip`、`tools/pc/3dsort-mirror/`、`tools/pc/3dsort-ftp-bridge.py` | — |
| Tulip3DS | PC 端 3DS 模拟器 + 无线直装 CIA 工具集 | PC：macOS（配 3DS 端 Client CIA/Installer） | [OasisAkari/Tulip3DS Releases](https://github.com/OasisAkari/Tulip3DS/releases) | `tools/pc/Tulip3DS.app`、`tools/pc/Tulip3DS-1.8-macos.zip` | — |
| YANBF | NDS 游戏转 HOME 菜单 forwarder CIA（macOS 原生，跳过 TWiLight 直启） | PC：macOS / Windows / Linux | [YANBForwarder/YANBF Releases](https://github.com/YANBForwarder/YANBF/releases) | `tools/3ds/YANBF/` | [YANBF与vcoven](YANBF与vcoven.md) |
| vcoven | GBA ROM 烘焙成 3DS VC CIA（Mac 原生 CLI，免 donor，替代 NSUI） | PC：macOS Apple Silicon | [vedoot/vcoven](https://github.com/vedoot/vcoven) | `tools/3ds/vcoven/`（手动安装实例） | [YANBF与vcoven](YANBF与vcoven.md) |

## 说明

- 所有下载链接均为各项目官方 GitHub Releases，已逐一核实可达。
- 「本仓库留档」列的文件为官方发布原包，存放路径相对于仓库根目录。
- 部分工具（3DSident、JKSM、PKSM、faketik、TWLFix、Cthulhu、BootNTR）未在仓库留档，使用时从官方 Releases 下载，或在机器上用 [Universal-Updater](Universal-Updater.md) 在线安装。
- mGBA（3DS 版）没声音是缺 DSP 固件：用 Luma Rosalina 导出（**L+十字键下+Select** → Miscellaneous options → Dump DSP firmware），生成 `/3ds/dspfirm.cdc`，一次性。3DS 版呼出菜单按 **X**（HOME 键是系统级的，呼不出模拟器菜单），**Y** 循环切换屏幕缩放模式。
- ⚠️ 老教程说的「GodMode9 → HOME → More... → Dump DSP firmware」是错的：GodMode9 全版本都没有这个功能，DSP 固件导出始终在 Luma 的 Rosalina 菜单里。
