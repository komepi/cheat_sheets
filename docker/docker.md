### \<none>イメージの削除
-fオプションを使うとイメージを抽出できる。
```
docker images -f "dangling=true"
```
この出力を使ってイメージ一括削除
```
docker rmi $(docker images -f "dangling=true" -q)
```

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
