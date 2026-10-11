# FBI

`.cia` 文件安装器：安装/卸载应用和游戏、批量安装、扫码（QR）安装，以及 tickets/titles/extdata 等系统数据管理。3DS 上装东西的入口工具。

## 下载与安装位置

- 官方下载：[Steveice10/FBI Releases](https://github.com/Steveice10/FBI/releases)（已核实，最新 2.6.1）
- 安装：优先用 CIA 版，装进系统后出现图标（用 GodMode9 的 `FBI.cia` 直装，或先放 `cias/` 目录装）；3DSX 版放 `/3ds/FBI/`
- 本仓库留档：`tools/3ds/FBI/`（`FBI.cia` + `FBI.3dsx`）

## 常用操作

### 安装 .cia（游戏或应用）

1. 把 `.cia` 文件放到 SD 卡 `/cias/` 目录（放别处也行）
2. 打开 FBI → `SD` → `cias`
3. 选中文件 → `Install and delete CIA`（装完自动删源文件省空间）或 `Install CIA`（保留源文件）

### 批量安装

进入目录后，列表第一行有伪条目 `<current directory>`（代表当前目录本身），在它上面按 A → `Install and delete all CIAs`（或 `Install all CIAs`），把当前目录下所有 `.cia` 排队装完。注意入口在伪条目上，对具体 `.cia` 文件按 A 只有单个安装选项。

### QR 安装

FBI 主界面按 R 或从菜单进 `Scan QR Code`，对准发布页提供的二维码直接下载安装（发布方常在 GitHub Release 附 QR，例如 Anemone3DS）。适合免电脑装自制程序。

### 远程安装（Remote Install）

FBI 的 `Remote install` 可与 PC 端工具（如 [Tulip3DS](README.md) 总表中的 Tulip3DS、3DS FBI Link）配合无线直传安装，机器 IP 会显示在屏幕上，PC 端按提示连接即可。

### Tickets / Titles / Ext Save Data 管理

- `Tickets`：查看/删除已导入的 ticket；ticket 缺失会导致已装游戏在 HOME 菜单不显示（用 [faketik](README.md) 重建）。
- `Titles`：按 NAND/SD/DSiWare 列出所有已安装标题，可导出/删除。
- `Ext Save Data`：管理额外数据（如游戏 DLC 存档）。

## 注意事项 / 坑

- 安装来源只限自己合法拥有的内容和自制软件；本文不提供游戏下载指引。
- 装大文件保持电量充足，中断安装可能留下残缺的 title，用 `Titles` 清理后重装。
- 装完游戏 HOME 菜单不出现 = ticket 缺失或 title.db 未更新，跑一遍 faketik 通常解决。
- ⚠️ 系统数据库（`title.db` / `import.db`）是配对的整体，跨机器拷数据库会 ARM9 崩溃，别手动替换，见 [坑与技巧](../01-系统折腾/14-坑与技巧.md)。

## 本仓库留档

| 文件 | 说明 |
| --- | --- |
| `tools/3ds/FBI/FBI.cia` | 安装器本体（CIA） |
| `tools/3ds/FBI/FBI.3dsx` | Homebrew Launcher 版 |

## 来源

- [Steveice10/FBI Releases](https://github.com/Steveice10/FBI/releases)（下载链接已核实）
- [备份与恢复](../01-系统折腾/10-备份与恢复.md)（SD 卡文件组织建议）
