- [postgresql](#postgresql)
  - [テーブル作成](#テーブル作成)
  - [テーブル一覧取得](#テーブル一覧取得)
  - [カラム一覧取得](#カラム一覧取得)
  - [レコード挿入](#レコード挿入)
  - [レコード削除](#レコード削除)
  - [最初の数件だけ表示](#最初の数件だけ表示)
  - [ソートして表示](#ソートして表示)
  - [jsonの使用](#jsonの使用)
    - [値の取得](#値の取得)
    - [キーの存在確認](#キーの存在確認)
    - [JSON関数](#json関数)
      - [json配列の長さを返す: json\_array\_length](#json配列の長さを返す-json_array_length)
      - [key/valueの組み合わせの集合: json\_each](#keyvalueの組み合わせの集合-json_each)
      - [キーの集合を返す: json\_object\_keys](#キーの集合を返す-json_object_keys)
      - [値の集合を返す](#値の集合を返す)
      - [JSON配列をレコード分割して返す: json\_array\_elements](#json配列をレコード分割して返す-json_array_elements)

# postgresql

## テーブル作成
```
create table [table name] ([dataname datatype] ...);
```
idを自動で連番を振るようにしたいときは、以下のようにする
```
create table test_table (id serial);
```

## テーブル一覧取得
```
\dt
```

## カラム一覧取得
```
\d テーブル名
```
## レコード挿入
```sql
INSERT INTO table_name [(column_name [,...])] VALUES (value [,...])
```
例えば以下のように
```sql
INSERT INTO test_table (col1, col2) VALUES ('test1', 'test2');
```
カラム名の箇所は省略可
複数のデータを一度に登録する場合は、VALUESの後にカンマ区切りで追加すればいい
```sql
INSERT INTO test_table (col1, col2) VALUES 
('test11', 'test12'),
('test21', 'test22'),
('test31', 'test32');
```

## レコード削除
```sql
DELETE FROM table_name (WHERE 条件)
```
WHEREの条件文に合致するレコードが消去される。WHERE文は省略可で、省略すると全て削除

## レコードの更新
```sql
UPDATE table_name SET column=new_value (WHERE 条件)
```
## 最初の数件だけ表示
```sql
SELECT * FROM table_name LIMIT 1;
```

## ソートして表示
column_nameをソートに使用するフィールド名とする
* 昇順でソート
  ```sql
  SELECT * FROM table_name ORDER BY column_name;
  ```
* 降順でソート
  ```sql
  SELECT * FROM table_name ORDER BY column_name DESC;
  ```

## jsonの使用
データをjsonとして入力したり、jsonのカラムのデータのうち特定のキーに対応する値を使用することができる
サンプルとして以下のようにテーブルとデータを作成
```sql
CREATE TABLE sample_json (id serial,value jsonb);
INSERT INTO sample_json (value) VALUES ('{"a":1,"b":{"c":[1,2,3]},"d":null,"e":true,"f":"hello"}'::jsonb);
```
### 値の取得
`-`・`#`・`>`・`>>`の組み合わせで指定
|記号|意味|
|:--|:--|
|`-`|子|
|`#`|パスを指定して取得|
|`>`|JSONオブジェクトとして取得|
|`>>`|テキストとして取得|

なので例えば「valueのキーbのさらにキーcの値」を取得の場合は以下のようになる
```sql
SELECT json->'b'->'c' FROM sample_json;
```
また配列から値を取得する際は、キーと同様にインデックスで指定できる。この時インデックスは0から始まる
```sql
SELECT value->'b'->'c'->0 FROM sample_json;
```
`->>`は指定したキーの値をテキストで返す
そのため、例えばWEHEREなどで数字と比較したいときはキャストする必要がある
```sql
SELECT value->>'a' FROM sample_json WHERE (value->>'a')::INT=1;
```

パスを指定する場合は、以下のようにパスを指定する
なので例えば「valueのキーbのさらにキーcの値、そのリストの2つ目の値」を取得の場合は以下のようになる
```sql
SELECT value#>>'{b,c,1}' FROM sample_json;
```

### キーの存在確認
`?`演算子により可能
```sql
SELECT value FROM sample_json WHERE value?'b';
```
しかし以下の場合はトップレベルに"c"キーがないためヒットしない
```sql
SELECT value FROM sample_json WHERE value?'c';
```
ネスト先にある要素を使用するには`->`と組み合わせる
```sql
SELECT value FROM sample_json WHERE value->'b'?'c';
```
### 指定したパスの値のみを変更
これはjson型ではできず、jsonb型はできる
使用するのはjsonb_set
第一引数に変更対象、第二引数にパス、第三引数に変更後の値を渡す
第四引数はオプションで、指定したパスが存在しない場合新たに作成するか否かを指定。Trueだと作成し、デフォルトはTrue
```sql
root=# insert into test_jsonb (json) values ('{"a":[{"c":1,"d":2},{"e":3,"f":4}],"b":1}'::jsonb);
root=# update test_jsonb set json=jsonb_set(json,'{a,1,e}','"new value"') where id=1;
root=# select * from test_jsonb;
 id |                             json                              
----+---------------------------------------------------------------
  1 | {"a": [{"c": 1, "d": 2}, {"e": "new value", "f": 4}], "b": 1}
root=# update test_jsonb set json=jsonb_set(json,'{a,1,g}','"new value"',False) where id=1;
root=# select * from test_jsonb;
 id |                             json                              
----+---------------------------------------------------------------
  1 | {"a": [{"c": 1, "d": 2}, {"e": "new value", "f": 4}], "b": 1}
root=# update test_jsonb set json=jsonb_set(json,'{a,1,g}','"new value"') where id=1;
root=# select * from test_jsonb;
 id |                                      json                                       
----+---------------------------------------------------------------------------------
  1 | {"a": [{"c": 1, "d": 2}, {"e": "new value", "f": 4, "g": "new value"}], "b": 1}
```

### JSON関数
json型を扱う場合は`json_xxx`、jsonb型の場合は`jsonb_xxx`という関数名になる。基本的な使用方法は同じ
#### json配列の長さを返す: json_array_length
```sql
SELECT jsonb_array_length(json->'b'->'c') FROM sample_json;
```
なおこの時arrayじゃなくdictを渡すとエラーになる
#### key/valueの組み合わせの集合: json_each
record型で返しているらしい
```sql
SELECT jsonb_each(value) FROM sample_json;
```
この場合結果は以下のようになる
```cmd
root=# SELECT jsonb_each(value) FROM sample_json;
        jsonb_each        
--------------------------
 (a,1)
 (b,"{""c"": [1, 2, 3]}")
 (d,null)
 (e,true)
 (f,"""hello""")
(5 rows)
```
以下のようにするとjson型に変換できるらしい
```sql
WITH records as (
  SELECT json_each('{"age": 9, "name": "二郎", "metas": ["hoge", "fuga"]}'::json) AS record
)
SELECT
  row_to_json(record)->'key' AS key
  , row_to_json(record)->'value' AS value
FROM
  records
;
```
結果以下のようになる
```
   key   |      value       
---------+------------------
 "age"   | 9
 "name"  | "二郎"
 "metas" | ["hoge", "fuga"]
(3 rows)
```

#### キーの集合を返す: json_object_keys
```sql
SELECT jsonb_object_keys(value) FROM sample_json;
```

#### 値の集合を返す
`json_object_values`は存在しない。
`json_each`でrecord型の集合に変換して、`row_to_json`でjson型に戻して`->'value'`で値だけ取得しないといけない
```sql
SELECT row_to_json(jsonb_each(value))->'value' FROM sample_json;
```

#### JSON配列をレコード分割して返す: json_array_elements
```sql
SELECT jsonb_array_elements(value->'b'->'c') FROM sample_json;
```
結果以下のようになる
```
root=# SELECT jsonb_array_elements(value->'b'->'c') FROM sample_json;
 jsonb_array_elements 
----------------------
 1
 2
 3
(3 rows)
```
この時array以外を渡すとエラー