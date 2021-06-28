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

上記のように `eval` コマンドを使うことで環境変数をファイルに埋め込むことができる。（評価したくない$はバックスラッシュでエスケープする）


同様の処理を行うコマンドに `envsubst` などがある。


### mkcert

今回はsslの自己証明書の発行に `mkcert` を利用。

host上でブラウザからアクセスしたときの警告が出ないかも。

opensslに比べて設定が少ないのも楽で良いです。

### daemon off

dockerコンテナはフォアグラウンドでプロセスが実行されていないと終了してしまうので、
nginxはバックグラウンドで実行されないよう設定する必要がある。
(他コンテナも同様)

もしコンテナrun時に行いたい処理がある場合、下記のように設定できる。

```Dockerfile title="Dockerfile"
CMD [ "sh", "/scripts/run.sh" ]

```

```shell title="run.sh"
# 任意の処理

exec nginx # 実行したいコマンド + 引数
```

※ alpineを利用しているときは、CMD命令で実行するシェルを指定したほうが良さそうです。  
スクリプト内にshebangを設定していてもexecでエラーになることがあります。