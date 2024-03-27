- [基本](#基本)
  - [変数](#変数)
    - [変数の削除](#変数の削除)
    - [特殊変数](#特殊変数)
  - [配列](#配列)
  - [for文](#for文)
  - [while文](#while文)
  - [if文](#if文)
  - [case](#case)

## 基本

拡張子は`.sh`

一文目に以下を必ず記載する
```sh
#!/bin/bash
```

`#!/bin/sh`でも、大抵は/bin/shはbin/bashのシンボリックリンクになっているので動く。ただ、一部挙動が変化することがあるらしい

###　コメント

コメントは行の先頭に`#`をつければいい
```sh
#!/bin/bash

# echo "bashでこの行はコメント"
echo "bashでこの行はコメントでない"
```

### 変数
変数と値を`=`で代入する
値にスペースが入った文字列の場合はダブルクオーテーションで囲む
コマンドの実行結果を入れたい場合は、バッククオートでコマンドを囲めばいい。もしくは`$(command)`でも可能


```sh
#!/bin/bash
MESSAGE1=Hello_bash
MESSAGE2="Hello Bash"
HOSTNAME=`hostname`

echo $MESSAGE1
echo $MESSAGE2
echo $HOSTNAME
```
実行結果
```
Hello_bash
Hello Bash
localhost.localdomain
```
参照するときは`$`を頭につける。変数名を明確にする場合には`{}`で変数名を囲む
いくつかの例が以下
* 変数VAR1の値を表示
```sh
echo $VAR1
```
* 変数VARの値と「1」を表示
```sh
echo ${VAR}1
```
* 変数VAR2の値と変数VAR3の値を表示
```sh
echo ${VAR2}${VAR3}
```
* 変数の値を他の変数へ設定
```sh
VAR="$VAR1"
```
* 変数VARに変数VARの値と「1」を設定
```sh
VAR="${VAR}1"
```
* 変数VARに変数VAR2の値と変数VAR3の値を結合したものを設定
```sh
VAR="${VAR2}${VAR3}"
```

参照方法には値が設定されていないときに指定した値を参照するなど特殊な参照方法がある

|参照方法|参照結果|
|:--|:--|
|`${VAR=aaa}`|変数 VAR が未使用の場合に限り、変数VARへ文字列「aaa」を代入し文字列「aaa」を返す。変数VARがNULL値を含み既に使用されている場合は、変数 VAR への代入を行わず、変数 VAR の値を返す。|
|`${VAR:=aaa}`|変数 VAR が未使用もしくは NULL の場合に限り、変数 VAR へ文字列「aaa」を代入し文字列「aaa」を返す。変数 VAR が NULL 値の場合を除き、既に使用されている場合は変数 VAR への代入を行わず、変数 VAR の値を返す。|
|`${VAR-aaa}`|変数 VAR が未使用の場合に限り、文字列「aaa」を返す。変数 VAR が NULL 値を含み既に使用されている場合は、変数 VAR の値を返す。また、変数 VAR が使用済み・未使用に関わらず、変数 VAR への代入は行われない。|
|`${VAR:-aaa}`|変数 VAR が未使用もしくは NULL 値の場合に限り、文字列「aaa」を返す。変数 VAR が NULL 値以外の値で既に使用されている場合は、変数 VAR の値を返す。また、変数 VAR が使用済み・未使用に関わらず、変数 VAR への代入は行われない。|
|`${VAR+aaa}`|変数 VAR が NULL 値も含み既に使用されている場合に限り、文字列「aaa」を返す。変数 VAR が未使用の場合は、NULL 値を返す。また、変数 VAR が使用済み・未使用に関わらず、変数 VAR への代入は行われない。|
|`${VAR:*aaa}`|変数 VAR が NULL 値以外で既に使用されている場合に限り、文字列「aaa」を返す。変数 VAR が NULL 値もしくは未使用の場合は、NULL 値を返す。また、変数 VAR が使用済み・未使用に関わらず、変数 VAR への代入は行われない。|

以下がそのまとめ
（値の設定: 変数VARに文字列"aaa"が設定されるかされないか)
(返す値: 変数VARを表にある形式で参照した時の値(`echo xxx`などの形で実行した時の出力))
|               | >        | >      | >        | >      | >        | 変数VARが |
| :------------ | :------- | :----- | :------- | :----- | :------- | :-------- |
|               | >        | 未使用  | >        | NULL   | >        | 設定済み  |
|               | 値の設定  | 返す値  | 値の設定   | 返す値  | 値の設定  | 返す値    |
| `${VAR=aaa}`  | あり     | aaa    | なし     | ""     | なし     | $VAR      |
| `${VAR:=aaa}` | あり     | aaa    | あり     | aaa    | なし     | $VAR      |
| `${VAR-aaa}`  | なし     | aaa    | なし     | ""     | なし     | $VAR      |
| `${VAR:-aaa}` | なし     | aaa    | なし     | aaa    | なし     | $VAR      |
| `${VAR+aaa}`  | なし     | ""     | なし     | aaa    | なし     | aaa       |
| `${VAR:*aaa}` | なし     | ""     | なし     | ""     | なし     | aaa       |

#### 変数の削除
unsetコマンドで削除できる
```sh
unset VAR
```

#### 特殊変数
| 変数名            | 設定値                                           |
| :---------------- | :----------------------------------------------- |
| `$!`              | バックグラウンドで実行されたコマンドのプロセスID |
| `$?`              | 直前に実行したコマンドの終了ステータス           |
| `$$`              | コマンドのプロセスID                             |
| `$#`              | シェルスクリプト実行時の引数の数                 |
| `$*`もしくは`$@`  | シェルスクリプト実行時の全引数                   |
| `$0`              | シェルスクリプトのファイル名                     |
| `$1, $2, $3, ...` | N個目の引数                                      |

### 配列
カッコで囲み、値ごとにスペースを入れる
```sh
ARRAY=(0 1 2 3 4 5)
```

それぞれの値の参照は以下
```sh
ARRAY=(0 1 2 3 4 5)

# 先頭
echo $ARRAY

# 任意の配列番号(0開始)
echo "${ARRAY[1]}"

# 全て
echo "${ARRAY[@]}"
```
実行結果
```
0
1
0 1 2 3 4 5
```

### for文
指定回数の繰り返しの場合は`seq`コマンドを使用できる
```sh
for i in `seq 0 5`
do
    echo $i
done
```

この場合は0〜5が表示される

配列のそれぞれに対してアクセスする場合
```sh
ARRAY=(0 1 2 3)

for i in ${ARRAY[@]}
do
    echo $i
done
```

### while文
```sh
while [ $count -lt 5]
do
    count=$((++count))
    echo $count
done
```
この例だと1〜5までが表示される
`[]`はtestコマンド。if文の箇所で詳細を記述

テキストファイルの全ての行に対して処理をする場合
```sh
while read line
do
    echo $line
done < test.txt
```

### if文
```sh
if [ 条件1 ]; then
    処理1
elif [ 条件2 ]; then
    処理2
else
    処理3
fi
```

`[]`はtestコマンド
こまんど使用できるパラメータとしては以下
|項目|例|説明|
|:--|:--|:--|
|-e|[ -e /tmp/testfile ]|testfileが存在していればtrue|
|-f|[ -f /tmp/testfile ]|testfileが存在していて通常ファイルであればtrue|
|-d|[ -d /tmp/testdir ]|testdirが存在しディレクトリであればtrue|
|-z|[ -z 文字列 ]|文字列の長さが0であればtrue|
|-n|[ -n 文字列 ]|文字列の長さが0でなければtrue|
|-eq|[ 数値1 -eq 数値2 ]|数値1と数値2が等しければtrue(=)|
|-ne|[ 数値1 -ne 数値2 ]|数値1と数値2が等し苦なければtrue(!=)|
|-gt|[ 数値1 -gt 数値2 ]|数値1が数値2より大きければtrue(>)|
|-ge|[ 数値1 -ge 数値2 ]|数値1と数値2以上であればtrue(>=)|
|-lt|[ 数値1 -lt 数値2 ]|数値1と数値2より小さければtrue(<)|
|-le|[ 数値1 -le 数値2 ]|数値1と数値2以下であればtrue(<=)|
|=|[ 文字列1 = 文字列2 ]|文字列1と文字列2が等しければtrue|
|!=|[ 文字列1 != 文字列2 ]|文字列1と文字列2が等しくなければtrue|

### case
特定の条件で分岐させる
```sh
case 値 in
    値1 )
        処理1
        ;;
    値2 )
        処理2
        ;;
    値3 )
        処理3
        ;;
    * )
        処理3 #それ以外の場合
esac
```

以下は例
```sh
#!/bin/bash
STRINGS=(January February March April May June July August September October November December)
for str in ${STRINGS[@]}
do
  case $str in
    January )
      echo "$str は1月です"
      ;;
    February )
      echo "$str は2月です"
      ;;
    March )
      echo "$str は3月です"
      ;;
    * )
      echo "$str は1～３月ではありません"
  esac
done
```
出力
```
January は1月です
February は2月です
March は3月です
April は1～３月ではありません
May は1～３月ではありません
June は1～３月ではありません
July は1～３月ではありません
August は1～３月ではありません
September は1～３月ではありません
October は1～３月ではありません
November は1～３月ではありません
December は1～３月ではありません
```

# 環境変数
使用する際は、他の変数と同じように`$ENV_NAME`として使用できる
## 設定
```
export [シェル変数]
export [変数＝値]
```
例えば以下のようになる
```sh
abc=12
export abc
export xyz=234
```

# その他
## rootかどうかの確認
```sh
if [[ "$EUID" -ne 0 ]]; then
    sudo='sudo'
else
    sudo=''
fi
```