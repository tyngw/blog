## Blog URL
https://tyngw.hatenablog.com/

## 概要
GitHubとはてなブログを連携するための環境です。
これは[push-to-hatenablog](http://github.com/mm0202/push-to-hatenablog)のテンプレートから作成されました。
更新は[blogsync](https://github.com/x-motemen/blogsync)で行われます。

## ワークフロー概要

このリポジトリでは、プルリクエストベースの運用により、安全にはてなブログと記事を同期します。

### 基本的な流れ

1. **ローカルで記事を編集** → feature/記事名 などのブランチで作業
2. **プルリクエストを作成** → masterブランチへ
3. **レビュー・マージ** → 自動的にはてなブログに反映
4. **はてなブログからの変更取得** → 自動PRで通知（要レビュー）

## 使い方

### 新しい下書き記事を作成する

1. `create`ブランチを作成してpush:
   ```bash
   git checkout -b create
   # draft.md を編集
   git add draft.md
   git commit -m "Add new draft article"
   git push origin create
   ```

2. GitHub Actionsが自動的に:
   - はてなブログに下書きとして投稿
   - 投稿内容を取得
   - `bot/new-draft-from-create`ブランチでPRを作成
   - `create`ブランチを削除

3. PRを確認してmasterにマージ

### 既存の記事を編集する（下書きを含む）

1. 作業ブランチを作成:
   ```bash
   git checkout -b feature/update-article
   # entries/配下のファイルを編集
   # 下書き記事も公開記事も同じ方法で編集可能
   git add entries/
   git commit -m "Update article content"
   git push origin feature/update-article
   ```

2. GitHubでプルリクエストを作成

3. レビュー後、masterにマージすると自動的にはてなブログに反映
   - 下書き記事の場合: 下書きとして更新されます
   - 公開記事の場合: 公開記事として更新されます
   - 記事の状態（下書き/公開）は、ファイル内の`Draft`フィールドで制御されます

### はてなブログから記事を同期する

はてなブログ側で直接編集した内容を取り込む方法:

1. GitHub ActionsのワークフローページでWorkflow `pull from hatena blog` を手動実行

2. GitHub Actionsが自動的に:
   - はてなブログから最新の記事を取得
   - 変更があれば`bot/sync-from-hatena`ブランチでPRを作成

3. PRの内容を確認してmasterにマージ
   - マージすると、はてなブログに変更が反映されます（同期のため問題なし）

### 下書き記事を公開する

[はてなブログの管理画面](https://blog.hatena.ne.jp/tyngw/tyngw.hatenablog.com/drafts)から公開してください。

公開後、`pull from hatena blog`ワークフローを実行すると、公開状態がリポジトリに反映されます。

## セキュリティに関する注意事項

### 重要: 認証情報の管理

- `blogsync.yml`や`blogsync.yaml`は`.gitignore`で除外されています
- 認証情報は**GitHub Secrets**で管理してください:
  - `BSY`: blogsyncの設定ファイル内容全体
  - `DOMAIN`: はてなブログのドメイン

### 設定例

GitHub SecretsのBSYには以下のような内容を設定:
```yaml
tyngw.hatenablog.com:
  username: tyngw
  password: your_api_key_here
default:
  local_root: entries
```

## トラブルシューティング

### ローカルブランチの掃除

リモートで削除されたブランチをローカルから削除:
```bash
git fetch --prune
```

### ワークフローが失敗する場合

1. GitHub Secretsが正しく設定されているか確認
2. `blogsync.yml`が誤ってコミットされていないか確認
3. Actions実行ログを確認して詳細を把握

## ワークフローの詳細

### push.yml
- masterブランチの`entries/**`に変更があると自動実行
- 変更されたファイルをはてなブログにpush
- ボットによるコミット(`[bot]`を含む)はスキップして無限ループを防止

### pull.yml
- 手動実行のみ（workflow_dispatch）
- はてなブログから記事を取得してPRを作成

### create-draft.yml
- `create`ブランチがpushされると実行
- 新しい下書き記事をはてなブログに投稿してPRを作成  