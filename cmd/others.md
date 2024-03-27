- [1. scpコマンド](#1-scpコマンド)
  - [1.1. オプション](#11-オプション)
  - [1.2. パス](#12-パス)
- [2. 実行権限について](#2-実行権限について)
  - [2.1. 確認: ll, ls -l](#21-確認-ll-ls--l)
  - [2.2. アクセス権限の変更: chmod](#22-アクセス権限の変更-chmod)
  - [2.3. 数値で指定](#23-数値で指定)
  - [2.4. アルファベットで指定](#24-アルファベットで指定)
  - [2.5. オプション](#25-オプション)
- [3. ファイル削除（linux）: rm](#3-ファイル削除linux-rm)
  - [3.1. オプション](#31-オプション)
- [4. ファイルの文字列検索: find, grep](#4-ファイルの文字列検索-find-grep)
  - [4.1. パイプでの使用](#41-パイプでの使用)
  - [4.2. and, or 検索](#42-and-or-検索)
  - [4.3. オプション](#43-オプション)
- [5. カレントディレクトリのパス確認：pwd](#5-カレントディレクトリのパス確認pwd)
- [6. メモリなどの状況を確認：vmstat](#6-メモリなどの状況を確認vmstat)
  - [6.1. オプション](#61-オプション)
  - [6.2. 見方](#62-見方)
  - [6.3. その他使い方](#63-その他使い方)
- [7. 実行中のプロセスの状態を取得：top](#7-実行中のプロセスの状態を取得top)
  - [7.1. オプション](#71-オプション)
  - [7.2. 操作](#72-操作)
  - [7.3. ヘッダーの見方](#73-ヘッダーの見方)
  - [7.4. プロセス一覧](#74-プロセス一覧)
  - [7.5. ファイルに出力](#75-ファイルに出力)
- [8. 公開鍵・秘密鍵の作成：ssh-keygen](#8-公開鍵秘密鍵の作成ssh-keygen)
- [JSON加工コマンド: jq](#json加工コマンド-jq)
  - [基本](#基本)
  - [コマンドライン](#コマンドライン)
  - [オプション](#オプション)
    - [出力関連](#出力関連)
      - [インデント数(--indent n)](#インデント数--indent-n)
      - [タブインデント(--tab)](#タブインデント--tab)
      - [コンパクト出力(-c | --compact-output)](#コンパクト出力-c----compact-output)
      - [キーソート出力(-S | --sort-keys)](#キーソート出力-s----sort-keys)
      - [RAW出力(-r | --raw-output)](#raw出力-r----raw-output)
      - [連続出力(-j | --join-output)](#連続出力-j----join-output)
      - [ASCII出力(-a | --ascii-output)](#ascii出力-a----ascii-output)
    - [入力関連](#入力関連)
      - [綴り入力(-s | --slurp)](#綴り入力-s----slurp)
      - [RAW入力(-R | --raw-input)](#raw入力-r----raw-input)
      - [入力なし(-n | --null-input)](#入力なし-n----null-input)
    - [キーアサイン](#キーアサイン)
      - [引数指定(--arg key value)](#引数指定--arg-key-value)
      - [引数指定(JSON) (--argjson key json)](#引数指定json---argjson-key-json)
      - [ファイル指定(--slurpfile key filename)](#ファイル指定--slurpfile-key-filename)
      - [ファイル指定(--argfile key filename)](#ファイル指定--argfile-key-filename)
      - [ファイル指定(--rawfile key filename)](#ファイル指定--rawfile-key-filename)
    - [引数指定(--args)](#引数指定--args)
      - [JSON指定](#json指定)
    - [その他](#その他)
      - [ストリーム処理(--stream)](#ストリーム処理--stream)
  - [フィルタ](#フィルタ)
    - [キーによる抽出](#キーによる抽出)
      - [キー(.key)](#キーkey)
      - [列挙(,)](#列挙)
    - [配列の抽出](#配列の抽出)
      - [配列(\[\])](#配列)
      - [配列のインデックス指定(\[n\], \[-n\], \[n:m\])](#配列のインデックス指定n--n-nm)
      - [パイプラインによる連結](#パイプラインによる連結)
      - [ドットによる連結](#ドットによる連結)
    - [抽出結果の整形](#抽出結果の整形)
      - [配列への整形(\[filter\])](#配列への整形filter)
      - [オブジェクトへの整形({filter})](#オブジェクトへの整形filter)
    - [再度抽出](#再度抽出)
  - [ビルトイン](#ビルトイン)
    - [セレクト](#セレクト)
    - [合計](#合計)
    - [配列処理](#配列処理)
      - [ソート(sort, sort\_by(path\_expression))](#ソートsort-sort_bypath_expression)
      - [グルーピング(group\_by(path\_expression))](#グルーピングgroup_bypath_expression)
      - [unique, unique\_by(path\_exp)](#unique-unique_bypath_exp)
      - [reverse](#reverse)
      - [flatten, flatten(depth)](#flatten-flattendepth)
      - [bsearch(x)](#bsearchx)
    - [長さ(length)](#長さlength)
    - [パス配列](#パス配列)
      - [path(path\_expression)](#pathpath_expression)
      - [getpath(path)](#getpathpath)
      - [setpath(path;value)](#setpathpathvalue)
      - [delpaths(paths)](#delpathspaths)
      - [paths, paths(node\_fileter)](#paths-pathsnode_fileter)
    - [キー・バリュー](#キーバリュー)
      - [keys, keys\_unsorted](#keys-keys_unsorted)
      - [キーの存在確認(has(key))](#キーの存在確認haskey)
      - [in(value)](#invalue)
    - [文字列処理](#文字列処理)
      - [startswith(str), ensswith(str)](#startswithstr-ensswithstr)
      - [ltrimstr(str), rtrimstr(str)](#ltrimstrstr-rtrimstrstr)
    - [文字列・配列](#文字列配列)
      - [split(str),split(reg:flags)](#splitstrsplitregflags)
      - [join(str)](#joinstr)
      - [contains(element)](#containselement)
    - [削除](#削除)
      - [del(path\_expression)](#delpath_expression)
    - [JSON](#json)
      - [tojson](#tojson)
      - [fromjson](#fromjson)
  - [文字列](#文字列)
    - [文字列の内挿((filter))](#文字列の内挿filter)
    - [型](#型)
# 1. scpコマンド
以下のコマンドを使用することで、ローカルから他サーバーに、もしくは逆パターンでファイルなどを転送できる。
```
$ scp [オプション] コピー元パス 保存先パス
```
## 1.1. オプション
|オプション|説明|
|:--|:--|
|-i 鍵ファイル|ssh接続に使用する鍵ファイルを指定する|
|-P ポート番号|(sshのポートを変更している場合などに)接続に使用するポートを指定する|
|-p|コピー元のタイムスタンプやパーミッションを保持する|
|-r|ディレクトリごとに再帰的にコピーする|

## 1.2. パス
パスは以下のような構成で記述する。
```
ユーザ名@サーバのホスト名(or IPアドレス):コピーしたいファイル、もしくは保存先のパス
```
ローカルの場合は`ユーザ名@サーバのホスト名(or IPアドレス):`を省略できる。

* リモートのファイル（test.txt）をローカルのカレントディレクトリへコピー
```
$ scp user@xxx.xxx.xxx.xxx:~/test.txt ./
```

* ローカルのカレントディレクトリのファイル（test.txt）をリモートのホームディレクトリ直下へコピー
```
$ scp test.txt user@xxx.xxx.xxx.xxx:~/
```

# 2. 実行権限について
以下はlinux環境でのもの
## 2.1. 確認: ll, ls -l

カレントディレクトリ内のファイルやディレクトリの情報を確認する。
```bash
$ ll
```
または
```bash
$ ls -l
```
上記コマンドを実行すると、以下のように表示
```bash
-rw-r--r--  1 user group  90  3月 16 11:50 test.txt
drwxr-xr-x  2 user group 253  3月 16 11:50 downloads
```

`-rw-r--r--`や`drwxr-xr-x`はファイルの情報を示す。
最初の1文字目はファイルの種類を以下のように示す。
|種別|意味|
|:--|:--|
|-|ファイル|
|d|ディレクトリ|
|l|シンボリックリンク|

それ以降は3文字区切りで順にファイルの所有者、所有グループ、その他に関する権限を示す。その文字が表示されていたら該当する権限があり、「-」だった場合は権限がない。
また文字それぞれの意味は以下の通り。
|文字|意味|
|:--|:--|
|r|読み取り|
|w|書き込み|
|x|実行|

## 2.2. アクセス権限の変更: chmod
変更には`chmod`コマンドを使う。指定方法には数字とアルファベットがある。
## 2.3. 数値で指定
以下はhoge.txtに対してパーミッションの確認→変更→確認を行っている。
```bash
$ ls -l　
-rw-r--r--  1 user group      9  1月 1 00:00 hoge.txt
$ chmod 764 hoge.txt
$ ls -l
-rwxrw-r--  1 user group      9  1月 1 00:00 hoge.txt
```
chmodのコマンドは数字で指定する場合以下のように実行する。
```bash
chmod モード 対象ファイル名
```
モードの数字は以下の通り
|モード（数字）|モード（アルファベット）|権限|
|:--|:--|:--|
|4|r|読み取り|
|2|w|書き込み|
|1|x|実行|

上記の合計値を「所有者」「所有グループ」「その他」の順で入力
例えば「764」の場合、それぞれ「読み取り」「書き込み」「実行」、「読み取り」「書き込み」、「読み取り」を付与している
組み合わせとしては以下のようになる
|合計値|読み取り|書き込み|実行|
|:--:|:--:|:--:|:--:|
|1|×|×|〇|
|2|×|〇|×|
|3|×|〇|〇|
|4|〇|×|×|
|5|〇|×|〇|
|6|〇|〇|×|
|7|〇|〇|〇|

## 2.4. アルファベットで指定
以下はhoge.txtに対して所有者、所有グループ、その他にそれぞれ「実行」、「書き込み」を追加している。（最初と最後は数字のものと同じ)
```bash
$ ls -l
-rw-r--r--  1 user group      9  1月 1 00:00 hoge.txt
$ chmod u+x hoge.txt
$ ls -l
-rwxr--r--  1 user group      9  1月 1 00:00 hoge.txt
$ chmod g+w hoge.txt
$ ls -l
-rwxrw-r--  1 user group      9  1月 1 00:00 hoge.txt
```
アルファベットで変更するときは以下のようにコマンドを実行
```bash
chmod (変更対象)(変更方法)(変更内容) 対象ファイル
```
以下を例にする
```bash
$ chmod u+x hoge.txt
```
uが変更対象、+が変更方法、xが変更内容
変更対象については以下のようになる
|変更対象|意味|
|:--|:--|
|u|ユーザー|
|g|グループ|
|o|その他|
|a|すべて|
変更方法については以下のようになる
|変更方法|意味|
|=|指定した権限にする|
|+|指定した権限を付与（追加）する|
|-|指定した権限を除去する|
よって先の例は「ユーザーに実行権限を付与」となる。
以下のように複数指定もできる。
```bash
$ chmod go+w hoge.txt
$ chmod a-wx hoge.txt
```
## 2.5. オプション
オプションを指定する際は以下のように実行
```bash
$ chmod [option] [mode] [file or directory]
```
chmodコマンドのオプションは以下のようになっている。
|オプション|意味|
|:--|:--|
|-f|エラーメッセージを表示しない|
|-c|実行時に変更があったときのみ結果を表示|
|-v|詳細を表示|
|-R|ディレクトリ内の複数ファイルを対象に|

# 3. ファイル削除（linux）: rm
`rm`コマンドでファイル、ディレクトリを削除する。
その際ファイル、ディレクトリは複数指定できる
以下書式
```cmd
$ rm [option] file1 file2 file3 ...
$ rm [option] dir1 dir2 dir3 ...
```

## 3.1. オプション
|短いオプション|長いオプション|意味|
|:--|:--|:--|
|-f|--force|存在しないファイルを無視する（確認もしない）|
|-i|--interactive|削除前に確認する|
|-v|--verbose|経過を表示する|
|-d|--directory|unlinkでディレクトリを削除する|
|-r, -R|--recursive|ディレクトリを再帰的に削除する|

# 4. ファイルの文字列検索: find, grep
windowsでは`find`、macやlinuxでは`grep`コマンドで検索する
* grep
```cmd
$ grap 検索正規表現 ファイル名
```
例) workディレクトリ内のファイルすべての中からaという文字列を検索
```cmd
$ grep a work/*
```
## 4.1. パイプでの使用
パイプによってあるコマンドの出力結果に`grep`を適応できる
```cmd
$ cat test.txt | grep "test"
```
windowsの場合は`Select-String`が代用できる
```
$ cat test.txt | Select-String "test"
```

## 4.2. and, or 検索
* and検索
  二つの条件両方に当てはまる行を表示する。基本的にはパイプで二つのコマンドをつなぐ
  ```cmd
  $ grep 検索文字列1 ファイル名 | grep 検索文字列2
  ```
  1文の中でのand検索は、以下のように正規表現を使う
  ```cmd
  $ grep スタートの検索文字列.*尾張の検索文字列 ファイル名
  ```
  例) workディレクトリ内のファイルすべての中からrで始まってpで終わる部分のある行
  ```cmd
  $ grep r.*p work/*
  ```
* or検索
  オプションで拡張正規表現を用いて|のor演算子を使う
  ```cmd
  $ grep -E 検索正規表現 ファイル名
  ```
  例) workディレクトリ内のファイルすべての中から大文字のpまたはeのいずれかを検索
  ```cmd
  $ grep -E 'p|e' work/*
  ```
  もしくはeオプションを使う
  ```cmd
  $ grep -e 検索正規表現1 -e 検索正規表現2 ファイル名
  ```
  例) workディレクトリ内のファイルすべての中から小文字のpまたはaのいずれかを検索
  ```cmd
  $ grep -e p -e a work/*
  ```
## 4.3. オプション
|オプション名|意味|
|:--|:--|
|-i|大文字と小文字を区別しない|
|-E|拡張正規表現を使う|
|-e|一致処理に指定した正規表現を使う|
|-v|一致しないものを検索|
|-n|検索結果に行番号を表示|
|-l|検索結果にファイル名のみ表示|
|-h|検索結果にファイル名を表示しない|
|-o|検索結果に一致した文字を表示|
|-C|検索結果に一致した個所から前後に指定した行数表示|
|-r|ディレクトリ内も検索対象とする|

# 5. カレントディレクトリのパス確認：pwd
`pwd`コマンドで確認できる
```cmd
$ pwd
/home/hoge/now_dir
```

# 6. メモリなどの状況を確認：vmstat
参考：[vmstatコマンドの使い方 - hana_shinのLinux技術ブログ](https://hana-shin.hatenablog.com/entry/2022/02/03/073240)
以下の状態を確認できる
* プロセスの状態
* メモリの使用状況
* スワッピングの状況
* ブロックI/Oの状況
* 割り込みやコンテキストスイッチの回数
* CPUの使用状況（ユーザーモード、カーネルモード等の使用割合）

## 6.1. オプション
|短いオプション|長いオプション|意味|
|:--|:--|:--|
|-a|--active|アクティブ/非アクティブなメモリの量を表示する|
|-d|--disk|ディスクの統計情報を表示|
|-D|--disk-sum|ディスクの統計情報を1項目1行で表示する|
|-f|--forks|ブート後のフォーク数を表示|
|-m|--slabs|スラブ情報を表示（root）|
|-n|--one-header|繰り返し表示の際にヘッダを１度だけ表示|
|-p パーティション|--partition パーティション|指定したパーティションの統計を表示する|
|-s|--stats|1項目1行でメモリの統計情報とイベントカウンタを表示|
|-S 単位|--unit 単位|単位をk,K,m,Mで指定|
|-t|--timestamp|タイムスタンプを表示（オプションなし、「-a」「-d」のみ）|
|-w|--wide|表示幅を広くして見やすくする|

## 6.2. 見方
`vmstat`コマンドを使用すると以下のように表示される。
```cmd
$ vmstat
procs -----------memory------------  --swap-- -----io---- --system-- ------cpu-----
 r  b    swpd   free   buff   cache   si   so    bi    bo    in   cs us sy id wa st
 4  0 1821184 616660     24  815716    1    9    63    25    38   76  3  3 94  0  0
```
それぞれの項目は以下の通り。
このとき、CPUでは総時間を100%として5種類それぞれに使用した時間をパーセンテージで表示している。
|欄|記号|意味|
|:--|:--|:--|
|procs|r|実行中と実行待ち中のプロセス数の合計|
|^|b|割り込み不可能なスリープ状態にあるプロセス数|
|memory|swpd|使用仲の仮想メモリの量|
|^|free|空きメモリの量|
|^|buff|バッファとして使用しているメモリの量|
|^|cache|キャッシュに使用しているメモリの量|
|^|inact|アクティブでないメモリの量|
|^|active|アクティブなメモリの量|
|swap|si|ディスクからスワップインしているメモリの量|
|^|so|スワップアウトしている量|
|io|bi|HDDのようなブロック型デバイスから受け取ったブロックス|
|^|bo|ブロック型デバイスに送ったブロック数|
|system|in|1秒当たりの割り込み回数|
|^|cs|コンテクストスイッチの回数|
|cpu|us|ユーザー空間のプログラムの実行に費やしたCPUの実行時間の割合|
|^|sy|カーネル空間のプログラムの実行に費やしたCPUの実行時間の割合|
|^|id|アイドル状態の時間の割合|
|^|wa|I/O待ち時間|
|^|st|仮想マシンがCPUを割り当てられなかった時間の割合|

## 6.3. その他使い方
1. 時間ごとに実行
  `$ vmstat {interval}`で`interval`秒ごとに実行する
  以下は１秒ごとに実行（わかりやすいようにタイムスタンプを表示している)
    ```cmd
    $ vmstat -t 1
    procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----  -----timestamp-----
    r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa  st                 JST
    3  0 1814784 530728     24 883448    1    9    63    25   38   75  3  3 94  0  0  2022-07-25 13:48:06
    1  0 1814784 529884     24 883448    0    0    24     0  497  816  2  3 95  0  0  2022-07-25 13:48:07
    1  0 1814784 529224     24 883492    0    0    20     0  494  742  6  6 88  0  0  2022-07-25 13:48:08
    ```
2. 実行回数を指定する
  `$ vmstat {interval} {time}`で`interval`秒ごとに`time`回実行する

# 7. 実行中のプロセスの状態を取得：top
topコマンドを使用することで、システム全体の負荷やプロセス、CPU、メモリ、スワップの統計情報をリアルタイムで取得できる。

## 7.1. オプション
主なオプションは以下の通り
|オプション|意味|
|:--|:--|
|-a|メモリ使用率順にソート|
|-p PID|特定のプロセスを監視|
|-d {iterval}|`interval`秒ごとに更新|
|-n {time}|`time`回表示を繰り返す|

## 7.2. 操作
* Shift + o
  表示された特定のキーを押してEnterすると任意の列でソート
* Shift + p
  CPU使用率順にソート
* Shift + m
  メモリ使用率順にソート
## 7.3. ヘッダーの見方
1. load average
    ```cmd
    top - 14:16:41 up 5 days, 4 min,  1 user,  load average: 0.37, 0.49, 0.47
          現在時間  サーバーの稼働時間  ログイン                1分   5分    15分
                                    ユーザー数  間の単位時間当たりの待ちタスク数
    ```
2. Task
    ```cmd
    Tasks: 307 total,   1 running, 306 sleeping,   0 stopped,   0 zombie
          合計タスク数      稼働中      待機中        停止タスク   ゾンビタスク
    ```
3. CPU
  * ni
    `nice`で実行優先度を変更したプロセスがユーザーモードでCPUを消費した時間
  * st
    OS仮想化利用時に他の仮想CPUの計算で待たされた時間
    ```
    %Cpu(s):  3.1 us,  3.8 sy,  0.0 ni, 93.2 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
              user     system    nice    idle    I/O wait hardware software  steal
                                                        interrupt interrupt
    ```
4. Merory/swap
  * buffers
    mallocなどバッファとして利用されているメモリ量
  * cached
    キャッシュとして利用されているメモリ量（ファイルシステムのキャッシュ）
    ```cmd
    Mem:  15144564k total,  1178112k used, 13966452k free,    28300k buffers
    Swap:        0k total,        0k used,        0k free,   289928k cached
    ```
## 7.4. プロセス一覧
```
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND                                                                                                                        
11678 kyosuke+  20   0 4390288   1.6g  12492 S  0.3 20.7  17:29.19 java                                                                                                                           
17071 kyosuke+  20   0 2014648 665564   7904 S  0.0  8.3   1:19.66 node                                                                                                                           
 3695 kyosuke+  20   0 2679168 482728   5324 S  0.0  6.0  15:54.19 java                                                                                                                           
16914 kyosuke+  20   0   11.0g 258044  11160 S  0.0  3.2   5:36.18 node                                                                                                                           
 5346 kyosuke+  20   0 2629108 239732   5240 S  0.3  3.0  26:08.14 java                                                                                                                           
12498 kyosuke+  20   0  507600 235680   3632 S  0.0  2.9   1:43.00 python  
```
|PR|NI|VIRT|RES|SHR|S|%CPU|%MEM|TIME+|
|:--|:--|:--|:--|:--|:--|:--|:--|:--|
|優先度|相対優先度|仮想メモリ|物理メモリ|共有メモリ|状態|CPU使用率|メモリ使用率|実行時間|
* NI: Nice value
  相対優先度。0が基準で、負だと優先度が高く、正だと優先度が低い
* VIRT: Virtual Image
  確保された仮想メモリすべて。スワップしたメモリ使用量を含む
* RES: Resident size
  スワップしていない、使用した物理メモリのサイズ
* SHR: Shared Mem size
  他のプロセスと共有される可能性のあるメモリのサイズ
* S: Process Status
  以下のいずれかの状態であるかを示す。
  * D: 割り込み不能
  * R: 実行中
  * S: スリープ状態
  * T: 停止中
  * Z: ゾンビプロセス
## 7.5. ファイルに出力
`-b`オプションを使用することでファイルに書き出しができる
```
$ top -b > result.txt
```
`ctrl + c`などで停止するまで実行される
`-d`オプションを使用して間隔を設定したり、`-n`オプションで回数を指定することもできる

# 8. 公開鍵・秘密鍵の作成：ssh-keygen
`ssh-keygen`コマンドで作成できる
```cmd
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/ユーザー名/.ssh/id_rsa): <--鍵の保存先 
Enter passphrase (empty for no passphrase):  <-- パスフレーズを入力（任意）
Enter same passphrase again:  <-- もう一度、パスフレーズを入力（任意）
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
```
設定で何も入力せずにEnterを押下するとデフォルトで作成される。
結果、`id_rsa`と`id_rsa.pub`が作成される。
それぞれ、`id_rsa`が**秘密鍵**、`id_rsa.pub`が**公開鍵**となる。

# JSON加工コマンド: jq
参考にしたサイトは[ここ](https://www.tohoho-web.com/ex/jq.html)
jqコマンドがインストールされていないことがある
以下のようにしてインストール
* centos
    ```cmd
    # yum -y install epel-release
    # yum -y install jq
    ```
* macos
    ```cmd
    # brew install jq
    ```
* windows
    https://zenn.dev/easy_easy/articles/e47b37b04dd1153d5b29
    ここを参考に

## 基本
JSONのプロセッサーツール
文字列のJSONを整形して出力できる
```cmd
$ echo '{"a":123,"b":456}' | jq
{
  "a": 123,
  "b": 456
}
```
JSONの中から指定したキーの情報を取得もできる
```cmd
$ echo '{"a":123, "b":456}' | jq '.b'
456
```
他にもageが40以下など条件を指定して取得したり、整形や合計値を求めることもできる

## コマンドライン
```
jq [options]
jq [options] filter [file...]
jq [options] --args filter [arguments...]
jq [options] --jsonargs filter [json_texts...]
```
以下のようにパイプラインで繋いだり、ファイルから取得したりする
```cmd
$ cat sample.json | jq
$ cat sample.json | jq '.users'
$ jq '.users' sample.json
```
## オプション
### 出力関連
#### インデント数(--indent n)
インデント数を指定
```
$ echo '{"name": "Yamada"}' | jq --indent 4
{
    "name": "Yamada"
}
```
#### タブインデント(--tab)
インデントにタブを使用
```
$ echo '{"name": "Yamada"}' | jq --tab
{
	"name": "Yamada"
}
```
#### コンパクト出力(-c | --compact-output)
改行やインデントなしで出力する
```
$ echo '{"name": "Yamada"}' | jq -c
{"name": "Yamada"}
```

#### キーソート出力(-S | --sort-keys)
キーをソートして出力
```
$ echo '{"b":"B", "c":"C", "a":"A"}' | jq -S
{
  "a": "A",
  "b": "B",
  "c": "C"
}
```

#### RAW出力(-r | --raw-output)
出力データの文字列からダブルクオーテーションを取り除く
```
$ echo '{"name":"Yamada"}' | jq '.name'
"Yamada"
$ echo '{"name":"Yamada"}' | jq -r '.name'
Yamada
```

#### 連続出力(-j | --join-output)
出力データの文字列からダブルクオーテーションと改行を取り除く
```
$ echo '{"name":"Yamada"}{"name":"Tanaka"}' | jq -j '.name'
YamadaTanaka
```

#### ASCII出力(-a | --ascii-output)
非ASCII文字をASCII文字で出力
```
$ echo '{"name":"山田"}' | jq -c
{"name":"山田"}
$ echo '{"name":"山田"}' | jq -c -a
{"name":"\u5c71\u7530"}
```

### 入力関連
#### 綴り入力(-s | --slurp)
複数のJSONが連続するデータを読み取り、配列に変換して出力
```
$ echo '{"name":"Yamada"}{"name":"Suzuki"}' | jq -c
{"name":"Yamada"}
{"name":"Suzuki"}
$ echo '{"name":"Yamada"}{"name":"Suzuki"}' | jq -c -s
[{"name":"Yamada"},{"name":"Suzuki"}]
```

#### RAW入力(-R | --raw-input)
入力をJSONとしてでなく、改行で区切られた文字列の集合として扱う
```
$ cat xx.csv
Yamada,26
Tanaka,32
$ cat xx.csv | jq -c -R 'split(",")'
["Yamada","26"]
["Tanaka","32"]
```

#### 入力なし(-n | --null-input)
入力を読み込まず、引数で渡した文字列をJSONとして扱う
```
$ jq -n '{"name":"Yamada"}'
{
  "name": "Yamada"
}
```

### キーアサイン
#### 引数指定(--arg key value)
$keyにvalueを割り当てる
```
$ jq -nc --arg name Yamada '{"name": $name}'
{"name": "Yamada"}
```

#### 引数指定(JSON) (--argjson key json)
$keyにJSON値valueを割り当てる
```
$ jq -cn --argjson foo '{"name":"Yamada"}' '{"foo":$foo}'
{"foo":{"name":"Yamada"}}
```

#### ファイル指定(--slurpfile key filename)
$keyにfilenameで指定したファイルの中身を割り当てる
```
$ echo '{"name":"Yamada"}{"name":"Tanaka"}' > sample.json
$ jq -nc --slurpfile foo sample.json '{"foo":$foo}'
{"foo":[{"name":"Yamada"},{"name":"Tanaka"}]}
```

#### ファイル指定(--argfile key filename)
--slurpfileと同じ。違いとしては、こちらはファイルの内容が１行の場合のみ使用できる
```
$ echo -n '"Yamada"' > sample.raw
$ jq -nc --argfile foo sample.raw '{"foo":$foo}'
{"foo":"Yamada"}
```

#### ファイル指定(--rawfile key filename)
$keyにfilenameで指定したファイルの中身(RAWデータ)を割り当てる
```
$ echo -n Yamada > sample.raw
$ jq -nc --rawfile foo sample.raw '{"foo":$foo}'
{"foo":"Yamada"}
```

### 引数指定(--args)
パラメータを引数で指定。パラメータは`$ARG.positional`で参照できる
```
$ jq -nc --args '$ARGS' AAA BBB
{"positional":["AAA","BBB"],"named":{}}
```

#### JSON指定
パラメータをJSON形式で指定
パラメータは`$ARG.positional`で参照できる
```
$ jq -nc --jsonargs '$ARGS' '{"arg1":"AAA","arg2":"BBB"}'
{"positional":[{"arg1":"AAA","arg2":"BBB"}],"named":{}}
```

### その他
#### ストリーム処理(--stream)
入力をストリーム的に読み込み、一つの値を[[値のパス配列],値]の列に変換しながら処理
JSONが完了していなくても値毎に逐次処理できるため、巨大なJSONファイルを逐次処理する際に利用されます。
例えばsample.jsonが以下だとする
```json
{
  "status": "OK",
  "count": 3,
  "users": [
    { "name": "Yamada", "age": 26 },
    { "name": "Tanaka", "age": 32 },
    { "name": "Suzuki", "age": 45 }
  ]
}
```
コマンドを使用すると以下
```cmd
$ cat sample.json | jq -c --stream
[["status"],"OK"]
[["count"],3]
[["users",0,"name"],"Yamada"]
[["users",0,"age"],26]
[["users",0,"age"]]
[["users",1,"name"],"Tanaka"]
[["users",1,"age"],32]
[["users",1,"age"]]
[["users",2,"name"],"Suzuki"]
[["users",2,"age"],45]
[["users",2,"age"]]
[["users",2]]
[["users"]]
```

## フィルタ
JSONからキーを指定して値を取り出す
以下におけるsample.jsonは以下の値とする
```json
{
  "status": "OK",
  "count": 3,
  "users": [
    { "name": "Yamada", "age": 26 },
    { "name": "Tanaka", "age": 32 },
    { "name": "Suzuki", "age": 45 }
  ]
}
```
### キーによる抽出
#### キー(.key)
`.key`は該当するキーの値を取り出す
```
$ cat sample.json | jq '.status'
"OK"
```
#### 列挙(,)
カンマで複数の値を取り出せる
```
$ cat sample.json | jq '.status, .count'
"OK"
3
```

### 配列の抽出
#### 配列([])
配列を抽出する
```
$ cat sample.json | jq -c '.users'
[{"name":"Yamada","age":26},{"name":"Tanaka","age":32},{"name":"Suzuki","age":45}]
$ cat sample.json | jq -c '.users[]'
{"name":"Yamada","age":26}
{"name":"Tanaka","age":32}
{"name":"Suzuki","age":45}
```

#### 配列のインデックス指定([n], [-n], [n:m])
0から数えてn番目の要素を取り出す
```
$ cat sample.json | jq -c '.users[1]'
{"name":"Tanaka","age":32}
```
`[-n]`とすると、最後からn番目の要素を取り出す
```
$ cat sample.json | jq -c '.users[-1]'
{"name":"Suzuki","age":45}
```
`[n:m]`とすると、n〜m番目の要素を配列形式で取り出す
```
$ cat sample.json | jq -c '.users[0:2]'
[{"name":"Yamada","age":26},{"name":"Tanaka","age":32}]
```

###　抽出結果から再抽出
パイプラインで複数のjqコマンドを連結する方法と、ドットで連結する方法がある
#### パイプラインによる連結
複数のjqコマンドを連結することができる
パイプで連結し一つ目のフィルタで抽出した結果を入力として再度抽出することもできる
```
$ cat sample.json | jq '.users[]' | jq '.name'
"Yamada"
"Tanaka"
"Suzuki"
$ cat sample.json | jq '.users[] | .name'
"Yamada"
"Tanaka"
"Suzuki"
```

#### ドットによる連結
```
$ cat sample.json | jq '.users[].name'
"Yamada"
"Tanaka"
"Suzuki"
```

### 抽出結果の整形
#### 配列への整形([filter])
抽出した結果を配列として整形するには全体を`[...]`で囲む
```$ cat sample.json | jq -c '[.users[].name]'
["Yamada","Tanaka","Suzuki"]
```
以下のように名前をキーとして配列にもできる
```
$ cat sample.json | jq -c '.users[] | [.name, .age]'
["Yamada",26]
["Tanaka",32]
["Suzuki",45]
```

#### オブジェクトへの整形({filter})
オブジェクトとして整形したいときは全体を`{key: ....}`で囲む
```
$ cat sample.json | jq -c '{name:.users[].name}'
{"name":"Yamada"}
{"name":"Tanaka"}
{"name":"Suzuki"}
```
オブジェクト配列にもできる
```
$ cat sample.json | jq -c '.users[] | {N:.name, A:.age}'
{"N":"Yamada","A":26}
{"N":"Tanaka","A":32}
{"N":"Suzuki","A":45}
```

キーを固定値でなく、値をキーと指定場合、以下のようにキー名を`(...)`で囲む
```
cat sample.json | jq -c '.users[] | {(.name):.age}'
{"Yamada":26}
{"Tanaka":32}
{"Suzuki":45}
```

### 再度抽出
`.filter1.filter2.filter3.filterx`を途中のフィルターを省略して`..|.filterx`と表現できる
例えばsample.jsonのデータ階層にかかわらずnameの値を抽出するには`..|.name`とする
.nameにマッチしない値もあるためエラー無視の`?`を付加して`..|.name?`とし、さらに`|strings`で文字列のみを抽出
```
$ cat sample.json | jq '..|.name?|strings'
"Yamada"
"Tanaka"
"Suzuki"
```

## ビルトイン
### セレクト
配列やJSON列を受け取り、条件にマッチした列のみを抽出
```
$ echo '[1,2,3,4,5]' | jq -c 'select(. > 2)'
[1,2,3,4,5]
$ echo '[{"k":"ABC","v":92},{"k":"DEF","v":76}]' | jq -c '.[] | select(.v > 80)'
{"k":"ABC","v":92}
```

### 合計
配列の値の合計を求める
```
$ echo '[1,2,3]' | jq -c 'add'
6
```

### 配列処理
#### ソート(sort, sort_by(path_expression))
配列をソートする
path_expressionを指定した場合はその指定したキーでソート
```
$ echo '[3,5,1,4,2]' | jq -c 'sort'
[1,2,3,4,5]
$ echo '[{"A":3,"B":2},{"A":1,"B":3},{"A":2,"B":1}]' | jq -c 'sort_by(.B)'
[{"A":2,"B":1},{"A":3,"B":2},{"A":1,"B":3}]
```

#### グルーピング(group_by(path_expression))
path_expression で指定したキーが同じ値を持つものをグルーピング
```
$ echo '[{"A":1,"B":3},{"A":3,"B":2},{"A":1,"B":1}]' | jq -c 'group_by(.A)'
[[{"A":1,"B":3},{"A":1,"B":1}],[{"A":3,"B":2}]]
```

#### unique, unique_by(path_exp)
重複した値を一つにまとめる
```
$ echo '[1,2,3,2,1]' | jq -c 'unique'
[1,2,3]
$ echo '[{"A":3,"B":1},{"A":1,"B":3},{"A":2,"B":1}]' | jq -c 'unique_by(.B)'
[{"A":3,"B":1},{"A":1,"B":3}]
```

#### reverse
配列の順を逆にする
```
$ echo '[1,2,3]' | jq -c 'reverse'
[3,2,1]
```

#### flatten, flatten(depth)
配列の階層をフラットにする
depthを指定すると、depth階層までの配列をフラットにする
```
$ echo '[1,[2,[3,4]]]' | jq -c 'flatten'
[1,2,3,4]
$ echo '[1,[2,[3,4]]]' | jq -c 'flatten(1)'
[1,2,[3,4]]
```

#### bsearch(x)
ソート済みの配列を入力として値xをバイナリサーチする
見つかればそのインデックス、見つからなければ見つからないと判断した一のインデックスの負数から１引いた値を返す
```
$ echo '[1,3,5,7,9]' | jq 'bsearch(5)'
2
$ echo '[1,3,5,7,9]' | jq 'bsearch(6)'
-4
```

### 長さ(length)
配列の個数、オブジェクトの要素数、文字列の文字数を返す
```
$ echo '["a","b","c"]' | jq 'length'
3
```

### パス配列
#### path(path_expression)
パスとして参照されるキー名、配列インデックスの配列を返す
```
$ echo 'null' | jq -c 'path(.a[0].b)'
["a",0,"b"]
```

#### getpath(path)
パス配列を指定して値を抽出
```
$ echo '{"a":{"b":{"c":123}}}' | jq -c 'getpath(["a", "b", "c"])'
123
```

#### setpath(path;value)
パス配列を指定して値を変更
```
$ echo '{"a":{"b":{"c":123}}}' | jq -c 'setpath(["a", "b", "c"]; 456)'
{"a":{"b":{"c":456}}}
```

#### delpaths(paths)
パス配列を指定して値を削除
```
$ echo '{"a":{"b":{"c":123}}}' | jq -c 'delpaths([["a", "b", "c"]])'
{"a":{"b":{}}}
```

#### paths, paths(node_fileter)
それぞれの値を得るためのパス配列の一覧を返す
node_filterに型名を指定すると型にマッチする要素に関してのみ返す
型については[ここ](#型)を参考にする
```
$ echo '{"a":123, "b":456, "c":{"d":789}}' | jq -c '[paths]'
[["a"],["b"],["c"],["c","d"]]
$ echo '{"a":123, "b":456, "c":{"d":789}}' | jq -c '[paths(scalars)]'
[["a"],["b"],["c","d"]]
```

### キー・バリュー
#### keys, keys_unsorted
キーのみの配列をソートして返す
```
$ echo '{"b":"B", "c":"C", "a":"A"}' | jq -c 'keys'
["a","b","c"]
```
ソートしない場合はkeys_unsorted
```
$ echo '{"b":"B", "c":"C", "a":"A"}' | jq -c 'keys_unsorted'
["b","c","a"]
```

#### キーの存在確認(has(key))
オブジェクトを受け取り、keyキーを持っていればtrue、そうでなければfalse
```
$ echo '{"foo":"FOO"}' | jq 'has("foo"), has("baa")'
true
false
```

#### in(value)
値の配列を受け取り、引数で指定したオブジェクトのキーとして存在すれば true、さもなくば false を返します。
```
$ echo '["foo","bar","baz"]' | jq '.[] | in({"foo":null})'
true
false
false
```

### 文字列処理
#### startswith(str), ensswith(str)
startswithは文字列がstrで始まっていればtrueを返す
ensswithは文字列がstrで終わっていればtrueを返す
```
$ echo '"Japanese"' | jq 'startswith("Jap")'
true
$ echo '"Japanese"' | jq 'endswith("ese")'
true
```

#### ltrimstr(str), rtrimstr(str)
先頭から（末尾から）strを取り除く
```
$ echo '"Japanese"' | jq 'ltrimstr("Jap")'
"anese"
$ echo '"Japanese"' | jq 'rtrimstr("ese")'
"Japan"
```

### 文字列・配列
#### split(str),split(reg:flags)
文字列をデリミタで分割
正規表現や正規表現のフラグも使える
```
$ echo '"foo,baa,baz"' | jq -c 'split(",")'
["foo","baa","baz"]
```
#### join(str)
配列をデリミタで連結
```
$ echo '["foo","baa","baz"]' | jq 'join(",")'
"foo,baa,baz"
```

#### contains(element)
入力が文字列→指定した文字列が含まれていればtrue
入力が配列→指定した配列要素が全て含まれていればtrue
入力がオブジェクト→指定したパスの値が合致していればtrue
```
$ echo '"ABCDEFGHIJ"' | jq 'contains("DEF")'
true
$ echo '["A","B","C"]' | jq 'contains(["A","C"])'
true
$ echo '{"A":12, "B":13, "D":{"E":14}}' | jq 'contains({"A":12, "D":{"E":14}})'
true
```


### 削除
#### del(path_expression)
オブジェクトを受け取り指定したキーを削除
配列を受け取り指定したインデックスの要素を削除
```
$ echo '{"foo":"FOO", "baa":"BAA", "baz":"BAZ"}' | jq -c 'del(.foo, .baz)'
{"baa":"BAA"}
$ echo '["foo", "baa", "baz"]' | jq -c 'del(.[0, 2])'
["baa"]
```

### JSON
#### tojson
データをJSON文字列に変換
```
$ echo '{"name":"Tanaka"}' | jq 'tojson'
"{\"name\":\"Tanaka\"}"
```
#### fromjson
JSON文字列をデータに変換
```
echo '"{\"name\":\"Tanaka\"}"' | jq -c 'fromjson'
{"name": "Tanaka"}
```
## 文字列
### 文字列の内挿(\(filter))
\(filter)を記述することで抽出した値を文字列の中に埋め込める
```
$ echo '{"name":"Tanaka"}' | jq '"My name is \(.name)."'
"My name is Tanaka."
```

### 型
jqにおける方は以下のようになっている
?のやつはよくわかってないやつ
|型名|例|
|:--|:--|
|arrays|[]|
|objects|{}|
|iterables|?|
|booleans|true|
|numbers|123|
|finites|?|
|strings|"test"|
|nulls|null|
|scalars|"test",123(arrays,objects以外)|