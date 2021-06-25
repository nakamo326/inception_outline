---
sidebar_position: 1
---

# NGINX

オープンソースのwebサーバー。
今回の課題では、port443でSSL通信を受け付け、リクエストされた通信を行う。

### eval


``` shell
eval "echo \"$(cat template.conf)\"" > config.conf
```

`eval` を使うことで環境変数をファイルに埋め込むことができる。（評価したくない$はバックスラッシュでエスケープする）

docker-compose.ymlからビルド時にオプションargsで環境変数を渡すことで、.envファイルのみで公開したくない情報の管理が行える。

同様の処理を行うコマンドにenvsubstなどがある。


### mkcert

今回はsslの自己証明書の発行にmkcertを利用。

host上でブラウザからアクセスしたときの警告が出ないかも。

opensslに比べて設定が少ないのも楽で良いです。

### daemon off

dockerコンテナはフォアグラウンドでプロセスが実行されていないと終了してしまうので、
nginxはバックグラウンドで実行されないよう設定する必要がある。