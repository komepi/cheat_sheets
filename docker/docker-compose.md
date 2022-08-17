# docker-compose.yml
参考：[docker-compose.ymlの書き方について解説してみた - Qiita](https://qiita.com/yuta-ushijima/items/d3d98177e1b28f736f04)
docker-composeを使用する際の設定のようなもの。ここに各コンテナの設定を記述する。拡張子は`yml`
以下公式リファレンスの例
```yml
version: '2'
services:
  db:
    image: mysql
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
```
## version
docker-composeを使用するバージョン
このサービス群のバージョンでないので注意

## services
アプリケーションを動かすための要素
それぞれのサービスがそれぞれのコンテナになるようなイメージ
servicesでコンテナを立てるには既存のイメージを使用する方法と新しくビルドする2通りがある。例では`db`が前者、`web`が後者にあたる
`service`名は任意。ただしログに表示されたり、そのサービスを示す名前として使用される。

### 各項目について
`services`の主な設定項目は以下
|項目|意味|
|:--|:--|
|restart|実行時に再起動するかどうか|
|environment|環境変数設定（パスワードなど）|
|ports|DockerImageを立ち上げる際のポート番号|
|volumes|マウントする設定ファイルのパス|
|build|ビルドされるときのパス|
|depends_on|service同士の依存関係|
|entrypoint|デフォルトのentrypointを上書き|
|driver|ボリュームに使用するドライバー（動かすための接続先）の指定|
|command|ビルド時に実行されるコマンド|
* restart
以下から選択して指定する。
  * `no`:デフォルト。再起動しない
  * `always`:コンテナが削除されるまで常に再起動する
  * `on-failure`:終了コードがエラーを示しているとき再起動する
  * `unless-stopped`:終了コードに関係なく再起動するが、サービスが停止、再起動されると再起動しなくなる
* environment
`環境変数名=値`の形で記述
* ports
`{portA}:{portB}`のような形で記述
この場合`portA`のポートが来たら、`portB`のポートを使う
* volumes
`{path1}:{path2}`の形で記述。
Composefileから見た相対パスで`path1`が呼び出されたら`path2`を指定して実行
もしくは`{path1}:{path2}:{mode}`の形で記述。この時`mode`は`w`(書き込み),`r`(読み込み)で表現する。
Composefileから見た相対パスで`path1`が呼び出されたら`path2`を使って`mode`を行う

# elasticsearchのデータを永続化
```yml
version: '3.3'
services:
  elasticsearch:
    # Docker Hubからイメージを取得
    image: elasticsearch
    ports:
      - "9200:9200"
    expose:
      - 9300
    volumes:
      # コンテナのディレクトリをvolumeへマウント
      # ボリューム名:マウント対象のパス
      - es-data:/usr/share/elasticsearch/data

volumes:
  es-data:
    # ボリューム'es-data'はlocalに保存します
    driver: local
```