# Luma3DS

自制固件（CFW）本体：开机时由 boot9strap 加载，接管系统启动，并提供 Rosalina 菜单、LayeredFS 游戏补丁、区域模拟等核心能力。折腾过的 3DS 上几乎所有功能都建立在 Luma 之上。

## 下载与安装位置

- 官方下载：[LumaTeam/Luma3DS Releases](https://github.com/LumaTeam/Luma3DS/releases)（已核实，最新 v13.4）
- 安装：解压后把 `boot.firm` 放到 **SD 卡根目录**；`boot.3dsx`（Homebrew Menu）也放根目录（Luma v13.1+ 的发布包自带）
- 本仓库留档：`tools/3ds/Luma3DS-v13.4/`（`boot.firm` + `boot.3dsx` + config）

## 启动链

```
开机 → boot9strap（引导程序，装在南桥） → 读 SD 卡根目录 boot.firm（Luma3DS） → Luma 启动 sysNAND 系统
```

- 关机后按住 **Select** 再开机：进入 Luma 配置页（第一行显示当前版本）。
- Luma v13 按住 **Start** 开机会先进配置页，chainloader 入口是菜单里的 **Boot chainloader**（再选 GodMode9 等 payload）。
- 配置改动必须选 **Save and exit** 才写入；只选 Boot chainloader 直接走，改动不保存。

## 配置页（Select+开机）常用项

| 选项 | 含义 | 建议 |
| --- | --- | --- |
| Enable game patching | 开启 LayeredFS / IPS 补丁 / 区域模拟（`luma/titles/` 生效的前提） | 开 `(x)` |
| Show NAND or user string... | 系统设置里显示自定义文字 | 随意 |
| Enable loading external FIRMs and modules | 加载 `luma/firms/`、`luma/sysmodules/` 外部模块 | 按需 |
| Enable region/language emulation via .txt files | 读取 `luma/titles/<ID>/locale.txt` 做区域/语言模拟 | 开（配合 LumaLocaleSwitcher） |
| Force routing... / Force quad-core... | New 3DS 锁频相关 | 默认 |
| Autoboot EmuNAND | 用 emuNAND 启动 | 一般用不到，保持关 |
| Show current time... / Show GBA boot screen... | 开机画面相关 | 随意 |

## game patching 与 LayeredFS

开启 `Enable game patching` 后，Luma 会按 TitleID 加载补丁：

```
luma/titles/<16位TitleID>/
├── code.ips      # 代码补丁（按固件/游戏版本针对原版代码，改过的代码不生效）
├── code.bin
├── locale.txt    # 区域模拟："区域 语言"，如 "JPN JP"、"USA EN"
└── romfs/        # 资源覆盖（汉化、贴图、字库等）
```

⚠️ 改机店的换系统机器有一个隐蔽坑：NAND 里的 HOME 菜单可能被注入过非原版代码，此时按原版固件制作的 LayeredFS `code.ips` 补丁会**完全静默失效**（系统照常启动、无报错）。判断线索：同版本机器上能用的补丁、这台不行，加上机器来源是改机店。解法是整体替换系统（官方简体中文安装器），见 [简体中文系统](../01-系统折腾/11-简体中文系统.md)。

区域模拟：对单个游戏强制区域/语言，用 [LumaLocaleSwitcher](LumaLocaleSwitcher.md)（同事整理中）生成 `locale.txt`，或在 PC 上手动写入。日版机玩美版卡带、或让某游戏显示特定语言时用。

## SecureInfo_C

- Luma 启动时优先读取 NAND 里的 `SecureInfo_C`（若存在）替代原厂的 `SecureInfo_A`，系统里显示的序列号由此而来。
- 修改序列号 = 替换 `SecureInfo_C` 里的序列号字段，可用 GodMode9 或脚本（`tools/pc/change_serial.py`）完成。详见 [修改序列号](../01-系统折腾/12-修改序列号.md)。

## Rosalina 菜单

开机后随时按 **L + 下 + Select** 呼出：

| 功能 | 说明 |
| --- | --- |
| Take screenshot | 截屏（存 `/luma/screenshots/`，PNG） |
| Change screen brightness | 调亮度 |
| Cheats... | 金手指（配合 plugin loader） |
| Plugin loader / Chainloader | 插件加载、加载其它 3DSX |
| Process list | 进程列表，可接管/挂载游戏进程 |
| System configuration | 强制 Wi-Fi 连接、音量覆盖等 |
| System Information | 屏幕类型（TN/IPS）等硬件信息 |
| Power off / Reboot | 关机/重启 |

## 更新方法

1. 先备份 SD 卡根目录旧 `boot.firm`（改名加 `.bak` 后缀留底，如 `boot_old.firm.bak`）。
2. 用新版 `boot.firm` 覆盖根目录。
3. 重启，Select+开机 确认版本号。

⚠️ 更新前先做 NAND 备份；Luma 版本必须 ≥ 目标系统固件的支持线——版本过低（如 v7.1）不支持较新系统固件（11.15+）会导致开机黑屏，电源灯亮但无画面，见 [坑与技巧](../01-系统折腾/14-坑与技巧.md)。

## 注意事项 / 坑

- ⚠️ Luma 默认只装在 SD 卡：不插卡完全无法开机。想不插卡也能开机，可用 GodMode9 把 `boot.firm` 写入 `1:/boot.firm` 并在 Luma 配置里启用对应选项（装进 CTRNAND）。
- 不要删 SD 卡根目录 `boot.firm`；删了开不了机。
- 二手卡的 `boot.firm` 不要默认能用，先在 Select 菜单或用 `strings boot.firm | grep Luma` 确认版本。
- IPS 屏闪屏/烧屏问题在 v13.3.3 已修复；v13.2 起 Rosalina 可校正 IPS 屏色偏。

## 来源

- [LumaTeam/Luma3DS Releases](https://github.com/LumaTeam/Luma3DS/releases)（下载链接已核实）
- [3ds.hacks.guide](https://3ds.hacks.guide/)（配置项通用说明）
- [简体中文系统](../01-系统折腾/11-简体中文系统.md)、[坑与技巧](../01-系统折腾/14-坑与技巧.md)（LayeredFS 静默失效、旧版 Luma 黑屏的详细说明）
