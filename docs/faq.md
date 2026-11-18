# よくある質問（FAQ）

## Maya に AI が接続できない

**症状**: 「Maya compatibility probe returned an invalid response」「Cannot connect to Maya commandPort」と出る。

**原因**: Maya の接続口（ポート 7001）を、`sourceType='mel'` で開いている。AI の接続は Python で行うため、MEL の口では応答できません。

**直し方**: 接続口を、`sourceType='python'` で開き直す。
1. Maya を終了する。
2. [05-maya.md](05-maya.md) §8 の `open_port.mel` を使って、Maya を起動し直す。
3. AI に、もう一度接続を頼む。

**確認したこと**: 同じ Maya を毎回起動し直して比べた結果、`python` では接続でき、`mel` では接続できませんでした（2026-11-18・Linux 環境）。Windows での確認は、まだ行っていません。
