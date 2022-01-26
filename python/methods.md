# pythonでよく使いそうなメソッド

## 正規表現での文字列抽出(re.search, re.findall)
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