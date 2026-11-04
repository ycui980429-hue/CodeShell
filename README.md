<div align="center">
  <img src="assets/icon.svg" width="88" alt="CodeShell" />
  <h1>CodeShell</h1>
  <p><b>一个会自己看服务器、自己动手的 AI 运维工作台。</b></p>
  <p>
    <img alt="platform" src="https://img.shields.io/badge/platform-macOS%20%7C%20Windows-3e8bff" />
    <img alt="electron" src="https://img.shields.io/badge/Electron-30-47848f" />
    <img alt="version" src="https://img.shields.io/badge/version-1.59.3-00b894" />
    <img alt="license" src="https://img.shields.io/badge/license-Proprietary-lightgrey" />
  </p>
</div>

不只是终端客户端 —— 它把 **AI 助手、智能巡检告警、自动化批量运维** 和终端 / SSH / SFTP / 数据库 / Docker 收在一个窗口里，
让日常运维从「逐个登录、逐条敲命令」变成「**描述目标、审阅执行**」。

> 所有连接与凭据只保存在本机（AES-256-CBC 加密落盘），不上传任何服务器。

---

## 快速看看

### AI 执行：描述目标 → 自主拆解 → 逐条审批

<p align="center">
  <img src="assets/screenshots/00-ai-run.png" alt="AI 运维助手正在执行任务" width="900" />
</p>

说一句「排查磁盘占用过高，清理可安全删除的文件」，
AI 自己规划出任务清单、跑 `df -h` 看分区、用 `du` 定位到具体目录，
再提出清理命令 —— **每条命令都要你点「允许执行」**，
命令与真实输出就显示在执行过程里。

<br>

### AI 结论：不是一堆输出，是一份可执行的小结

<p align="center">
  <img src="assets/screenshots/00-ai-done.png" alt="AI 给出的结构化结论" width="900" />
</p>

执行完给出**结论 + 判定依据 + 风险提示 + 后续建议**：
哪一步验证了什么、这条命令会不会误删、下一步该改哪个配置，一目了然。

<br>

### 子代理：把"输出几十屏、只要一个结论"的活外包出去

<p align="center">
  <img src="assets/screenshots/12-subagent-parallel.png" alt="三个子代理并行排查" width="900" />
</p>

大范围排查（多个目录、多个服务同时找线索）可以派给子代理：它们有**各自独立的上下文**，
中间几十屏输出不进主对话，只回传结论。多个子代理会**同时跑**，互不阻塞。

<p align="center">
  <img src="assets/screenshots/13-subagent-done.png" alt="子代理结论与被权限拦下的操作" width="900" />
</p>

子代理**不会替你决定**：「逐条确认」模式下它只能跑只读命令，
任何变更操作都会被拦下并**明确标在卡片上**（哪条命令、为什么被拦），
由你决定要不要在主任务里批准执行。

<br>

### 长任务不再靠 nohup 猜日志

编译、备份、`rsync` 大文件、全盘扫描这类活，提交后立刻返回任务 id，AI 可以继续往下走；
稍后读增量输出、看全貌、或中断。**命令仍然跑在你眼前那个真实终端里**，审批规则不变。
任务结束会主动通知 AI，不需要反复轮询。

<br>

### 接外部工具：Kubernetes / 云厂商 / Prometheus 都能用

<p align="center">
  <img src="assets/screenshots/15-mcp-client.png" alt="接入外部 MCP 服务器" width="900" />
</p>

以前 CodeShell 只能把自己的能力**暴露**给别人；现在也能**使用**别人的工具 ——
任何 MCP 服务器（K8s、云厂商、Prometheus、GitHub…）都能接进来，
工具直接出现在 AI 的工具列表里。这类工具做的是 CodeShell 以外的事，**每次调用都会请你确认**。

没有现成配置时给三条可点的示例（Kubernetes / GitHub / Prometheus），
点一下直接填入，不用手抄命令行：

<p align="center">
  <img src="assets/screenshots/15-mcp-empty.png" alt="常见 MCP 服务器示例" width="900" />
</p>

