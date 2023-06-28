- [redis](#redis)
  - [redis-cli](#redis-cli)
    - [起動](#起動)
    - [基本操作](#基本操作)
      - [通常の取得](#通常の取得)
      - [有効期限(ttl)の取得: ttl, pttl](#有効期限ttlの取得-ttl-pttl)
      - [通常のセット: set, setnx](#通常のセット-set-setnx)
      - [ttlありセット: setex, psetex](#ttlありセット-setex-psetex)
      - [取得と更新を同時に行う: getset](#取得と更新を同時に行う-getset)
      - [値の長さの取得: strlen](#値の長さの取得-strlen)
      - [値の削除: del](#値の削除-del)


# redis

インメモリデータベース
要するにNoSQLデータベースの一つ
キャッシュとかに使える

## redis-cli
redisのコマンド
### 起動
`redis-cli`コマンドでそのアプリケーションに入る
この時、DBが複数ある場合はオプションをつけないと0に入る
`-n`オプションで指定できる
```cmd
$ redis-cli
127.0.0.1:6379> quit
$ redis-cli -n 2
127.0.0.1:6379[2]> 
```

### 基本操作
#### 通常の取得
`get`で値を取得できる
```cmd
> GET key
```
値が存在しないキー、存在しないキーに対しては(nil)が返ってくる
```cmd
//　存在する値
> get mystr1
"abc"

// 存在しない値
> get xxx
(nil)
```
#### 有効期限(ttl)の取得: ttl, pttl
`ttl`, `pttl`で有効期限を取得できる
```cmd
> ttl key
> pttl key
```
`ttl`は秒単位で、`pttl`はミリ秒単位で取得できる
ttlが存在すれば削除されるまでの時間、ttlが存在しなければ-1、キー自体が存在しない場合は-2が返ってくる
```cmd
// ttlが存在するキー
> ttl abc
(integer) 8190

// ttlが存在しないキー
> ttl def
(integer) -1

// 存在しないキー
> ttl ghi
(integer) -2
```
#### 通常のセット: set, setnx
set, setnxで値をセットする
```cmd
> set key value [EX seconds] [PX milliseconds] [NX|XX]
> setnx key value
```
`set`は新しく作ることもできるし、既に存在するキーに対して上書きができる(update)
`setnx`はupdateはできない
```cmd
> set mystr1 "abc"
OK
> get mystr1
"abc"

// このキーは存在するので更新はされない
> setnx mystr1 "def"
(integer) 0
> get mystr1
"abc"

// 新しいキーの登録はできる
> setnx mystr2 "def"
(integer) 1
> get mystr2
"def"
```

#### ttlありセット: setex, psetex
```cmd
> SETEX key seconds value
> PSETEX key milliseconds value
```

`setex`は秒単位、`psetex`はミリ秒単位で有効期限(ttl)をつけて値をセットする

```cmd
// キーに10秒の期限を設定して登録する
> setex mystr1 10 "abc"
OK

// 有効期限を確認します、この時点で残り6秒
> ttl mystr1
(integer) 6
```

#### 取得と更新を同時に行う: getset
```cmd
> getset key value
```
現在の値を取得し、新たな値を登録する
この時表示されるのは変更前の値
```cmd
// 新規の登録
> getset mystr1 "abc"
(nil)
// 値の更新
> getset mystr1 "def"
"abc"
> get mystr1
"def"
```

#### 値の長さの取得: strlen
```cmd
> strlen key
```
値の長さを取得する
この時strlenはバイト数を返すので、日本語だと挙動が少し変わる
```cmd
> set mystr1 "abc"
OK
> strlen mystr1
(integer) 3

// 日本語入力
> set mystr2 "うまい棒A"
OK
// 取得するとシリアライズされた結果になる
> get mystr2
"\x82\xa4\x82\xdc\x82\xa2\x96_A"
> strlen mystr2
(integer) 9
```
#### 値の削除: del
```cmd
> del key
```
`del`コマンドを使用してキーを指定して削除できる
```cmd
> get mystr1
"abc"
> del mystr1
(integer) 1
> get mystr1
(nil)
```