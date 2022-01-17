# Git関係コマンド

## 設定
git の設定ファイルは以下の3種類
|種類|対象範囲|場所の例|備考|
|:-:|:--|:--|:--|
|system|システム全体(全ユーザーの全リポジトリ)|`/etc/gitconfig`| - |
|global|該当ユーザーの全リポジトリ|`~/.gitconfig`|ホーム直下|
|local|該当リポジトリ|`repository/.git/config`|リポジトリの`.git`直下|

system, global, localの順に読み込まれ、localが最も優先される。

### 設定の確認
`git config <name>`で各項目を確認できる。コマンドを実行した場所で有効になっている値が表示される。
```
# 例
$ git config user.email
```
local, system, globalそれぞれにおける項目の設定値を確認したい場合はオプションを付ける
```
$ git config --local <name>
$ git config --global <name>]
$ git config --system <name>
```

### 設定の変更
`git config <name> <value>`で設定。オプションを付けることでsystemやlocalを設定できる（デフォルトだとlocal）

### 設定項目
設定項目の一覧およびその詳細は以下のリンクから
* [Git - git-config Documentation](https://git-scm.com/docs/git-config.html#_variables)

例えば、
* `color.ui`: Gitの出力の色分け(通常は`auto`と設定)
* `core.editor`: コミットメッセージなどの編集で用いるエディタ
* `user.name`: ユーザー名
* `user.email`: Eメールアドレス