反过来，「对外提供服务」是另一个方向 —— 让 Claude Desktop、Cursor 等外部 agent
通过本机回环地址使用 CodeShell 的资产列表与只读执行能力：

<p align="center">
  <img src="assets/screenshots/16-mcp-server.png" alt="MCP 服务端设置" width="900" />
</p>

<br>

### 子代理可以调，也可以管

<p align="center">
  <img src="assets/screenshots/14-subagent-settings.png" alt="子代理设置" width="900" />
</p>

嵌套层数、同时进行数量、结论长度上限都能调，不需要改配置文件。
更重要的是把权限口径写在明面上：**自动执行**模式下可跑普通命令，**逐条确认**模式下只能只读 —— 子代理永远不会替你点"允许"。

<br>

### 安全基线检查：一次扫一批机器，出报告

<p align="center">
  <img src="assets/screenshots/09-security-scan.png" alt="安全基线检查列表" width="900" />
</p>

内置 **25 项只读检测**（SSH 加固 / 空口令 / 防火墙 / 口令策略 / SUID / 审计 / 补丁 / 磁盘…），
选一批机器并发扫描，按机器列出台账：名称、地址、状态、**进度**、风险计数、评分与等级。
列表可搜索、分页、编辑备注（标注环境/负责人）、移除条目，
勾选任意几台还能随时重扫。

<p align="center">
  <img src="assets/screenshots/10-security-detail.png" alt="单台机器的检查明细" width="900" />
</p>

点进任意一台看**逐项明细**：当前值是什么、建议怎么改，可按等级（高危/中危/提示/合规）筛选。
扫描全程只读，不修改目标系统；随时可取消，已完成的机器结果保留。

<p align="center">
  <img src="assets/screenshots/11-security-ai.png" alt="AI 解读扫描结果并给出整改方案" width="900" />
</p>

光有检查项还不够 —— 点「**AI 分析**」，把该机的问题清单交给模型，
它会给出风险判断、按「紧急 / 重要 / 可选」分档的整改步骤（附可直接执行的命令）、以及验证方式。
结论可一键复制，也能连同明细导出成 **PDF / Word** 报告，或导出 CSV 汇总。

<br>

### 其它能力

| | |
|---|---|
| <img src="assets/screenshots/01-overview.png" alt="CodeShell 主界面" /> | <img src="assets/screenshots/02-terminal.png" alt="终端与实时状态" /> |
| **一个窗口装下运维日常**<br>终端 / SSH / SFTP / 数据库 / Docker / AI | **终端 · 实时状态条**<br>打开即采样 CPU / 内存 / 负载 / 全部分区磁盘 |
| <img src="assets/screenshots/04-database.png" alt="数据库管理" /> | <img src="assets/screenshots/05-batch-ops.png" alt="批量运维" /> |
| **数据库**<br>10 种库 · 连上不自动打开库，自己选；支持搜索 | **批量运维**<br>广播 / 公钥下发 / 跨服直传 / 表同步 |
| <img src="assets/screenshots/06-cross-transfer.png" alt="跨服务器传输" /> | <img src="assets/screenshots/08-themes.png" alt="五套主题" /> |
| **跨服务器传输**<br>源左目标右，文件不经本机磁盘 | **五套外观**<br>深色 / 浅色 / 霓虹 / **暖色深色** / **柔和浅色**，另可整体缩放字号 |

---

## 三个核心能力

### 🤖 AI 运维助手 —— 真的能操作你的服务器

不是"聊天框旁边放个终端"，而是让 AI 自主规划并动手：

- **任务模式**：一句话描述目标，AI 自主规划步骤、逐条执行、读取输出后决定下一步
- **人在环上**：每条命令执行前都要你批准；危险命令在提示词层就被禁止
- **原生工具调用**：协议级的工具调用（不是"让模型输出 JSON 再解析"），工具 16 个：执行命令 / 跑 SQL / 后台任务 / 读写文件 / 派子代理 / 加载技能 / 接入外部 MCP 工具
- **长任务后台化**：编译、备份、扫盘提交即返回，不再靠 `nohup` + 轮询日志猜进度
- **子代理**：大范围排查外包出去，独立上下文、只回传结论，可并行、可中断、可看到它在做什么
- **主动提问**：遇到歧义会暂停问你（支持选项、多选、自定义输入），而不是自己瞎猜
- **上下文压缩**：长任务接近窗口上限时自动摘要旧历史、保留最近原文，不会中途撞墙
- **数据库助手**：连上库后直接用自然语言查询/改数据，读写分级（只读 SQL 直接执行，写操作需确认）
- **IM 接入**：微信（iLink 扫码绑定）/ 钉钉 / 飞书 / 企微 / QQ 里发消息就能查资产、看状态

