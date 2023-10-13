- [gclog](#gclog)
- [設定ファイル](#設定ファイル)
- [用語](#用語)
- [restapi](#restapi)
  - [クラスタのヘルスチェック](#クラスタのヘルスチェック)
  - [クラスタ内のノードリスト](#クラスタ内のノードリスト)
  - [インデックス](#インデックス)
    - [全インデックスのリスト](#全インデックスのリスト)
    - [作成](#作成)
    - [削除](#削除)
  - [ドキュメント](#ドキュメント)
    - [作成](#作成-1)
    - [確認](#確認)
    - [変更](#変更)
    - [更新](#更新)
    - [削除](#削除-1)
  - [バッチ処理](#バッチ処理)
  - [検索](#検索)
    - [クエリ](#クエリ)
  - [確認形](#確認形)
    - [設定](#設定)
    - [マッピング](#マッピング)
    - [アナライザ](#アナライザ)

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
* curl
```
curl -XGET "localhost:9200/_cat/health?v"
```
* kibana
```
GET /_cat/health?v
```
## クラスタ内のノードリスト
* curl
```
curl -XGET "localhost:9200/_cat/nodes?v"
```
* kibana
```
GET /_cat/nodes?v
```
## インデックス
### 全インデックスのリスト
```
curl -XGET "localhost:9200/_cat/indices?v"
```
エイリアスも取得したい場合は以下
```
curl http://localhost:9200/_aliases?pretty
```
### 作成
```
curl -XPUT "localhost:9200/[index name]?pretty

# customerという名前のインデックスを作成する場合
curl -XPUT "localhost:9200/customer?pretty
```

マッピングと一緒に作成する場合
```
curl -XPUT -H 'Content-Type: application' 'localhost:9200/[index name]?pretty' -d '
{mapping}
`
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
PUTでは指定ドキュメントIDを持つドキュメントを丸ごと置き換える
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
POSTでは指定ドキュメントIDを持つドキュメントの一部フィールドを置き換える

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
例)`curl -XGET "localhost:9200/test_index/_search?pretty"`
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

### クエリ
クエリを使用しての検索は以下のようになる
```
curl -XGET -H 'Content-Type: application/json' 'localhost:9200/[index name]/_search?pretty' -d '
{query}
'
```
基本的は以下
```
{
  "query":{"match_all":{}}
  "from":10,
  "size":2,
  "sort":{<field_name>:{"order":<desc or asc>}}
  "_source":[<field_name>, ...]
  }
}
```
`match_all`は指定したインデックスの全てを検索
`from`は何番目からの結果を表示するかの設定。0から始まり、デフォルトは0
`size`は何個の結果を表示するかの設定（デフォルトは10)
`sort`は検索結果をソートする。フィールド名とdesc(降順)とasc(昇順)
`_source`は検索結果の中で表示するフィールド名をリストで設定する

`match`は`<field_name>`に`<value>`を含むものを全て返す
```
{
  "query":{"match":{<field_name>:<value>}}
}
```

`bool`は、複数のクエリを組み合わせることができる。`bool`内の要素がtrueである項目を返す
この時、`must`,`should`,`must_not`を使用することができる
それぞれ以下のように対応
|es|db|
|:--|:--|
|must|AND|
|should|OR|
|must_not|NOR|
以下のように使用
以下は40歳であり、名前にたけしを含み、アドレスに"lane"か"mill"を含み、"state"が”ID"でない項目を返す
```
{
  "query":{
    "bool":{
      "must":[
        {"match": {"age":"40"}}
        {"match": {"name":"takesi"}}
      ],
      "should":[
        {"match": {"address": "lane"}},
        {"match": {"address": "mill"}}
      ],
      "must_not":[
        {"match": {"state":"ID"}}
      ]
    }
  }
}
```

* `query_string`
参考: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html
文字列を使用して検索する
`query`のみが必須で、それ以外はオプション
`query`に渡された文字列を解析して、検索する。イメージとしてはgoogle検索のときに使う文字列みたいな感じ
`default_operator`は区切られたキーワード間の関係性を指定する。ANDとORの二種類で、デフォルトはOR。`capital of Hungry`がキーワードとして与えられた時、ORの場合は`capital OR of OR Hungry`に、ANDの場合は`capital AND of AND Hungry`になる
`fields`は検索するフィールドをリストで指定する。

```
{
  "query":{
    "query_string":{
      "query":<value>,
      "default_operator":"and",
      "fields":[<field_name>,...]
    }
  }
}
```

* `range`
参考: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html
値を範囲で検索する
`field_name`は必須で、それ以外はオプション
演算子は以下のように対応
|記号|文字列|意味|
|:--|:--|:--|
|>|gt|より大きい|
|>=|gte|以上|
|<|lt|未満|
|<=|lte|以下|
`format`は`date`での比較の際に使用される日付のフォーマット
```
{
  "query":{
    "range":{<field_name>:{"gte":<value_gte>,"lte":<value_lte>}}
  }
}
```
## 確認形
### 設定
```cmd
$ curl -XGET "http://localhost:9200/[index name]/_settings?pretty"
```
### マッピング
```cmd
$ curl -XGET "http://localhost:9200/[index name]/_mapping?pretty"
```

### アナライザ
アナライザがどのように働くかのチェック
```cmd
$ curl -XPOST 'http://localhost:9200/[index_name]/_analyze?pretty' -H "Content-type: application/json" -d '{"text": [test_text],"analyzer":[used_analyzer]}'
```
例)"default"という名前のアナライザを使用して、"テスト"を検証
```cmd
$ curl -XPOST 'http://localhost:9200/test_index/_analyze?pretty' -H "Content-type: application/json" -d '{"text": "テスト","analyzer":"default"}'

{
  "tokens" : [
    {
      "token" : "テ",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "テス",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "テスト",
      "start_offset" : 0,
      "end_offset" : 3,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "ス",
      "start_offset" : 1,
      "end_offset" : 2,
      "type" : "word",
      "position" : 3
    },
    {
      "token" : "スト",
      "start_offset" : 1,
      "end_offset" : 3,
      "type" : "word",
      "position" : 4
    },
    {
      "token" : "ト",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "word",
      "position" : 5
    }
  ]
}
```

アナライザを指定しない場合、"standard"というデフォルトのアナライザが使用される