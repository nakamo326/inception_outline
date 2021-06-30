---
sidebar_position: 2
---

# よく使いそうなコマンド例

#### docker-composeのコンテナに入る

docker-compose.ymlがあるディレクトリで  
`docker-compose exec [service_name] [sh or bash]`

#### Docker network一覧

`docker network ls`

#### Docker volume一覧

`docker volume ls`

#### Docker volume 詳細

`docker volume inspect [volume_name]`

### SQL

#### ログイン
`mysql -u [user name] --password=[user password]`

#### ユーザー情報取得
`select User, Host from mysql.user;`

#### wordpressデータベース(例 post検索)
`select post_author, post_content from [database name].wp_posts;`

