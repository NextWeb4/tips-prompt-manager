


<p align="center">
  <a href="README.md"><img src="https://img.shields.io/badge/English-0969da?style=flat-square" alt="English"></a>
  <a href="README.zh-CN.md"><img src="https://img.shields.io/badge/%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-c8102e?style=flat-square" alt="简体中文"></a>
  <a href="README.ja.md"><img src="https://img.shields.io/badge/%E6%97%A5%E6%9C%AC%E8%AA%9E-8250df?style=flat-square" alt="日本語"></a>
</p>

# TIPS 提示词管理器

一款 Windows 离线提示词资料库，使用本地 Tkinter 界面和 SQLite 存储来分类、搜索、编辑和复制提示词。

[![最近提交](https://img.shields.io/github/last-commit/NextWeb4/tips-prompt-manager?style=flat-square)](https://github.com/NextWeb4/tips-prompt-manager/commits/main)
[![仓库大小](https://img.shields.io/github/repo-size/NextWeb4/tips-prompt-manager?style=flat-square)](https://github.com/NextWeb4/tips-prompt-manager)
[![GitHub Stars](https://img.shields.io/github/stars/NextWeb4/tips-prompt-manager?style=flat-square)](https://github.com/NextWeb4/tips-prompt-manager)
![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)
[![MIT 许可证](https://img.shields.io/github/license/NextWeb4/tips-prompt-manager?style=flat-square)](LICENSE)

## 功能

- 在本地 SQLite 数据库中新建、编辑和删除提示词。
- 新增、重命名和删除自定义分类，并按分类筛选。
- 搜索提示词标题与正文，并高亮匹配文本。
- 一键复制当前提示词正文到剪贴板。
- 使用本地密码保护应用入口，同一次 Windows 开机期间通过后不重复验证。
- 切换中英文界面，并把语言偏好保存在本机。
- 无需浏览器、账号、服务端或网络连接。

## 环境要求与兼容性

- **操作系统：**Tkinter 桌面 UI、`.pyw`/批处理启动器、PowerShell 冒烟测试和 PyInstaller 发布包均面向 Windows。
- **Python：**`pyproject.toml` 要求 Python 3.10 或更高版本；`requirements.txt` 确认运行时代码仅使用 Python 标准库。
- **写入权限：**程序目录必须允许创建和更新 `prompts.db`、`config.json`、`.auth_session.json` 与 `settings.json`。
- **构建工具：**`requirements-dev.txt` 固定 PyInstaller 6.20.0，它只用于生成发布产物。
- **包格式：**仓库生成单文件 EXE 和 portable ZIP；当前没有 MSI 配置或 MSI 构建命令。

## 安装发布包

下载 `tips-prompt-manager-v1.0.0-windows-x64.exe` 后直接运行，或下载 `tips-prompt-manager-v1.0.0-windows-x64.zip`，解压后运行其中的 EXE。

EXE 未进行数字签名。运行前请使用同一 Release 中的 `SHA256SUMS.txt` 校验文件。

## 从源码运行

项目声明需要 Python 3.10 或更高版本，运行时仅使用 Python 标准库。在仓库根目录执行：

```powershell
python PromptManager.pyw
```

如需无控制台窗口启动，可使用 `pythonw.exe PromptManager.pyw`，或双击 `启动提示词管理器.bat`。批处理启动器使用仓库自身目录，不依赖开发机固定路径。

## 使用方法

1. 首次启动时设置本地密码。
2. 添加用于整理提示词的分类。
3. 点击“新建”，填写标题、分类和提示词正文。
4. 点击“保存”，将内容写入本地 SQLite。
5. 按分类筛选，或搜索标题和正文。
6. 点击“复制”，把当前正文放入剪贴板。

应用实现的快捷键包括：`Ctrl+N` 新建、`Ctrl+S` 保存、`Ctrl+F` 聚焦搜索，以及 `Delete` 删除选中提示词。

## 本地数据与安全

应用会在程序旁创建以下个人数据文件：

| 文件 | 用途 |
| --- | --- |
| `prompts.db` | SQLite 中的提示词和分类数据 |
| `config.json` | 密码盐和密码哈希，不保存明文密码 |
| `.auth_session.json` | 本次开机的认证状态 |
| `settings.json` | 界面语言偏好 |

这些路径已被 `.gitignore` 排除，不得提交或打入发布包。密码只保护应用入口，**不会**加密 `prompts.db`。能够访问文件的人可能直接查看数据库，因此应保护存储目录，也不要把真实提示词数据用于测试 fixture 或截图。

应用保持离线运行，不包含云账号、同步、遥测或自动更新。

## 项目结构

| 路径 | 职责 |
| --- | --- |
| `PromptManager.pyw` | 源码启动器 |
| `src/tips_prompt_manager/app.py` | Tkinter 界面与用户交互 |
| `src/tips_prompt_manager/auth.py` | 密码哈希与开机周期认证 |
| `src/tips_prompt_manager/storage.py` | SQLite schema 和提示词/分类操作 |
| `src/tips_prompt_manager/i18n.py` | 中英文界面文案 |
| `src/tips_prompt_manager/meta.py` | 应用元数据常量 |
| `src/tips_prompt_manager/paths.py` | 本地数据路径 |
| `tests/` | 认证、本地化、存储和 GUI 冒烟检查 |
| `scripts/build.ps1` | 测试、编译、PyInstaller、ZIP 与校验流水线 |

## 测试

```powershell
$env:PYTHONPATH = "src"
python -m unittest discover -s tests -v
```

GUI/发布包冒烟脚本：

```powershell
.\tests\run_ui_smoke.ps1
```

在 CI 或无交互桌面环境中使用 `-SkipLaunch`。

## 构建

```powershell
powershell -ExecutionPolicy Bypass -File scripts/build.ps1
```

构建脚本会运行测试和 `compileall`，然后使用 PyInstaller 6.20.0 生成 Windows x64 单文件 EXE、包含文档和声明的便携 ZIP，以及 SHA-256 校验值。仓库当前没有 MSI 配置。

## 项目状态与限制

- 这是一个活跃的公开 Windows 应用，已记录 v1.0.0 发布流程。
- 应用界面目前支持中文和英文；README 提供三种语言并不表示应用已有日文 UI。
- 密码验证只控制从应用入口访问，不会加密 SQLite 中的提示词内容。
- 已记录的设计不包含云同步、遥测或自动更新，审计后的目录树中也没有自动备份命令。
- 公开的可执行文件未签名；校验和能发现文件变化，但不能建立受信任发布者身份。
- Tkinter 布局、剪贴板行为和打包后启动需要在交互式 Windows 桌面中才能完整验证。

## 参与贡献

请将 UI 行为保留在 `src/tips_prompt_manager/app.py`，认证/会话逻辑保留在 `auth.py`，SQLite 操作保留在 `storage.py`，本地化文案保留在 `i18n.py`，文件位置决策保留在 `paths.py`。测试和截图只使用合成提示词，中英文 UI 文案必须同步更新，并运行与改动相关的单元测试和 GUI/发布包冒烟检查。新增运行时依赖前必须明确审计兼容性、许可证、体积、维护状态和离线影响；项目当前没有 lint 或 format 命令。

## 作者

- HaoXiang Huang
- [Rays688888@Gmail.com](mailto:Rays688888@Gmail.com)
- [项目主页](https://nextweb4.github.io/)

## 许可证

项目采用 [MIT License](LICENSE)。运行时模块均来自 Python 标准库，PyInstaller 仅用于构建。详见 [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md)。
