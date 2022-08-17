# ターミナルのプロンプトを短くする
参考：[ターミナルのプロンプトを短くする。 - Qiita](https://qiita.com/fugee/items/8ccd0439cf197cfce0aa)
普段のプロンプトは以下みたいな感じ
`username@pcname:~$ cd /opt`
`username@pcname:/opt $ mkdir -p /opt/dir1/dir2/dir3`
ユーザー名、PCネーム、フルパスがすべて入ってるから長くなったりする。

プロンプトの設定は環境変数`PS1`に入ってる。
デフォルトは以下
```
\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$
```
それぞれの部分は以下のような意味
* 前半部分
    通常時表示されない
    `\[\e]0;\u@\h: \w\a\]` : ターミナルのタイトル
    `{debian_chroot:+($debian_chroot)}` : ルートディレクトリを変えたときのみ表示
* 後半部分
    通常表示される
    `\[\033[01;32m\]\u@\h` : ユーザー名@ホスト名
    `\[\033[00m\]:` : コロン（：）
    `\[\033[01;34m\]\w` : カレントディレクトリの絶対パス
    `[\033[00m\]\$` : $マーク(rootの時は#)

## それぞれの解説
 * `\[...\]`: この二つに囲われた部分は非表示
 * `\033[...m`:文字表示の仕方を`...`の部分に指定
使用される特殊文字は以下
|特殊文字|意味|
|:--|:--|
|`\u`|ユーザー名|
|`\h`|ホスト名|
|`\w`|カレントディレクトリ（絶対パス）|
|`\W`|現在の作業ディレクトリ|
|`\s`|$マーク（rootの時は#）|
|`\t`|24時間表記の現在時刻|
|`\n`|改行|
詳しくは[bashのプロンプトを変更するには](https://atmarkit.itmedia.co.jp/flinux/rensai/linuxtips/002cngprmpt.html)

## 変更方法
### linux
`.bashrc`の以下の個所を編集
```
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```
編集例
```
if [ "$color_prompt" = yes ]: then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@:\[\033[01;34m\]\W\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\W\$ '
fi
```
これによって、`username@:~$`みたいになって絶対パスではなく現在のディレクトリのみの表示になる。