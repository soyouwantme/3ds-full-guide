# TYSS（天涯的3DS存档工作室）

游戏存档备份/恢复工具，JKSM 的现代化继承者（作者 R-YaTian，国内开发者）。按游戏列出全部存档（含卡带、eShop 版），一键备份到 SD 卡或从 SD 卡恢复。换机、重装机、刷系统前的存档保险。**本仓库用它替代已停更的 Checkpoint。**

相比 Checkpoint 的增量功能：

- 原生简体中文界面（多语言 UI，可配置游戏标题语言）
- 云端存档上传/下载（阿里云盘或自建 Google Drive）
- 支持备份/恢复 DSiWare 存档、GBA VC 存档（raw 或 .sav 格式）、追加数据与系统存档
- 内置金手指数据库（英文和简中），可直接安装金手指文件
- 存档可压缩为 zip；浅色/深色双主题
- 存档工作室工具箱：游玩时间/步数历史记录管理、缓存图标清理、试玩次数重置等（老三需用 Mode3 版本）

## 下载与安装位置

- 官方下载：[R-YaTian/TYSS Releases](https://github.com/R-YaTian/TYSS/releases)（已核实，最新 v1.1.2）
- 安装：CIA 版用 FBI 安装；3DSX 版放 `/3ds/TYSS/`；老 3DS 机型可装 `TYSS_mode3.cia`
- 本仓库留档：`tools/3ds/TYSS/`（`TYSS.cia` + `TYSS.3dsx`）
- 从 JKSM 迁移：把 SD 卡上的 `JKSV` 文件夹改名为 `TYSS` 即可沿用旧备份

## 常用操作

### 备份存档

1. 打开 TYSS，等它扫描完所有游戏（自动过滤无存档数据的自制软件）
2. 标题列表界面：A 打开选中的游戏，Y 收藏，X 打开选项菜单
3. 在备份项目界面按 A 新建备份（或覆盖已有备份）
4. 备份存到 SD 卡 `/TYSS/` 目录下

### 恢复存档

1. 选中游戏 → 在备份项目界面按 **Y** 恢复高亮的备份
2. ⚠️ 恢复会覆盖当前存档，确认最近进度已不需要再操作

### 其他操作（备份项目界面）

- X：删除高亮的备份
- R：上传高亮的 zip/sav/bin 文件到云端
- B：返回标题选择

### 云端备份（可选）

- 云端上传只支持「导出到 ZIP」选项创建的备份
- 阿里云盘：从发布页下载 PC 端 `adrivehelper`（本仓库已留档 macOS 版：`tools/pc/adrivehelper/`），运行后它会打开浏览器引导登录授权
- 自建 Google Drive：按仓库 [GD_INSTRUCTIONS.MD](https://github.com/R-YaTian/TYSS/blob/master/GD_INSTRUCTIONS.MD) 操作（英文）

#### 阿里云盘授权实操（已验证，两台机器均完成）

1. Mac 上运行 `tools/pc/adrivehelper/adrivehelper`（无命令行输出、直接挂起是正常行为，它在起本地授权服务），浏览器自动打开阿里云盘登录页，登录授权后页面提示"授权码已保存到 drive.json"
2. 生成的 `drive.json`（约 55 字节的授权令牌）在 adrivehelper 同目录，**10 分钟内**要放进 SD 卡根目录的 `TYSS/` 文件夹并启动 TYSS 完成客户端授权
3. 推荐走 ftpd 无线传输（3DS 开 ftpd 后，见 [ftpd与文件传输](ftpd与文件传输.md)）：

   ```bash
   curl --ftp-create-dirs -T tools/pc/adrivehelper/drive.json ftp://<3DS的IP>:5000/TYSS/drive.json
   ```

   `--ftp-create-dirs` 会自动建好 `TYSS/` 目录。传完在 3DS 上按 HOME 正常退出 ftpd，立刻启动 TYSS
4. 授权失败/超时：重跑 adrivehelper 生成新 `drive.json` 再传一次即可，几秒钟的事

坑：

- ⚠️ **重跑 adrivehelper 报 `Address already in use`（10304 端口）**：上一次的进程还挂着，先 `lsof -ti :10304 | xargs kill` 再重启
- ⚠️ `drive.json` 是阿里云盘授权令牌，等同账号凭证：不要提交 git（本仓库 `tools/` 已 gitignore）、不要外传
- 多台机器可以共用同一次授权生成的 `drive.json`（两台机器各放一份、各自启动 TYSS 即可）

## 备份结构

```
/TYSS/
└── <游戏>/<备份名>/   # 存档文件，数量与格式随游戏而异，整目录拷走即可
```

- 备份就是普通文件，可直接用 ftpd 拷到电脑长期保存。
- 建议命名带日期和事由（如 `2026-01-01_系统更新前`），关键操作前各备一份。

## 注意事项 / 坑

- ⚠️ **备份和恢复必须在同一游戏版本下进行**：游戏更新（尤其大版本）可能改变存档格式，旧备份恢复后可能损坏或不识别。养成"更新前先备份"的习惯。
- 卡带游戏和数字版是不同 title，存档不通用；恢复时认准来源。
- 恢复前不要运行目标游戏，避免冲突。
- 老 3DS（非 New）用工具箱的缓存图标清理等功能时，需要 `TYSS_mode3.cia` 版本。

## 本仓库留档

| 文件 | 说明 |
| --- | --- |
| `tools/3ds/TYSS/TYSS.cia` | 安装版 |
| `tools/3ds/TYSS/TYSS.3dsx` | Homebrew Launcher 版 |
| `tools/pc/adrivehelper/` | 阿里云盘授权助手（macOS 通用二进制，`tools/pc/adrivehelper-macos-universal2.zip` 为原包） |

## 来源

- [R-YaTian/TYSS Releases](https://github.com/R-YaTian/TYSS/releases)（下载链接已核实）
- [TYSS 中文 README](https://github.com/R-YaTian/TYSS/blob/master/README_zh.MD)
- [备份与恢复](../01-系统折腾/10-备份与恢复.md)（存档备份策略）
