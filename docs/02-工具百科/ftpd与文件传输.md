# ftpd 与文件传输（电脑 ↔ 3DS）

不拔卡、不用读卡器，通过 Wi-Fi 在电脑和 3DS 的 SD 卡之间传文件。本文覆盖：3DS 端 ftpd 服务器、macOS 访达 FTP 的只读坑、curl 命令行操作、PC 端 3DSort 镜像整理方案。

## ftpd（3DS 端 FTP 服务器）

一句话用途：3DS 上运行的 FTP 服务器，启动后把 SD 卡暴露给局域网内的 FTP 客户端，电脑端即可浏览、上传、下载、删除。

### 下载与安装

- 官方发布：[mtheall/ftpd Releases](https://github.com/mtheall/ftpd/releases)（已核实，最新 v3.2.1，验证可用）。
- ⚠️ **v3.2.0 起官方移除了 3DS 的 CIA 构建**（"DO NOT ASK ME TO SUPPORT CIA"）：新版只发 `ftpd.3dsx`，想要 `.cia` 需用 v3.1.0 及更早版本。推荐直接用 3dsx 版。
- 3dsx 版：`ftpd.3dsx` 放 SD 卡 `/3ds/ftpd/ftpd.3dsx`，从 Homebrew Launcher 启动。
- CIA 版：放 SD 卡里用 FBI 安装（本仓库留档的 ftpd.cia 即旧版构建）。
- 直连 GitHub 下载中断时可用 `gh release download --repo mtheall/ftpd --pattern 'ftpd.*'`。

### 用法

1. 电脑和 3DS 连**同一个 Wi-Fi**。
2. 在 3DS 上启动 ftpd，屏幕显示本机 IP 和端口（默认 **5000**），例如 `192.168.1.78:5000`。
3. 用户名密码留空（匿名）或填 `anonymous`；如服务端设了账号密码，客户端连接时对应填写。
4. 传完文件后**按 HOME 正常退出 ftpd**，让写入落盘，再重启/进 HOME 菜单验证。

### 注意事项 / 坑

- ⚠️ **macOS 访达的 FTP 连接是只读的**：访达（⌘K 连接 `ftp://IP:5000`）只能浏览和下载，不能上传、删除、重命名。要写入就用 FileZilla 或命令行 curl。
- ⚠️ **FTP 删除是永久删除**，不进回收站，删错无法找回。
- **430 Invalid user**：ftpd-3ds 是单客户端服务，上一次连接没正常释放时新连接会被拒（连匿名也报 430）。处理：在 3DS 上重启一次 ftpd。
- 传输中断就重试；3DSort 的 FTP 桥接脚本（见下文）每次传输都有大小校验，中断会报错而不是静默损坏。

## 命令行传输（curl）

上传文件（以主题包为例）：

```bash
curl -T "主题名.zip" ftp://192.168.1.78:5000/Themes/
```

上传到尚不存在的目录（`--ftp-create-dirs` 自动建目录，实测用于往 `/TYSS/` 放授权文件）：

```bash
curl --ftp-create-dirs -T drive.json ftp://192.168.1.78:5000/TYSS/drive.json
```

下载文件到当前目录：

```bash
curl -O "ftp://192.168.1.78:5000/Themes/主题名.zip"
```

删除文件：

```bash
curl --quote "DELE /Themes/主题名.zip" ftp://192.168.1.78:5000/
```

删除空目录：

```bash
curl --quote "RMD /Themes/目录名" ftp://192.168.1.78:5000/
```

## 3DSort：镜像 SD 卡 + FTP 桥接（不拔卡整理 HOME 菜单）

[3DSort](https://github.com/SalustLab/3DSort)（已核实）是电脑端工具：在本地建一个「镜像 SD 卡」，编辑 HOME 菜单图标布局（拖拽图标、文件夹、系统应用，带实机屏幕预览），再通过 3DS 上的 FTP 服务和 GodMode9 脚本把数据同步回真机。**全程不用拔卡**。`tools/` 目录备好了一整套可用配置：

| 位置 | 内容 |
| --- | --- |
| `tools/pc/3DSort-v1.1.0-macos-arm64.zip` | 3DSort macOS ARM64 安装包（v1.1.0） |
| `tools/pc/启动3DSort.command` | 启动器：以 `tools/pc/3dsort-mirror` 为 SD 镜像打开 3DSort |
| `tools/pc/启动3DSort-服务模式.command` | 服务模式启动（`--serve`，日志在 `/tmp/3dsort-serve.log`） |
| `tools/pc/3dsort-ftp-bridge.py` | FTP 桥接脚本：discover / pull / push 三个子命令 |
| `tools/pc/3dsort-mirror/` | 本地镜像 SD 卡（已含 dump 出的密钥与 HOME 菜单存档） |
| `tools/pc/3DSort-FTP-使用说明.md` | 完整使用说明（权威文档，以它为准） |

日常流程（详见 `tools/pc/3DSort-FTP-使用说明.md`）：

```
3DS 开 ftpd → pull（镜像拉取）→ 启动3DSort.command 编辑/WRITE → push →
3DS 关 ftpd → START 开机进 GodMode9 跑 3DSort_inject → 正常重启
```

只同步少量系统数据（`gm9/scripts/`、`/3DSort/` 密钥与 HOME 菜单存档、HOME 菜单 extdata），游戏本体和存档一概不动。

红线（3DSort 硬规则）：

- **PC 写完到跑完 inject 之间，不要启动 3DS 的 HOME 菜单**，否则 NAND 存档被系统改写、inject 会拒绝执行（安全门禁）。遇到就重跑一次 `3DSort_dump`，不丢数据。
- push 前确保 3DS 上 ftpd 已停止，没有其他程序在读写 SD 卡。
- push 是覆盖式同步不做删除：镜像里删掉的文件不会从 SD 上删，想删就去 3DS 上删。

## 本仓库留档

| 位置 | 内容 |
| --- | --- |
| `tools/3ds/ftpd/` | `ftpd.3dsx`（v3.2.1）、`ftpd.cia`（旧版构建；v3.2.0 起官方不再发布 CIA） |
| `tools/` 下 3DSort 相关 | 见上表 |

## 来源

- [mtheall/ftpd Releases](https://github.com/mtheall/ftpd/releases)（已核实，最新 v3.2.1；v3.2.0 移除 3DS CIA 构建）
- [SalustLab/3DSort](https://github.com/SalustLab/3DSort)（已核实仓库存在）
