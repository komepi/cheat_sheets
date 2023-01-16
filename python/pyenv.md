# 1. pyenv
- [1. pyenv](#1-pyenv)
  - [1.1. パスについて](#11-パスについて)
  - [1.2. バージョン確認](#12-バージョン確認)
  - [1.3. 特定バージョンのインストール](#13-特定バージョンのインストール)
  - [1.4. バージョンの切り替え](#14-バージョンの切り替え)
  - [1.5. 現在のバージョン、インストール済みのバージョンを確認](#15-現在のバージョンインストール済みのバージョンを確認)
- [インストール](#インストール)
  - [centos7](#centos7)
## 1.1. パスについて
だいたいパスが通ってないとかでバージョンが変わらないことが多い
その場合、起動時に実行されるパスを追加したりするファイル（.zshrcや.bash_profileなど）に以下のコマンドを追記
```cmd
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/shims:$PATH"
eval "$(pyenv init -)"
```
初めて記述したときは、記述後に以下を実行し適用(centosの場合)
```cmd
$ source ~/.bash_profile
```
## 1.2. バージョン確認
現在インストール可能なバージョンのリスト
```cmd
$ pyenv install --list
```
## 1.3. 特定バージョンのインストール
```cmd
$ pyenv install 3.9.7
```

## 1.4. バージョンの切り替え
globalメソッドで切り替える
```cmd
$ pyenv global 3.9.7
```

## 1.5. 現在のバージョン、インストール済みのバージョンを確認
```cmd
$ pyenv versions
```

# インストール
## centos7
参考:[pyenvを用いたpython環境構築手順（CentOS7.1） - Qiita](https://qiita.com/ksugawara61/items/ba9a51ebfdaf8d1a1b48)
1. 必要モジュールのインストール
   ```cmd
   $ yum install gcc zlib-devel bzip2 bzip2-devel readline readline-devel sqlite sqlite-devel openssl openssl-devel git libffi-devel
   ```
2. pyenvのインストール
   pyenvのリポジトリをクローンし、構築
   * リポジトリのクローン
      ```
      $ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
      ```
   * bash_profileにpyenvのパスを追加
      ```
      $ echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.bash_profile
      $ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
      $ source ~/.bash_profile
      ```
   * pyenvのバージョン確認
      ```
      $ pyenv --version
      pyenv 1.2.2-6-g694b551
      ```