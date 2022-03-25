# 1. ファイル操作関係
- [1. ファイル操作関係](#1-ファイル操作関係)
  - [1.1. 開く](#11-開く)
  - [1.2. 読み込み](#12-読み込み)
  - [1.3. 書き込み](#13-書き込み)
  - [1.4. エンコード指定](#14-エンコード指定)
  - [1.5. csv](#15-csv)
    - [1.5.1. 区切り文字を指定: 引数delimiter](#151-区切り文字を指定-引数delimiter)
    - [1.5.2. 引用符について](#152-引用符について)
    - [1.5.3. ヘッダー](#153-ヘッダー)
    - [1.5.4. 辞書として読み込み](#154-辞書として読み込み)
    - [1.5.5. 書き込み](#155-書き込み)
    - [1.5.6. 辞書での書き込み](#156-辞書での書き込み)


## 1.1. 開く
openメソッドを用いてファイルを開く。インスタンスを作成するパターンと、withを用いるパターンが存在
* インスタンスを作成するパターン
```python
f = open("test.txt", "r")
# なんか処理
f.close()
```
最後に必ずclose()メソッドで閉じる必要がある。書き込みの場合、この閉じたときに適用されるため閉じないと書き変えられない
* withを用いるパターン
```python
with open("test.txt", "r") as f:
    # なんか処理
```
with外に出てしまえば、勝手に閉じられる。ただし、with外で使用する変数に代入する際はwith外で宣言する必要がある。

openの第二引数はモードを指定。モードは以下がある
|値|説明|
|:--|:--|
|r|読み込み（デフォルト）|
|w|書き込み（上書き）|
|a|追記|
|x|排他的書き込み（すでにファイルがあるとエラーに）|
|b|バイナリモード|
|t|テキストモード（デフォルト）|
|+|更新用。r, w, aと一緒に指定する必要がある|

## 1.2. 読み込み
テキストファイルを読み込むメソッドは以下の3種類
例とした以下のようなファイルを読み込むとする
```txt
apple is red.
banana is yellow.
green apple is green.
```

* read
    ファイルの文字列をすべて読み込み、1つの文字列とする
    ```python
    f.read()
    # 'apple is red.\banana is yellow.\ngreen apple is gree.'
    ```
* readline
    ファイルから1行を読み込む
    ```python
    f.readline()
    # 'apple is red.\n'
    f.readline()
    # 'banana is yellow.\n'
    ```
* readlines
    ファイルすべてを読み込み、各行を要素とするリストを返す
    ```python
    f.readlines()
    # ['apple is red.\n','banana is yellow.\n','green apple is green.']
    ```

1行ずつ読み込みたい場合は、以下のようにする
```python
with open("test.txt") as f:
    for line in f:
        print(line)
```

## 1.3. 書き込み
書き込むメソッドは以下の2種類
* write
    printメソッドと似た感じに書き込む。
    ただしprintと違い改行コードはデフォではつかないので、以下のように指定する必要あり。
    ```python
    f.write("this is apple.\n")
    f.write("this is pen.")
    f.write("this is man.")
    # this is apple.
    # this is pen.this is man.
    ```
* writelines
    リストを引数にとり、一気に書き込む。writeと同じように改行コードは挿入されないので自分で指定
    ```python
    lines = ["this is apple.\n", "this is pen.", "this is man."]
    f.writelines(lines)
    # this is apple.
    # this is pen.this is man.
    ```

書き込みモードの時にpassをすると、空のファイルを作成できる
```python
with open("test.txt","w") as f:
    pass
```

openの引数を`'r+'`とすると、読み書き用で開く。このとき`write`や`writelines`をすると先頭から上書きされる（<u>挿入ではない</u>）
ファイルオブジェクトのメソッド`seed`で位置を移動して書き込むことも可能。ただし`seed`は行単位でなく文字単位での指定
```python
# 123456
with open("test.txt", "r+") as f:
    f.seek(3)
    f.write("-")
# 123-56
```
`readlines`とリストの`insert`を使うと途中に挿入できる
```python
# 123456
# three
# four
with open("test.txt") as f:
    l = f.readlines()
l.insert(0, "first\n")
with open("test.txt","w") as f:
    f.writelines(l)
# first
# 123456
# three
# four
```

## 1.4. エンコード指定
読み込みの際のデコード、書き込みの際のエンコードで使われるエンコーディングを`open`の引数`encoding`で指定できる
指定する文字列は大文字小文字、ハイフン`-`アンダースコア`_`を問わない。つまり`UTF-8`でも`utf_8`でもいい
よく使われるのは`cp932`,`euc_jp`,`shft_jis`,`utf-8`
`encoding`のデフォルト値はプラットフォームによって違う。`locale.getpreferrendencoding()`で確認できる。

## 1.5. csv

テキストファイルで読み込んでもいいが、それ用のメソッドも存在
使用するには`csv`をインポートする
以下は読み込みの際の例
```python
with open("test.csv") as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)
# ['11', '12', '13', '14']
# ['21', '22', '23', '24']
# ['31', '32', '33', '34']
with open("test.csv") as f:
    reader = csv.reader(f)
    l = [row for row in reader]
print(l)
# [['11', '12', '13', '14'], ['21', '22', '23', '24'], ['31', '32', '33', '34']]
```
各行の値を要素とするリストが順に返される
デフォルトではすべてstr型で返される。
内包表記で二次元リストとしての取得もできる

### 1.5.1. 区切り文字を指定: 引数delimiter
csv.readerはデフォは","を区切り文字としている。readerの引数`delimiter`でその区切り文字を指定できる
```python
# 以下のような空白区切りのファイルを読み込む
# 11 12 13 14
# 21 22 23 24
# 31 32 33 34
with open("test.txt") as f:
    reader = csv.reader(f, delimiter=" ")
    l = [row for row in reader]]
print(l)
# [['11', '12', '13', '14'], ['21', '22', '23', '24'], ['31', '32', '33', '34']]
```
TSVファイル（タブ区切り）の場合は`delimiter="\t"`とする
書き込みの場合は`csv.writer`の引数で`delimiter`を指定する。

### 1.5.2. 引用符について
要素が引用符（`"`など）で区切られたものもある。
デフォルトだと、以下のように引用符は要素としては含まれない
```python
# 1,2,"3"
# "a,b,c",x,y
with open(csv_file) as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)
# ["1","2","3"]
# ["a,b,c","x","y"]
```

引数`quoting`を`csv.QUOTE_NONE`とすると引用符に対しての処理がなくなる。引用符で囲まれた部分の区切り文字も要素の区切りとして扱われる
```python
with open(csv_file) as f:
    reader = csv.reader(f, quoting=csv.QUOTE_NONE)
    for row in reader:
        print(row)
# ['1','2','"3"']
# ['"a', 'b', 'c"', 'x', 'y']
```
デフォルトでは引用符は`"`。これを変更したい場合は引数`quotechar`で指定できる(`quotechar="'"`など)


### 1.5.3. ヘッダー
見出し行、見出し列について
```txt
,a,b,c,d
ONE,11,12,13,14
TWO,21,22,23,24
THREE,31,32,33,34
```
つまり
||a|b|c|d|
|:--|:--|:--|:--|:--|
|ONE|11|12|13|14|
|TWO|21|22|23|24|
|TREE|31|32|33|34|

特別に処理する方法はない。データ部分だけを取り出したいなら内包表記とスライスを組み合わせるなどの必要がある。
```python
with open(csv_file) as f:
    reader = csv.reader(f)
    l = [row for row in reader]
print([row[1:] for row in l[1:]])
# [['11', '12', '13', '14'], ['21', '22', '23', '24'], ['31', '32', '33', '34']]
```

### 1.5.4. 辞書として読み込み
各行を辞書として読み込める
以下のファイルを読み込む
```txt
a,b,c,d
11,12,13,14
21,22,23,24
31,32,33,34
```
つまり
|a|b|c|d|
|:--|:--|:--|:--|
|11|12|13|14|
|21|22|23|24|
|31|32|33|34|

デフォルトでは1行目の値がフィールド名として扱われ辞書のキーになる
```python
with open(csv_file) as f:
    reader = csv.DictReader(f)
    l = [row for row in reader]
# [OrderedDict([('a', '11'), ('b', '12'), ('c', '13'), ('d', '14')]),
#  OrderedDict([('a', '21'), ('b', '22'), ('c', '23'), ('d', '24')]),
#  OrderedDict([('a', '31'), ('b', '32'), ('c', '33'), ('d', '34')])]
```

1行目にヘッダーがない場合は引数`fieldnames`で指定
```python
# 11,12,13,14
# 21,22,23,24
# 31,32,33,34
with open(csv_file) as f:
    reader = csv.DictReader(f, fieldnames=["a","b","c","d"])
    for row in reader:
        print(row)
# OrderedDict([('a', '11'), ('b', '12'), ('c', '13'), ('d', '14')])
# OrderedDict([('a', '21'), ('b', '22'), ('c', '23'), ('d', '24')])
# OrderedDict([('a', '31'), ('b', '32'), ('c', '33'), ('d', '34')])
```

### 1.5.5. 書き込み
`csv.writer`を使う。

`writerow()`メソッドで1行ずつ書き込む。引数はリスト
また`writerows()`では二次元配列を書き込める
```python
with open(csv_file,"w") as f:
    writer = csv.writer(f)
    writer.writerow([0,1,2])
    writer.writerow(["a","b","c"])
    writer.writerows([[11, 12, 13, 14], [21, 22, 23, 24], [31, 32, 33, 34]])
```

### 1.5.6. 辞書での書き込み
`csv.DictWriter`を使う
使い方はcsv.DictReaderとほぼ一緒