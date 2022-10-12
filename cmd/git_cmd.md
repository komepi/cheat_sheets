# 1. Git関係コマンド
- [1. Git関係コマンド](#1-git関係コマンド)
  - [1.1. 設定](#11-設定)
    - [1.1.1. 設定の確認](#111-設定の確認)
    - [1.1.2. 設定の変更](#112-設定の変更)
    - [1.1.3. 設定項目](#113-設定項目)
  - [1.2. git clone](#12-git-clone)
    - [1.2.1. ブランチ・タグを指定して取得](#121-ブランチタグを指定して取得)
    - [1.2.2. ディレクトリ名を指定して取得](#122-ディレクトリ名を指定して取得)
  - [1.3. ブランチ](#13-ブランチ)
    - [1.3.1. 作成](#131-作成)
      - [1.3.1.1. `/`を含むブランチ名作成でエラーが発生した場合](#1311-を含むブランチ名作成でエラーが発生した場合)
    - [1.3.2. 削除](#132-削除)
    - [1.3.3. 名前を変更](#133-名前を変更)
    - [1.3.4. 現在のブランチを確認](#134-現在のブランチを確認)
    - [1.3.5. 一覧](#135-一覧)
    - [1.3.6. 切り替え](#136-切り替え)
    - [1.3.7. 派生元の更新を取り込む](#137-派生元の更新を取り込む)
  - [1.4. fork](#14-fork)
    - [1.4.1. フォーク元の変更を反映する](#141-フォーク元の変更を反映する)
  - [リモート先の変更を適応](#リモート先の変更を適応)


## 1.1. 設定
git の設定ファイルは以下の3種類
|種類|対象範囲|場所の例|備考|
|:-:|:--|:--|:--|
|system|システム全体(全ユーザーの全リポジトリ)|`/etc/gitconfig`| - |
|global|該当ユーザーの全リポジトリ|`~/.gitconfig`|ホーム直下|
|local|該当リポジトリ|`repository/.git/config`|リポジトリの`.git`直下|

system, global, localの順に読み込まれ、localが最も優先される。

### 1.1.1. 設定の確認
`git config <name>`で各項目を確認できる。コマンドを実行した場所で有効になっている値が表示される。
```
# 例
$ git config user.email
```
local, system, globalそれぞれにおける項目の設定値を確認したい場合はオプションを付ける
```
$ git config --local <name>
$ git config --global <name>
$ git config --system <name>
```

### 1.1.2. 設定の変更
`git config <name> <value>`で設定。オプションを付けることでsystemやlocalを設定できる（デフォルトだとlocal）

### 1.1.3. 設定項目
設定項目の一覧およびその詳細は以下のリンクから
* [Git - git-config Documentation](https://git-scm.com/docs/git-config.html#_variables)

例えば、
* `color.ui`: Gitの出力の色分け(通常は`auto`と設定)
* `core.editor`: コミットメッセージなどの編集で用いるエディタ
* `user.name`: ユーザー名
* `user.email`: Eメールアドレス

## 1.2. git clone
リポジトリをローカルにコピーする。

### 1.2.1. ブランチ・タグを指定して取得
`-b`オプションを使用することでブランチ・タグを指定して取得できる。
```
git clone [RepositoryURL] -b [branch/tag name]
```
### 1.2.2. ディレクトリ名を指定して取得
基本的には指定しないとリポジトリ名がディレクトリ名となる。
以下のように指定もできる
```
git clone [RepositoryURL] [directory name]
```


## 1.3. ブランチ
### 1.3.1. 作成
下記いずれかのコマンドでブランチ作成
```
git branch [ブランチ名]

git checkout -b [ブランチ名]
```
特定のブランチからローカルブランチを作成する
```
$ git checkout -b [作成するブランチ名] [作成元のブランチ名]
```
#### 1.3.1.1. `/`を含むブランチ名作成でエラーが発生した場合
例えばfixture/test_branchのような`/`を含むブランチを作成した場合エラーが発生しブランチの作成が失敗する場合がある。
gitでは`/`を含むブランチを作成すると`/`以前の文字列をディレクトリのように扱う機能があるが、`/`以前の文字列のみのブランチが存在すると作成に失敗する
例）
```
$ git branch --contains
* master
  fixture
$ git checkout -b fixture/test_branch
```
上記の例だと作成に失敗する
よってfixtureを別の名前にするなどの対処が必要

### 1.3.2. 削除
`-d`オプションで削除できる
```
$ git branch -d [ブランチ名]
```
### 1.3.3. 名前を変更
`-m`オプションで名前を変更する
```
$ git branch -m [現在のブランチ名] [新しいブランチ名]
```
現在のブランチを変更する場合、以下のように新しいブランチ名だけで実行でも行ける
```
$ git branch -m [新しいブランチ名]
```
### 1.3.4. 現在のブランチを確認
`--contains`フラグを付けることで現在自分がいるブランチが出力される
```
$ git branch --contains
* main
```
### 1.3.5. 一覧
ブランチ一覧を見る
```
$ git branch -a
```

### 1.3.6. 切り替え
ブランチを切り替える
```
$ git checkout [ブランチ名]
```
### 1.3.7. 派生元の更新を取り込む
1. 元ブランチ(develop)にチェックアウト
  ```
  $ git checkout develop
  ```
2. developブランチの最新情報を取得
  ```
  $ git pull origin develop
  ```
3. 作業用ブランチ(feature/eda01)にチェックアウト
  ```
  $ git checkout feature/eda01
  ```
4. developブランチの内容をリベースする
  ```
  $ git rebase develop
  ```
## push
ローカルの変更をリモートに適応する
以下のようなパターン
|コマンド|概要|
|:--|:--|
|`git push {push先リポジトリ} {push対象ブランチ名}`|対象ローカルブランチを、指定したリポジトリにプッシュする。clone元に対してプッシュするときはpush先リポジトリを`origin`にする|
|`git push -d {push先リポジトリ} {push対象ブランチ名}`|対象リモートブランチを削除|
|`git push -f`|強制プッシュ|

## 1.4. fork
### 1.4.1. フォーク元の変更を反映する
1. upstream先が登録されているか確認
  ```
  $ git remote -v
  origin	https://github.com/user/hoge.git (fetch)
  origin	https://github.com/user/hoge.git (push)
  upstream	https://github.com/user/hoge.git (fetch)
  upstream	https://github.com/user/hoge.git (push)
  ```
  ここにupstreamがない場合、以下のコマンドで追加する
  ```
  $ git remote add upstream https://github.com/user/hoge.git
  ```
2. upstream先の最新をfetchで追いかけて自身のリポジトリにマージ
  ```
  $ git fetch upstream
  $ git merge upstream/[入れたいブランチ名]
  ```

## リモート先の変更を適応
```cmd
$ git fetch [remote]
```


## プルリクエスト
### プルリクエストをローカルにチェックアウト
基本は以下のコマンド
```
$ git fetch origin pull/{ID}/head:{branch name}
```
`release`に`develop`のプルリクエスト（#11)を送った場合は、`git fetch origin pull/11/head:develop`になる
`main_repo:release`に`sub_repo:develop`のプルリクエスト(#21)を送った場合は、`git fetch origin pull/11/head/develop`になる
その後、通常通りブランチを`checkout`などで変更する

リモートのブランチのプルリクエストをfetchする際は、以下のように.git/configに追記する
参考：https://www.web-dev-qa-db-ja.com/ja/git/github%E3%81%AE%E3%83%97%E3%83%AB%E3%83%AA%E3%82%AF%E3%82%A8%E3%82%B9%E3%83%88%E3%82%92%E3%83%81%E3%82%A7%E3%83%83%E3%82%AF%E3%82%A2%E3%82%A6%E3%83%88%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95/1050174470/
```
[remote "remote_repo"]
  ...
  fetch = +refs/pull/*/head:refs/remotes/remote_repo/pr/*
```
この状態で`git fetch remote_repo`を実行すると、全てのプルリクエストが取得できる
