# テンプレ

**用途：** Godot / Xogot 共通スタータープロジェクト（新しいゲームを始めるときに複製して使う空のテンプレート）

## 含まれるもの

| 名前 | 場所 | 状態 |
| --- | --- | --- |
| Cyclops Level Builder 1.5.0 | `addons/cyclops_level_builder/` | インストール済み・有効化済み（Autoload `CyclopsAutoload` 登録済み） |
| Godot MCP Toolkit 1.0.2 | `addons/godot_mcp_toolkit/` | インストール済み・有効化済み（Autoload `MCPRuntimeServer` 登録済み） |
| `.mcp.json` | プロジェクト直下 | MCP Toolkit の設定生成機能（macOS 用の出力）と同じ内容 |
| `scenes/` `scripts/` `assets/` | プロジェクト直下 | 空（`.gitkeep` のみ） |

サンプルのシーン・スクリプト・素材は入っていません。

## バージョン記録（2026-09-23 作成時点）

| 項目 | バージョン | 確認方法 |
| --- | --- | --- |
| 作成・検証に使った Godot | **4.7.2-stable**（Mac の /Applications/Godot.app と同じ版） | 公式ビルドの headless エディタで読み込み検証 |
| Xogot（iPhone / iPad・App Store 版） | Xogot 1.6.10 ＝ **Godot 4.6 系**（1.6.0 で 4.6.1 に移行） | Xogot 公式ドキュメント・ブログ |
| Xogot（Mac 版 / TestFlight） | **Godot 4.7 系**へ移行中 | Xogot 公式ブログ「Xogot Is Moving to Godot 4.7」 |
| Cyclops Level Builder | **1.5.0**（公式リリース zip、SHA256 `c7a604ad…6ca4`） | 公式 README：「1.5.0 は Godot 4.7 以降で動作」 |
| Godot MCP Toolkit | **1.0.2**（公式リリース zip、SHA256 `0416b234…2f53`） | 公式：Godot 4.2 以上、4.7.0 までテスト済み、デスクトップ専用 |
| MCP サーバー | `@npgamedev/godot-mcp-server`（npx が最新版を取得） | package.json：`"node": ">=22"` |
| 必要な Node.js | **22 以上**（Mac のみ） | 同上 |

### Godot 4.6（現行の iPhone 版 Xogot）との互換性について

- Cyclops 1.5.0 の公式対応は **Godot 4.7 以降**です。現行の iPhone 版 Xogot（4.6 系）は公式対応の範囲外です。
- 念のため **Godot 4.6.1 公式ビルドでも検証済み**です。両プラグインを有効にした状態でエディタ読み込み・ゲーム実行ともに Parse Error なし。Cyclops が 4.7 で追加された API を使っていないことも API 一覧の差分で確認しました。
- iPhone 版 Xogot が 4.7 系に上がれば公式対応の範囲に入ります。
- 4.7 で保存したプロジェクトを 4.6 系で開くと「新しいバージョンで作られたプロジェクトです」という確認が出ることがあります。そのまま開いて問題ありません。

## 役割の違い

| 環境 | 使い方 |
| --- | --- |
| **Mac（Godot / Xogot for Mac）** | Cyclops ＋ MCP Toolkit を使う。MCP 対応 AI（Claude Code など）から `.mcp.json` 経由で Godot エディタを操作できる |
| **iPhone / iPad（Xogot）** | 通常の Godot プロジェクトとして使う。Cyclops は動作確認できた範囲で使用。**外部 MCP サーバーはデスクトップ専用**（iPhone では Node.js を動かせないため MCP は使えない） |

### iPhone で MCP Toolkit / Cyclops が原因の不具合が出たら

プラグインは GDScript 製で、Xogot は GDScript 製プラグインに対応しています。ただし iPhone 実機での動作は未検証です。
もしエラーでエディタが使いにくい場合は、iPhone の Xogot で **設定 → General → Plugins** を開き、該当プラグインのチェックを外してください。
プラグイン本体は `addons/` に残るので、Mac 側ではそのまま使えます。

