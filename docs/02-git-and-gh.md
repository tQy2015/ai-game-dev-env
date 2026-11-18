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

## 6. 自分のリポジトリを用意する

先生のリポジトリをそのまま使うのではなく、**自分専用のリポジトリ**を用意します。先生の案内に従って、次のどちらかで用意してください。

- **既存のリポジトリをクローンする**: 先生のリポジトリをFork（ブラウザで先生のリポジトリを開き、右上の **Fork** ボタンを押す）する、または GitHub Classroom などで配られたURLをそのまま使う。どちらも、自分のGitHubアカウントに紐づく「自分のリポジトリ」のURLが手に入る。
- **新規にリポジトリを作る**（先生の指示がある場合）:
  ```powershell
  gh repo create 自分のリポジトリ名 --private
  ```

どちらの方法でも、最終的に「自分のリポジトリのURL」（例: `https://github.com/自分のアカウント/リポジトリ名`）が手に入ります。次の手順で使います。

## 7. ローカルに取り込む

1. VSCode で `Ctrl+Shift+P` を押して、**Git: Clone** を選ぶ。
2. **自分のリポジトリ**（§6で用意したもの）のURLを貼り付ける。
3. 保存先のフォルダを選ぶ。

これで `origin` が自分のリポジトリを指す状態になります。

**先生のリポジトリの更新を取り込みたい場合だけ**（Forkした場合など）、先生のリポジトリを `upstream` として追加します。新規に作ったリポジトリの場合は、この追加は不要です。

```powershell
git remote add upstream 先生のリポジトリのURL
```

確認する。

```powershell
git remote -v
```

- **origin**: 自分のリポジトリ。commit、push、pull の通常の対象。
- **upstream**: 先生のリポジトリ（追加した場合のみ）。取り込み（pull）のみに使う。

## 8. 日常の3手順（PULL → COMMIT → PUSH）

作業のたびに、この順番で行います。基本は AI への日本語の指示で行い、コマンドは参考として併記します。

| 手順 | 意味 | 日本語での指示の例 | 対応するコマンド（参考） |
|---|---|---|---|
| **1. PULL** | リモート（自分のリポジトリ）から最新を得る | 「作業の前に、自分のリポジトリの最新を取り込んで」 | `git pull origin main` |
| **2. COMMIT** | 変更にバージョンを書き込む（ローカルの記録） | 「今の変更を、○○を直したというメッセージでコミットして」 | `git add 変えたファイル` → `git commit -m "○○を直した"` |
| **3. PUSH** | 記録したバージョンをリモートへ送る | 「今のコミットを自分のリポジトリに push して」 | `git push origin main` |

- **PULL は作業を始める前に行う。** 別の場所で更新があると、後で衝突しやすくなります。
- **COMMIT は、意味のある区切りごとに行う。** メッセージには「何を変えたか」を短く書きます。
- **PUSH は、COMMIT の後に行う。** COMMIT していないものは、PUSH されません。
- 手順の前後には、`git status`（変更の一覧）と `git log`（履歴）で、AI が何をしたかを確認します。

### AI に守らせる約束（任意）

作品フォルダに `templates/CLAUDE.md.example`（Claude Code 用）または `templates/AGENTS.md.example`（Codex 用）を、`CLAUDE.md` や `AGENTS.md` の名前でコピーして置くと、AI が毎回この作法を読んでから作業します。中身は、作品に合わせて書き足してください。

### 先生の更新を取り込む（upstream）

**§7で`upstream`を追加していない場合（新規にリポジトリを作った場合）は、この節は不要です。**

先生のリポジトリ（upstream）からの取り込みは、PULL の一種です。

| やりたいこと | 日本語での指示の例 | 対応するコマンド（参考） |
|---|---|---|
| 先生の更新を確認する | 「先生のリポジトリに更新があるか確認して」 | `git fetch upstream` |
| 先生の更新を取り込む | 「先生の更新を取り込んで」 | `git pull upstream main` |

- 先生のリポジトリ（upstream）へは **PUSH しない**。
- **コンフリクト（同じ箇所の競合）が出たら、AI に自動で解決させず、その場で止めて先生に相談する。**

## 次へ

[03-claude-code.md](03-claude-code.md)
