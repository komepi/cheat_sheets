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
|`-`|子要素を取得|
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