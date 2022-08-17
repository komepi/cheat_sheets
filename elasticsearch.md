# gclog
場所は`/{elasticsearchのインストール場所}/logs/gc.log

# 設定ファイル
jvmのヒープサイズを変更するには`/{elasticsearchのインストール場所}/config/jvm.options`を編集する必要がある。
このファイルの中でも、以下の個所を編集する
```
################################################################
## IMPORTANT: JVM heap size
################################################################
##
## You should always set the min and max JVM heap
## size to the same value. For example, to set
## the heap to 4 GB, set:
##
## -Xms4g
## -Xmx4g
##
## See https://www.elastic.co/guide/en/elasticsearch/reference/current/heap-size.html
## for more information
##
################################################################

# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms1g
-Xmx1g
```
だいたいファイルの先頭あたりにある。最後２行を変更する。1gbなら`-Xms1g`など、4gbなら`-Xms4g`など、200MBなら`-Xms200m`など

# 用語
それぞれ以下のような位置づけ
|es用語|db用語|
|:--|:--|
|index|DB|
|type|テーブル|
|field|カラム|
|document|レコード|
|mapping|スキーマ定義|
# restapi
基本的には以下の構文
`<REST Verb> /<Index>/<Type>/<ID>`
`REST Verb`は`GET``PUT``DELETE`など
## クラスタのヘルスチェック
```
curl -XGET "localhost:9200/_cat/health?v"
```
## クラスタ内のノードリスト
```
curl -XGET "localhost:9200/_cat/nodes?v
```
## インデックス
### 全インデックスのリスト
```
curl -XGET "localhost:9200/_cat/indices?v"
```
### 作成
```
curl -XPUT "localhost:9200/[index name]?pretty

# customerという名前のインデックスを作成する場合
curl -XPUT "localhost:9200/customer?pretty
```
### 削除
```
curl -XDELETE "localhost:9200/[index name]?pretty"
```

## ドキュメント

### 作成
`id`を指定して作成する場合
以下の構文
```
curl -XPUT "localhost:9200/[index name]/[type name]/[id]?pretty" -d [data] -H "Content-Type:application/json"
```
以下の条件で登録したいときはこのようになる
|項目|値|
|:--|:--|
|index name|costomer|
|type name|external|
|id|1|
|data|{"name": "John Doe"}|
```
curl -XPUT "localhost:9200/customer/external/1?pretty" -H "Content-Type:application" -d '
{
    "name": "John Doe"
}'
```
`id`を指定せずに作成する場合
この場合、ランダムにIDが作成される
```
curl -XPOST "localhost:9200/[index name]/[type name]?pretty" -H "Content-Type:application/json" -d [data]
```
### 確認
`id`を指定して作成する場合
```
curl -XGET "localhost:9200/[index name]/[type name]/[id]?pretty"
```
### 変更
```
curl -XPUT "localhost:9200/[index name]/[type name]/[id]?pretty" -H "Content-Type:application/json" -d [data]
```
すでにその`id`のドキュメントが存在する場合、`data`の内容に置換される。既存する同じ`id`のドキュメントを置換する、という形なので、編集とはまた違う
この時存在しない`id`を指定すると新たに作成される。

### 更新
ドキュメントを更新する
```
curl -XPOST "localhost:9200/[index name]/[type name]/[id]/_update?pretty" -H
"Content-Type:application/json" -d [data]
```
このとき`data`は例えばドキュメントの値を直接変更するときは以下のようになる。
```
{
  "doc":{ [key]:[new value]}
}
```
この時`key`が既に存在するものであれば変更され、存在しない`key`であれば新たに追加される。
また`"doc"`ではなく簡単なスクリプトを使用することもできる。
`GET customer/external/1?pretty`の結果以下のような表示だったとする。
```
{
  "_index" : "customer",
  "_type" : "external",
  "_id" : "1",
  "_version" : 3,
  "_seq_no" : 3,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "Jes Doe",
    "age" : 12
  }
}
```
例えば`age`を5増加させる場合、`data`は以下のように表現できる。
```
{
  "script": "ctx._source.age += 5"
}
```
ここで`ctx._source`は更新される現在のソースドキュメントを示す。

### 削除
```
curl -XDELETE "localhost:9200/customer/external/1?pretty"
```

## バッチ処理
`_bulk`APIを使用することで複数の処理を一括で実行できる。
```
curl -XPOST "localhost:9200/[index name]/[type name]/_bulk?pretty" -----------
"Content-Type:application/json" -d [operations]
```
`operations`は1. 対象ドキュメントと操作, 2. 操作内容で構成する。
例えば`id`が1のドキュメントを作成し、`{"name":"takasi"}`を追加する際は以下のようになる。
```
{"index":{"_id":1}}
{"name":"takasi"}
```
また、以下のように複数の操作を羅列することもできる。
以下は`id`が1のドキュメントを`name`を`tanaka`に変更し、新たに`id`を2として`name`が`sigeru`のドキュメントを作成している。
```
{"update":{"_id":1}}
{"doc":{"name":"tanaka"}}
{"index":{"_id":2}}
{"name":"sigeru"}
```
※以下注意
> この時、「対象ドキュメントと操作」と「操作内容」は1対1でなければならない(操作がdeleteの場合> は内容は不要)。例えば以下はエラーが出る。
> ```
> {"update":{"_id":1}}
> {"doc":{"name":"tanaka"}}
> {"doc":{"age":12}}
> ```
> またそれぞれの括弧(`{}`)は改行で区切られていないといけない
> `{"update":{"_id":1}}{"doc":{"name":"tanaka"}}`はエラーが出る。
> Bulk APIは複数捜査の中の１つがエラーを出してもほかの操作は実行してしまう。

## 検索
```
curl -XGET "localhost:9200/[index name]/_search?[conditions]
```
結果、以下のようなデータを取得できる。
```cmd
{
  "took" : 63,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "failed" : 0
  },
  "hits" : {
    "total" : 1000,
    "max_score" : null,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "account",
      "_id" : "0",
      "sort": [0],
      "_score" : null,
      "_source" : {"account_number":0,"balance":16623,"firstname":"Bradshaw","lastname":"Mckenzie","age":29,"gender":"F","address":"244 Columbus Place","employer":"Euron","email":"bradshawmckenzie@euron.com","city":"Hobucken","state":"CO"}
    }, {
      "_index" : "bank",
      "_type" : "account",
      "_id" : "1",
      "sort": [1],
      "_score" : null,
      "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    }, ...
    ]
  }
}
```
それぞれの項目の意味は以下の通り
* `took`: 検索の実行にかかった時間(ミリ秒)
* `timed_out`: 検索がタイムアウトしたかどうか
* `_shards`: 検索されたシャードの数と検索に成功/失敗したシャードの数
* `hits`: 検索結果
* `hits.total`: 検索基準に一致したドキュメントの数
* `hits.hits`: 検索結果の実際の配列（デフォルトで最初の１０個のドキュメントを表示）
* `hits.sort`: 結果のソートキー（スコアでソートする場合は欠落）

検索条件の渡し方はurlに含ませる方法とJSON形式のリクエストボディを`_search`APIにPOSTする方法がある。