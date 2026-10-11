# Universal-Updater

自制程序在线更新器：机内直连 Universal-DB 商店，搜索、下载、更新自制程序（含 CIA 直装和 3DSX），不用开电脑。装完 CFW 后第一批要装的工具之一。

## 下载与安装位置

- 官方下载：[Universal-Team/Universal-Updater Releases](https://github.com/Universal-Team/Universal-Updater/releases)（已核实）
- 安装：CIA 版用 FBI 安装；3DSX 版放 `/3ds/Universal-Updater/`
- 首次启动会下载 UniStore（应用商店索引），之后即可在线浏览
- 本仓库留档：`tools/3ds/Universal-Updater/`（`Universal-Updater.cia`）

## 常用操作

| 操作 | 说明 |
| --- | --- |
| 搜索/浏览 | 按名称或分类找应用；可按 GitHub stars / 更新时间排序 |
| 安装/更新 | 选中应用 → 加入队列 → 执行队列，CIA 直接装、3DSX 自动放 `/3ds/` |
| 已安装标记 | 自动识别已装应用（SD 卡图标），可筛选只看未装的 |
| 自身更新 | Universal-Updater 可更新自己；新破解的机子首启如遇崩溃，先用一次旧版再更新 |

## 注意事项 / 坑

- 需要联网；首次加载失败多是 Wi-Fi 还没连上，等图标变绿再操作，或重启应用。
- SSL 证书校验是开着的：系统日期偏差太大会导致所有下载失败，先用 Rosalina 菜单或系统设置把日期调对。
- CA 证书包从 SD 卡 `config/ssl/cacert.pem` 读取（Luma v13.1+ 自带），别删。
- 商店里的应用来自各作者自己的发布渠道，安装前认准官方条目；本文只收录上述官方 Releases 链接。
- 界面默认跟随系统语言：日版机启动就是日文。在应用内 **設定 → 言語** 可改成简体中文；找不到入口就电脑上改 SD 卡 `/3ds/Universal-Updater/Config.json` 里的 `"Language"`。注意 F君 简体补丁只换 HOME 菜单显示，系统上报的语言码仍是日文，所以装完补丁 UU 依旧日文。

## 本仓库留档

| 文件 | 说明 |
| --- | --- |
| `tools/3ds/Universal-Updater/Universal-Updater.cia` | 安装版 |

## 来源

- [Universal-Team/Universal-Updater Releases](https://github.com/Universal-Team/Universal-Updater/releases)（下载链接已核实）
- [Universal-DB](https://db.universal-team.net/)（商店索引，机内默认源）
- [备份与恢复](../01-系统折腾/10-备份与恢复.md)（联网与时间设置相关排查）