## Mac での MCP のセットアップ

1. Node.js 22 以上を入れる（未インストールの場合）
   ```sh
   brew install node
   node --version   # v22 以上であること
   ```
2. Godot でこのプロジェクト（の複製）を開く。MCP Toolkit のドックに `[MCPServer] listening on 127.0.0.1:6550` と表示されれば OK。
3. プロジェクトのフォルダで MCP 対応 AI（例：Claude Code）を起動すると、`.mcp.json` の設定で `npx -y @npgamedev/godot-mcp-server` が起動して Godot につながります。
   - `.mcp.json` を作り直したいときは Godot のメニュー **プロジェクト → ツール → MCP Toolkit → Write .mcp.json** を使います。

接続の流れ：Godot（MCP Toolkit プラグイン）⇄ MCP Server（npx / Node.js）⇄ MCP 対応 AI

## 新しいゲームを始めるとき（テンプレは直接使わない）

1. このフォルダ `テンプレ` を丸ごと複製し、フォルダ名を新しいゲーム名に変える
2. 複製したフォルダの中の `.git` フォルダを削除する（テンプレの履歴を引き継がないため）
3. Godot で開き、**プロジェクト設定 → Application → Config → Name** を新しいゲーム名に変える
4. 必要なら `git init` して、新しいゲーム用の GitHub リポジトリ（Private 推奨）を作る

Cyclops と MCP Toolkit は `addons/` に入っているので、再ダウンロードは不要です。

## GitHub / iPhone への持っていき方

```
Mac でテンプレを作る → git commit → GitHub（Private）へ push
→ iPhone の Xogot → Project Manager →「↓」Download from GitHub → 開く
```

- iPhone で必要なものはすべて Git の管理対象です。`addons/` も含まれています。`.godot/` だけ除外しています。
- このリポジトリは **Git LFS を使っていません**。Cyclops 本家のリポジトリは画像を LFS で管理していますが、ここには公式リリース zip の実ファイルを入れてあります。GitHub の zip ダウンロードでも画像が欠けません。

### Private リポジトリが Xogot の「Download from GitHub」で取得できない場合

リポジトリを Public に変えずに、次の方法を使ってください（Xogot 公式ドキュメントの推奨方法）。

1. iPhone で **Working Copy** を開き「+」→「Clone Repository」
2. GitHub アカウントでログインし、このリポジトリを選ぶ
3. クローン先（Files app Location）に **「このiPhone内」→「Xogot」** フォルダを選ぶ
4. Xogot を再起動するとプロジェクト一覧に出てくる

## Mac と iPhone で同じゲームを編集し続ける場合

「Download from GitHub」は最初の取得用です。両方で編集を続けるなら、毎回次の手順が必要です。

- 作業を始める前に **Git Pull**（最新を取り込む）
- 作業が終わったら **Git Commit → Git Push**

iPhone でこれを行うには Working Copy（Push には Pro 版が必要）などの Git クライアントを使います。テンプレ自体は Working Copy がなくても使えます。

## 補足

- Godot で初めて開いたとき、`addons/` 内の `.import` ファイルに既定値の行が追加されることがあります。`cyclops_settings.config` というファイルが作られることもあります。どちらも正常な動作です。`cyclops_settings.config` は `.gitignore` で除外済みです。
- エディタを閉じるときに Cyclops 1.5.0 が `Cannot call method 'queue_free' on a null value`（cyclops_level_builder.gd:356）というエラーを 1 行出すことがあります。プラグイン本体の既知の不具合で、終了時だけに出るものです。プロジェクトには影響しません。
- プロジェクト内の参照はすべて `res://` です。特定の Mac の絶対パスや `.godot/` 内のファイルには依存していません。
