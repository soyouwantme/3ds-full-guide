# GodMode9

万能文件管理器（ARM9 payload）：浏览和拷贝 SD/NAND/卡带上的文件、做 NAND 备份与恢复、解密加密文件、运行 GM9/Lua 脚本。3DS 折腾的瑞士军刀，相当于"3DS 上的 PE 系统 + 磁盘管理器"。

## 下载与安装位置

- 官方下载：[d0k3/GodMode9 Releases](https://github.com/d0k3/GodMode9/releases)（已核实，最新 v2.2.3）
- 安装：`GodMode9.firm` 放到 `luma/payloads/`；发布包里的整个 `gm9/` 文件夹（脚本 + 语言包，含简体中文）拷到 SD 卡根目录
- 进入方式：关机后按住 **Start** 开机（Luma v13 先显示配置页时，选 **Boot chainloader** → `GodMode9.firm`）
- 本仓库留档：`tools/3ds/GodMode9-v2.2.3/`（`GodMode9.firm` + 完整 `gm9/` + `elf/`）；自写脚本留档 `tools/3ds/gm9-scripts/`（**有坑，勿直接复用**）

## 界面基础

| 按键 | 作用 |
| --- | --- |
| 方向键 + A | 浏览/确认；选中文件按 A 弹出操作菜单 |
| B | 返回/取消 |
| X / Y | 剪切/复制；到目标目录按 A → `Paste` |
| R+A | 文件上显示附加操作（如解密、解密并生成 CIA） |
| HOME | 主菜单（Title manager、Scripts、More...） |

盘符：`0:/` SD 卡，`1:/` SysNAND，`A:/` NAND 里 SD 安装的游戏（title.db 视图），`G:/` 卡带。

## NAND 备份（强烈建议，折腾前第一步）

1. START+开机进 GodMode9 → **HOME** → `Scripts...` → `GM9Megascript` → `Backup Options` → `SysNAND Backup`
2. 完成后按提示把备份留在 `gm9/out/`（约 1.21GB，带 SHA 校验文件）
3. 拷到电脑上单独保存

相关产物：

- `*.bin` + `*.sha`：整盘 sysNAND 备份，还原的兜底
- `essential.exefs`：**essential 文件**（含 `movable.sed`、`boot9.bin` 等密钥），解密该机器数据必需的敏感文件

⚠️ `essential.exefs`、NAND 备份、`boot9.bin`、`movable.sed` 均可用于解密这台机器的 NAND，不要上传网盘/GitHub/聊天群，单独加密备份（见 [备份与恢复](../01-系统折腾/10-备份与恢复.md)）。

## NAND 恢复

1. START+开机进 GodMode9（Luma v13：配置页选 Boot chainloader → GodMode9）
2. **HOME** → `Scripts...` → `GM9Megascript` → `Restore Options` → `SysNAND Restore (safe)`
3. 选择 `gm9/out/` 里的备份 `.bin`，按提示解锁写入，保持充电等待完成（约 20-40 分钟）

⚠️ `Restore (safe)` 还原成功后会**删除 SD 卡上的源备份 .bin**（只留电脑存档那份，属正常）。想留 SD 副本先手动复制走。

## 文件浏览 / 拷贝 / 解密

- SD ↔ 电脑交换文件：直接当文件管理器用（`0:/`），拷出 `movable.sed`、`boot9.bin` 等敏感文件就是这么做的。
- 解密游戏/系统文件：选中 NCCH/NCSD/CIA → R+A → `Decrypt file (...)`。GodMode9 只导出 `.3ds`（即 CCI），给 Azahar 等模拟器用时需先解密并改扩展名 `.cci`。
- 卡带转储：插卡带，浏览 `G:/` 可 dump 卡带镜像和存档。
- Title manager：HOME → `Title manager`，查看已装 titles、批量卸载。

## GM9 脚本

- 脚本放 `gm9/scripts/`，进 GM9 后 **HOME → Scripts...** 运行；官方 `GM9Megascript` 覆盖备份/还原/格式化等常用流程。
- 实例：简体中文系统安装器的 `Chinese_System_Installer.gm9`（配合 `luma/payloads/Chinese_System_Installer.firm`），可一次装成简体中文系统，见 [简体中文系统](../01-系统折腾/11-简体中文系统.md)。
- v2.2.0 起支持 Lua 脚本（GM9Script 已停止开发），文档在发布包的 `lua-doc.md`。

⚠️ **自写 GM9Script 的两个行为特性**（容易踩坑，详见 [坑与技巧](../01-系统折腾/14-坑与技巧.md)）：

1. `cp <目录> <目标>`：目标**不能已存在**，否则报"用目录覆盖文件"；想合并只能界面手动 Paste 或先删目标。
2. 脚本某行报错后按 A"继续"**会继续执行后续行**（不是终止），配合 `rm` + `cp -s` 可能造出"删了旧的、新的没拷进去"的半成品。

结论：NAND 写操作别用自写脚本，用官方安装器或界面手动操作。

## 格式化 SD 卡

GM9Megascript → `SD format menu` 可把 SD 卡格式化为 3DS 可用格式（FAT32 + 合适簇大小）。也可以先在电脑上格式化为 FAT32（簇 32KB）再拷回引导文件，见 [备份与恢复](../01-系统折腾/10-备份与恢复.md)。

## 注意事项 / 坑

- 版本升级（如 1.4.x → v2.2.3）只需替换 `GodMode9.firm`；建议同时更新 `gm9/`（语言包、脚本）。旧版 firm 改名加 `.bak` 留备份即可。
- 首次进入若提示时间不对属正常（备份时间戳 2001 年是已知现象）。
- NAND 写操作前务必有可用备份；写操作需要按键组合解锁，别连按过确认。

## 来源

- [d0k3/GodMode9 Releases](https://github.com/d0k3/GodMode9/releases)（下载链接已核实）
- 发布包内置 `README.md`（本仓库 `tools/3ds/GodMode9-v2.2.3/README.md` 有留档）
- [简体中文系统](../01-系统折腾/11-简体中文系统.md)、[坑与技巧](../01-系统折腾/14-坑与技巧.md)（GM9Script 两个行为特性的详细说明）
