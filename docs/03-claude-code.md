# 03. Claude Code の導入と安全な設定（Windows）

**前提**: [02-git-and-gh.md](02-git-and-gh.md) まで終わっていること。アカウントは有料プラン（Pro、Max、Team、Enterprise、Console のいずれか）が必要です（公式）。

## 1. インストールする【公式確認】

VSCode の統合ターミナル（PowerShell）で実行する。

```powershell
irm https://claude.ai/install.ps1 | iex
```

- winget を使う方法もあります（公式）: `winget install Anthropic.ClaudeCode`。この方法は自動更新されないため、定期的に `winget upgrade Anthropic.ClaudeCode` を実行します。
- **インストール後、VSCode を完全に終了して起動し直す。** 見つからない場合は [cli-path.md](cli-path.md) を参照。

確認する。

```powershell
claude --version
claude doctor
```

`claude doctor` は、インストールの状態と設定ファイルのエラーを、読み取り専用で表示します（公式）。

## 2. Git Bash について【公式確認】

- Git for Windows は**任意**です。無い場合、Claude Code は PowerShell でコマンドを実行します。
- Git for Windows が入っているのに bash を見つけられない場合だけ、設定ファイルの `env` に場所を書きます（[cli-path.md](cli-path.md) の §4）。

## 3. 起動と認証

1. ターミナルで `claude` と入力して Enter。
2. 初回はブラウザでの認証を求められるので、アカウントでログインする。
3. 「今のフォルダの中身を教えて」と頼み、応答が返ることを確認する。

## 4. 設定ファイル（settings.json）を置く

設定ファイルは `%USERPROFILE%\.claude\settings.json` に置きます（ユーザー設定。全フォルダに適用されます）。

1. `%USERPROFILE%` 直下に `.claude` フォルダを作る（例: `C:\Users\自分の名前\.claude`）。
2. このリポジトリの `templates/settings.json.example` の中身を、`settings.json` という名前で `.claude` の中に保存する。
3. 1文字も変えずに使う。

### 4-1. 中身の意味と、できないこと（大事）

テンプレートの仕組みは次のとおりです（公式の仕様で確認。ルールは **deny → ask → allow** の順で評価され、最初に一致したものが効きます）。

| 区分 | 中身 | 動き |
|---|---|---|
| allow | `git status` / `diff` / `log` | 確認なしで実行 |
| ask | `git commit` / `push`、削除、パッケージの導入、MCP | 実行の前に必ず確認が出る |
| deny | 秘密情報ファイルの読み取り、設定の書き換え、ネットワーク取得、シェルの切り替え | 実行されない |

**ただし、次の限界があります。**

- deny は、Claude のファイル操作と、認識されたコマンド（`cat` など）に効きます。**Python や Node のスクリプトが内部で開くファイルは止まりません。**
- Bash や PowerShell のルールは、コマンドの文字列への照合です。完全な安全の境界ではありません。
- OS レベルでファイルを守るのは、サンドボックスの役割です。ネイティブ Windows での対応は、公式ページでの再確認が必要です。

**だから、設定ファイルより前に、秘密情報を置かない・貼らないことを守ってください。** 設定は、最後の防御線です。

### 4-2. 確認する（3つ）

1. `claude doctor` で、設定ファイルのエラーが出ないこと。
2. Claude Code を開き、`/permissions` と入力して、ルールが表示されること。
3. 「curl で example.com を取得して」と頼む。**止まって確認を求めれば成功。** 同様に PowerShell 側でも確認すること（`Invoke-WebRequest` などを頼む）。片方だけ止まる場合は設定を見直す。

分からない、止まらない、エラーが出る場合は、自分で直さず先生に画面を見せてください。

### 4-3. 承認の習慣

- 確認が出たら、内容を読んでから承認する。毎回同じ操作を「いつも許可」にしない。
- 確認を全部飛ばすオプション（`--dangerously-skip-permissions`）は使わない。

## 5. CLAUDE.md と STATE.md（自分用のメモ）

Claude Code は、作業フォルダに `CLAUDE.md` と `STATE.md` があると、作業の最初に読みます。

| ファイル | 役割 | 例 |
|---|---|---|
| CLAUDE.md | ずっと変わらないこと | 作品の基本ルール、使うツール、守る決まり |
| STATE.md | 今の状況だけ | 今やっていること、次にやること、困っていること |

**コツ**: 終わった項目は STATE.md から消し、HISTORY.md に移す。「STATE.md の終わった項目を HISTORY.md に移して」と頼めば、AI がやってくれます。

## 6. Codex を使う場合（任意・一部未検証）

授業で Codex を使う場合だけ読んでください。Claude Code と Codex のどちらか一方で十分です。

- **Node.js** の LTS 版を入れる（版の要件は未確認。二次情報では 18 以上、22 以上の両方が見られる）。
- インストール（二次情報・公式未確認）: `npm install -g @openai/codex`。スコープ付きの名前を使うこと。
- **サンドボックス**【公式確認（OpenAI の Windows ページ）】: 設定ファイルの `[windows]` の表で `sandbox = "elevated"` を指定する。管理者による承認済みの設定が必要。使えない場合のみ `"unelevated"`。
- 設定ファイルの場所は `CODEX_HOME` の下と案内されている（既定の場所は未確認）。
- 本格的な利用の手順は、学生機で確認してから追記します。

## 次へ

[04-unity.md](04-unity.md)
