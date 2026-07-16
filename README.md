[English](README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

# TIPS Prompt Manager

An offline Windows prompt library for organizing, searching, editing, and copying prompts with local Tkinter and SQLite storage.

[![Last commit](https://img.shields.io/github/last-commit/NextWeb4/tips-prompt-manager?style=flat-square)](https://github.com/NextWeb4/tips-prompt-manager/commits/main)
[![Repository size](https://img.shields.io/github/repo-size/NextWeb4/tips-prompt-manager?style=flat-square)](https://github.com/NextWeb4/tips-prompt-manager)
[![GitHub stars](https://img.shields.io/github/stars/NextWeb4/tips-prompt-manager?style=flat-square)](https://github.com/NextWeb4/tips-prompt-manager)
![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)
[![MIT License](https://img.shields.io/github/license/NextWeb4/tips-prompt-manager?style=flat-square)](LICENSE)

## Features

- Creates, edits, and deletes prompts stored in a local SQLite database.
- Adds, renames, and deletes custom categories, with category-based filtering.
- Searches prompt titles and bodies and highlights matching text.
- Copies the current prompt body to the clipboard.
- Protects application entry with a local password and skips repeated verification during the same Windows boot session.
- Switches between Chinese and English and saves the preference locally.
- Runs without a browser, user account, server, or network connection.

## Install a Release

Download `tips-prompt-manager-v1.0.0-windows-x64.exe` and run it directly, or download `tips-prompt-manager-v1.0.0-windows-x64.zip`, extract it, and run the EXE inside.

The EXE is not digitally signed. Verify it against `SHA256SUMS.txt` from the same release before running it.

## Run From Source

The project declares Python 3.10 or newer and uses only the Python standard library at runtime. From the repository root, run:

```powershell
python PromptManager.pyw
```

For a windowless launch, use `pythonw.exe PromptManager.pyw` or double-click `启动提示词管理器.bat`. The batch launcher uses the repository directory rather than a hard-coded machine path.

## Use the App

1. Set a local password on first launch.
2. Add categories for the kinds of prompts you maintain.
3. Choose **New**, then enter a title, category, and prompt body.
4. Choose **Save** to write the prompt to local SQLite storage.
5. Filter by category or search the title and body.
6. Choose **Copy** to put the current prompt body on the clipboard.

Keyboard shortcuts implemented by the application include `Ctrl+N` for a new prompt, `Ctrl+S` to save, `Ctrl+F` to focus search, and `Delete` to remove the selected prompt.

## Local Data and Security

The application creates these personal files next to the program:

| File | Purpose |
| --- | --- |
| `prompts.db` | Prompt and category data in SQLite |
| `config.json` | Password salt and password hash; no plaintext password |
| `.auth_session.json` | Authentication state for the current boot session |
| `settings.json` | Interface-language preference |

These paths are excluded by `.gitignore` and must not be committed or packaged. The password protects the application entry point; it does **not** encrypt `prompts.db`. Anyone with file access may be able to inspect the database, so protect the storage directory and do not use production prompt data as test fixtures or screenshots.

The application is intentionally offline: it has no cloud account, synchronization, telemetry, or automatic-update behavior.

## Project Structure

| Path | Responsibility |
| --- | --- |
| `PromptManager.pyw` | Source launcher |
| `src/tips_prompt_manager/app.py` | Tkinter interface and user interaction |
| `src/tips_prompt_manager/auth.py` | Password hashing and boot-session authentication |
| `src/tips_prompt_manager/storage.py` | SQLite schema and prompt/category operations |
| `src/tips_prompt_manager/i18n.py` | Chinese and English interface strings |
| `src/tips_prompt_manager/paths.py` | Local data paths |
| `tests/` | Authentication, localization, storage, and GUI smoke checks |
| `scripts/build.ps1` | Test, compile, PyInstaller, ZIP, and checksum pipeline |

## Test

```powershell
$env:PYTHONPATH = "src"
python -m unittest discover -s tests -v
```

The GUI/package smoke script is:

```powershell
.\tests\run_ui_smoke.ps1
```

Use `-SkipLaunch` in a CI or non-interactive desktop session.

## Build

```powershell
powershell -ExecutionPolicy Bypass -File scripts/build.ps1
```

The build script runs tests and `compileall`, then uses PyInstaller 6.20.0 to create a Windows x64 single-file EXE, a portable ZIP containing the documentation and notices, and SHA-256 checksums. The repository has no MSI configuration.

## Author

- HaoXiang Huang
- [didadida1688@gmail.com](mailto:didadida1688@gmail.com)
- [Project homepage](https://nextweb4.github.io/)

## License

Released under the [MIT License](LICENSE). Runtime modules come from the Python standard library; PyInstaller is a build-only dependency. See [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md).
