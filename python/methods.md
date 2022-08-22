- [1. 正規表現](#1-正規表現)
  - [1.1. 正規表現での文字列抽出(re.search, re.findall)](#11-正規表現での文字列抽出research-refindall)
  - [1.2. 正規表現での文字列置換(re.sub)](#12-正規表現での文字列置換resub)
- [2. JSONの取り扱い](#2-jsonの取り扱い)
  - [2.1. JSONファイル](#21-jsonファイル)
  - [2.2. 辞書をJSON形式の文字列として出力](#22-辞書をjson形式の文字列として出力)
- [3. any, all](#3-any-all)
  - [3.1. all](#31-all)
  - [3.2. any](#32-any)
- [4. コマンドライン引数の制御](#4-コマンドライン引数の制御)
  - [4.1. sys.argv](#41-sysargv)
  - [4.2. argsparseモジュール](#42-argsparseモジュール)
    - [4.2.1. 基本](#421-基本)
    - [4.2.2. ヘルプの作り方](#422-ヘルプの作り方)
    - [4.2.3. 引数の設定](#423-引数の設定)
      - [4.2.3.1. 基本](#4231-基本)
        - [4.2.3.1.1. 必須引数](#42311-必須引数)
        - [4.2.3.1.2. オプション引数](#42312-オプション引数)
      - [4.2.3.2. 応用](#4232-応用)
        - [4.2.3.2.1. デフォルト値](#42321-デフォルト値)
        - [4.2.3.2.2. 型指定](#42322-型指定)
        - [4.2.3.2.3. フラグ](#42323-フラグ)
        - [4.2.3.2.4. 選択](#42324-選択)
        - [4.2.3.2.5. 複数個の受け取り](#42325-複数個の受け取り)
        - [4.2.3.2.6. 必須のオプション](#42326-必須のオプション)
- [5. ディレクトリ関係](#5-ディレクトリ関係)
  - [5.1. 親ディレクトリのモジュールインポート](#51-親ディレクトリのモジュールインポート)
    - [5.1.1. パッケージ内で別ディレクトリから](#511-パッケージ内で別ディレクトリから)
    - [5.1.2. 別ディレクトリからインポート](#512-別ディレクトリからインポート)
  - [5.2. カレントディレクトリ取得](#52-カレントディレクトリ取得)
- [6. pathlib](#6-pathlib)
  - [6.1. Pathオブジェクト生成・処理](#61-pathオブジェクト生成処理)
  - [6.2. パスの存在・種類](#62-パスの存在種類)
  - [6.3. 取得](#63-取得)
    - [6.3.1. ファイル名、ディレクトリ名：name, stem](#631-ファイル名ディレクトリ名name-stem)
    - [6.3.2. 拡張子：suffix](#632-拡張子suffix)
    - [6.3.3. 親ディレクトリ](#633-親ディレクトリ)
    - [6.3.4. カレントディレクトリ：cwd](#634-カレントディレクトリcwd)
    - [6.3.5. 同じディレクトリの別のファイル：with_name()](#635-同じディレクトリの別のファイルwith_name)
    - [6.3.6. 拡張子を変更したパス：with_suffix](#636-拡張子を変更したパスwith_suffix)
  - [6.4. 作成](#64-作成)
    - [6.4.1. ディレクトリ](#641-ディレクトリ)
  - [6.5. 削除](#65-削除)
    - [6.5.1. ディレクトリ](#651-ディレクトリ)
    - [6.5.2. ファイル](#652-ファイル)
  - [6.6. 連結・追加](#66-連結追加)
  - [6.7. 絶対パス↔相対パス](#67-絶対パス相対パス)
  - [6.8. その他](#68-その他)
    - [6.8.1. パスの同一判定：samefile](#681-パスの同一判定samefile)
    - [6.8.2. str型への変換](#682-str型への変換)
    - [6.8.3. osモジュールとpathlibの対応](#683-osモジュールとpathlibの対応)
- [7. 変換](#7-変換)
  - [7.1. 大文字小文字](#71-大文字小文字)
    - [7.1.1. 全ての文字を小文字に: lower](#711-全ての文字を小文字に-lower)
    - [7.1.2. 全ての文字を大文字に: upper](#712-全ての文字を大文字に-upper)
    - [7.1.3. 最初を大文字、他は小文字に：capitalize](#713-最初を大文字他は小文字にcapitalize)
    - [7.1.4. 単語ごとに最初を大文字に、他は小文字に：title](#714-単語ごとに最初を大文字に他は小文字にtitle)
    - [7.1.5. 大文字を小文字に、小文字を大文字に: swapcase](#715-大文字を小文字に小文字を大文字に-swapcase)
- [8. 文字列操作](#8-文字列操作)
  - [8.1. 置換](#81-置換)
    - [8.1.1. 文字列を指定して置換：replace](#811-文字列を指定して置換replace)
- [9. 文字列からメソッドを実行](#9-文字列からメソッドを実行)
  - [9.1. 関数を変数に代入：getattr](#91-関数を変数に代入getattr)
  - [9.2. 文字列から関数を呼び出す：locals, globals](#92-文字列から関数を呼び出すlocals-globals)
- [10. 可変長引数:*args, **kwargs](#10-可変長引数args-kwargs)
  - [10.1. *args](#101-args)
  - [10.2. **kwargs](#102-kwargs)
- [11. ラムダ式: lambda](#11-ラムダ式-lambda)
  - [11.1. def との対応](#111-def-との対応)
  - [11.2. if文](#112-if文)
  - [11.3. 名前について](#113-名前について)
- [12. 日時について：datetime](#12-日時についてdatetime)
  - [12.1. datetimeオブジェクトと現在情報：now](#121-datetimeオブジェクトと現在情報now)
    - [12.1.1. datetime ⇒ date](#1211-datetime--date)
  - [12.2. dateオブジェクトと現在日時：today](#122-dateオブジェクトと現在日時today)
  - [12.3. timeオブジェクト](#123-timeオブジェクト)
  - [12.4. 経過日時や時間差：timedelta](#124-経過日時や時間差timedelta)
    - [12.4.1. オブジェクトの作成](#1241-オブジェクトの作成)
    - [12.4.2. 引き算、足し算](#1242-引き算足し算)
  - [12.5. 文字列に変換：strftime, 文字列から変換：strptime](#125-文字列に変換strftime-文字列から変換strptime)
    - [12.5.1. 文字列に変換：strftime](#1251-文字列に変換strftime)
- [13. pickle](#13-pickle)
  - [13.1. pickle化対象](#131-pickle化対象)
  - [13.2. シリアライズとデシリアライズ](#132-シリアライズとデシリアライズ)
    - [13.2.1. 通常オブジェクト](#1321-通常オブジェクト)
    - [13.2.2. クラスのインスタンス](#1322-クラスのインスタンス)
    - [13.2.3. クラスや関数](#1323-クラスや関数)
  - [13.3. pickleと_pickle](#133-pickleと_pickle)
  - [13.4. deepcopyとの対応](#134-deepcopyとの対応)
- [14. 名前でアクセスできるtuple: namedtuple](#14-名前でアクセスできるtuple-namedtuple)
- [15. schema定義： marshmallow](#15-schema定義-marshmallow)
  - [15.1. model定義](#151-model定義)
  - [15.2. schema定義](#152-schema定義)
    - [15.2.1. Metaクラス](#1521-metaクラス)
    - [15.2.2. fields](#1522-fields)
  - [15.3. 読み込み/書き込み/検証](#153-読み込み書き込み検証)
    - [15.3.1. シリアル化： dump](#1531-シリアル化-dump)
    - [15.3.2. 読み込み](#1532-読み込み)
    - [15.3.3. 検証](#1533-検証)
    - [15.3.4. その他](#1534-その他)
  - [15.4. 検証](#154-検証)
  - [15.5. フィールドのカスタム](#155-フィールドのカスタム)
- [16. その他組み込み関数](#16-その他組み込み関数)
  - [16.1. 型判定: isinstance](#161-型判定-isinstance)

# 1. 正規表現
## 1.1. 正規表現での文字列抽出(re.search, re.findall)
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

## 1.2. 正規表現での文字列置換(re.sub)
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

# 2. JSONの取り扱い
## 2.1. JSONファイル
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
## 2.2. 辞書をJSON形式の文字列として出力
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

# 3. any, all
## 3.1. all
すべての要素がTrueであればTrueを返す

## 3.2. any
いずれかの要素がTrueであればTrueを返す。
配列内の文字列のどれかが含まれるかどうかの例
```python
lists = ["a","b","c"]
text = "axy"
any(word in text for word in lists)
# True
```

# 4. コマンドライン引数の制御
## 4.1. sys.argv
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

## 4.2. argsparseモジュール
参考：[ArgumentParserの使い方を簡単にまとめた - Qiita](https://qiita.com/kzkadc/items/e4fc7bc9c003de1eb6d0#%E5%9F%BA%E6%9C%AC%E5%BD%A2)
公式ドキュメント：[16.4. argparse --- コマンドラインオプション、引数、サブコマンドのパーサー — Python 3.6.15 ドキュメント](https://docs.python.org/ja/3.6/library/argparse.html)
### 4.2.1. 基本
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
使用する際は例の`args.x`のように、コマンドライン引数の解析の変数に引数名でアクセスする。

### 4.2.2. ヘルプの作り方
通常のコマンドと同じように`python {プログラム名} -h`でコマンドライン引数のヘルプを表示できる。
ヘルプに追加できるのは、プログラムの説明と、それぞれの引数の説明。プログラムの説明はparserを作成するときに引数として渡し、引数の説明はそれぞれの解析方法を追加するときに引数として`help`を渡す。
以下に例
```python
# test.py
import argparse
parser = argparse.ArgumentParser(description='これはプログラムの説明です。')

parser.add_argument('arg1', help='一つ目の引数')
parser.add_argument('arg2')
parser.add_argument('--arg3')
parser.add_argument('--arg4', help='オプション引数です')

args = parser.parse_args()
```
これを`python test.py -h`で実行すると以下が表示される。
```
$ python test.py -h
usage: arg_test.py [-h] [--arg3 ARG3] [--arg4 ARG4] arg1 arg2

これはプログラムの説明です。

positional arguments:
  arg1         一つ目の引数
  arg2

optional arguments:
  -h, --help   show this help message and exit
  --arg3 ARG3
  --arg4 ARG4  オプション引数です
```

### 4.2.3. 引数の設定
#### 4.2.3.1. 基本
##### 4.2.3.1.1. 必須引数
`add_argument`で引数名を`args`のように通常の文字列にすると必須引数になる。
この引数を指定し忘れるとエラーになる。
##### 4.2.3.1.2. オプション引数
`add_argument`で引数名を`--args`のようにハイフン('`-`')を1～2個つけるとオプション引数として登録される。
この場合、引数を指定しなくてもエラーは発生しない。この時、その値は`None`になる。また指定する順番も自由
また、正式名称と略称をそれぞれ以下のように設定することができる。
```python
parser.add_argument('-i','--input')
```
実行の際は`-i`でも`--input`でも指定できるが、プログラム内で取得する際は正式名称(`input`)でしか取得できない。
#### 4.2.3.2. 応用
##### 4.2.3.2.1. デフォルト値
オプションでは指定されない場合は`None`となるが、以下のようにしてデフォルト値を設定することが可能
```python
parser.add_argument('--message', default='hello!')
```

##### 4.2.3.2.2. 型指定
コマンドライン引数で指定された値はデフォルトでは文字列型。以下のように指定することができる
```python
parser.add_argument('--number', type=int)
```
これは`type`に指定された型じゃないとエラーが出るんじゃなくて、`type`に渡されたもので変換する、といった処理なので、この`type`に好きなメソッドなどをわたすことでいろいろできる。
ピリオド(`'.'`)区切りの数値をリストで受け取る例
```python
tp = lambda x:list(map(int, x.split('.')))
parser.add_argument('--addres', type=tp, help='IP Adress')
```

##### 4.2.3.2.3. フラグ
`add_argument`に`action`を指定するとフラグになる
```python
parser.add_argument('--flg', action='store_true')
```
この時、実行時に`--flg`が指定されていると`True`、指定されていないと`False`になる。
また`store_true`ではなく`store_false`にすると挙動が逆になり、実行時に`--flg`が指定されていると`False`、されていないと`True`になる。

##### 4.2.3.2.4. 選択
`add_argument`で`choices`を指定すると、`choices`に渡した値の中にあるもののみに限定でき、それ以外を指定するとエラーになる。
また選択肢は`--help`で確認できる
```python
parser.add_argument('--fruit', choices=['apple','orange','banana'])
```

##### 4.2.3.2.5. 複数個の受け取り
`nargs='*'`を指定することで、可変長で複数個をリストで受け取ることができる。また`'*'`の代わりに整数を指定すると、固定長にもできる。
```python
parser.add_argument('--colors',nargs='*')
args = parser.parse_args()
print(args.colors)
```
```cmd
$ python test.py --colors red green blue
['red', 'green', 'blue']
```
##### 4.2.3.2.6. 必須のオプション
`required=True`を指定することで、オプション引数を必須にすることができる。
```python
parser.add_argument('-a',required=True)`
```

# 5. ディレクトリ関係
## 5.1. 親ディレクトリのモジュールインポート
参考：[Pythonの相対インポートで上位ディレクトリ・サブディレクトリを指定 | note.nkmk.me](https://note.nkmk.me/python-relative-import/)
### 5.1.1. パッケージ内で別ディレクトリから
以下のようなディレクトリ構造を例とする。
```
my_package/
├── __init__.py
├── mod1.py
├── mod2.py
├── sub_package1
│   ├── __init__.py
│   └── sub_mod1.py
└── sub_package2
    ├── __init__.py
    └── sub_mod2.py
```
1. mod1.py ← mod2.py
  ```python
  # mod1.py
  from . import mod2
  ```
2. mod2.py ← sub_package1/sub_mod1.py
  ```python
  # mod2.py
  from .sub_package1 import sub_mod1
  ```
3. sub_package2/sub_mod2.py ← mod1.py
  ```python
  from .. import mod1
  ```
4. sub_package2/sub_mod2.py ← sub_package1/sub_mod1.pyenv
  ```python
  from ..sub_package1 import sub_mod1
  ```
  なお`...`（ピリオド3つ）はさらに上の階層（2階層上）となり、`.`を増やすとさらに上の階層を表せる。

この相対インポートでは、`python`コマンドで実行したファイルより上の階層には遡れない。

### 5.1.2. 別ディレクトリからインポート
以下のようなディレクトリ構造とする。
```
dir_import_test/
├── dir
│   ├── main_absolute.py
│   ├── main_relative.py
│   └── main_sys_path_append.py
├── dir_for_mod
│   └── mod2.py
├── main_base.py
└── mod1.py
```
1. main_base.py ← mod1.py
  ```python
  import mod1
  ```
2. main_base.py ← dir_for_mod/mod2.py
  ```python
  import dir_for_mod.mod2
  ```
3. `dir`内ファイル ← dir_for_mod/mod2.pyやmod1.py
  モジュール探索パスを追加して絶対インポートする
  dir/main_sys_path_append.pyを例とする
  ```python
  # dir/main_sys_path_append.py
  import os
  import sys
  sys.path.append(os.path.join(os.path.dirname(__file__), '..'))
  
  import mod1
  import dir_for_mod import mod2
  ```
  パスを追加しなくても相対インポートや絶対インポートもできる。ただ、コマンドにオプションが必要だったりカレントディレクトリによってはエラーが発生する。
  一方パスを追加するとオプションは不要でカレントディレクトリに関わらずインポートできるので、基本はパスの追加でいいと思う。
  詳細は参考ページを参照。

## 5.2. カレントディレクトリ取得
`os.getcwd()`で取得できる
```python
import os
path = os.getcwd()
print(path)
# /Users/mbp/Documents/my-project/python-snippets/notebook
print(type(path))
# <class 'str'>]
```

# 6. pathlib
ファイル・ディレクトリ（フォルダ）のパスをオブジェクトとして捜査する。
以下のようなディレクトリ構造を例とする。
```
temp
├── dir
│   └── sub_dir
│       └── file2.txt
└── file.txt
```

## 6.1. Pathオブジェクト生成・処理
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

## 6.2. パスの存在・種類
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

## 6.3. 取得
### 6.3.1. ファイル名、ディレクトリ名：name, stem
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

### 6.3.2. 拡張子：suffix
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
### 6.3.3. 親ディレクトリ
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

### 6.3.4. カレントディレクトリ：cwd
pythonが実行されている作業ディレクトリ（カレントディレクトリ）は`cwd`メソッドで取得できる。
```python
print(Path.cwd())
# /Users/mbp/Documents/my-project/python-snippets/notebook
```
### 6.3.5. 同じディレクトリの別のファイル：with_name()
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

### 6.3.6. 拡張子を変更したパス：with_suffix
元のパスの`suffix`属性を変更したPathオブジェクトを返す。引数で新たな拡張子を文字列で指定する。
この時、先頭にピリオドがないとエラーを返す。
```python
p_file = Path('temp/file.txt')
print(p_file.with_suffix('.csv'))
# temp/file.csv
print(p_file.with_suffix('csv'))
# ValueError: Invalid suffix 'csv'
```

## 6.4. 作成
### 6.4.1. ディレクトリ
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

## 6.5. 削除
### 6.5.1. ディレクトリ
ディレクトリの削除には`rmdir`メソッドを使う。
```python
p=Path('temp/dir')
p.rmdir()
```
### 6.5.2. ファイル
ファイルの削除には`unlink`メソッドを使う
第一引数にTrueを指定すると、対象が存在しなくてもエラーを出さない
```python
p=Path('temp/dir/test.txt')
p.unlink(missing_ok=True)
```

## 6.6. 連結・追加
Pathオブジェクトに対して`/`演算子を使ってパスを追加できる。
また、`joinpath`メソッドも同様の動きをする。複数連結する場合は引数に複数渡す。
```python
path = Path('temp/dir')
# 以下は同じ処理
print(path / 'sub_dir' / 'file2.txt')
print(path.joinpath('sub_dir', 'file2.txt'))
```

## 6.7. 絶対パス↔相対パス
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

## 6.8. その他

### 6.8.1. パスの同一判定：samefile
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
### 6.8.2. str型への変換
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

### 6.8.3. osモジュールとpathlibの対応
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

# 7. 変換
## 7.1. 大文字小文字
### 7.1.1. 全ての文字を小文字に: lower
`str.lower()`ですべての文字を小文字に変換する。
```python
word = "Hello"
print(word.lower())
# hello
```
### 7.1.2. 全ての文字を大文字に: upper
`str.upper()`ですべての文字を大文字に変換する。
```python
word = "Hello"
print(word.upper())
# HELLO
```
### 7.1.3. 最初を大文字、他は小文字に：capitalize
`str.capitalize()`で先頭を大文字に、それ以降を小文字に変換できる。
```python
word = "hello python"
print(word.capitalize())
# Hello python
```
### 7.1.4. 単語ごとに最初を大文字に、他は小文字に：title
`str.title()`で文字列に含まれる単語ごとに最初の文字を大文字に、それ以外の文字を小文字に変換する。
```python
word = "hello python"
print(word.title())
# Hello Python
```
### 7.1.5. 大文字を小文字に、小文字を大文字に: swapcase
`str.swapcase()`で大文字を小文字に、小文字を大文字に変換する。
```python
word = "Hello"
print(word.swapcase())
# hELLO
```
# 8. 文字列操作
## 8.1. 置換
### 8.1.1. 文字列を指定して置換：replace
`replace`メソッドを用いて文字列を置換できる。
第一引数に置換もと文字列、第二引数に置換先文字列を指定する
```python
s = "one two one two one"
print(s.replace(" ", "-"))
# "one-two-one-two-one"
```
置換先を`""`(空文字)にすると削除される
```python
s = "one two one two one"
print(s.replace(" ", ""))
# "onetwoonetwoone"
```
* 最大置換回数を指定：count
  `replace`はデフォルトだと該当箇所すべてを置換する。しかし第三引数`count`で最大置換回数を指定できる。
  ```python
  s = "one two one two one"
  print(s.replace("one", "XXX"))
  # XXX two XXX two XXX
  print(s.replace("one", "XXX", 2))
  # XXX two XXX two one
  ```
* 複数の文字列を置換
  複数の文字列を同じ文字列に置換する場合は正規表現を使う（別述）
  複数の文字列をそれぞれ別の文字列に置換するには以下のようにreplaceをつなげるといい
  ```python
  s = "one two one two one"
  print(s.replace("one", "XXX").replace("two", "YYY"))
  # XXX YYY XXX YYY XXX
  ```
  これは複数のreplaceを順番に呼んでいるだけなため、一つ目の置換の結果によっては意図していない置換が発生する場合がある。

# 9. 文字列からメソッドを実行
## 9.1. 関数を変数に代入：getattr
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

## 9.2. 文字列から関数を呼び出す：locals, globals
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

# 10. 可変長引数:*args, **kwargs
引数に`*`と`**`を付けることで、可変長引数を指定できる。この時変数名はargsやkwargsでなくてもいい（大事なのはアスタリスク）
それぞれ以下の意味を持つ
* `*args`:複数の引数をタプルで受け取る。
* `**kwargs`:複数のキーワード引数を辞書で受け取る。

## 10.1. *args
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

## 10.2. **kwargs
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
```

# 11. ラムダ式: lambda
lambdaで名前を持たない無名関数を作成することができる。
sortやmax, minなどの引数にlambda式を使うことで、挙動を制御できたりする
## 11.1. def との対応
defとの対応は以下の通り
```python
def 名前(引数1, 引数2, ...):
  return 式

名前 = lambda 引数1, 引数2, ... : 式
```
具体例は以下
```python
def add_def(a, b=1):
  return a+b
add_lambda = lambda a, b=1:a+b
print(add_def(3,4))# 7
print(add_def(3))# 4
print(add_lambda(3,4))# 7
print(add_lambda(3))# 4
```
## 11.2. if文
その形式上、lambdaでは1行での式しか使用することができない。ただ、if分に相当する三項演算子は使用可能
```python
get_odd_even = lambda x: 'even' if x % 2 == 0 else 'odd'
print(get_odd_even(3))# odd
print(get_odd_even(4))# even
```
## 11.3. 名前について
これまでの例では`名前= lambda`のようにlambda式に変数名を付けていたが、PEP8では名前を付けないことが推奨されている。
つけると、コーディング規約の自動チェックツールではWarningが出たりする（Errorではないので実行はできる）

# 12. 日時について：datetime
参考：[Pythonのdatetimeで日付や時間と文字列を変換（strftime, strptime） | note.nkmk.me](https://note.nkmk.me/python-datetime-usage/)
datetimeモジュールを使うことで日時（日付や時間・時刻）の処理ができる。また文字列に変換したり文字列から返還することもできる。
引き算や足し算も可能

## 12.1. datetimeオブジェクトと現在情報：now
日付（年、月、日）と時刻（時、分、秒、マイクロ秒）を持つオブジェクト
以下の情報を持つ
|情報名|属性|
|:--|:--|
|年|`year`|
|月|`month`|
|日|`day`|
|時|`hour`|
|分|`minute`|
|秒|`second`|
|マイクロ秒|`microsecond`|

コンストラクタは以下の通りで、任意の`datetime`オブジェクトを作成できる
```python
datetime(year, month, day, hour=0, minute=0, second=0, microsecond=0, tzinfo=None)
import datetime
dt = datetime.datetime(2022, 7, 21, 12, 15, 30, 2000)
```
`year`, `month`, `day`以外はデフォルト値があるので、引数として渡さなくても使用できる。
また`datetime.now()`を使うことで今日の日付と現在時刻の`datetime`オブジェクトを取得できる。
```python
import datetime
now = datetime.datetime.now()
```
### 12.1.1. datetime ⇒ date
`date()`で`datetime`オブジェクトを`date`オブジェクトに変換できる
```python
dt_now = datetime.datetime.now()
print(dt_now)
# 2022-07-14 12:31:13.12345
print(type(dt_now))
# <class 'datetime.datetime'>

print(dt_now.date())
# 2022-07-14
print(type(dt_now.date()))
# <class 'datetime.date>
```

## 12.2. dateオブジェクトと現在日時：today
`date`オブジェクトは以下の情報を持つ
|情報名|属性|
|:--|:--|
|年|`year`|
|月|`month`|
|日|`day`|

コンストラクタとオブジェクトの作成は以下の通り
```python
date(year, month, day)

d = datetime.date(2022,7,11)
```
また`date.today()`を使うことで現在の日付を取得できる
```python
d_today = datetime.date.today()
```

## 12.3. timeオブジェクト
`time`オブジェクトは以下の情報を持つ。
|情報名|属性|
|:--|:--|
|時|`hour`|
|分|`minute`|
|秒|`second`|
|マイクロ秒|`microsecond`|

コンストラクタとオブジェクトの作成は以下の通り
```python
time(hour=0, minute=0, second=0, microsecond=0, tzinfo=None)

t = datetime.time(12,15,30,2000)
```

## 12.4. 経過日時や時間差：timedelta
`timedelta`オブジェクトは二つの日時の時間差、経過時間を持つ。
以下の情報を持つ
|情報名|属性|
|:--|:--|
|日|`day`|
|秒|`seconds`|
|マイクロ秒|`microseconds`|

### 12.4.1. オブジェクトの作成
1. datetime,dateオブジェクトの引き算
  `datetime`オブジェクト同士を引き算`-`すると、`timedelta`オブジェクトを得られる。
  ```python
  dt_now = datetime.datetime.now() # 2018-02-02 18:31:13.271231
  dt = datetime.datetime(2018, 2, 1, 12, 15, 30, 2000)
  td = dt_now - dt
  print(td) # 1 day, 18:31:13.271231
  print(td.days) # 1
  print(td.seconds) # 66673
  print(td.microseconds) # 271231
  print(td.total_seconds()) # 153073.271231
2. コンストラクタ
  引数としては`days`, `seconds`, `microseconds`, `milliseconds`, `minutes`, `hours`, `weeks`が存在し、すべてデフォルト値があるため省略可能。
  `timedelta`オブジェクトが保持しているのはあくまでも日数`days`、秒数`seconds`、マイクロ秒数`microseconds`のみなので、`weeks=1`は`days=7`と等しくなる
  ```python
  timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)
  ```

### 12.4.2. 引き算、足し算
`timedelta`オブジェクトは`datetime`や`date`と引き算や足し算などの演算ができる。
これによって、1週間後や10日後の日付や50分後の時刻などを取得できる。
```python
d_today = datetime.date.today() # 2018-02-02
td_1w = datetime.timedelta(weeks=1) # 7 days, 0:00:00
d_1w = d_today - td_1w # 2018-01-26
dt_now = datetime.datetime.now() # 2018-02-02 18:31:13.271231
td_10d = datetime.timedelta(days=10) # 10 days, 0:00:00
dt_10d = dt_now + td_10d # 2018-02-12 18:31:13.271231
td_50m = datetime.timedelta(minutes=50) # 0:50:00
print(td_50m.seconds) # 3000
dt_50m = dt_now + td_50m # 2018-02-02 19:21:13.271231
```

## 12.5. 文字列に変換：strftime, 文字列から変換：strptime
変換する際に文字列と属性を対応させる書式化コードを使用する。
主なものをいかに挙げる。
* `%d`:0埋めした10進数で表記した月中の日にち
* `%m`:0埋めした10進数で表記した月
* `%y`:0埋めした10進数で表記した西暦の下2桁
* `%Y`:0埋めした10進数で表記した西暦4桁
* `%H`:0埋めした10進数で表記した時（24時間表記）
* `%I`:0埋めした10進数で表記した時（12時間表記）
* `%M`:0埋めした10進数で表記した分
* `%S`:0埋めした10進数で表記した秒
* `%f`:0埋めした10進数で表記したマイクロ秒（6桁）
* `%A`:ロケールの曜日名
* `%a`:ロケールの曜日名（短縮形）
* `%B`:ロケールの月名
* `%b`:ロケールの月名（短縮形）
* `%j` : 0埋めした10進数で表記した年中の日にち（正月が`'001'`）
* `%U` : 0埋めした10進数で表記した年中の週番号 （週の始まりは日曜日）
* `%W` : 0埋めした10進数で表記した年中の週番号 （週の始まりは月曜日）

そのほかは以下を参考
* [8.1. datetime --- 基本的な日付型および時間型 — Python 3.6.15 ドキュメント](https://docs.python.org/ja/3.6/library/datetime.html#strftime-and-strptime-behavior)
### 12.5.1. 文字列に変換：strftime
`datetime`オブジェクト、`date`オブジェクトのP`strftime`日時の情報を任意の書式フォーマットの文字列に変換できる。
```python
dt_now = datetime.datetime.now() # 2018-02-02 18:31:13.271231
print(dt_now.strftime('%Y-%m-%d %H:%M:%S'))
# 2018-02-02 18:31:13
print(dt_now.strftime('%y%m%d))
# 180202
```
# 13. pickle
オブジェクトの直列化（シリアライズ）とその復元（デシリアライズ）ができる。
## 13.1. pickle化対象
pickle化できるものは以下のようになる。
* None値、boolean値（True/False）
* 整数値、浮動小数点数値、複素数値
* 文字列、バイト列（bytesオブジェクト）、バイト配列（bytearrayオブジェクト）
* pickle化可能なオブジェクトだけを要素とするリスト、タプル、辞書、集合
* モジュールトップレベルで定義された組み込み関数、関数（ラムダ式を除く）、クラス
* __dict__属性の値がpickle化可能なクラスのインスタンス。または__getstate__メソッドの戻り値がpickle化可能なクラスのインスタンス

これ以外のファイルオブジェクトなどはpickle化できない
## 13.2. シリアライズとデシリアライズ
pickle化には`dump`、非pickle化には`load`を使用する
### 13.2.1. 通常オブジェクト
```python
favs = ['beer', 'sake']
mydata = {'name':'田中', 'age':999,'weight':123.4,'favs':favs}

# pickle化して書き込み
with open('pickled.pkl', 'wb') as f:
  pickle.dump(mydata, f)

# 非pickle化
with open('pickled.pkl', 'rb') as f:
  mydata2 = pickle.load(f)
  favs2 = mydata2['favs']
```
この時mydataとmydata2は値は同じ（mydata == mydata2 = True)だが、オブジェクトは同じではない（mydata is mydata2 = False）
### 13.2.2. クラスのインスタンス
また、クラスのインスタンスもpickle化することができる
```python
class Foo:
  def __init__(self, name, age):
    self.name = name
    self.age = age

foo = Foo('田中', 999)
with open('pickled.pkl', 'wb') as f:
  pickle.dump(foo, f)

del foo
with open('pickle.pkl', 'rb') as f:
  foo = pickle.load(f)
```
この時pickle化できるのはそれぞれの変数が持つ値のみで、クラスの構造はpickle化できない。
すなわち、非pickle化するときにはそのプログラム上でFooクラスを定義していないといけない。
またクラス名が同じであるが別のクラスを定義していた場合、復元できてしまう。
```python
class Foo:
  def __init__(self, name, age):
    self.name = name
    self.age = age
foo = Foo('たなか', 99)
with open('pickled.pkl', 'wb') as f:
  pickle.dump(foo, f)
del Foo, foo
with open('pickled.pkl', 'rb') as f:
  foo = pickle.load(f) # FooクラスがないのでAttributeError

class Foo:
  def __init__(self, a, b):
    self.a = a
    self.b = b

with open('pickled.pkl', 'rb') as f:
  foo = pickle.load(f) # 復元できる
print(foo.a) # AttributeError (復元したfooにはa属性がない)
```
### 13.2.3. クラスや関数
関数やクラス事態をpickle化することもできる
```python
class Foo:
  def __init__(self, name, age):
    self.name = name
    self.age = age
def hello():
  print('hello')

with open('pickled.pkl', 'wb') as f:
  pickle.dump(Foo, f)
  pickle.dump(hello, f)

with open('pickled.pkl', 'rb') as f:
  Bar = pickle.load(f) # FooクラスをBarクラスに復元
  greet = pickle.load(f) # hello関数をgreet関数に復元
```
ただし関数やクラスのコードそのものがpickle化されたのではなく、完全修飾された名前参照（それが定義されているモジュール名と関数名またはクラス名だけ）がpickle化されている。そのため関数やクラスを非pickle化するときはその関数やクラスを定義しているモジュールがインポートされている必要がある。
## 13.3. pickleと_pickle
_pickleはC言語的に最適化されたもの？python2ではCPickleという名称だった
基本的には使用可能な場合自動的に_pickleが適用される。そのため_pickleを直接インポートする必要がない。
## 13.4. deepcopyとの対応
参考：[python - copy.deepcopy vs pickle - Stack Overflow](https://stackoverflow.com/questions/1410615/copy-deepcopy-vs-pickle)
以下の二つは同じ挙動をする
```python
data = ["value1", "value2"]

import copy
data_c = copy.deepcopy(data)

import pickle
data_p = pickle.loads(pickle.dumps(data,-1))
```

# 14. 名前でアクセスできるtuple: namedtuple
参考：[namedtupleで美しいpythonを書く！（翻訳） - Qiita](https://qiita.com/Seny/items/add4d03876f505442136)
通常のtupleは以下のように使う
```python
a = (1,'hello', objet())
print(a[1])
```
しかし以下の問題点がある
* 格納したデータに整数のインデックスでしかアクセスできない。
* tupleの構造が毎回変わる（同じ数のフィールドとプロパティを持つ保証はない）
namedtupleはこれらの問題を解決する。また従来のtupleと同じくイミュータブル（一度格納すると変更できない）である。

`namedtuple`は以下のように使用する
```python
from collections import namedtuple # インポート
Car = namedtuple('Car', ['color', 'mileage']) # 定義
# 第二引数はnamedtuple('Car', 'color mileage')のように空白区切りの文字列でもいい
my_car = Car('red',3812.4)
# もしくはmy_car=Car(color='red', mileage=3812.4)
print(mycar.color)
print(mycar.mileage)
```


# 15. schema定義： marshmallow
公式:[marshmallow: 簡素化されたオブジェクトのシリアル化 — marshmallow 3.17.0 ドキュメント](https://marshmallow.readthedocs.io/en/stable/index.html)
marshmallowはschema定義用のpythonライブラリ
使用することでschemaを定義してpython以外のデータ（jsonなど）を読み込める
以下のようにインストールすれば使用可能`
```cmd
$ pip install marshmallow
```
marshmallow配下の順序で使用する
1. model定義
2. schema定義
3. 読み込み/書き出し

## 15.1. model定義
シリアライズ、デシリアライズする対象となる。
例：
```python
class User:
  def __init__(self,name,age,email):
    self.name=name
    self.age=age
    self.email=email
    self.created=created=datetime.datetime.now()
```
## 15.2. schema定義
```python
from marshmallow import Schema, fileds, post_load, validates, ValidationError

class UserSchema(Schema):
  name=fields.Str()
  age=fields.Int()
  email=fields.Email()
  created=fields.DateTime()
  
  @post_load
  def make_user(self,data,**kwargs): # デシリアライズする際に使用
    return User(**data)
  
  @validates("age") # 検証用。必須ではない
  def validate_age(self,value):
    if age < 0:
      raise ValidationError("age must be greater than 0.")
    if value > 100:
      raise ValidationError("age must not be greater than 100.")
```
もしくは`from_dict`メソッドによって辞書型で作成
```python
UserSchema = Schema.from_dict(
  {
    "name": fileds.Str(),
    "age": fields.Int(),
    "email": fields.Email(),
    "created": fields.DateTime()
  }
)
```
基本的には`filed名=fields.{クラス}`で定義する。クラスの一覧は[こちら](https://marshmallow.readthedocs.io/en/stable/marshmallow.fields.html#api-fields)
またフィールドに他のスキーマを使用したい時などは以下のように
```python
class SubSchema(Schema):
  sub_fields1=fields.Str()
  sub_fields2=fields.Bool()

class MainSchema(Schema):
  main_fields1=fields.Float()
  sub_fields=fields.Nested(SubSchema())
```
### 15.2.1. Metaクラス
フィールドをそれぞれ定義せずに、Metaクラス内で指定することでmarshmallowが自動で型を作成する
```python
class UserSchema(Schema):
    uppername = fields.Function(lambda obj: obj.name.upper())

    class Meta:
        fields = ("name", "email", "created_at", "uppername")
```
このとき`name`は`String`で、`created_at`は`DateTime`で自動でフォーマットされる。
`fields`での指定ではフィールドで指定しているものも含む必要があるが、`additional`ではフィールドで定義しているものに加えるものを指定できる。
```python
class UserSchema(Schema):
    uppername = fields.Function(lambda obj: obj.name.upper())

    class Meta:
        # No need to include 'uppername'
        additional = ("name", "email", "created_at")
```

デフォルトではスキーマ内で一致しないキーに遭遇しloadがされると`ValidationError`が発生する。
以下の`unknown`にこれらオプションを渡すことでこの挙動を変更できる。
* RAISEValidationError:デフォルト。ValidationErrorを投げる
* EXCLUDE:不明なフィールドを除外
* INCLUDE:不明なフィールドを受け入れて含める
`Meta`クラスでこのオプションのデフォルトを指定できる
```python
class TESTSchema(Schema):
  class Meta:
    unknown = INCLUDE
```
なおこのオプションはインスタンス化やロード時に`schema=TESTSchema(unknown=EXCLUDE)`のようにしてオーバーライドできる。

またフィールドの順序を維持するには、`orderd`オプションを`True`に設定する。
これにより`dump`などをした際に`collections.OrderedDict`で返される。
```python
class UserSchema(Schema):
    first_name = fields.String()
    last_name = fields.String()
    email = fields.Email()

    class Meta:
        ordered = True


u = User("Charlie", "Stones", "charlie@stones.com")
schema = UserSchema()
result = schema.dump(u)
assert isinstance(result, OrderedDict)
pprint(result, indent=2)
#  OrderedDict([('first_name', 'Charlie'),
#              ('last_name', 'Stones'),
#              ('email', 'charlie@stones.com')])
```
### 15.2.2. fields
それぞれのフィールドに設定を付け加えることができる。
1. 必須
  `required=True`で必須項目として設定できる。これにより`Schema.load`した際に不足しているとエラーが出る。
  `error_messages`でそのエラー文をカスタマイズできる。辞書型でエラーメッセージ、もしくはエラーコードなどを設定する。
  ```python
  field1=fields.String(required=True)
  field2=fields.Integer(required=True, error_messages={"required":"this is required."})
  field3=fields.String(required=True, error_messages={"required":{"message":"this is required.","code":400}})
  ```
2. デフォルト
  `load_default`でデフォルトの逆シリアル化の値を指定、`dump_default`でデフォルトのシリアル化の値を指定する。
  ```python
  field1=fileds.UUID(load_default=uuid.uuid1)
  field2=fileds.DateTime(dump_default=datetime.datetime(2020,1,1))
  ```
3. 読み取り、書き込み専用
  `dump_only`で読み取り専用、`load_only`で書き込み専用フィールドにできる。
  ```python
  password=fields.Str(load_only=True)
  created=fields.DateTime(dump_only=True)
  ```
4. シリアライズ、デシリアライズ時のキー
  ```python
  class UserSchema(Schema):
     name = fields.String()
     email = fields.Email(data_key="emailAddress")
  s = UserSchema()
  data = {"name": "Mike", "email": "foo@bar.com"}
  result = s.dump(data)
  # {'name': u'Mike',
  # 'emailAddress': 'foo@bar.com'}

  data = {"name": "Mike", "emailAddress": "foo@bar.com"}
  result = s.load(data)
  # {'name': u'Mike',
  # 'email': 'foo@bar.com'}
  ```
5. 検証
  引数に`validate`を渡すことでフィールドの検証ができる。
  この時渡すものは`marshmallow.validate`やオリジナルのメソッド。検証の種類は[こちら](https://marshmallow.readthedocs.io/en/stable/marshmallow.validate.html#api-validators)
  ```python
  def validate_quantity(n):
    if n < 0:
        raise ValidationError("Quantity must be greater than 0.")
    if n > 30:
        raise ValidationError("Quantity must not be greater than 30.")
  class UserSchema(Schema):
    name = fields.Str(validate=validate.Length(min=1))
    permission = fields.Str(validate=validate.OneOf(["read", "write", "admin"]))
    age = fields.Int(validate=validate.Range(min=18, max=40))
    quantity = fields.Integer(validate=validate_quantity)
  ```
## 15.3. 読み込み/書き込み/検証
以下使用するモデルとスキーマは以下とする。
```python
import datetime as dt
from marshmallow import Schema, fields
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
        self.created_at = dt.datetime.now()

    def __repr__(self):
        return "<User(name={self.name!r})>".format(self=self)

class UserSchema(Schema):
    name = fields.Str()
    email = fields.Email()
    created_at = fields.DateTime()
```
### 15.3.1. シリアル化： dump
オブジェクトのスキーマをメソッドに渡してシリアル化する。

```python
user = User(name="Monty", email="monty@python.org")
schema = UserSchema()
result = schema.dump(user)
pprint(result)
# {"name": "Monty",
#  "email": "monty@python.org",
#  "created_at": "2014-08-17T14:54:16.049594+00:00"}
```
`dumps`によってJSONでエンコードされた文字列にもできる。
```python
json_result = schema.dumps(user)
pprint(json_result)
# '{"name": "Monty", "email": "monty@python.org", "created_at": "2014-08-17T14:54:16.049594+00:00"}'
```
`only`でフィールドを指定することで出力するフィールドを制限できる。
```python
summary_schema = UserSchema(only=("name", "email"))
summary_schema.dump(user)
# {"name": "Monty", "email": "monty@python.org"}
```
### 15.3.2. 読み込み
`load`を使用することで辞書型をモデルに変換する。
```python
from pprint import pprint

user_data = {
    "created_at": "2014-08-11T05:26:03.869245",
    "email": "ken@yahoo.com",
    "name": "Ken",
}
schema = UserSchema()
result = schema.load(user_data)
pprint(result)
# {'name': 'Ken',
#  'email': 'ken@yahoo.com',
#  'created_at': datetime.datetime(2014, 8, 11, 5, 26, 3, 869245)},
```
辞書型で返していて、`created_at`が文字列から`datetime.datetime`型になっている。
またこの時デコレータに`post_load`を付けたメソッドを追加することで、それに従って処理を行う。
```python
class UserSchema(Schema):
    name = fields.Str()
    email = fields.Email()
    created_at = fields.DateTime()

    @post_load
    def make_user(self, data, **kwargs):
        return User(**data)
user_data = {"name": "Ronnie", "email": "ronnie@stones.com"}
schema = UserSchema()
result = schema.load(user_data)
print(result)  # => <User(name='Ronnie')>
```
### 15.3.3. 検証
`load`や`loads`を行ったときにフィールドが存在しないなどの無効なデータが渡されるとエラーが出る。
デフォルトだと`ValidationError`。`ValidationError`は`message`でメッセージ、`valid_data`で対象のデータが取得できる。
```
from marshmallow import ValidationError

try:
    result = UserSchema().load({"name": "John", "email": "foo"})
except ValidationError as err:
    print(err.messages)  # => {"email": ['"foo" is not a valid email address.']}
    print(err.valid_data)  # => {"name": "John"}
```
### 15.3.4. その他
1. 反復処理
  読み込み、書き込み対象が複数の場合、`many`を指定することでリスト化された対象すべてに対して処理ができる。
  ```
  user1 = User(name="Mick", email="mick@stones.com")
  user2 = User(name="Keith", email="keith@stones.com")
  users = [user1, user2]
  schema = UserSchema(many=True)
  result = schema.dump(users)  # OR UserSchema().dump(users, many=True)
  pprint(result)
  # [{'name': u'Mick',
  #   'email': u'mick@stones.com',
  #   'created_at': '2014-08-17T14:58:57.600623+00:00'}
  #  {'name': u'Keith',
  #   'email': u'keith@stones.com',
  #   'created_at': '2014-08-17T14:58:57.600623+00:00'}]
  ```

## 15.4. 検証
`dump`や`load`なしに入力データの検証のみが必要な場合は`Schema.validate()`で検証できる。
```python
errors = UserSchema().validate({"name": "Ronnie", "email": "invalid-email"})
print(errors)  # {'email': ['Not a valid email address.']}
```

## 15.5. フィールドのカスタム
スキーマで定義するフィールドはいかの3種類のカスタムの仕方がある。
* カスタムFieldクラスの作成
* Methodフィールドの使用
* Functionフィールドの使用
1.  カスタムFieldクラスの作成
  `fields.Field`を継承したサブクラスを作成し、`_serialize`メソッド、`_deserialize`メソッドを定義する。
  また`default_error_message`を定義することでデフォルトのエラーメッセージを設定できる。
  またこれは`fields.Date`や`fields.Int`などを継承してもいい。
  ```python
  class PinCode(fields.Field):
      """Field that serializes to a string of numbers and deserializes
      to a list of numbers.
      """
      default_error_message = {"invalid": "Please provide a valid date"}

      def _serialize(self, value, attr, obj, **kwargs):
          if value is None:
              return ""
          return "".join(str(d) for d in value)

      def _deserialize(self, value, attr, data, **kwargs):
          try:
              return [int(c) for c in value]
          except ValueError as error:
              raise ValidationError("Pin codes must contain only digits.") from error
  ```
2. Methodフィールド
  スキーマのメソッドによって返される値にシリアル化される。`obj`はその受け取ったパラメータを指す。
  ```python
  class UserSchema(Schema):
    name = fields.String()
    email = fields.String()
    created_at = fields.DateTime()
    since_created = fields.Method("get_days_since_created")

    def get_days_since_created(self, obj):
        return dt.datetime.now().day - obj.created_at.day
  ```
3. Functionフィールド
  引数に一つのメソッドを受け取る。`obj`は受け取ったパラメータを指す。
  ```python
  class UserSchema(Schema):
    name = fields.String()
    email = fields.String()
    created_at = fields.DateTime()
    uppername = fields.Function(lambda obj: obj.name.upper())
  ```

Method, Functionフィールドではシリアル化する際のメソッドを指定するが、`deserialize`を使用することで、デシリアライズに使用するメソッドを指定することもできる。
```python
class UserSchema(Schema):
    # `Method` takes a method name (str), Function takes a callable
    balance = fields.Method("get_balance", deserialize="load_balance")

    def get_balance(self, obj):
        return obj.income - obj.debt

    def load_balance(self, value):
        return float(value)

schema = UserSchema()
result = schema.load({"balance": "100.00"})
result["balance"]  # => 100.0
```

`context`を用いてスキーマに状態を持たせることができる。
```python
class UserSchema(Schema):
    name = fields.String()
    # Function fields optionally receive context argument
    is_author = fields.Function(lambda user, context: user == context["blog"].author)
    likes_bikes = fields.Method("writes_about_bikes")

    def writes_about_bikes(self, user):
        return "bicycle" in self.context["blog"].title.lower()


schema = UserSchema()

user = User("Freddie Mercury", "fred@queen.com")
blog = Blog("Bicycle Blog", author=user)

schema.context = {"blog": blog}
result = schema.dump(user)
result["is_author"]  # => True
result["likes_bikes"]  # => True
```

# 16. その他組み込み関数
## 16.1. 型判定: isinstance
1番目の引数に指定したオブジェクトが2番目の引数に指定したデータ型と等しいかどうかを返す
```python
isinstance(object, classinfo)
```
以下使用例
```python
isinstance(1, int)
>> True
isinstance(1, str)
>> False
isinstance({"a":1}, dict)
>> True
```
複数のデータ型と比較したい場合には、第二引数をタプルにする。or条件なのでどれかと一致すればTrue
```python
isinstance(1, (str, int))
>> True
```
type関数の違いとしては、isinstance