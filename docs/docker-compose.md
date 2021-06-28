---
sidebar_label: 'docker-compose.yml'
sidebar_position: 3
---

## Tips

デフォルトだと、サービス起動時に作成されるネットワークに各コンテナが接続される。

今回は、明示する必要があるためyml内最上位にnetworks: を設定してそのネットワークに全てのコンテナが接続するよう設定した。

### .envファイル

今回の課題では、dockerfileやdocker-compose.ymlの中にパスワード等の個人情報を記載してはいけない。

docker-composeでは公開したくない情報の管理に.envファイルが利用できる。

Docker-composeは実行時にdocker-compose.ymlと同じ階層にある.envファイル内に定義された環境変数を読み込み、コンテナに渡すことができる。（読み込む.envファイルのパスは設定で変更できる。）

``` .env title=".env example"
COMPOSE_PROJECT_NAME=inception

#nginx domain
DOMAIN_NAME="ynakamot.42.fr"
...
```
key=valueの形式で.envファイル内に記載する。

例：ビルド時
```docker-compose title="docker-compose.yml"
...
services:
    nginx:
        build:
            context: ./requirements/nginx
            // highlight-start
            args:
                DOMAIN_NAME: ${DOMAIN_NAME}
            // highlight-end
...
```

```dockerfile title="dockerfile"
ARG DOMAIN_NAME
```

`build:` 内オプション`args:` に渡したい環境変数名: 値の形で指定し、Dockerfile内でARG命令で環境変数を読み込む。

例：run時
```docker-compose title="docker-compose.yml"
    wordpress:
        build:
            context: ./requirements/wordpress
        environment:
            ADMIN_USER: ${ADMIN_USER}
            ADMIN_MAIL: ${ADMIN_MAIL}
            ADMIN_PASS: ${ADMIN_PASS}
```

サービス定義直下の`environment:` で同様に指定。

上記のように定義することでコンテナに任意の環境変数を渡すことができる。
公開リポジトリでも、.envファイルのみ.gitignoreに追加すれば安全に公開できるので便利！