### コンテナ一覧閲覧
#### コンテナリストの表示
```cmd
$ docker ps
CONTAINER ID        IMAGE                      COMMAND                  CREATED             STATUS                    PORTS                               NAMES
e80a2e378dd0        redis                      "docker-entrypoint.s…"   4 days ago          Exited (255) 2 days ago   0.0.0.0:6379->6379/tcp              redis
c25d0803c172        handson/tutorial2:latest   "python index.py"        3+++++++++++++++++++++++++++++++++++12 days ago         Exited (255) 2 days ago   0.0.0.0:8080->8080/tcp              tutorial2_application_1
```
`-a`オプションで停止しているコンテナも取得できる

#### イメージリストの表示
```cmd
docker images
```

### \<none>イメージの削除
-fオプションを使うとイメージを抽出できる。
```
docker images -f "dangling=true"
```
この出力を使ってイメージ一括削除
```
docker rmi $(docker images -f "dangling=true" -q)
```

### ローカルからdockerコンテナにコピー
```
docker cp {送信するファイル} {送信先}
```
例：
`docker cp scripts/demo/item_type.sql` $(docker-compose ps -q web):/tmp/`
なおこれはdocker-composeの部分でidを指定している
### 滅びの呪文

docker-composeで作成されたコンテナ、イメージ、ネットワーク、ボリューム、未定義コンテナすべてを一括削除する
```
docker-compose down --rmi all --volumes --remove-orphans
```
* `down`
    `up`の逆で、`up`コマンドで作られるイメージ、コンテナ、ボリューム、ネットワークをすべて削除する。
* `--rmi`
    削除するイメージの種類を指定。`all`はすべて。`local`はフィールドにカスタムタグのないイメージのみ。`--rmi`オプションを省略するとイメージは消えない。

* `--volumes`
    `docker-compose.yml`の`volumes`セクションに書かれた名前付きボリュームとコンテナにアタッチされたanonymous volumeが削除される。
* `--remove-orphans`
    `docker-compose.yml`で定義から削除されたサービス用の未定義コンテナも削除される。
    
これらのオプションを付けないと削除されるのはコンテナとネットワークのみ。
