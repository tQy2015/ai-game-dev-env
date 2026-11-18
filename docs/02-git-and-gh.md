# 02. Git と GitHub CLI（gh）の導入（Windows）

**前提**: [01-vscode.md](01-vscode.md) が終わっていること。VSCode の統合ターミナル（PowerShell）で操作する。

**状態ラベル**: 本文の手順は旧版のガイドからの移植で、【未検証】です。学生機で通した後に更新します。

## 1. Git for Windows を入れる

1. [git-scm.com](https://git-scm.com/) から Windows 用のインストーラーを取得し、既定の設定で実行する。
2. **VSCode を再起動する。**
3. ターミナルで確認する。

```powershell
git --version
where.exe git
```

## 2. GitHub CLI（gh）を入れる

```powershell
winget install --id GitHub.cli
```

winget が使えない場合は [cli.github.com](https://cli.github.com/) からインストーラーを取得する。

**VSCode を再起動する。** その後、確認する。

```powershell
gh --version
```

## 3. GitHub のアカウントを作る

[github.com](https://github.com/) でアカウントを作る。

**注意**: GitHub に登録するメールアドレスは、**GitHub が用意する noreply アドレス**（`アカウント名@users.noreply.github.com` の形式）を使うことを勧めます。GitHub の設定画面の「Emails」で確認できます。公開されるリポジトリに、個人のメールアドレスが載らなくなります。

## 4. gh で認証する

```powershell
gh auth login
```

対話形式で次を選ぶ。

1. `GitHub.com`
2. `HTTPS`
3. `Login with a web browser`

画面に出た一時コード（`XXXX-XXXX`）を控え、Enter を押してブラウザで認証する。

確認する（アカウント名と認証の有無だけを見る）。

```powershell
gh auth status
```

認証が通っていれば、AIが git の操作をするときも、認証で止まりにくくなります。

## 5. Git の初期設定（初回のみ）

```powershell
git config --global user.name "表示する名前"
git config --global user.email "GitHubのnoreplyアドレス"
```

## 6. 先生のリポジトリを clone する

1. VSCode で `Ctrl+Shift+P` を押して、**Git: Clone** を選ぶ。
2. 先生から案内されたURLを貼り付ける。
3. 保存先のフォルダを選ぶ。

## 7. 自分のリポジトリとの関係

- **upstream**: 先生のリポジトリ。取り込み（pull）のみに使う。
- **origin**: 自分のリポジトリ。commit、push、pull の通常の対象。

授業の案内に従って、リモートの向きを設定してください。

```powershell
git remote rename origin upstream
git remote add origin 自分のリポジトリのURL
```

## 8. 日常の操作

基本は、AIへの日本語の指示で行います。コマンドは参考として併記します。

| やりたいこと | 日本語での指示の例 | 対応する git コマンド（参考） |
|---|---|---|
| 変更を記録する | 「今の変更を、○○を直したというメッセージでコミットして」 | `git add .` → `git commit -m "..."` |
| 自分のリポジトリへ送る | 「今のコミットを自分のリポジトリに push して」 | `git push origin main` |
| 先生の更新を取り込む | 「先生のリポジトリの更新を確認して、あれば取り込んで」 | `git fetch upstream` → `git pull upstream main` |

- **コンフリクト（同じ箇所の競合）が出たら、AIに自動で解決させず、その場で止めて先生に相談してください。**
- 先生のリポジトリ（upstream）へは push しないでください。授業でガードレールを設定する場合は、その案内に従ってください。

## 次へ

[03-claude-code.md](03-claude-code.md)
