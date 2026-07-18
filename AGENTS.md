# AGENTS.md

## 1. Project structure
- `PromptManager.pyw` and `启动提示词管理器.bat` are launchers; maintainable application code is under `src/tips_prompt_manager/`.
- Keep Tkinter UI in `app.py`, password/session logic in `auth.py`, SQLite operations in `storage.py`, localized strings in `i18n.py`, and data-path decisions in `paths.py`.
- Tests live in `tests/`; `scripts/build.ps1` owns release packaging. Personal runtime files are not source files.

## 2. Run commands
- Run with `python PromptManager.pyw` from the repository root.
- Use `pythonw.exe PromptManager.pyw` or `启动提示词管理器.bat` for a windowless launch.
- The application must remain usable without a browser, server, account, or network connection.

## 3. Test commands
- Set `$env:PYTHONPATH = "src"`, then run `python -m unittest discover -s tests -v`.
- Run package/GUI smoke checks with `.\tests\run_ui_smoke.ps1`; use `-SkipLaunch` in CI or a non-interactive session.
- Run `python -m compileall -q src scripts tests` as a syntax check when build packaging is not being exercised.

## 4. Build commands
- Build the release with `powershell -ExecutionPolicy Bypass -File scripts/build.ps1`.
- The script must run tests and `compileall` before creating the single-file EXE, portable ZIP, and `SHA256SUMS.txt`.
- No MSI build command or installer configuration was found; do not claim MSI output.

## 5. Code style
- Keep the centered Shields language selector in all three root READMEs with the exact visible labels `English`, `简体中文`, and `日本語`, linked in that order to `README.md`, `README.zh-CN.md`, and `README.ja.md`; do not replace the SVG labels with browser-translatable text.
- Keep the three README versions aligned in section order, facts, commands, paths, links, images, numbers, and code fences; translate headings and prose naturally while preserving identifiers.
- Use UTF-8, four-space Python indentation, type hints, and small functions consistent with existing modules.
- Keep Chinese and English strings centralized in `src/tips_prompt_manager/i18n.py`.
- Runtime code currently uses only the standard library; audit compatibility, license, size, maintenance, and offline impact before adding a dependency.
- No lint/format command was found in the current repository; add one before claiming lint or formatter coverage.

## 6. Module boundaries
- `auth.py` owns password hashing, configuration, and boot-session checks; dialogs only call its API.
- `storage.py` owns schema and category/prompt CRUD; UI code must not embed business SQL.
- `paths.py` owns runtime file locations. Build scripts must never read, modify, or package a user's database or authentication files.

## 7. Prohibited changes
- Do not commit or package `prompts.db`, `config.json`, `.auth_session.json`, `settings.json`, personal prompts, credentials, caches, logs, or local shortcuts.
- Do not describe password-gated startup as database encryption; `prompts.db` remains readable to a user with file access.
- Do not add cloud sync, cloud accounts, telemetry, network updates, or other background networking.
- Do not describe unsigned binaries as signed; retain checksum verification guidance.

## 8. Completion criteria
- Run `python -m unittest discover -s tests -v` with `PYTHONPATH=src` and report the actual result.
- Authentication, session, schema, migration, category, or prompt CRUD changes include focused regression tests.
- Packaging changes pass `scripts/build.ps1`, contain README/LICENSE/third-party notices, and exclude all personal runtime data.
- GUI changes receive an interactive smoke check where a desktop is available, or an explicit limitation report otherwise.

## 9. Review criteria
- Verify the language selector renders through GitHub without browser-translatable text and all three README versions keep the same facts, commands, links, and images.
- Review the offline boundary, ignored runtime data, password hash/session expiry, SQLite migration/CRUD behavior, search highlighting, and clipboard handling.
- Confirm UI strings remain complete in both supported languages.
- Inspect release metadata, unsigned status, ZIP contents, and SHA-256 output after build changes.
- Reject tests or screenshots made from real user prompts.

## 10. Common risks
- `prompts.db` contains prompt text and can leak sensitive material through fixtures, screenshots, logs, or release packaging.
- `config.json` and `.auth_session.json` contain authentication state and must remain local even though no plaintext password is stored.
- Headless environments cannot fully validate Tkinter layout and clipboard behavior.
- Unsigned PyInstaller single-file binaries may trigger Windows security warnings; users need checksums and clear provenance.
