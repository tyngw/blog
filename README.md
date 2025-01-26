## 概要
GitHubとはてなブログを連携するための環境です。
これは[push-to-hatenablog](http://github.com/mm0202/push-to-hatenablog)のテンプレートから作成されました。

## 新しい記事を投稿する場合

```
docker-compose run --rm blogsync post --title=draft --draft --custom-path=${path} ${domain} < draft.md
```

## 既に投稿した記事を修正する場合
`draft`ブランチにpushしてください

## はてなブログから`publish`ブランチに記事を同期する場合
Workflow `pull from hatena blog` を実行してください