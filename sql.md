# postgresql

## テーブル作成
```
create table [table name] ([dataname datatype] ...);
```
idを自動で連番を振るようにしたいときは、以下のようにする
```
create table test_table (id serial);
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

