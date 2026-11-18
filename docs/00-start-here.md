# はじめに：生成AIで作るゲーム制作環境

この手順書は、Windowsのパソコンに、生成AIに日本語で指示して作業するゲーム制作環境を作るためのものです。コマンドを暗記する必要はありません。やりたいことを伝え、AIが出した手順と結果を、自分の目で確かめながら進めます。

## 作る環境

| 役割 | ツール | 導入文書 |
|---|---|---|
| 作業場所 | VSCode | [01-vscode.md](01-vscode.md) |
| 記録と共有 | Git / GitHub CLI（gh） | [02-git-and-gh.md](02-git-and-gh.md) |
| 相棒（AI） | Claude Code（Codexは任意） | [03-claude-code.md](03-claude-code.md) |
| ゲーム制作 | Unity（Unity CLI を含む） | [04-unity.md](04-unity.md) |
| 3Dモデル | Maya 2026 | [05-maya.md](05-maya.md) |

困ったときは [cli-path.md](cli-path.md)（ツールが見つからない場合）と [safety-and-co-creation.md](safety-and-co-creation.md)（安全な使い方）を読みます。

## 始める前に

- **OS**: Windows 10（1809以降）または Windows 11（64bit）。
- **Unity CLI**: Windows 10 21H1 以降が必要です（公式）。
- **管理者権限**: 多くの手順は不要です。インストールで求められた場合は、学校の機器の管理者に相談してください。
- **Claude Code のアカウント**: 無料プランでは使えません。Pro、Max、Team、Enterprise、Console のいずれかが必要です（公式）。授業で用意された方法に従ってください。
- **Maya のアカウント**: 学校の方針と、Autodesk の教育ライセンスの条件に従ってください（[05-maya.md](05-maya.md)）。
- **所要時間**: 未計測です。学生機で通した後に追記します。

## 手順の進め方

1. [01-vscode.md](01-vscode.md)
2. [02-git-and-gh.md](02-git-and-gh.md)
3. [03-claude-code.md](03-claude-code.md)（Codexを使う場合は同文書の節を読む）
4. [04-unity.md](04-unity.md)
5. [05-maya.md](05-maya.md)（学校の方針で必要な場合のみ）

インストールのあとは、**VSCode を完全に終了して起動し直してから**、次の手順に進んでください。

## 表示の約束（各文書の状態ラベル）

- **【公式確認】**: 2026-11-18 に公式ページで内容を確認した。
- **【二次情報】**: 大学、ブログ、旧版の文書による。公式では未確認。
- **【未検証】**: 学生機で一度も通していない。

学生機で通すまで、【未検証】の手順は、先生に相談してから実行してください。
