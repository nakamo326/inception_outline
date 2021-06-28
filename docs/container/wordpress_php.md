---
sidebar_position: 2
---

# WordPress + PHP-FPM

webからCGIを通してphpを動作させるための実装php-fpmが設定されたコンテナ。  
webからphpがリクエストされるとnginxサービスからport9000番を通してこのサービスで稼働するphp-fpmがプログラムを実行する。

### wp-config

~~wp-config.phpはボリューム内に設置されているため、ビルド時に設定できない。~~  
~~そのため、CMDからphp-fpmを起動する前にwp-config.phpに環境変数を埋め込んで、ボリューム内にコピーするシェルスクリプトを設定した。（正解かどうかは分からないです。）~~  
~~環境変数の展開はNGINXコンテナのビルド時と同様、run時にスクリプトが走るのでdocker-compose.yml内environment: で環境変数を設定。~~

```docker-compose
volumes:
    wordpress-vol:
        driver: local
        driver_opts:
            type : none
            o: bind
            device: /home/ynakamot/data/wordpress
```

上記のように名前付きボリュームでバインドマウントを行うことで、ボリューム作成時にコンテナの内容をホストにコピーすることができる。

参考: [How to set a path on host for a named volume in docker-compose.yml](https://stackoverflow.com/questions/36387032/how-to-set-a-path-on-host-for-a-named-volume-in-docker-compose-yml)

### PHP-module

WordPressの実行に必要なPHP拡張モジュールは、[ここ](https://make.wordpress.org/hosting/handbook/server-environment/)を参照。


### PHP-fpm

ft_serverと違い、同一コンテナ内でNGINXとphp-fpmが稼働していないため、UNIXドメインソケットでの通信ができない。
TCPでport9000で通信を行うよう設定が必要。

### WP-CLI

課題の要件として、サービス起動時にはWordPressはすでにインストールされており、ユーザーが2つ作成された状態でないといけない。
WP-CLIによってコマンドラインからWordPressの操作が行える。

参考:  
[WP-CLI HP](https://wp-cli.org/ja/)  
[コマンドリファレンス](https://developer.wordpress.org/cli/commands/)