---
sidebar_label: 'docker-compose.yml'
sidebar_position: 3
---

### Tips

デフォルトだと、サービス起動時に作成されるネットワークに各コンテナが接続される。

今回は、明示する必要があるためyml内最上位にnetworks: を設定してそのネットワークに全てのコンテナが接続するよう設定した。
