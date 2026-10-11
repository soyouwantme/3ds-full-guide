# TWiLight Menu++ 与 NDS/GBA

TWiLight Menu++ 的安装、ROM 放置和常见问题。

## 版本信息

- TWiLight Menu++ v27.24.1（2026-05-24 发布），内置 nds-bootstrap Release v2.16.0
- 下载：GitHub Releases 的 `TWiLightMenu-3DS.7z`（本仓库留档：`tools/3ds/TWiLightMenu/TWiLightMenu-3DS.7z`）
- GBA/复古平台另需同版本 Release 的 `AddOn-VirtualConsoleFull-3DS.7z`（本仓库留档：`tools/3ds/TWiLightMenu/AddOn-VirtualConsoleFull-3DS.7z`）

## 安装步骤

1. 把 `TWiLight Menu.cia`、`BOOT.NDS`、`_nds/`、`roms/` 等文件复制到 SD 卡根目录。
2. 3DS 开机进入 FBI，安装 SD 卡根目录的 `TWiLight Menu.cia`。
3. 回到 HOME 菜单出现 TWiLight Menu++ 图标；也可以用根目录 `BOOT.NDS` 直接启动。

## ROM 放置

- NDS 游戏：`roms/nds/`
- GBA 游戏：`roms/gba/`
- DSi 独占/DSiWare：`roms/dsi/`

放好即出现在 TWiLight 的列表中；GBA 还需要下面的 Virtual Console 扩展包才能显示。

## 运行原理：NDS/GBA 是原生还是模拟

结论：NDS 和 GBA 在 3DS 上**都不是软件模拟**，而是重启进另一套模式，跑在 SoC 里保留的向下兼容硬件上。只有 NES/GB/GBC/SNES/GG 的 VC 才是模拟器——3dbrew 原文：*"Except for GBA VC, the games are ran using emulators"*。

| 路径 | 模式/机制 | 说明 |
| --- | --- | --- |
| NDS 卡带、DSiWare | TWL_FIRM（DS/DSi 模式） | ARM11 关闭，用 DS 的 ARM9 + ARM7 真硬件，无兼容性列表 |
| GBA VC、open_agb_firm | AGB_FIRM（GBA 模式） | 用 3DS 内置 GBA 硬件原生运行 |
| GBARunner2（TWiLight） | DS 模式 hypervisor | 借 ARM7 执行游戏代码，兼容性/音画不如 AGB 模式 |
| mGBA、RetroArch 等 | 纯软件模拟 | 与硬件无关，兼容性看模拟器 |

- NDS：插 DS 卡带即重启进 TWL_FIRM，DS 模式 ARM9 67MHz、DSi 模式 133MHz；DS 侧 ARM7 代码跑在 SoC 内真实的 ARM7 核上（[3dbrew: FIRM](https://www.3dbrew.org/wiki/FIRM)）。所以 NDS 游戏 100% 原生，代价只是用不上 3DS 机能、画面不增强。
- GBA 原生路线：[3dbrew: 3DS Virtual Console](https://www.3dbrew.org/wiki/3DS_Virtual_Console) 说明 GBA VC 由 AGB_FIRM 负责；open_agb_firm 文档也写明用 "the 3DS's built-in GBA hardware"，且必须以 FIRM 运行才拿得到 GBA 硬件访问权（[open_agb_firm](https://github.com/profi200/open_agb_firm)）。3DS 没有 Slot-2 卡槽，所以限制来自硬件缺失而非模拟精度：
  - ROM 上限 32MB（>256Mbit 不支持）
  - 无联机线（GBA 串口没接出来）
  - 卡带附加硬件（陀螺仪、震动、阳光传感器）需要 patch
  - 无即时存档，换游戏必须重启
  - 声音有 aliasing（硬件 bug，无解）
  - AGB_FIRM 有画面底部回卷错行的 bug，open_agb_firm 已修
- GBARunner2（配合 TWiLight 使用的 GBA 方案）：TWiLight 在 3DS 上跑的是 DS 模式，GBA 由 GBARunner2 这个 hypervisor 承载（[GBARunner2](https://github.com/Gericom/GBARunner2)）；DSi/3DS + SD 卡必须用 ARM7 dldi 版本，即上面扩展包里的 `_nds/GBARunner2_*.nds`。
- 想要最接近真机的 GBA 体验：做 GBA VC 注入，或改用 open_agb_firm（`open_agb_firm.firm` 放 `luma/payloads`，开机按住 **START** 启动）。

## HOME 菜单图标直启（forwarder 与 VC 注入）

不想每次从 TWiLight 列表选游戏，可以把 NDS/GBA 游戏做成 HOME 菜单图标点开直玩，macOS 上均已验证可用：

| 游戏类型 | 方案 | 产物 | 依赖 |
| --- | --- | --- | --- |
| NDS | [YANBF](https://github.com/YANBForwarder/YANBF) forwarder | 几百 KB 的 CIA（启动器，rom 本体仍须在卡上原路径） | forwarder 支持文件 + bootstrap.cia（SD 侧一次性配置） |
| GBA | [vcoven](https://github.com/vedoot/vcoven) VC 注入 | 完整 VC CIA（AGB_FIRM 原生运行，Mac 原生 CLI，免 Windows/NSUI） | 无 |

安装、生成命令与踩坑详见 [YANBF与vcoven](../02-工具百科/YANBF与vcoven.md)。与 TWiLight 是互补关系：图标直启适合常玩的少量游戏（每加一个要回电脑生成），游戏库批量管理仍靠 TWiLight（rom 丢进 `roms/` 即玩）。

## GBA 游戏不显示的问题

- 现象：TWiLight 里能看到 NDS 游戏，但看不到 `roms/gba/` 下的 `.gba`。
- 原因：TWiLight 在 3DS 上默认只显示 NDS 类文件；GBA 需要安装 Virtual Console 扩展包，`GBARunner2` 也随扩展包提供。
- 修复：从官方 v27.24.1 Release 下载并合并 `AddOn-VirtualConsoleFull-3DS.7z`，把 `_nds/GBARunner2_*.nds`、`_nds/TWiLightMenu/addons/Virtual Console/`、`_nds/TWiLightMenu/emulators/` 和 `roms/` 下的复古平台文件夹复制到 SD 卡。

## 校验

- 复制后用 `cmp` 或 `rsync` 逐文件校验；SD 卡上的 `._*` 只是 macOS 元数据文件，可忽略。

参考：[TWiLight Menu++ Releases](https://github.com/DS-Homebrew/TWiLightMenu/releases)
