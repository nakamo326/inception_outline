---
sidebar_position: 2
---

# WordPress + PHP-FPM

webからCGIを通してphpを動作させるための実装php-fpmが設定されたコンテナ。

webからphpがリクエストされるとnginxサービスからport9000番を通してこのサービスで稼働するphp-fpmがプログラムを実行する。
