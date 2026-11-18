# 04. Unity と Unity CLI の導入（Windows）

**前提**: [03-claude-code.md](03-claude-code.md) まで終わっていること。授業で指定された Unity の版に従ってください。検証は 6000.3.23f1 で行っています。

**状態ラベル**: 【公式確認】は 2026-11-18 に公式ページで確認した内容。【二次情報】は、大学や旧版、ブログなどの情報（公式では未確認）。【未検証】は学生機で一度も通していない内容です。

## 1. Unity Hub と Unity Editor を入れる【二次情報（標準の手順）】

1. Unity の公式サイトから **Unity Hub** を入れる。
2. Unity Hub で、授業で指定された版の Editor をインストールする（「インストール」から選ぶ）。
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

> **旧版との違い**: 旧版のガイドにあった public-cdn の URL とベータ版の指定は、公式ページに載っていません。上の方法を使ってください。

## 4. 環境パス（PATH）を確認する

Unity の CLI は、ターミナルから `unity` と打てる必要があります。PATH が通っていないと、「認識されません」と出ます。

**確認**（VSCode の統合ターミナルで実行）:

```powershell
where.exe unity
unity --version
```

- 場所が表示されれば OK です。インストール先は、既定では公式に明記されていません（上の確認で場所を見てください）。
- 何も出ない場合は、次の手順で PATH を追加します。
  1. スタートメニューで「環境変数」と検索 →「環境変数を編集」を開く。
  2. 上段「ユーザー環境変数」の `Path` を選び、「編集」→「新規」で、`unity.exe` が入っているフォルダを 1 行追加する。
  3. OK を押して閉じ、**VSCode を完全に終了して起動し直す。**
  4. `where.exe unity` を再実行する。
- それでも通らない場合は [cli-path.md](cli-path.md) を見てください。

**注意【二次情報】**: 旧版の調査では、インストーラーが PATH を自動で追加する、という記録がありました。ただし、学生機で再確認していません。

## 4-1. VSCode から使う設定（Unity CLI とセット）

Unity CLI は、**VSCode の統合ターミナルから使う前提**です。次の 3 つを、この順でそろえてください。

1. **PATH を通す**（§4）。Windows の環境変数に、`unity.exe` のフォルダを入れる。
2. **VSCode を完全に終了して起動し直す。** VSCode は、起動した時点の PATH を引き継ぐためです。
3. **統合ターミナルの既定を PowerShell にする**（[01-vscode.md](01-vscode.md) §2）。

**確認**（統合ターミナルで実行）:

```powershell
where.exe unity
unity --version
unity doctor
```

- `where.exe unity` が場所を返し、`unity --version` が版を返せば、VSCode からの設定は完了です。
- VSCode の設定で PATH を直接書き足す方法もあります（`terminal.integrated.env.windows` に `Path` を指定する）。ただし、この方法は学生機で未検証です【未検証】。通常は §4 の環境変数で十分です。
- 設定ファイル（`settings.json`）は、VSCode の「設定を開く（JSON）」から編集できます。編集は、先生の確認を受けてから行ってください。

**よくある失敗**
- インストール直後に、開いたままの VSCode で `unity` が認識されない → VSCode を完全に終了して起動し直す。
- 統合ターミナルが、PowerShell 以外（WSL など）になっている → 既定を PowerShell に戻す。
- `unity doctor` で、サインインの項目が未ログイン → `unity auth login` を本人が行う。

## 5. 基本のコマンド【公式確認】

| コマンド | 役割 |
|---|---|
| `unity doctor` | 環境の診断（読み取りのみ） |
| `unity auth login` | ブラウザでサインイン（本人が行う） |
| `unity auth status` | サインインの状態を確認 |
| `unity editors -i` | インストール済みの Editor を一覧 |
| `unity open <プロジェクトのパス>` | プロジェクトを Editor で開く |

- `unity auth login` は、先生の案内があるときに本人が行ってください。
- 公式の説明では、コマンドから Editor を操作するには **Unity Pipeline** パッケージが必要です。この手順書では扱いません。

## 6. 作品フォルダを作って開く

