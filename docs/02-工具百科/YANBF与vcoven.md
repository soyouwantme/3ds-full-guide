# YANBF 与 vcoven（NDS/GBA 游戏做成 HOME 菜单图标）

把 NDS、GBA 游戏从「TWiLight 列表里选」升级成「HOME 菜单一个图标点开直玩」的两个 PC 端工具，均为 macOS 原生可用。本文覆盖：安装、生成、SD 卡配套文件、踩坑记录。

背景与运行原理见 [NDS与GBA](../04-功能模块/45-NDS与GBA.md)；与 TWiLight 的关系：**图标直启和 TWiLight 列表互补**——常玩的少量游戏做图标，游戏库管理仍靠 TWiLight。

## YANBF：NDS 游戏 → forwarder CIA

一句话用途：把 `.nds` 生成几百 KB 的 forwarder CIA，装到 HOME 菜单后点图标直接经 nds-bootstrap 启动游戏，跳过 TWiLight 二级菜单。从 3DS 模式启动（不走 DSiWare 模板），不受 40 个 DSiWare 标题限制，只占 HOME 菜单 300 图标空间。

- 官方发布：[YANBForwarder/YANBF Releases](https://github.com/YANBForwarder/YANBF/releases)（macOS/Windows/Linux 均有，v1.6.3 已验证）
- 配套 SD 卡文件包：[RocketRobz/NTR_Forwarder Releases](https://github.com/RocketRobz/NTR_Forwarder/releases)（下载 `DS.Game.Forwarder.pack.nds-bootstrap.7z`，取其中 `for SD Card root/` 全部内容拷到 SD 卡根目录，与已有 `_nds/` 合并）

### 安装（SD 卡侧，一次性）

1. Forwarder pack 的 `for SD Card root/_nds/` 合并进 SD 卡根目录（提供 `ntr-forwarder/sdcard.nds`、自带 nds-bootstrap，**不依赖 TWiLight 已装**）。
2. YANBF 的 `bootstrap.cia` 拷到 `sd:/cias/`，FBI 安装（必装，先装）。
3. 从 TWiLight 7z 的 `_nds/TWiLightMenu/extras/` 把 `widescreen.pck` 拷到 `sd:/_nds/ntr-forwarder/`（宽屏补丁支持，可选但推荐）。
   - ⚠️ YANBF README 还提到 `apfix.pck`——**v27 起 TWiLight 已不再附带**（AP 补丁内置于 nds-bootstrap），找不到是正常的，跳过即可。

### 生成 forwarder（PC 侧，每个游戏一次）

```bash
cd <generator 目录>   # 必须在该目录内运行，脚本用相对路径找 data/
./generator -p "/roms/nds/游戏名.nds" -o "输出.cia" "本地rom路径.nds"
```

- `-p`：ROM 在 **SD 卡上的路径**（POSIX 风格，从 `/` 起）；不插卡转 ROM 时必给，且必须与将来卡上的实际位置**完全一致**
- 生成的 CIA 只有几百 KB（图标/标题从 ROM 提取），游戏本体仍是卡上的 `.nds`——**rom 文件必须留在卡上对应路径**，移动/改名后图标失效
- 装了 TWiLight 的机器也可以让 Universal-Updater 装 YANBF Forwarder Pack 来更新 SD 侧文件

### 坑

- ⚠️ 生成器必须在 `generator/` 目录内运行，否则报 `data/icon.png` 找不到。
- 每新增一个 NDS 游戏都要回电脑重新生成 CIA + FBI 安装；批量游戏不如直接丢 `roms/nds/` 用 TWiLight 方便。

## vcoven：GBA ROM → Virtual Console CIA

一句话用途：Mac 原生 CLI，把 `.gba` 烘焙成官方样式的 GBA VC CIA（HOME 菜单 VC 图标、AGB_FIRM 原生硬件运行、系统级存档管理）。免 donor、免 Wine、免 Windows——是 NSUI（Windows 专用）在 Mac 上的替代。

- 官方发布：[vedoot/vcoven](https://github.com/vedoot/vcoven)（v0.1 仅支持 macOS Apple Silicon，稳定；Intel Mac 未提供）及网页版 [vcoven.com](https://vcoven.com)
- 安装：`brew tap vedoot/vcoven && brew install vcoven`

### 用法

```bash
vcoven build 游戏.gba \
  --title "短标题" --long-title "完整标题" --publisher "发行方" \
  --title-id 0004000000F00100 \
  --icon icon.png --banner banner.png \
  -o 输出.cia
```

- `--title`、`--title-id`、`--icon`、`--banner` 必填
- `--title-id` 选未占用段（如 `0004000000F00X00`），避免与已装标题冲突
- 存档类型自动识别（扫描 ROM 内 `EEPROM_V`/`FLASH*_V`/`SRAM_V` 签名），也可 `--save-type` 强制
- 产物是完整 CIA（16MB ROM ≈ 16.5MB CIA），FBI 安装即可，**不依赖卡上保留 rom**
- GBA 硬件限制不变：ROM ≤32MB、无联机、无即时存档，见 [NDS与GBA](../04-功能模块/45-NDS与GBA.md)

### 坑（2026-10 macOS 实测）

- ⚠️ **图标/横幅必须自备，且不给图就是空白图标**（YANBF 会自动从 ROM 提图标，vcoven 不会——这是它相对 YANBF 的主要短板）。`--icon`、`--banner` 必填，想要封面图的获取姿势：从 [libretro-thumbnails](https://github.com/libretro-thumbnails/Nintendo_-_Game_Boy_Advance) 封面库按 `官方英文名 (区域).png` 抓盒图（jsDelivr 可直连：`https://cdn.jsdelivr.net/gh/libretro-thumbnails/Nintendo_-_Game_Boy_Advance@master/Named_Boxarts/<名字>.png`），再裁剪：方形居中裁出 icon、整图缩放 letterbox 到 256×128 做 banner。
- ⚠️ **Homebrew 安装当前会失败**：bannertool 预编译资源的 sha256 与上游重传文件不符（校验和报错）；且依赖安装可能触发本机 CLT 与 macOS 27 不匹配的报错。临时解法：手动安装——clone 源码 + Python 3.11 venv 装 Pillow + 下载 makerom/ctrtool（arm64）、3dstool（x86_64，走 Rosetta）、bannertool（vcoven release 里的 macOS arm64 包）放 `tools/` 目录。本仓库 `tools/3ds/vcoven/` 即是一套可用的手动安装实例（含从盒图自动裁剪 icon/banner 的脚本思路）。
- 生成图标/横幅需要自备 PNG（任何图片即可，自动裁剪缩放到 24×24、48×48 和 256×128）。

## 本仓库留档

| 位置 | 内容 |
| --- | --- |
| `tools/3ds/YANBF/` | YANBF-macOS.zip（v1.6.3）、bootstrap.cia、DS.Game.Forwarder.pack（SD-29，已解压）、生成器 app 及本次生成的 forwarder CIA（`sd-staging/cias/`） |
| `tools/3ds/TWiLightMenu/AddOn-VirtualConsoleFull-3DS.7z` | TWiLight VC 扩展包（官方 Release 原包） |
| `tools/3ds/vcoven/` | vcoven 手动安装实例：`src/`（v0.1.0 源码+tools）、`venv/`（Python 3.11 + Pillow）、生成的「节奏天国 GBA VC.cia」及占位图标/横幅素材 |

## 来源

- [YANBForwarder/YANBF](https://github.com/YANBForwarder/YANBF)（v1.6.3 已验证）
- [RocketRobz/NTR_Forwarder](https://github.com/RocketRobz/NTR_Forwarder)（SD-29 已验证）
- [vedoot/vcoven](https://github.com/vedoot/vcoven)（v0.1.0 已验证）