### 📊 智能监控与告警 —— 不用盯，出事它先知道

- **实时状态条**：打开终端即采样 CPU / 内存 / 负载 / **全部分区磁盘**，数值跳动直观可见
- **自动巡检**：按间隔巡检所有 SSH 资产，超过阈值自动触发
- **AI 分析**：告警不只是报数，AI 会分析原因并给出处置建议
- **多通道推送**：钉钉机器人 / 邮件（SMTP）/ 阿里云短信
- **大文件排查**：按阈值只扫超标分区，**全程只读**（命中任何删除类命令立即中止）
- **告警归档**：超期记录打包成摘要发到已启用渠道，7 天保留 + 30 天硬清理

### ⚡ 自动化批量运维 —— 一手打到多台机器

- **广播命令**：同一批命令并行打到多台机器，逐机看输出与退出码
- **上传公钥**：ssh-copy-id 等价（自动建 `.ssh`、修权限、去重）
- **跨服务器传输**：文件从 A 直传 B，**不经过本机磁盘**（同机自动退化为服务端复制）
- **批量上传**：拖拽一批文件分发到多台服务器，目标目录可视化浏览选择
- **表数据/结构同步** · **数据库迁移**：同类型库直接搬 DDL；**跨类型（如 MySQL → 达梦）自动做字段类型映射**，无法映射时明确指出是哪一列
- **多机操作台**：分屏最多 **4 格** + **同步输入**（一次键入，四台机器同时执行）

---

## 安全是怎么做的

AI 能动手，就得有人管得住。这几条是硬约束，不是提示词里的"建议"：

