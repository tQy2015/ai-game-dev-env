# よくある質問（FAQ）

## Maya に AI が接続できない（方法B: commandPort）

**症状**: 「Maya compatibility probe returned an invalid response」「Cannot connect to Maya commandPort」と出る。

**原因**: Maya の接続口（ポート 7001）を、`sourceType='mel'` で開いている。AI の接続は Python で行うため、MEL の口では応答できません。

**直し方**: 接続口を、`sourceType='python'` で開き直す。
1. Maya を終了する。
2. [05-maya.md](05-maya.md) §9 方法B の `open_port.mel` を使って、Maya を起動し直す。
3. AI に、もう一度接続を頼む。

**確認したこと**: 同じ Maya を毎回起動し直して比べた結果、`python` では接続でき、`mel` では接続できませんでした（2026-11-18・Linux 環境）。Windows での確認は、まだ行っていません。

## Unity Editor が「ライセンスが無い」と言って止まる

**症状**: `No valid Unity Editor license found. Please activate your license.` と出て、実行後数秒で Editor が終了する。

**原因**: Unity CLI のサインイン（`unity auth login`）と、Unity Editor 本体（Unity Hub 経由のアカウント連携・ライセンス取得）は別の仕組みです。CLI だけサインインしていても、Editor 本体のライセンスが有効化されていないとこのエラーになります。

**直し方**: Unity Hub を開いてサインインし、ライセンス（Personal 等）を取得してから、もう一度実行してください。

**確認したこと**: 2026-11-18・Windows機で実機確認。CLI側の`unity auth status`は成功していても、Editor本体側が未確認だとこのエラーで止まりました。

## AIに作ってもらった新しいシーンが、Unityを開いても出てこない

**症状**: AI に新しいシーンを作ってもらったはずなのに、Unity Editor を開くと前に開いていたシーンのまま。

**原因**: Unity Editor は、起動時に「前回のセッションで最後に開いていたシーン」を復元します。新しく作ったシーンには自動で切り替わりません。

**直し方**: Unity の **Project** ウィンドウから、新しく作られたシーン（`Assets/Scenes/` の中）をダブルクリックして開く。[06-natural-language-placement.md](06-natural-language-placement.md) も参照。

## Unity Editorに「Account API did not become accessible」という警告が出る

**症状**: コンソールに `Account API did not become accessible within 30 seconds.` という警告（エラーではない）が出る。

**原因**: Unity 純正の「AI Assistant」機能（`Unity.AI.Toolkit`）が、クラウド側の設定を確認しようとして応答待ちになっているだけです。Editor ウィンドウにフォーカスが無いときに出やすい。

**対応**: 無視してよい。保存・コンパイル・シーン作成などの作業には影響しません。

## Maya を複数のパソコンで同時に開くと、片方が使えなくなる

**症状**: 学校の Maya のライセンスは、同時に使える台数に上限がある場合があります。1台で開いたままにすると、別のパソコンで Maya が起動できなくなることがあります。

**対応**: 確認が終わった Maya は、使い終わったら忘れずに終了する。AI に作業してもらった後（[05-maya.md](05-maya.md) §9 方法A）も同じです。
