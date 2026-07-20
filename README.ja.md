<p align="center">
  <a href="README.md"><img src="https://img.shields.io/badge/English-0969da?style=flat-square" alt="English"></a>
  <a href="README.zh-CN.md"><img src="https://img.shields.io/badge/%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-c8102e?style=flat-square" alt="简体中文"></a>
  <a href="README.ja.md"><img src="https://img.shields.io/badge/%E6%97%A5%E6%9C%AC%E8%AA%9E-8250df?style=flat-square" alt="日本語"></a>
</p>

# TIPS Prompt Manager

Tkinter と SQLite を使ってプロンプトを分類、検索、編集、コピーする Windows 向けオフラインライブラリです。

[![最終コミット](https://img.shields.io/github/last-commit/NextWeb4/tips-prompt-manager?style=flat-square)](https://github.com/NextWeb4/tips-prompt-manager/commits/main)
[![リポジトリサイズ](https://img.shields.io/github/repo-size/NextWeb4/tips-prompt-manager?style=flat-square)](https://github.com/NextWeb4/tips-prompt-manager)
[![GitHub Stars](https://img.shields.io/github/stars/NextWeb4/tips-prompt-manager?style=flat-square)](https://github.com/NextWeb4/tips-prompt-manager)
![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)
[![MIT ライセンス](https://img.shields.io/github/license/NextWeb4/tips-prompt-manager?style=flat-square)](LICENSE)

## 機能

- ローカル SQLite データベースでプロンプトを作成、編集、削除します。
- 任意カテゴリの追加、名前変更、削除とカテゴリ絞り込みに対応します。
- タイトルと本文を検索し、一致したテキストを強調表示します。
- 現在のプロンプト本文をクリップボードへコピーします。
- ローカルパスワードでアプリ入口を保護し、同じ Windows 起動セッションでは再認証を省略します。
- 中国語/英語 UI を切り替え、設定をローカルに保存します。
- ブラウザー、アカウント、サーバー、ネットワーク接続を必要としません。

## 動作要件と互換性

- **OS:** Tkinter デスクトップ UI、`.pyw`/バッチランチャー、PowerShell スモークテスト、PyInstaller リリースは Windows 向けです。
- **Python:** `pyproject.toml` は Python 3.10 以上を要求します。`requirements.txt` から、実行時コードが Python 標準ライブラリだけを使用することを確認できます。
- **書き込み権限:** プログラムのディレクトリに `prompts.db`、`config.json`、`.auth_session.json`、`settings.json` を作成、更新できる必要があります。
- **ビルドツール:** `requirements-dev.txt` は PyInstaller 6.20.0 を固定しており、リリース成果物の作成時だけ必要です。
- **パッケージ形式:** 単一ファイル EXE とポータブル ZIP を生成します。MSI 設定と MSI ビルドコマンドはありません。

## リリース版のインストール

`tips-prompt-manager-v1.0.0-windows-x64.exe` を直接実行するか、`tips-prompt-manager-v1.0.0-windows-x64.zip` を展開して中の EXE を実行します。

EXE はデジタル署名されていません。同じ GitHub Release の `SHA256SUMS.txt` と照合してから実行してください。

## ソースから実行

Python 3.10 以上を対象とし、実行時は Python 標準ライブラリだけを使います。リポジトリのルートで実行します。

```powershell
python PromptManager.pyw
```

コンソールを表示しない場合は `pythonw.exe PromptManager.pyw` または `启动提示词管理器.bat` を使います。バッチランチャーは固定された開発者パスではなく、配置されたディレクトリを使います。

## 使い方

1. 初回起動時にローカルパスワードを設定します。
2. プロンプト整理用のカテゴリを追加します。
3. **New** を選び、タイトル、カテゴリ、本文を入力します。
4. **Save** でローカル SQLite に保存します。
5. カテゴリまたはタイトル/本文の検索で絞り込みます。
6. **Copy** で現在の本文をクリップボードへ入れます。

実装済みショートカットは、`Ctrl+N`（新規）、`Ctrl+S`（保存）、`Ctrl+F`（検索へ移動）、`Delete`（選択項目の削除）です。

## ローカルデータとセキュリティ

プログラムの隣に次の個人データが作られます。

| ファイル | 用途 |
| --- | --- |
| `prompts.db` | SQLite のプロンプトとカテゴリ |
| `config.json` | パスワードのソルトとハッシュ。平文は保存しません |
| `.auth_session.json` | 現在の起動セッションの認証状態 |
| `settings.json` | UI 言語設定 |

これらは `.gitignore` の対象で、コミットや配布物に含めてはいけません。パスワードはアプリ入口を保護しますが、`prompts.db` 自体を**暗号化しません**。ファイルへアクセスできる人が内容を読める可能性があるため、保存先を保護し、実データをテストフィクスチャやスクリーンショットに使わないでください。

アプリは意図的にオフラインで、クラウドアカウント、同期、テレメトリー、自動更新を含みません。

## プロジェクト構成

| パス | 役割 |
| --- | --- |
| `PromptManager.pyw` | ソースランチャー |
| `src/tips_prompt_manager/app.py` | Tkinter UI とユーザー操作 |
| `src/tips_prompt_manager/auth.py` | パスワードハッシュと起動セッション認証 |
| `src/tips_prompt_manager/storage.py` | SQLite スキーマとプロンプト/カテゴリ操作 |
| `src/tips_prompt_manager/i18n.py` | 中国語/英語 UI 文言 |
| `src/tips_prompt_manager/meta.py` | アプリケーションのメタデータ定数 |
| `src/tips_prompt_manager/paths.py` | ローカルデータパス |
| `tests/` | 認証、ローカライズ、ストレージ、GUI スモークテスト |
| `scripts/build.ps1` | テスト、コンパイル、PyInstaller、ZIP、チェックサム |

## テスト

```powershell
$env:PYTHONPATH = "src"
python -m unittest discover -s tests -v
```

GUI/パッケージのスモークテスト:

```powershell
.\tests\run_ui_smoke.ps1
```

CI や対話デスクトップのない環境では `-SkipLaunch` を指定します。

## ビルド

```powershell
powershell -ExecutionPolicy Bypass -File scripts/build.ps1
```

スクリプトはテストと `compileall` を実行し、PyInstaller 6.20.0 で Windows x64 単一 EXE、文書と通知を含むポータブル ZIP、SHA-256 チェックサムを作ります。MSI 設定はありません。

## プロジェクトの状態と制限

- v1.0.0 のリリース手順が文書化された、公開中の Windows アプリです。
- アプリの UI は中国語と英語に対応しています。README が 3 言語で用意されていても、日本語 UI が実装済みという意味ではありません。
- パスワード認証はアプリからのアクセスを制御しますが、SQLite 内のプロンプト本文は暗号化しません。
- 文書化された設計にはクラウド同期、テレメトリー、自動更新がなく、監査済みのツリーにも自動バックアップコマンドはありません。
- 公開 EXE は未署名です。チェックサムはファイルの変更を検出できますが、信頼された発行者の身元までは証明しません。
- Tkinter のレイアウト、クリップボード動作、パッケージ版の起動を完全に確認するには、対話可能な Windows デスクトップが必要です。

## コントリビューション

UI の動作は `src/tips_prompt_manager/app.py`、認証/セッション処理は `auth.py`、SQLite 操作は `storage.py`、翻訳文言は `i18n.py`、ファイル配置は `paths.py` に保ってください。テストとスクリーンショットには合成プロンプトだけを使用し、中国語と英語の UI 文言を同時に更新して、変更に対応する単体テストと GUI/パッケージのスモークテストを実行します。実行時依存を追加する前に、互換性、ライセンス、サイズ、保守状況、オフライン動作への影響を明示的に監査してください。現在リント/フォーマットコマンドは定義されていません。

## 作者

- HaoXiang Huang
- [didadida1688@gmail.com](mailto:didadida1688@gmail.com)
- [プロジェクトサイト](https://nextweb4.github.io/)

## ライセンス

[MIT License](LICENSE) で公開されています。実行時は Python 標準ライブラリのみで、PyInstaller はビルド時だけ使います。詳細は [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) を参照してください。
