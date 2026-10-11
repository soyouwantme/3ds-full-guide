# NSUI（New Super Ultimate Injector，GBA VC 注入的 Windows 方案）

一句话用途：Windows 专用的 VC 注入器，把 GBA 等复古平台 ROM 烘焙成官方样式的 VC CIA（HOME 菜单 VC 图标、AGB_FIRM 原生硬件运行、系统级存档管理）。macOS 上的对应方案是 vcoven，见 [YANBF与vcoven](YANBF与vcoven.md)。

背景、与 TWiLight 的关系、GBA 硬件限制见 [NDS与GBA](../04-功能模块/45-NDS与GBA.md)。NSUI 只负责 PC 侧生成 CIA，3DS 侧只需 FBI 安装。

## 版本信息

- NSUI **Beta 28**（文件版本 v0.28.0.0，2019-08-19 构建），最后一个公开版本
- 官方发布：[GBAtemp 讨论帖](https://gbatemp.net/threads/discussion-new-super-ultimate-injector-nsui.500376/)（Beta 28 链接在该帖指定楼层；GBAtemp 有 Cloudflare 防护，网络环境不合适时打不开，用本仓库留档即可）
- 系统要求：Windows + .NET Framework 4.5.2+（Win10/11 预装 4.8，开箱即用，无需安装任何运行时）

## 安装与初始化（Windows，2026-10 实测）

1. 下载解压到任意目录（本仓库留档在 `tools/3ds/NSUI/`，解压即用）。
2. 运行 `New Super Ultimate Injector for 3DS.exe`。
3. 首次启动默认**西班牙语界面**：`Herramientas → opciones… → Idioma → English → Guardar`，重启后生效；也可以直接改目录下 `config.ini` 的 `language=` 行（`en`）。
4. ⚠️ 首次使用前必跑 TLS 补丁，否则内置盒图数据库下载必报错（见「坑」第 1 条）：运行一次 `TlsPatcher-1.1.1.exe`（写注册表强制 .NET 走 TLS 1.2，需管理员权限），装完重启 NSUI。

## 生成 GBA VC CIA

1. `File → New → Game Boy Advance`
2. `Project → Load ROM...` 选择 `.gba`（弹出 edit footer banner 提示点确定即可）
3. `CIA metadata`（= 图标）和 `CIA banner`（= 上屏横幅）两区：
   - 点下载按钮可从内置数据库自动抓盒图（需已完成 TLS 补丁）
   - 或 `Load image from file` 手动加载本地图（自动裁剪缩放到 24×24 / 48×48 / 256×128）
4. `Project → Export CIA...`，导出到 SD 卡 `cias/` 文件夹
5. 3DS 上 FBI 安装（`Install and delete`），HOME 菜单出现 VC 图标

- 产物是完整 CIA，**不依赖 SD 卡上保留 rom**；GBA 硬件限制不变：ROM ≤32MB、无联机、无即时存档，见 [NDS与GBA](../04-功能模块/45-NDS与GBA.md)
- ⚠️ 非美版机器（日/欧/韩版）装完 banner 缺 VC 文字框：v28 已知 bug（locale 字节写死 USA_EN），用 [nsui_banner_fixer](https://github.com/pivotiiii/nsui_banner_fixer) 处理生成的 CIA 即可

## 坑（2026-10 Windows 实测）

- ⚠️ **download icon 报 "error unable to download image"：不是网络问题**。NSUI 编译目标是 .NET 4.5.2，老 .NET 的 HTTPS 请求默认不走 TLS 1.2，被现代服务器直接拒绝。解法是跑 [TheLevelUp/pos-tls-patcher](https://github.com/TheLevelUp/pos-tls-patcher) 的 `TlsPatcher-1.1.1.exe`（见安装第 4 步）；若打了补丁仍超时，才轮到网络因素（服务器在境外）。
- ⚠️ 首次启动是西班牙语界面，语言设置藏得深（见安装第 3 步）。
- Beta 28 发布的是 **debug 构建**：exe 有 390MB、启动和导出偏慢，属正常现象。
- 解包 zip 时 `Library/Magnavox Odyssey²` 目录名带特殊字符，Windows 自带 tar / PowerShell `Expand-Archive` 可能报错或漏解，换 7-Zip 或 Git Bash 的 `unzip` 可完整解开；该目录是 Magnavox Odyssey 模板，与 GBA 无关，缺了不影响注入。
- NSUI 支持的系统很多（NES/GB/GBC/GBA/SNES/SFC 系/MD/PCE/PS1 等），但**不支持 N64**——N64 在 3DS 上没有原生硬件，官方 N64 VC 本身也是模拟器且仅新三可用，社区没有好用的注入器，3DS 上玩 N64 只有 DaedalusX64 一条 emulation 路线，体验有限。

## 本仓库留档

| 位置 | 内容 |
| --- | --- |
| `tools/3ds/NSUI/` | NSUI Beta 28 解压即用目录（exe + 各语言 dll + Library 模板 + databases），`config.ini` 已置 `language=en` |
| `tools/3ds/NSUI/downloads/nsui-beta28.zip` | 官方发布原包（105MB，sha256 `38e6c24901bb8c0d07b2380a0e0c88c6fe8e682c491eeba3706fe1315b5a9a8a`） |
| `tools/3ds/NSUI/downloads/TlsPatcher-1.1.1.exe` | TLS 补丁（必装，见「坑」第 1 条） |

## 来源

- [GBAtemp：Discussion - New Super Ultimate Injector (NSUI)](https://gbatemp.net/threads/discussion-new-super-ultimate-injector-nsui.500376/)（Beta 28 发布帖）
- [hacks.guide：3DS Virtual Console / Creation](https://wiki.hacks.guide/wiki/3DS:Virtual_Console/Creation)（流程与 TlsPatcher 依据）
- [TheLevelUp/pos-tls-patcher](https://github.com/TheLevelUp/pos-tls-patcher)（v1.1.1）
- [pivotiiii/nsui_banner_fixer](https://github.com/pivotiiii/nsui_banner_fixer)（非美版机器 banner 修复）
