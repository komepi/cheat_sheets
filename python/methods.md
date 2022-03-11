# 1. pythonでよく使いそうなメソッド
- [1. pythonでよく使いそうなメソッド](#1-pythonでよく使いそうなメソッド)
  - [1.1. 正規表現](#11-正規表現)
    - [1.1.1. 正規表現での文字列抽出(re.search, re.findall)](#111-正規表現での文字列抽出research-refindall)
    - [1.1.2. 正規表現での文字列置換(re.sub)](#112-正規表現での文字列置換resub)
  - [1.2. JSONの取り扱い](#12-jsonの取り扱い)
    - [1.2.1. JSONファイル](#121-jsonファイル)
    - [1.2.2. 辞書をJSON形式の文字列として出力](#122-辞書をjson形式の文字列として出力)

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
