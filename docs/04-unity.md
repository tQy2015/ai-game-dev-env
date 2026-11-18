# 04. Unity と Unity CLI の導入（Windows）

**前提**: [03-claude-code.md](03-claude-code.md) まで終わっていること。授業で指定された Unity の版に従ってください。検証は 6000.3.23f1 で行っています。

**状態ラベル**: 【公式確認】は 2026-11-18 に公式ページで確認した内容。【未検証】は学生機で一度も通していない内容です。

## 1. Unity Hub と Unity Editor を入れる【未検証（標準の手順）】

1. Unity の公式サイトから **Unity Hub** を入れる。
2. Unity Hub で、授業で指定された版の Editor をインストールする（Hub の「インストール」から選ぶ）。
3. 必要なモジュール（例: Windows Build Support）は、授業の指示に従って追加する。

**初回起動の注意【未検証（Windows）】**: 新しい版の Editor を初めて起動すると、利用規約（Software Terms）の画面が出ることがあります。画面は先生の前で、指示に従って承諾してください。CLI や自動化で突破しようとしないでください。

## 2. Hub のコマンドラインは使わない【公式確認】

Unity Hub の CLI（`Unity Hub -- --headless`）は、Unity Hub 3.18.0 から非推奨です。Hub 4.0 以降では Unity CLI を使うよう案内されています。この手順書では使いません。

## 3. Unity CLI を入れる【公式確認】

VSCode の統合ターミナル（PowerShell）で、次のどちらかを実行する。

```powershell
irm https://unity.com/install.ps1 | iex
```

```powershell
winget install Unity.CLI
```

- 対応 OS: Windows 10 21H1 以降（公式）。
- **インストール後、VSCode を完全に終了して起動し直す。**
- 確認する:

```powershell
where.exe unity
unity --version
```

- `where.exe unity` が何も出ない場合は [cli-path.md](cli-path.md) を参照してください。
- インストール先は、既定では公式に明記されていません。`where.exe unity` で確認してください。

> **旧版との違い**: 旧版のガイドにあった public-cdn の URL とベータ版の指定は、公式ページに載っていません。上の方法を使ってください。

## 4. 基本のコマンド【公式確認】

| コマンド | 役割 |
|---|---|
| `unity doctor` | 環境の診断（読み取りのみ） |
| `unity auth login` | ブラウザでサインイン（本人が行う） |
| `unity auth status` | サインインの状態を確認 |
| `unity editors -i` | インストール済みの Editor を一覧 |
| `unity open <プロジェクトのパス>` | プロジェクトを Editor で開く |

- `unity auth login` は、先生の案内があるときに本人が行ってください。
- 公式の説明では、コマンドから Editor を操作するには **Unity Pipeline** パッケージが必要です。この手順書では扱いません。

## 5. 作品フォルダを開く

1. Unity Hub で「プロジェクト」→「新規プロジェクト」、または、授業で配られたプロジェクトを開く。
2. 初回は、読み込みに時間がかかることがあります。

## 6. 困ったとき

- 版が違う: Unity Hub で、プロジェクトが指定する版の Editor を入れる。
- `unity` が見つからない: [cli-path.md](cli-path.md)。
- サインインが通らない: 先生に相談する。

## 次へ

[05-maya.md](05-maya.md)（学校の方針で必要な場合のみ）