1. Unity Hub で「プロジェクト」→「新規プロジェクト」で作る。授業で配られたプロジェクトを使う場合は、そのフォルダを開く。
2. **プロジェクトを作ったら、先に Git の設定をする**（§7）。後から設定を変えると、ファイルの形式が変わって差分が大きくなります。

## 7. Unity プロジェクトの Git 管理

### 7-1. 先に Editor の設定を 2 つ変える【二次情報・教員側の実機で確認】

Unity の **Project Settings → Editor** で、次の 2 つを確認します。
- **Asset Serialization → Mode: Force Text**（テキスト形式で保存する。差分が読める）
- **Version Control → Mode: Visible Meta Files**（`.meta` ファイルを表示する）

実プロジェクトでは、`ProjectSettings/EditorSettings.asset` の `m_SerializationMode: 2` で、Force Text を確認しています。画面の名称は版によって違うことがあるので、見つからない場合は先生に相談してください。

### 7-2. どのフォルダを Git に入れるか

| 入れる | 入れない（.gitignore） |
|---|---|
| `Assets/`（スクリプト、シーン、Prefab、素材） | `Library/`（Unity が作り直す中間ファイル） |
| `Packages/`（パッケージの一覧） | `Temp/`、`Obj/`、`Logs/` |
| `ProjectSettings/`（プロジェクトの設定） | `UserSettings/`（個人のエディタ設定） |
| `.gitignore`、`.gitattributes` | `Builds/`、`Build/`（書き出し結果） |
| | `*.csproj`、`*.sln`（エディタが作り直す） |

- **`.meta` ファイルは必ず入れる。** `.meta` には、各素材の番号（GUID）が入っています。消すと、シーンの参照が壊れます。
- ファイルの名前は、**Explorer ではなく Unity の Project ウィンドウで変える。** 番号が保たれます。

### 7-3. 最初に置く 2 つのファイル

テンプレートを、作品フォルダ（`Assets` などがあるフォルダ）の直下にコピーします。
- `templates/unity.gitignore.example` → `.gitignore`
- `templates/unity.gitattributes.example` → `.gitattributes`

### 7-4. 最初のコミット

```powershell
git init
git add .gitignore .gitattributes
git commit -m "chore: Unity プロジェクトの Git 設定を追加"
git add Assets Packages ProjectSettings
git status
git commit -m "feat: 作品の初期状態を追加"
```

- `git status` で、`Library/` などが出ていないことを確認してから commit する。
- 以降の作業は、[02-git-and-gh.md](02-git-and-gh.md) の PULL → COMMIT → PUSH に従う。

### 7-5. 大きなバイナリの扱い【二次情報】

- 画像、音声、3D モデル（`.png`、`.psd`、`.fbx`、`.wav` など）は、大きくなると Git が重くなります。
- 小さな作品なら、そのまま入れても構いません。
- 大きくなる場合は、**Git LFS**（大容量ファイルを別に管理する仕組み）を使えます。`.gitattributes` の LFS の行のコメントを外し、`git lfs install` を一度実行します。
- LFS を使うかは、先生と決めてください。学生機では、まだ通していません【未検証】。

### 7-6. やってはいけないこと

- `Library/` を入れる（容量が大きく、他の人の環境で壊れやすい）。
- `Assets` のフォルダ構成を、Explorer で直接いじる（`.meta` が外れます）。
- 他の人が作ったシーンを、同時に編集する（同じファイルの衝突が起きやすい。作業を分けてください）。

## 8. 困ったとき

- 版が違う: Unity Hub で、プロジェクトが指定する版の Editor を入れる。
- `unity` が見つからない: §4 と [cli-path.md](cli-path.md)。
- サインインが通らない: 先生に相談する。
- 差分が大量に出る: §7-1 の設定（Force Text）と §7-2 の .gitignore を確認する。
- **`No valid Unity Editor license found` と出て、数秒で Editor が終了する**【実機確認・2026-11-18】: これは `unity auth login`（CLI 側のサインイン）とは**別の問題**です。`unity auth status` が成功していても、Editor 本体（Unity Hub 経由のアカウント連携・ライセンス取得）側が未完了だとこのエラーになります。Unity Hub を開いてサインインし、ライセンス（Personal 等）を取得してから、もう一度実行してください。

## 次へ

[05-maya.md](05-maya.md)（学校の方針で必要な場合のみ）
