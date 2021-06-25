---
sidebar_position: 1
---

# Intro

## About Inception

Inceptionは、**docker-compose**を使いwebアプリケーションサービスの構築を行う課題です。

**docker-compose**とは、dockerfileからビルドされたコンテナの挙動や連携を設定し、複数のコンテナから一つのサービスを構築するツールです。

これらの設定は、**docker-compose.yml**に記載されます。

Inceptionでは、**nginx**、**wordpress**、**php-fpm**、**mariadb**が稼働するコンテナを設定し、web上からアクセスできるwordpressサービスを構築します。
<!-- それぞれリンクはる
e.g. [new.docusaurus.io](https://new.docusaurus.io)
-->


## Installation

課題の要件を満たすため、/home/login/へのディレクトリ作成、/etc/hostsファイルの編集が必要になります。

```Makefile title="Makefile"
...
set_up: volume add_host extract_wp
	docker-compose -f $(YML) up -d --build
// highlight-start
# root権限が必要！
volume:
	mkdir -p /home/ynakamot/data/mariadb
	mkdir -p /home/ynakamot/data/wordpress

add_host:
	echo "127.0.0.1 $(DOMAIN)" >> /etc/hosts
// highlight-end
extract_wp:
...
```

そのため、初回起動時のみ `sudo` コマンド経由での `make set_up` の実行をお願いします。

```shell title="初回セットアップ"
sudo make set_up
```

二回目以降は `make` のみで問題ありません。


稼働しているサービスを停止、コンテナ、ネットワークを削除する際は、 `make down` をお願いします。
