- [click](#click)
  - [使用方法](#使用方法)
- [標準出力](#標準出力)
- [表示操作](#表示操作)
- [エディタ、アプリケーションを開く](#エディタアプリケーションを開く)
- [プログレスバー](#プログレスバー)
- [引数, オプション](#引数-オプション)
  - [オプションの設定](#オプションの設定)
    - [検証](#検証)
      - [callback](#callback)
      - [整数の範囲](#整数の範囲)
      - [選択肢](#選択肢)
    - [タプルで受け取る](#タプルで受け取る)
    - [同じオプションを複数回使用](#同じオプションを複数回使用)
    - [オプションの数をカウント](#オプションの数をカウント)
    - [真偽値](#真偽値)
    - [異なる名前のオプションで一つの引数を使う](#異なる名前のオプションで一つの引数を使う)
    - [動的なデフォルト値](#動的なデフォルト値)
    - [オプションのデフォルトに環境変数を使用](#オプションのデフォルトに環境変数を使用)
  - [引数の設定](#引数の設定)
    - [複数の引数を指定](#複数の引数を指定)
    - [引数でファイルを使用](#引数でファイルを使用)
    - [パス文字列を使用](#パス文字列を使用)
  - [インタラクティブに値を受け取る](#インタラクティブに値を受け取る)
    - [プロンプトから入力](#プロンプトから入力)
    - [パスワードの入力](#パスワードの入力)
    - [ユーザー確認](#ユーザー確認)
  - [サブコマンド](#サブコマンド)
    - [グループコマンドのオプションや引数を参照](#グループコマンドのオプションや引数を参照)
    - [サブコマンドを追加した状態で、グループコマンドも実行できるようにする](#サブコマンドを追加した状態でグループコマンドも実行できるようにする)
    - [サブコマンドを複数実行](#サブコマンドを複数実行)
    - [サブコマンドの結果を使用して別のサブコマンドを使用（チェイン）する](#サブコマンドの結果を使用して別のサブコマンドを使用チェインする)


# click
コマンドラインパーサの一つ
argparseもあるけど、かなりコードがすっきりしてる

## 使用方法
pipによってインストール
```
$ pip install click
```
その後、プログラム内でimportする
```python
import click

@click.command()
@click.option("--name1","-n",default="World")
@click.argument("name2")
def cmd(name1,name2):
    msg = "name1:{}, name2:{}".format(name1, name2)
    click.echo(msg)
if __name__ == "__main__":
    cmd()
```
また実行時は以下のようにコマンドラインにて実行
```cmd
$ python file.py [args] [option]
```


# 標準出力
以下のような種類がある
* `click.echo()`
* `click.secho()`
* `click.style()`
* `click.echo_via_paper()`

```python
def main():
    message="Hello World of 2021!"
    # 普通に標準出力
    click.echo(message)
    # エラーメッセージとして標準出力
    click.echo(message, err=True)
    # フォントの色を緑
    click.echo(click.style(message, fg="green"))
    # フォントの色を緑、バックグラウンドを青
    click.echo(click.style(message, fg="green", bg="blue"))
    # 太文字、下線を引く、フォント色とバックグラウンド色を反転
    click.echo(click.style(message,bold=True, underline=True, reverse=True))
    # 太文字、下線を引く、フォント色を緑、バックグラウンドを青にしてフォント色とバックグラウンド色を反転
    click.secho(message, bold=True, underline=True, reverse=True, fg="green", bg="blue")
```

`click_echo_via_paper()`では長い複数行の表示をスクロール表示する
```python
def main():
    click.echo_via_pager("\n".join(f"Line {idx}" for idx in range(100)))
```
↑↓やスペースキーでスクロールし、その状態からqキーで戻ることができる

# 表示操作
ターミナルの表示を操作できる
`click.clear()`で標準出力をクリア（clearコマンドと同等）、`click.pause()`でキーボードが押されるまで停止することができる

# エディタ、アプリケーションを開く
`click.edit()`でエディタを開く
この時開かれるのはおそらくvim
また引数として`click.edit("filename.txt")とすると該当ファイルを開ける

`click.launch()`によってブラウザを開くことができる
引数としてurlが必須
```python
click.launch("https://google.com")
```

# プログレスバー
以下のようにしてプログレスバーを表示できる
```python
@click.command()
@click.argument('count', type=int)
def countdown(count):

    numbers = range(count, -1, -1)
    with click.progressbar(numbers,show_pos=True) as bar:
        for num in bar:
            time.sleep(1)
```
オプションとしては以下のようなものがある
* iterable : イテレート対象となるiterable。Noneの場合はlength指定が必須となる。
* length : イテレートするアイテムの数。iterableが渡されている場合デフォルトではそちらに長さを問い合わせる（が長さが判明しない場合もある）。lengthパラメータが指定されている場合は（iterableの長さではなく）指定された長さだけイテレートする。
* label : プログレスバーの隣りに表示するラベル。
* show_eta : 推定所要時間を表示するか否か。長さが決定できない場合には自動的に非表示にされる。
* show_percent : パーセンテージを表示するか否か。（デフォルトは、iterableが長さをもつ場合はTrueで、持たない場合は* False）
* show_pos : 絶対位置を表示するか（デフォルトはFalse）
* item_show_func : 処理中のアイテムを表示する際に呼ばれる関数。この関数は、処理中のアイテムについてプログレスバーに* 表示されるべき文字列を返す。アイテムはNoneである可能性もあるので注意。
* fill_char : プログレスバーの塗りつぶし部分に用いる文字。
* empty_char : プログレスバーの塗りつぶされていない部分に用いる文字。
* bar_template : バーのテンプレートとして用いられるフォーマット文字列。中で用いられるパラメータは label, bar, * info の３つ。
* info_sep : 複数の情報アイテム（eta等）のセパレータ。
* width : プログレスバーの幅（半角文字数単位で）。0を指定するとターミナルの全幅
* file – 書き込み先ファイル。ターミナル以外ならラベルのみが表示される。
* color : ターミナルがANSIカラーをサポートしているか。（自動識別）
* 
# 引数, オプション
デコレータとして登録する
```python
@click.command()
@click.option("--name1","-n",default="World")
@click.argument("name2")
def cmd(name1,name2):
    msg = "name1:{}, name2:{}".format(name1, name2)
    click.echo(msg)
if __name__ == "__main__":
    cmd()
```
この際、optionは"--xxx"のxxx部分、argumentは引数名ををのまま引数として登録される
同じにしない場合以下のエラーが発生
```cmd
TypeError: {メソッド名}() got an unexpected keyword argument {メソッドに指定した引数名}
```

## オプションの設定
`@click.option`デコレータをメソッドの頭につけることで設定できる
```python
@click.commando()
@click.option("--name","-n",default="World")
def cmd(name):
    msg = "Hello, {name}!".format(name=name)
    click.echo(msg)
```
第一引数に長いオプション名、第二引数に短いオプション名も指定できる。
defaultは指定しないとNoneになる

```python
@click.command()
@click.option(
    "--name",                 # 長いオプション名
    "-n",                     # 短いオプション名
    default="World",          # デフォルト値(ないとNone)
    type=str,                 # 型指定
    help="this is name value" # 説明（helpコマンドで表示)
)
def cmd(name):
    msg = "Hello, {name}!".format(name=name)
    click.echo(msg)
```
### 検証
デコレータのオプションを用いて値について検証を行うことができる
#### callback
検証を行うメソッドを定義し、callback関数として指定する
```python
def validate_age(ctx, param, value):
    age = value

    if age < 1:
        raise click.BadParameter('You are liar!')
    if age > 100:
        raise click.BadParameter('Really?!')


@click.command()
@click.option('--age', callback=validate_age, default=18)
def cmd(age):
    msg = 'Your age: {age}'.format(age=age)
    click.echo(msg)
```

#### 整数の範囲
整数の範囲であれば、組み込みで検証が可能
```python
@click.command()
@click.option('--age', type=click.IntRange(0, 100))
def cmd(age):
    msg = 'Your age: {age}'.format(age=age)
    click.echo(msg)

```
これで0以上100以下以外の値はエラーが出る
実行すると詳しく表示される
```cmd
$ python intrange.py --age 101
Usage: intrange.py [OPTIONS]

Error: Invalid value for "--age": 101 is not in the valid range of 0 to 100.
$ python intrange.py --age -1
Usage: intrange.py [OPTIONS]

Error: Invalid value for "--age": -1 is not in the valid range of 0 to 100.
```

この時、clampパラメータをTrueにすると、範囲外の値が渡された時に近い値に補正する
```python
@click.command()
@click.option('--age', type=click.IntRange(0, 100, clamp=True))
def cmd(age):
    msg = 'Your age: {age}'.format(age=age)
    click.echo(msg)
```
これにより101は100に、-1は0に補正された
```cmd
$ python intrange.py --age 101
Your age: 100
$ python intrange.py --age -1
Your age: 0
```

#### 選択肢
typeにclick.Choiceを指定することで、選択肢にすることができる
```python
@click.command()
@click.option('--language', type=click.Choice(['Japanese', 'English']))
def cmd(language):
    click.echo(language)
```
選択肢内に存在しない値を指定するとエラーになる
```cmd
$ python choice.py --language French
Usage: choice.py [OPTIONS]

Error: Invalid value for "--language": invalid choice: French. (choose from Japanese, English)
```

### タプルで受け取る
typeにタプルを指定すれば、一つのオプションで複数の値を受け取れる
```python
@click.command()
@click.option('--values', type=(str, int))
def cmd(values):
    msg = '{s} {i}'.format(s=values[0], i=values[1])
    click.echo(msg)
```
```cmd
$ python tuplevalues.py --values Hello 123
Hello 123
```
### 同じオプションを複数回使用
multipleをTrueにすると、同じオプションを複数回使える
この時、引数に渡される値はタプルになる
```python
@click.command()
@click.option('--values', multiple=True)
def cmd(values):
    click.echo(type(value))
    for value in values:
        click.echo(value)
```
```cmd
$ python multiopts.py --values 1 --values 2
<class 'tuple'>
1
2
```

### オプションの数をカウント
countパラメータをTrueにするとオプションが指定された数がわかる
```python
@click.command()
@click.option('-v', '--verbose', count=True)
def cmd(verbose):
    msg = 'Verbosity: {v}'.format(v=verbose)
    click.echo(msg)
```
```cmd
$ python count.py -vvvv
Verbosity: 4
$ python count.py -v -v -v
Verbosity: 3
$ python count.py -v --verbose
Verbosity: 2
```
よくあるのは、vの数で出力される内容のくわしさが変化するやつ

### 真偽値
is_flagオプションをTrueにすると、オプションの有無に応じてTrue/Falseが取得できる
```python
@click.command()
@click.option('--shout', is_flag=True)
def cmd(shout):
    msg = 'Hello, World!'
    if shout:
        msg = msg.upper()
    click.echo(msg)
```
オプションがあるとTrue, ないとFalseが取得できる
```cmd
$ python boolflag.py
Hello, World!
$ python boolflag.py --shout
HELLO, WORLD!
```

`/`記号で二つのオプションを挟んで指定しデフォルト値を真偽値にすることでも実現できる
```python
@click.command()
@click.option('--shout/--no-shout', default=False)
def cmd(shout):
    msg = 'Hello, World!'
    if shout:
        msg = msg.upper()
    click.echo(msg)
```
```cmd
$ python boolflag.py
Hello, World!
$ python boolflag.py --shout
HELLO, WORLD!
$ python boolflag.py --no-shout
Hello, World!
```
この場合、shoutが有効なオプションとして判断され、--shoutがあるとTrue

### 異なる名前のオプションで一つの引数を使う
以下のようにすると実現できる
```python
@click.command()
@click.option('--upper', 'transformation', flag_value='upper')
@click.option('--lower', 'transformation', flag_value='lower', default=True)
def cmd(transformation):
    click.echo(transformation)
```
--upper オプションを指定した場合には transformation 引数に 'upper' が、反対に --lower オプションを指定した場合には 'lower' が代入される
二つを同時に実行した場合、後に指定したものが適用される
```cmd
$ python switch.py
lower
$ python switch.py --upper
upper
$ python switch.py --lower
lower
$ python switch.py --upper --lower
lower
$ python switch.py --lower --upper
upper
```

### 動的なデフォルト値
defoultにlambdaなど関数を指定すると、動的なデフォルト値にできる
```python
@click.command()
@click.option('--age', default=lambda: random.randint(1, 100))
def cmd(age):
    msg = 'Your age: {age}'.format(age=age)
    click.echo(msg)
```
```cmd
$ python dynamicdefault.py
Your age: 98
$ python dynamicdefault.py
Your age: 63
$ python dynamicdefault.py
Your age: 41
```

関数を指定した場合も同じ
```python
def set_default(value):
    return value+100

@click.command()
@click.option('--age', default=set_default(10))
def cmd(age):
    msg = 'Your age: {age}'.format(age=age)
    click.echo(msg)
```
```cmd
$ python dynamicdefault.py
Your age: 110
```

### オプションのデフォルトに環境変数を使用
envvarパラメータに変数名を指定すれば実現可能
```python
@click.command()
@click.option('--shell', envvar='SHELL')
def cmd(shell):
    click.echo(shell)
```
```cmd
$ python env.py
/bin/zsh
$ python env.py --shell /bin/bash
/bin/bash
```

## 引数の設定
`@click.argument`デコレータをメソッドの頭につけることで設定できる
```python
@click.command()
@click.argument('name')
def cmd(name):
    msg = 'Hello, {name}!'.format(name=name)
    click.echo(msg)
```

### 複数の引数を指定
nargsパラメータで指定できる
その中で一つだけ-1を指定することができる。その場合は、その引数が何個でも指定できることを意味する
```python
@click.command()
@click.argument('src', nargs=-1)
@click.argument('dst1', nargs=1)
@click.argument('dst2', nargs=2)
def cmd(src, dst1, dst2):
    for i in src:
        msg = 'move from {src} to {dst}'.format(src=i, dst=dst)
        click.echo(msg)
```
```cmd
$ python nargs.py src1 src2 src3 dst
move from src1 to dst
move from src2 to dst
move from src3 to dst
```
この時、-1以外のパラメータは後ろから数えて定義される。
```python
@click.command()
@click.argument('s',nargs=1)
@click.argument('src', nargs=-1)
@click.argument('dst1', nargs=1)
@click.argument('dst2', nargs=2)
def cmd(s, src, dst1, dst2):
    for i in src:
        for d in dst2:
            msg = 'move from {s}:{src} to {dst},{d}'.format(s=s,src=i, dst=dst1,d=d)
            click.echo(msg)
```
dst2_xを正しく2つだと正常に動作する
dst2_xを3つにするとdst2_2,dst2_3がdst2、 dst2_1がdst1、 src1~dst1をsrcと認識される
```cmd
$ python click_sample.py s src1 src2 src3 dst1 dst2_1 dst2_2
move from s:src1 to dst1,dst2_1
move from s:src1 to dst1,dst2_2
move from s:src2 to dst1,dst2_1
move from s:src2 to dst1,dst2_2
move from s:src3 to dst1,dst2_1
move from s:src3 to dst1,dst2_2

$ python click_sample.py s src1 src2 src3 dst1 dst2_1 dst2_2 dst2_3
move from s:src1 to dst2_1,dst2_2
move from s:src1 to dst2_1,dst2_3
move from s:src2 to dst2_1,dst2_2
move from s:src2 to dst2_1,dst2_3
move from s:src3 to dst2_1,dst2_2
move from s:src3 to dst2_1,dst2_3
move from s:dst1 to dst2_1,dst2_2
move from s:dst1 to dst2_1,dst2_3
```

### 引数でファイルを使用
typeパラメータにclick.File()オプションを指定するとオープン済みのファイルオブジェクトが代入される
```python
@click.command()
@click.argument('file', type=click.File('r'))
def cmd(file):
    with file as f:
        while True:
            line = f.readline()
            if not line:
                break
            click.echo(line, nl=False)
```

### パス文字列を使用
ファイルを開いてではなく、事前にいろいろチェックしてから、といった場合にはパス文字列を指定できる
またexistsパラメータをTrueにすることでファイルの有無をチェックすることもできる
```python
@click.command()
@click.argument('path', type=click.Path(exists=True))
def cmd(path):
    click.echo(path)
    filename = click.format_filename(path, shorten=True)
    click.echo(filename)
```
存在するファイルであればそのまま、存在しない場合にはエラーになる
```cmd
$ python filepath.py /dev/random
/dev/random
random
$ python filepath.py /dev/foo
Usage: filepath.py [OPTIONS] PATH

Error: Invalid value for "path": Path "/dev/foo" does not exist.
```

## インタラクティブに値を受け取る
### プロンプトから入力
オプション追加時にpromptパラメータを指定すると、コマンドラインでオプションを指定しなかった場合にプロンプトを表示して入力を受け付ける
```python
@click.command()
@click.option('--name', prompt='Name', default='World',
              help='The person to greet.')
def cmd(name):
    msg = 'Hello, {name}!'.format(name=name)
    click.echo(msg)
```
次のように、オプションの指定がない場合にはプロンプトを表示して入力を受け付け、指定された場合にはそれが使われるようになる。
```cmd
$ python prompt.py
Name [World]:
Hello, World!
$ python prompt.py
Name [World]: Sekai
Hello, Sekai!
$ python prompt.py --name Click
Hello, Click!
```

またpromptはメソッドとして呼び出すこともできる
```python
@click.command()
def cmd():
    age = click.prompt('Please enter your age', type=int)
    click.echo(age)
```

### パスワードの入力
オプション追加時に、prompt=True, hide_input=Trueとすると入力がエコーバックされなくなる
またconfirmation_prompt=Trueとすることで、入力を2回促して両者が一致する場合のみ処理をする、という挙動になる

```python
@click.command()
@click.option('--password', prompt=True, hide_input=True,
              confirmation_prompt=True, help='Your password')
def cmd(password):
    msg = 'Your password: {password}'.format(password=password)
    click.echo(msg)
```
```cmd
$ python password.py
Password:
Repeat for confirmation:
Your password: password123
```

### ユーザー確認
継続するかユーザの確認をするには、click.confirm())を使用する
返り値はy/NがTrue/Falseで帰ってくる
処理を中断する場合はclick.Abort()をraise?

```python
@click.command()
def cmd():
    if not click.confirm('Do you want to continue?'):
        raise click.Abort()

    click.echo('Hello, World!')
```
```cmd
$ python confirm.py
Do you want to continue? [y/N]:
Aborted!
$ python confirm.py
Do you want to continue? [y/N]: y
Hello, World!
```

## サブコマンド
@click.group()を使用して、サブコマンドを作成できる

```python
@click.group()
def cmd():
    pass

@cmd.command()
def english():
    click.echo("Hello, World")

@cmd.command()
def japanese():
    click.echo('Konnichiwa, Sekai!')

if __name__ == "__main__":
    main()
```
```$ python subcommand.py
Usage: subcommand.py [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  english
  japanese
$ python subcommand.py english
Hello, World!
$ python subcommand.py japanese
Konnichiwa, Sekai!
$ python subcommand.py french
Usage: subcommand.py [OPTIONS] COMMAND [ARGS]...
```

### グループコマンドのオプションや引数を参照
@click.pass_context デコレータを使ってコンテキストを各コマンドを表した関数のシグネチャに追加する。
コンテキストはコマンド間で共有されるオプション

```python
@click.group()
@click.option('--debug', is_flag=True)
@click.pass_context  # コンテキストをインジェクトする
def cmd(ctx, debug):
    # インジェクトしたコンテキストに情報を詰める
    ctx.obj['DEBUG'] = debug

@cmd.command()
@click.pass_context
def subcmd(ctx):
    if ctx.obj['DEBUG']:
        # サブコマンドではコンテキストから情報を取り出す
        click.echo('DEBUG MODE!')
    click.echo('Hello, World!')

if __name__ == "__main__":
    cmd(obj={})
```
```cmd
$ python passcontext.py subcmd
Hello, World!
$ python passcontext.py --debug subcmd
DEBUG MODE!
Hello, World!
```

### サブコマンドを追加した状態で、グループコマンドも実行できるようにする
@click.group()デコレータにinvoke_without_command=Trueとすると、グループコマンド単体でも実行できるようになる
グループコマンドの引数をctxとしたときにctx.invoked_subcommand is Noneで条件文を追加することで、サブコマンドが実行されていない場合のみ処理を行うように設定できる。これがないと、サブコマンド実行時にもグループコマンドの処理が走る

```python
@click.group(invoke_without_command=True)
@click.pass_context
def cmd(ctx):
    if ctx.invoked_subcommand is None:
        click.echo('This is parent!')

@cmd.command()
def subcmd():
    click.echo('This is child!')
```
```cmd
$ python withoutsubcmd.py subcmd
This is child!
$ python withoutsubcmd.py
This is parent!
```

### サブコマンドを複数実行
@click.group()デコレータのパラメータchainにTrueとすると、サブコマンドを複数つなげて実行できる
```python
@click.group(chain=True)
def cmd():
    pass

@cmd.command()
def subcmd1():
    click.echo('subcmd1')

@cmd.command()
def subcmd2():
    click.echo('subcmd2')
```
```cmd
$ python commandchain.py subcmd1 subcmd2
subcmd1
subcmd2
```

### サブコマンドの結果を使用して別のサブコマンドを使用（チェイン）する
それぞれのサブコマンドの戻り値をオプションや引数を処理する関数とした上で、それをグループコマンドのresultcallback()デコレータで就職した関数でまとめて処理する。
```python
@cmd.resultcallback()
def pipeline(processors, textfile):
    ite = (line.rstrip('\r\n') for line in textfile)
    for processor in processors:
        ite = processor(ite)
    for line in ite:
        click.echo(line)

@cmd.command()
def shout():
    def processor(ite):
        for line in ite:
            shouted = '{line}!!!'.format(line=line)
            yield shouted
    return processor

@cmd.command()
def upper():
    def processor(ite):
        for line in ite:
            yield line.upper()
    return processor
```
```$ python pipeline.py greet.txt upper shout
HELLO,!!!
WORLD!!!!
```