| 约束 | 做法 |
|---|---|
| **变更必须经你批准** | 审批门统一在一处，工具自己不审批；破坏性命令与高危信息外泄即使「自动执行」模式也强制确认 |
| **子代理不能替你点"允许"** | 「逐条确认」模式下子代理只放行**只读命令**（按命令名 + 参数一起判定），任何变更交回主任务 |
| **危险命令识别看命令位** | `rm -rf`、`mkfs`、`dd of=/dev/sda` 等；解释器包装（`bash -c "…"`、`eval`）、命令替换（`$(…)`）、`find -delete`、`base64 -d \| bash` 都会递归判定，超过 3 层包装直接按危险处理 |
| **泄露自查** | 读私钥、凭据硬编码进命令行、`INTO OUTFILE`、打包 `~/.ssh` 外传等单独提示，与"破坏性"分开 |
| **循环守卫** | 同一命令/工具调用连续重复先提醒、10 次终止（防止模型打转烧 token） |
| **凭据只在本机** | AES-256-CBC 加密落盘，`~/Library/Application Support/CodeShell/`（Windows 为 `%APPDATA%\CodeShell\`），不上传任何服务器 |

---

## 完整功能

| 模块 | 能力 |
|---|---|
| **终端** | 本地 shell（Windows 用 PowerShell）· 分屏最多 4 格 · 同步输入 · 命令广播 |
| **SSH** | 密码/私钥认证 · 跳板隧道 · 断线自动重连 · 实时状态条 · **通过服务器开 SOCKS5 代理拉起浏览器**（访问内网后台） |
| **SFTP** | 上传/下载/新建目录/删除 · 拖拽上传（**支持整个文件夹**）· 列排序 · 进度与取消 · **跨服务器直传** |
| **数据库** | MySQL / MariaDB / PostgreSQL / 金仓 / Redis / SQLite / SQL Server / Oracle / **达梦** / ClickHouse · 连上不自动展开库（自己选，支持搜索库名与表名） |
| **SQL 编辑器** | Monaco · 全方言自动限行 · 结果分页 · 表格内增删改 · **导入 .sql 文件**（正确切分字符串/注释里的分号与 DELIMITER 存储过程）· 多格式导出 |
| **Docker** | 以标签页打开（可与其他面板并看）· 容器/镜像/卷/网络 · **拉取镜像（带进度）** · **自定义私有仓库** · **Compose 编辑与启停** · 容器内终端与文件管理 · 本地 socket / TCP / SSH 隧道三种连接 |
| **AI 助手** | 原生工具调用（16 个工具）· 逐条审批 · 任务清单 · 子代理并行 · 后台任务 · 本机文件读写 · 外部 MCP 工具 · 上下文压缩 · 数据库助手 |
| **资产源** | 从 **git 仓库或本地文件** 同步资产清单（团队共享配置），支持全部导入格式 |
| **资产导入导出** | CodeShell JSON / CSV / PuTTY .reg / MobaXterm ini / Xshell .xsh / SecureCRT / Tabby |
| **安全基线检查** | **25 项只读检测** · 多机并发扫描（逐机进度）· 按等级分级与筛选 · **AI 分析整改方案** · 导出 PDF / Word / CSV |
| **MCP** | **服务端**：把资产与只读执行能力暴露给外部 agent；**客户端**：接入 Kubernetes / 云厂商 / Prometheus 等外部工具 |
| **外观** | 深色 / 浅色 / **霓虹深色（分色显示）** / **暖色深色（护眼）** / **柔和浅色（纸感）** / 跟随系统 —— **终端配色随主题切换** |

## 快捷键

| 快捷键 | 功能 |
|---|---|
| `⌘ ⇧ F` / `⌘ F` | 全局搜索资产 |
| `⌘ R` | 刷新当前面板 |
| `⌘ E` | 重开最近关闭的标签 |
| `⌘ W` | 关闭当前标签 |
| `⌘ 0` | 显示 / 隐藏资产列表 |
| `⌘ 1` ~ `⌘ 9` | 切换到第 N 个标签 |
| `⌘ Home` | 回到首页 |
| `⌘ /` | 快捷键速查 |
| 终端内右键 | 复制 / 粘贴 / 全选 / 导出日志 / 清屏 / 文件传输 |
| `⌘ Enter` | 执行 SQL（编辑器内） |
| `⇧ ⌥ F` | 格式化 SQL（编辑器内） |

## 下载安装

从 **[Releases](../../releases)** 下载：

- **macOS（Apple Silicon）**：`CodeShell-<version>-arm64.dmg`
- **Windows（x64）**：`CodeShell-<version>-win-x64-setup.exe`（安装版）或 `-portable.exe`（免安装）

> ⚠️ 安装包未购买代码签名证书，首次运行系统会拦一下 —— 这是正常现象，不是文件损坏也不是病毒。
> 详细步骤见 **[安装说明.txt](./安装说明.txt)**。简单说：
> **Windows** 点「更多信息」→「仍要运行」；**macOS** 在「应用程序」里右键 CodeShell → 打开。
> 每个版本都在 Release 页公布 SHA-256 供校验。

## 技术栈

Electron 30 · React 18 · TypeScript · Zustand · xterm.js · Monaco · ssh2 · dockerode ·
node-pty · mysql2 / pg / mssql / oracledb / dmdb / ioredis / better-sqlite3 ·
MCP SDK（客户端 + 服务端）

## 关于本仓库

本仓库是 CodeShell 的**发布与文档页**：这里提供安装包下载、功能说明与更新日志。
**应用源码为闭源**，不在本仓库中。

- 安装包下载：[Releases](../../releases)
- 问题反馈与功能建议：[Issues](../../issues)（欢迎提需求）
- 更新日志：[CHANGELOG.md](./CHANGELOG.md)

## 许可

Copyright © 2026. 保留所有权利（All rights reserved）。

本软件为闭源商业软件，仅授权**个人免费使用**。未经书面许可，不得：
反编译、反汇编、逆向工程，修改、二次分发、转售或用于商业用途。

详见 [LICENSE](./LICENSE)。
