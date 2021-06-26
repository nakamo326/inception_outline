---
sidebar_position: 2
---

# 主な課題の要件

ファイル構成等は省略

- ビルドされたドッカーイメージは対応したサービスと同じ名前にする。
- コンテナはすべてひとつ前の安定版alpineか、debian:busterからビルドする。
- サービスごとにDockerfileを作り、そのDockerfileはdocker-compose.ymlで使用する。
- 構築するものは以下の通り。
	- TLSv1.2もしくはTLSv1.3のみで通信するNGINXを含むコンテナ
	- WordPressとphp-fpmが設定されたコンテナ（NGINXは含まない）
	- MariaDBを含むコンテナ（NGINXは含まない）
	- WordPressのデータベース用のvolume
		- 2つのユーザーが必要。一つは管理者でユーザーネームにadmin等を含んではいけない。
	- WordPressのWebサイト用のvolume
	- コンテナを接続するdocker-network
		- network: host, network: --link, linksの使用は禁止
		- networkはdocker-compose.yml内に明示される必要がある
- コンテナはクラッシュしたとき再起動する必要がある
	- `docker-compose service-name kill 1` でコンテナ終了、`docker ps` で再起動が確認できる。
- コンテナは無限ループを含むコマンドで開始してはいけない。（tail -f, bash, sleep, while true）
- volumeは/home/login/data内に作成する。
- ローカルIPをにリダイレクトするドメインネームを設定する。（login.42.fr: loginは各自のログイン名）
- latestタグは禁止
- Dockerfileにパスワードを含んではいけない。環境変数を利用する。.envファイルの使用を推奨。
- NGINXコンテナは構築したサービスへの唯一のエントリーポイントであり、ポート443からTLS1.2かTLS1.3を使用して通信する。

構築例
![structure](/img/structure.png)
