# 1. pythonでよく使いそうなメソッド
- [1. pythonでよく使いそうなメソッド](#1-pythonでよく使いそうなメソッド)
  - [1.1. 正規表現](#11-正規表現)
    - [1.1.1. 正規表現での文字列抽出(re.search, re.findall)](#111-正規表現での文字列抽出research-refindall)
    - [1.1.2. 正規表現での文字列置換(re.sub)](#112-正規表現での文字列置換resub)
  - [1.2. JSONの取り扱い](#12-jsonの取り扱い)
    - [1.2.1. JSONファイル](#121-jsonファイル)
    - [1.2.2. 辞書をJSON形式の文字列として出力](#122-辞書をjson形式の文字列として出力)
  - [1.3. any, all](#13-any-all)
    - [1.3.1. all](#131-all)
    - [1.3.2. any](#132-any)
  - [1.4. コマンドライン引数の制御](#14-コマンドライン引数の制御)
    - [1.4.1. sys.argv](#141-sysargv)
    - [1.4.2. argsparseモジュール](#142-argsparseモジュール)
  - [ディレクトリ関係](#ディレクトリ関係)
    - [親ディレクトリのモジュールインポート](#親ディレクトリのモジュールインポート)
    - [カレントディレクトリ取得](#カレントディレクトリ取得)
  - [pathlib](#pathlib)
    - [Pathオブジェクト生成・処理](#pathオブジェクト生成処理)
    - [パスの存在・種類](#パスの存在種類)
    - [取得](#取得)
      - [ファイル名、ディレクトリ名：name, stem](#ファイル名ディレクトリ名name-stem)
      - [拡張子：suffix](#拡張子suffix)
      - [親ディレクトリ](#親ディレクトリ)
      - [カレントディレクトリ：cwd](#カレントディレクトリcwd)
      - [同じディレクトリの別のファイル：with_name()](#同じディレクトリの別のファイルwith_name)
      - [拡張子を変更したパス：with_suffix](#拡張子を変更したパスwith_suffix)
    - [作成](#作成)
      - [ディレクトリ](#ディレクトリ)
    - [削除](#削除)
      - [ディレクトリ](#ディレクトリ-1)
      - [ファイル](#ファイル)
    - [連結・追加](#連結追加)
    - [絶対パス↔相対パス](#絶対パス相対パス)
    - [その他](#その他)
      - [パスの同一判定：samefile](#パスの同一判定samefile)
      - [str型への変換](#str型への変換)
      - [osモジュールとpathlibの対応](#osモジュールとpathlibの対応)
  - [変換](#変換)
    - [大文字小文字](#大文字小文字)
      - [全ての文字を小文字に: lower](#全ての文字を小文字に-lower)
      - [全ての文字を大文字に: upper](#全ての文字を大文字に-upper)
      - [最初を大文字、他は小文字に：capitalize](#最初を大文字他は小文字にcapitalize)
      - [単語ごとに最初を大文字に、他は小文字に：title](#単語ごとに最初を大文字に他は小文字にtitle)
      - [大文字を小文字に、小文字を大文字に: swapcase](#大文字を小文字に小文字を大文字に-swapcase)
  - [文字列からメソッドを実行](#文字列からメソッドを実行)
    - [関数を変数に代入：getattr](#関数を変数に代入getattr)
    - [文字列から関数を呼び出す：locals, globals](#文字列から関数を呼び出すlocals-globals)
  - [可変長引数:*args, **kwargs](#可変長引数args-kwargs)
    - [*args](#args)
    - [**kwargs](#kwargs)
- [3](#3)

## 1.1. 正規表現
### 1.1.1. 正規表現での文字列抽出(re.search, re.findall)
`search(pattern, str)`で正規表現にマッチするstr内の文字列を検索し、最初にヒットしたものを返す。マッチオブジェクトを返すため、文字列の取得は`group()`メソッドを使う。`findall()`メソッドの場合は、マッチするものがすべてリストで返ってくる。
```python
s = 'date: 2021/03/23, time: 05:30'
p = r'time: (.*)'  # 「time: 」の後ろにある時間だけを抽出したい
m = re.search(p, s)

print(m.group(1))  # 05:30

p = r'date: (.*),'  # 「date: 」の後ろにある日付だけを抽出したい
m = re.search(p, s)

print(m.group(1))  # 2021/03/23
```
この時、`group()`だと`"date: 2021/03/23"`が、`group(1)`だと`"20221/03/23"`が返ってくる。

また`search()`メソッドは見つからない場合Noneを返す。そのためヒットしたかのif文は以下のようにする。

```python
if re.search(pattern, string):
    print("collect")
else:
    print("fault")
```

### 1.1.2. 正規表現での文字列置換(re.sub)
第一引数に正規表現パターン、第二引数に置換先文字列、第三引数に処理対象文字列を指定。

```python
import re
s = 'aaa@xxx.com bbb@yyy.com ccc@zzz.com'

print(re.sub('[a-z]*@', 'ABC@', s))
# ABC@xxx.com ABC@yyy.com ABC@zzz.com
```

第四引数に最大置換回数を指定できる。
```python
print(re.sub('[a-z]*@', 'ABC@', s, 2))
# ABC@xxx.com ABC@yyy.com ccc@zzz.com
```

## 1.2. JSONの取り扱い
### 1.2.1. JSONファイル
* 読み込み
    `json.load()`関数でJSONファイルを辞書型で読み込む
    ```python
    with open('data/test.json', 'r') as f:
        df = json.load(f)
    ```

* 書き込み
    `json.dump()`で辞書をJSONファイルとして保存
    ```python
    d = {'A': {'i': 1, 'j': 2}, 'B': [{'X': 1, 'Y': 10},{'X': 2,'Y': 20}],'C': '    あ'}
    with open('data/test.json', 'w') as f:
        json.dump(d, f, indent=4)
    ```
### 1.2.2. 辞書をJSON形式の文字列として出力
`json.dumps()`関数で出力
```python
d = {'A': {'i': 1, 'j': 2}, 'B': [{'X': 1, 'Y': 10},{'X': 2,'Y': 20}],'C': 'あ'}
sd = json.dumps(d)
```

* 区切り文字を指定：引数separators
    デフォルトではキーと値の間が`:`、要素間が`,`で区切られる
    引数`separators`でそれぞれの区切り文字をタプルで指定でき、空白をなくして切り詰めたり全く違う形にできる。
    ```python
    print(json.dumps(d, separators=(',', ':')))
    # {"A":{"i":1,"j":2},"B":[{"X":1,"Y":10},{"X":2,"Y":20}],"C":"\u3042"}

    print(json.dumps(d, separators=(' / ', '-> ')))
    # {"A"-> {"i"-> 1 / "j"-> 2} / "B"-> [{"X"-> 1 / "Y"-> 10} / {"X"-> 2 / "Y"-> 20}] / "C"-> "\u3042"}
    ```
* インデントを指定：引数indent
    引数`indent`でインデント幅を指定すると要素ごとに改行・インデントされる
    ```python
    print(json.dumps(d, indent=4))
    # {
    #     "A": {
    #         "i": 1,
    #         "j": 2
    #     },
    #     "B": [
    #         {
    #             "X": 1,
    #             "Y": 10
    #         },
    #         {
    #             "X": 2,
    #             "Y": 20
    #         }
    #     ],
    #     "C": "\u3042"
    # }
    ```
    デフォルトは`indent=None`で改行なし、`indent=0`でインデントはなしで改行だけされる

## 1.3. any, all
### 1.3.1. all
すべての要素がTrueであればTrueを返す

### 1.3.2. any
いずれかの要素がTrueであればTrueを返す。
配列内の文字列のどれかが含まれるかどうかの例
```python
lists = ["a","b","c"]
text = "axy"
any(word in text for word in lists)
# True
```

## 1.4. コマンドライン引数の制御
### 1.4.1. sys.argv
`sys`をインポートする
使用するには`sys.argv[n]`のようにする。
この時n=0には実行したファイル名、それ以降はコマンドライン引数が格納されている
```python
# 以下のコマンドで実行したとする
# python test.py sample1 sample2
import sys
print(sys.argv[0]) # test.py
print(sys.argv[1]) # sample1
print(sys.argv[2]) # sample2
```

### 1.4.2. argsparseモジュール

必要な流れは以下
1. argsparseモジュールをインポート
2. argsparse.ArgumentParserクラスのインスタンス作成
3. add_argumentメソッドで解析方法を追加していく
4. parse_argsメソッドで与えられたコマンドライン引数を解析
5. コマンドライン引数の値がparse_argsの戻り値に格納

以下簡単な例
```python
import argparse  # argparseモジュールのインポート

parser = argparse.ArgumentParser()  # ArgumentParserクラスのインスタンスを生成
parser.add_argument('x', type=int)  # コマンドライン引数の解析方法を追加（1）
parser.add_argument('y', type=int)  # コマンドライン引数の解析方法を追加（2）
args = parser.parse_args()  # コマンドライン引数の解析
result = args.x + args.y  # 戻り値に格納された値を使用

print(result)
```

```python
parser = argparse.ArgumentParser(description="test")
parser.add_argument("x",type=int,help="an integer to be added") # 基本の形
parser.add_argument("-y",type=int,default=0) # --option value形式
parser.add_argument("-s","--switch",action="store_true") # --option 形式のフラグ、スイッチ
args.parser.parse_args()

result = args.x+args.y

if args.switch:
    print("result: "+str(result)+" and switch on")
else:
    print("result: "+str(result)+" and switch off")
```
## ディレクトリ関係
### 親ディレクトリのモジュールインポート
### カレントディレクトリ取得
`os.getcwd()`で取得できる
```python
import os
path = os.getcwd()
print(path)
# /Users/mbp/Documents/my-project/python-snippets/notebook
print(type(path))
# <class 'str'>]
```

## pathlib
ファイル・ディレクトリ（フォルダ）のパスをオブジェクトとして捜査する。
以下のようなディレクトリ構造を例とする。
```
temp
├── dir
│   └── sub_dir
│       └── file2.txt
└── file.txt
```

### Pathオブジェクト生成・処理
コンストラクタ`pathlib.Path()`でPathオブジェクトを生成。引数にはパス（絶対パス、相対パス）を文字列で与える。
```python
import pathlib
p_file = pathlib.Path("temp/file.txt")
print(p_file)
# temp/file.txt
print(type(p_file))
# <class 'pathlib.PosifixPath'>
```
PathのクラスはUnix系のOSだと`PosifixPath`、Windowsで実行すると`WindowsPath`になる。

### パスの存在・種類
* 存在
    Pathオブジェクトは存在しないパスを指定しても生成することができる。
    また`exists`メソッドを用いて存在を確認できる。存在するときは`True`を返す。
    また、`touch`メソッドを用いることで存在しないパスから新しいファイルやディレクトリを作成することもできる。
    ```python
    p_file = Path('temp/new_file.txt')
    print(p_file.exists())
    # False
    p_file.touch()
    print(p_file.exists())
    # True
    ```
    また`Path('temp/new_file.txt').touch()`のように一文でもできる

* 種類
    パスがファイル、ディレクトリかどうかの確認にはそれぞれ`is_file`、`is_dir`メソッドを用いる。その種類であるときは`True`が返ってくる。

### 取得
#### ファイル名、ディレクトリ名：name, stem
パス末尾のファイル名（basename）の文字列を取得するには`name`属性を使う。
拡張子なしでの取得には`stem`属性を使う。
またディレクトリを示すPathオブジェクトの場合は、どちらも末尾のディレクトリ名の文字列を返す。
```python
p_file = Path('temp/file.txt')
print(p_file.name)
# file.txt
print(p_file.stem)
# file
p_dir = Path('temp/dir')
print(p_dir.name)
# dir
print(p_dir.stem)
# dir
```

#### 拡張子：suffix
拡張子は`suffix`属性で取得。このときピリオド`.`付き文字列となる。
またディレクトリの場合は空文字列になる。
```python
p_file = Path('temp/file.txt')
print(p_file.suffix)
# .txt
p_dir = Path('temp/dir')
print(p_dir.suffix)
# 
```
ピリオドが必要ない場合は`lstrip('.')`で削除するか、スライスで2文字目意向を取得(`p_file.suffix[1:]`)
#### 親ディレクトリ
パスに対して相対パス`..`を連結すると親ディレクトリへの移動になる。
```python
path = Path('temp/dir').joinpath('..','file.txt')
print(path)
# temp/dir/../file.txt
```
`parent`属性を使うことでも移動可能
```python
print(p_dir)
# temp/dir
print(p_dir.parent)
# temp
print(p_dir.parent.joinpath('file.txt'))
# temp/file.txt
```

#### カレントディレクトリ：cwd
pythonが実行されている作業ディレクトリ（カレントディレクトリ）は`cwd`メソッドで取得できる。
```python
print(Path.cwd())
# /Users/mbp/Documents/my-project/python-snippets/notebook
```
#### 同じディレクトリの別のファイル：with_name()
`with_name`を使うと、現在のパスの`name`属性を変更したPathオブジェクトが返される。引数に新たな`name`属性を指定。
これはファイルを対象としたPathオブジェクトでもディレクトリを対象としたPathオブジェクトでも同じ。
また、`with_name`は`name`属性の変更であるため、ディレクトリを対象としたPathオブジェクトに新たなファイル名を指定することもできる。
```python
p_file = Path('temp/file.txt')
print(p_file.with_name('new_file.txt'))
# temp/new_file.txt
p_dir = Path('temp/dir')
print(p_dir.with_name('new_dir'))
# temp/new_dir
print(p_dir.with_name('new_file.txt'))
# temp/new_file.txt
```

#### 拡張子を変更したパス：with_suffix
元のパスの`suffix`属性を変更したPathオブジェクトを返す。引数で新たな拡張子を文字列で指定する。
この時、先頭にピリオドがないとエラーを返す。
```python
p_file = Path('temp/file.txt')
print(p_file.with_suffix('.csv'))
# temp/file.csv
print(p_file.with_suffix('csv'))
# ValueError: Invalid suffix 'csv'
```

### 作成
#### ディレクトリ
Pathオブジェクトの`mkdir`メソッドで作成可能
```python
p = pathlib.Path('temp')
p.mkdir()
print(p.exists())
# True
print(p.is_dir())
# True
```
引数`parents`を`True`にすると、Pathオブジェクトの中間ディレクトリも作成できる。
デフォルトではエラーとなる
```python
p = pathlib.Path('temp/dir/sub_dir/sub_dir2').mkdir(parents=True)
```

引数`exist_ok`では、すでに存在しているディレクトリを対象としていてもエラーにならない。デフォルトではエラーになる。
```python
p=Path('temp/dir')
print(p.exists())
# True
p.mkdir(exist_ok=True)
```

### 削除
#### ディレクトリ
ディレクトリの削除には`rmdir`メソッドを使う。
```python
p=Path('temp/dir')
p.rmdir()
```
#### ファイル
ファイルの削除には`unlink`メソッドを使う
第一引数にTrueを指定すると、対象が存在しなくてもエラーを出さない
```python
p=Path('temp/dir/test.txt')
p.unlink(missing_ok=True)
```

### 連結・追加
Pathオブジェクトに対して`/`演算子を使ってパスを追加できる。
また、`joinpath`メソッドも同様の動きをする。複数連結する場合は引数に複数渡す。
```python
path = Path('temp/dir')
# 以下は同じ処理
print(path / 'sub_dir' / 'file2.txt')
print(path.joinpath('sub_dir', 'file2.txt'))
```

### 絶対パス↔相対パス
* 相対パス→絶対パス
    `resolve`メソッドを使用
    ```python
    path = Path('temp/dir').joinpath('..','file.txt')
    # temp/dir/../file.txt
    print(path.resolve())
    # /Users/mbp/Documents/my-project/python-snippets/notebook/temp/file.txt
    ```
    またこの時resolveが返すのはPathオブジェクト（str型ではない）
* 絶対パス→相対パス
    `relative_to`メソッドで変換。引数に指定したパスを基準とする相対パスを返す
    ```python
    p_file_join = Path('tmep/dir').joinpath('..','file.txt')
    p_resolve = p_file_join.resolve()
    # /Users/mbp/Documents/my-project/python-snippets/notebook/temp/file.txt
    p_cwd = Path.cwd()
    # /Users/mbp/Documents/my-project/python-snippets/notebook
    print(p_resolve.relative_to())
    # temp/file.txt

### その他

#### パスの同一判定：samefile
パスが参照するファイルが同一かどうかを判定。相対パスと絶対パスなどを判定する。この場合は`==`演算子では一致しない
```python
print(p_file1)
# temp/file.txt
print(p_file2)
# temp/dir/../file.txt
print(p_file1.samefile(p_file2))
# True
print(p_file1 == p_file2)
# False
```
#### str型への変換
Path型のオブジェクトを`print`で出力すると文字列が表示されるが、あくまで型は`Path`で`str`ではない
文字列に変換したい場合は`str()`を用いる。
```python
path = Path('temp/file.txt')
print(type(path))
# <class 'pathlib.PosifixPath'>
print(type(str(path)))
# <class 'str'>
```
なお、nameでファイル名を文字列で取得したりsuffix属性で拡張子を文字列で取得もできる。

#### osモジュールとpathlibの対応
|処理内容|osおよびos.path|pathlib|
|:--|:--|:--|
|カレントディレクトリ取得|`os.getcwd()`|`Path.cwd()`|
|先頭の`~`をホームディレクトリに置換|`os.path.expanduser()`|`Path.expanduser()`, `Path.home()`|
|パスの存在確認|`os.path.exists()`|`Path.exists()`|
|ディレクトリ判定|`os.path.isdir()`|`Path.is_dir()`|
|ファイル判定|`os.path.isfile()`|`Path.is_file()`|
|シンボリックリンク判定|`os.path.islink()`|`Path.is_symlink()`|
|絶対パス判定|`os.path.isabs()`|`PurePath.is_absolute()`|
|絶対パスに変換|`os.path.abspath()`|`Path.resolve()`|
|ステータスを取得|`os.stat()`|`Path.stat()`<br>`Path.owner()`<br>`Path.group()`|
|パス連結|`os.path.join()`|`PurePath.joinpath()`|
|ファイル名取得|`os.path.basename()`|`PurePath.name`|
|親ディレクトリ取得|`os.path.dirname()`|`PurePath.parent`|
|拡張子分割・取得|`os.path.splitext()`|`PurePath.suffix`

## 変換
### 大文字小文字
#### 全ての文字を小文字に: lower
`str.lower()`ですべての文字を小文字に変換する。
```python
word = "Hello"
print(word.lower())
# hello
```
#### 全ての文字を大文字に: upper
`str.upper()`ですべての文字を大文字に変換する。
```python
word = "Hello"
print(word.upper())
# HELLO
```
#### 最初を大文字、他は小文字に：capitalize
`str.capitalize()`で先頭を大文字に、それ以降を小文字に変換できる。
```python
word = "hello python"
print(word.capitalize())
# Hello python
```
#### 単語ごとに最初を大文字に、他は小文字に：title
`str.title()`で文字列に含まれる単語ごとに最初の文字を大文字に、それ以外の文字を小文字に変換する。
```python
word = "hello python"
print(word.title())
# Hello Python
```
#### 大文字を小文字に、小文字を大文字に: swapcase
`str.swapcase()`で大文字を小文字に、小文字を大文字に変換する。
```python
word = "Hello"
print(word.swapcase())
# hELLO
```

## 文字列からメソッドを実行
### 関数を変数に代入：getattr
オブジェクト、もしくはモジュールから属性の値を返す。第一引数にオブジェクトもしくはモジュールの名前、第二引数には属性の名前を含む文字列の値を渡す。
いかのUserクラスがあるとする。
```python
# Filename: user.py
class User():
  name = 'John'
  age = 33
  def doSomething():
    print(name + ' did something.')
```
このdoSomethingを変数に入れ、それを用いて実行する。
```python
from user import User
doSomething = getattr(user, 'doSomething')

doSomething(user)
# John did something.
```

### 文字列から関数を呼び出す：locals, globals
組み込み関数の`locals`と`globals`を用いて文字列からメソッドを実行できる。これらの関数はソースコードの現在のシンボルテーブルを表すpython辞書を返す。
このとき`locals`はローカル変数を含む辞書を返し、`globals`はローカル変数を含む辞書を返す。関数名も文字列の形式で返される。
```python
def myFunc(args):
  print('This is '+args+' function.')

def myFunc2(args):
  print('This is another '+args+'function.')

locals()['myFunc']('plus')
# This is plus function.
globals()['myFunc2']('minus')
# This is minus function.
```

## 可変長引数:*args, **kwargs
引数に`*`と`**`を付けることで、可変長引数を指定できる。この時変数名はargsやkwargsでなくてもいい（大事なのはアスタリスク）
それぞれ以下の意味を持つ
* `*args`:複数の引数をタプルで受け取る。
* `**kwargs`:複数のキーワード引数を辞書で受け取る。

### *args
以下に例
```python
def func_args(arg1, arg2, *args):
  print('arg1: ', arg1)
  print('arg2: ', arg2)
  print('args: ', args)
  print('type: ', type(args))
  print('sum: ' sum(args))

func_args(0, 1, 2, 3, 4)
# arg1: 0
# arg2: 1
# args: (2, 3, 4)
# type: <class 'tuple'>
# sum: 9
func_args(0, 1)
# arg1: 0
# arg2: 1
# args: ()
# type: <class 'tuple'>
# sum: 0
```

以下のように`*`が付いた引数の後ろで定義されているものはキーワード形式`引数名=値`で指定する。
```python
def func_args2(arg1, *args, arg2):
    print('arg1: ', arg1)
    print('arg2: ', arg2)
    print('args: ', args)

# func_args2(0, 1, 2, 3, 4)
# TypeError: func_args2() missing 1 required keyword-only argument: 'arg2'

func_args2(0, 1, 2, 3, arg2=4)
# arg1:  0
# arg2:  4
# args:  (1, 2, 3)
```

### **kwargs
`**kwargs`のように`**`を付けた引数を定義すると任意の数のキーワード引数を指定できる。
この時関数の中では引数名がキー`key`、値が`value`となる辞書として受け取られる。
```python
def func_kwargs_positional(arg1, arg2, **kwargs):
    print('arg1: ', arg1)
    print('arg2: ', arg2)
    print('kwargs: ', kwargs)

func_kwargs_positional(0, 1, key1=1)
# arg1:  0
# arg2:  1
# kwargs:  {'key1': 1}
```
関数呼び出し時に辞書型オブジェクトに`**`を付けて渡すことができる。
```python
d = {'key1': 1, 'key2': 2, 'arg1': 100, 'arg2': 200}

func_kwargs_positional(**d)
# arg1:  100
# arg2:  200
# kwargs:  {'key1': 1, 'key2': 2}
```
メソッドで`**`付き引数を定義しなくても渡すときに`**`を付けて渡すこともできる
```python
def func_test(a, b):
  return a + b

d = {"a": 1, "b": 2}
func_test(**d)
# 3