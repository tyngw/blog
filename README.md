## Blog URL
https://tyngw.hatenablog.com/

## 概要
GitHubとはてなブログを連携するための環境です。  
これは[push-to-hatenablog](http://github.com/mm0202/push-to-hatenablog)のテンプレートから作成されました。  
更新は[blogsync](https://github.com/x-motemen/blogsync)で行われます。  

## 新しい下書き記事を作成する場合
`create`ブランチを作成してください。  
hatena blogに新しい下書き記事をpostした後、`master`ブランチに反映されます。

ローカルで作成した場合は、リモートで削除されたローカルブランチを削除してください。
```
git fetch --prune
```

## 下書き記事を公開する場合
[人力で頑張る](https://blog.hatena.ne.jp/tyngw/tyngw.hatenablog.com/drafts)のです。

## 既に投稿した記事を修正する場合
`draft`ブランチにpushしてください  

## はてなブログから`publish`ブランチに記事を同期する場合
Workflow `pull from hatena blog` を実行してください  