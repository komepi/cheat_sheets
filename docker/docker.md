- [コンテナ一覧閲覧](#コンテナ一覧閲覧)
  - [コンテナリストの表示](#コンテナリストの表示)
  - [イメージリストの表示](#イメージリストの表示)
- [images](#images)
  - [一覧](#一覧)
  - [削除](#削除)
  - [\<none\>イメージの削除](#noneイメージの削除)
- [volume](#volume)
  - [リスト](#リスト)
  - [削除](#削除-1)
  - [全て削除](#全て削除)
  - [リンク切れ削除](#リンク切れ削除)
- [network](#network)
  - [一覧](#一覧-1)
  - [削除](#削除-2)
- [ローカルからdockerコンテナにコピー](#ローカルからdockerコンテナにコピー)
- [滅びの呪文](#滅びの呪文)
- [logの見方](#logの見方)
  - [ホストに存在するログ](#ホストに存在するログ)
- [コンテナの状態を監視](#コンテナの状態を監視)
  - [オプション](#オプション)
- [docker demonの自動起動](#docker-demonの自動起動)

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

### images
#### 一覧
```
docker images
```
#### 削除
```
docker rmi [コンテナID]
```
`-f`オプションで強制的に削除できる

#### \<none>イメージの削除
-fオプションを使うとイメージを抽出できる。
```
docker images -f "dangling=true"
```
この出力を使ってイメージ一括削除
```
docker rmi $(docker images -f "dangling=true" -q)
```

### volume
#### リスト
```
docker volume ls
```
#### 削除
```
docker volume rm <id>
```
#### 全て削除
```
docker volume rm $(docker volume ls -qf dangling=true)
```
#### リンク切れ削除
```
docker volume ls -qf dangling=true | xargs -r docker volume rm
```

### network
#### 一覧
```
docker network ls
```
#### 削除
```
docker network rm [<id,network_name> ...]
```
ただし何かのコンテナが使用していると削除できない。強制削除のオプションもなし
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
docker compose down --rmi all --volumes --remove-orphans
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

### logの見方
#### ホストに存在するログ
以下のコマンドでパスが取得できる
```cmd
$ docker inspect {コンテナidやname} | grep 'LopPath'
```
このパスの先のファイルの閲覧をしようとすると権限がないといわれることがあるので、その時はsudoなどを使用する。

### コンテナの状態を監視
`docker stats`で調べることができる。
```
$ docker stats
CONTAINER ID        NAME                     CPU %           MEM USAGE / LIMIT       MEM %       NET I/O             BLOCK I/O           PIDS
0d173431ff71        concourse-worker_1       14.37%          32.16MiB / 1.943GiB     1.62%       53.4MB / 86.6MB     0B / 0B             22
91e1a99d4d30        concourse-web_1          24.25%          32.19MiB / 1.943GiB     1.62%       356MB / 318MB       0B / 0B             13
189cc19df954        concourse-db_1           0.82%           49.21MiB / 1.943GiB     2.47%       249MB / 230MB       0B / 0B             24
47590fe69e09        sonarqube-db_1           0.00%           6.246MiB / 1.943GiB     0.31%       6.57kB / 1.22kB     0B / 0B             7
```
コマンドを実行すると上のように表示され、リアルタイムで更新される。
`ctrl + c`などで終了できる。

各項目の意味は以下
|項目|意味|
|:--|:--|
|CONTAINER ID|コンテナのID|
|NAME|コンテナの名前|
|CPU %|コンテナが使用しているCPU使用率
|MEM USAGE/LIMIT|使用しているメモリ/Dockerに許可されたメモリ（Docker > Setting > Resource）|
|MEM %|コンテナが使用しているメモリ使用率|
|NET I/O|コンテナが送受信したデータ量|
|BLOCK I/O|コンテナがブロックデバイスに読み書きしたデータ量|
|PID|プロセスID|
#### オプション
* 起動中の全コンテナ表示: -a, --all
* フォーマットして表示： --format
    `docker stats --format "{{.Name}} : {{.MemUsage}}"`みたいにフォーマットを指定できる
    `.Container`, `.Name`, `.ID`, `.CPUPerc`, `.MemUsage`, `.NetIO`, `BlockIO`, `.MemPerc`, `.PIDs`を選択できる
    参考：[docker stats | Docker Documentation](https://docs.docker.com/engine/reference/commandline/stats/#formatting)
* 一度だけ表示： --no-stream
    通常はリアルタイムで更新するのをオフにする
* 切り捨て表示しない： --no-trunc
    コンテナIDを切り捨てせずに全表示

### docker demonの自動起動
Dockerデーモンを起動するには以下のコマンド
```cmd
$ sudo systemctl start docker
# 他のディストリビューションでは、次のように実行します
$ sudo service docker start
```
ブート時に自動起動するには以下のコマンドを使用する
```cmd
$ sudo systemctl enable docker
# 他のディストリビューションでは、次のように実行します
$ sudo chkconfig docker on
```