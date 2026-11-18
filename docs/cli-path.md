# CLI が使えない（PATH）の確認と直し方（Windows・VSCode のターミナル）

**対象**: git、gh、code、node、npm、claude、codex、unity、maya（mayapy）。VSCode の統合ターミナルで操作する学生向け。

## 1. 原則（最初に読む）

- VSCode のターミナルは、**VSCode を起動した時点の PATH** を引き継ぎます。
- インストール直後は、開いている VSCode にも PowerShell にも新しい PATH が反映されていません。
- そのため、**インストールしたら VSCode を完全に終了してから起動し直してください。** 「ターミナルを開き直す」だけでは足りないことがあります。
- 「Reload Window」（ウィンドウの再読み込み）でも反映されないことがあるため、使わないでください。

## 2. 手順

1. インストールが終わったら、VSCode の**全ウィンドウを閉じる**。タスクトレイに VSCode が残っていないか確認し、残っていれば終了する。
2. VSCode を起動し、統合ターミナル（`Ctrl` + `` ` ``）を開く。
3. 次のコマンドを順に実行する。**「見つかりません」と出たものだけ** 4 に進む。

```powershell
where.exe git
where.exe gh
where.exe code
where.exe node
where.exe npm
where.exe claude
where.exe unity
where.exe mayapy
```

- 例: `where.exe git` が `C:\Program Files\Git\cmd\git.exe` を返せば OK。
- 「INFO: 指定されたファイルが見つかりません」と出た場合は、そのツールの PATH がまだ通っていません。

4. まだ通らない場合は、PATH を手で追加する（§3）。

## 3. 手動で PATH を追加する

1. スタートメニューで「環境変数」と検索 →「環境変数を編集」を開く。
2. 上段「ユーザー環境変数」の `Path` を選び「編集」→「新規」で、次のフォルダを 1 行ずつ追加する。
   - Unity CLI: `%LOCALAPPDATA%\Unity\bin`（未確認。`where.exe unity` の結果を優先する）
   - 他のツールは、`where.exe` が返した場所のフォルダ（`.exe` があるフォルダ）を追加する。
3. OK を押して閉じ、**VSCode を完全に終了して起動し直す**。
4. §2 の `where.exe` を再実行する。

## 4. Claude Code の注意（Windows）【公式確認】

- Git for Windows は**任意**です。無い場合、Claude Code は PowerShell でコマンドを実行します。
- Git for Windows が入っているのに bash を見つけられない場合は、設定ファイル（`%USERPROFILE%\.claude\settings.json`）に次を追記します。

```json
{
  "env": {
    "CLAUDE_CODE_GIT_BASH_PATH": "C:\\Program Files\\Git\\bin\\bash.exe"
  }
}
```

- 既知の不具合: この変数を設定しても認識されない報告があります（GitHub の issue）。その場合は先生に相談してください。

## 5. Codex の注意

- `npm install -g @openai/codex` の後、`where.exe codex` が見つからない場合は、npm のグローバル保存先（`npm config get prefix` の結果）が PATH に入っていないことが原因です。
- 対処: 保存先のフォルダを §3 の手順で `Path` に追加し、VSCode を起動し直す。
- Codex の本文（設定ファイル、サンドボックス）は未作成です。

## 6. Maya の注意

- `maya`・`mayapy` は既定で PATH に入っていないことがあります【未検証】。
- 既定のインストール先（二次情報・未確認）: `C:\Program Files\Autodesk\Maya<版>\bin\`（この中に `maya.exe`・`mayapy.exe` がある）。`<版>` は `Maya2026`・`Maya2027` のように、実際にインストールした版の名前に置き換える。
- 対処: 上記フォルダを §3 の手順で `Path` に追加する。見つからない場合は、AI に「mayapy の場所を探して」と頼むのも有効です。

## 7. ターミナルの種類を確認する

- VSCode 下部のターミナルの「+」の横の矢印で、プロファイルを確認する。
- 学生の標準は **PowerShell** を推奨します。WSL 系のプロファイルを選んでいると、Windows の PATH が見えないことがあります。

## 8. よくある症状

| 症状 | 原因 | 直し方 |
|---|---|---|
| `認識されません`（インストール直後） | VSCode が古い PATH を持っている | VSCode を完全に終了して起動し直す |
| `where.exe` が何も返さない | PATH が未追加 | §3 |
| VSCode では動くが、外部の PowerShell では動かない（逆も）| 別々に開いた環境の PATH が違う | 両方を開き直す |
| Claude Code が bash を見つけられない | Git for Windows の bash が無い、または PATH に無い | §4 |

## 9. 未検証の点

- 学生機（Windows、クリーンな環境）での通し確認は未実施です。
- Unity CLI の既定のインストール先は公式に明記されていません。`where.exe unity` で確認してください。
