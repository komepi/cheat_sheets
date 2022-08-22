# pydocstring
## google スタイル
以下例
```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
"""モジュールの説明タイトル

* ソースコードの一番始めに記載すること
* importより前に記載する

Todo:
    TODOリストを記載
    * conf.pyの``sphinx.ext.todo`` を有効にしないと使用できない
    * conf.pyの``todo_include_todos = True``にしないと表示されない

"""

import json
import inspect

class testClass() :
    """クラスの説明タイトル

    クラスについての説明文

    Attributes:
        属性の名前 (属性の型): 属性の説明
        属性の名前 (:obj:`属性の型`): 属性の説明.

    """

    def print_test(self, param1, param2) :
        """関数の説明タイトル

        関数についての説明文

        Args:
            引数の名前 (引数の型): 引数の説明
            引数の名前 (:obj:`引数の型`, optional): 引数の説明.

        Returns:
            戻り値の型: 戻り値の説明 (例 : True なら成功, False なら失敗.)

        Raises:
            例外の名前: 例外の説明 (例 : 引数が指定されていない場合に発生 )

        Yields:
            戻り値の型: 戻り値についての説明

        Examples:

            関数の使い方について記載

            >>> print_test ("test", "message")
               test message

        Note:
            注意事項などを記載

        """
        print("%s %s" % (param1, param2) )

if __name__ == '__main__':

    test_object = testClass()
    test_object.print_test("test", "message")
```
### 各セクションについて
#### Attributes
クラスの属性の型、名前などクラスの属性の説明
```
Attributes:
    属性の名前 (属性の型): 属性の説明
    属性の名前 (:obj:`属性の型`): 属性の説明
```
#### Args
引数についての説明。
名前や型、オプションかどうかやデフォルト値など
この時`self`は省略する。
```
Args:
    引数の名前 (引数の型): 引数の説明
    引数の名前 (:obj:`属性の型`, optional): 引数の説明
```
#### Returns
`return`による返り値の説明
```
Returns:
    戻り値の型: 戻り値の説明
```
#### Yields
yieldを用いた関数の戻り値の説明
```
Yields:
    戻り値の型: 戻り値の説明
```
#### Raises
例外処理についての説明
```
Raises:
    例外の名前: 例外の説明
```
#### Examples
関数、クラスの実行例について記述
またこの時、クラス、メソッド以外で使用する場合は`::`でコードブロックにする必要がある。また`\`はエスケープ文字なので、記述する際は`\\`とする
```
Examples:
    関数の使い方について説明

    >>> print_test("test", "message")
        test message
    
    ::

        $ python main.py \\
            "arg1" \\
            "arg2"
```
#### Note
メモ書き
```
Note:
    メモ書き
```
#### Todo
```
Todo:
    TODOリストを記載
```
Shphixでドキュメントに表示させるにはconf.pyを編集し、下記のようにする必要がある。
```
extentions = [ 'shinx.ext.todo' ]
todo_include_todos = True
```

# Sphinx
参考：[Sphinxの使い方．docstringを読み込んで仕様書を生成 - Qiita](https://qiita.com/futakuchi0117/items/4d3997c1ca1323259844)
Sphinxはpydocstringをhtmlのドキュメントとして出力する
## 使用法
1. インストール
    `pip install sphinx`でインストール
2. プロジェクト準備
    以下のようにディレクトリを作る（main.pyが対象プログラム）
    ```
    .
    ├─ docs
    └─ main.py
    ```
    コマンドラインで`shinx-quickstart docs`で`docs`内にファイルが自動生成される。
    また対話形式で設定を行う
    * `Separate source and build directories (y/n) [n]`
        ソースとビルドのディレクトリを分けるかどうか
    * `Project name`
        プロジェクト名。デフォルトなしなので必須
    * `Author name(s)`
        作者名。デフォルトなしなので必須
    * `Project release`
        プロジェクトのバージョン
    * `Project language [en]`
        作成するページの言語。enでも日本語は使える。
    
    結果、以下のようなディレクトリ構造になる
    ```
    .
    └── docs
        ├── Makefile
        ├── _build
        ├── _static
        ├── _templates
        ├── conf.py
        ├── index.rst
        └── make.bat
    ```
    `conf.py`は設定ファイル、`index.rst`はトップページ
3. 設定ファイル編集
    `conf.py`を編集する。
    1. 読み込むpythonファイルの設定
        `docs/conf.py`には以下のコードがコメントアウトされている
        ```python
        # import os
        # import sys
        # sys.path.insert(0, os.path.abspath('.'))
        ```
        これらをアンコメントし、読み込むpythonファイルの場所によって`'.'`の個所を変える。
        例えばconf.pyから見て一つ上のディレクトリのファイルを視る場合は`'../'`になる。
    2. 拡張機能の設定
        `extentions`の個所を以下のように編集
        ```python
        extentions = ['sphinx.ext.autodoc', 'sphinx.ext.napoleon']
        ```
        `autodoc`はdocstringを読み込む機能
        `napoleon`はNumpyスタイルかGoogleスタイルのdocstringをパースする
        `extentions`の下に以下の文面を追加
        ```python
        napoleon_google_docstring = True
        ```
    3. index.rstの編集
        トップページで表示したいモジュールを`index.rst`に追加
        `main`を追加する際は以下
        ```rst
        .. toctree::
            :maxdepth: 2
            :caption: Contents:

            main
        ```
    4. ドキュメントの生成
        以下コマンドで作成
        ```
        $ sphinx-apidoc [options] -o <OUTPUT_PATH> <MODULE_PATH>
        ```
        以下オプション
        |オプション|意味|
        |:--|:--|
        |-F|Sphinxプロジェクトをfullで生成|
        |-H|プロジェクト名を指定|
        |-A|著者を指定|
        |-V|プロジェクトのバージョンを指定|
        |-o|出力先を指定|
        |-f|ファイルの上書き|
        以下例
        ```
        $ sphinx-apidoc -f -o ./docs .
        ```
        次にrstファイルからhtmlファイルを作成
        ```
        $ sphinx-build [options] <sourcedir> <outputdir> [filenames ...]
        ```
        `<sourcedir>`は`conf.py`があるフォルダ（今回は`docs`）
        `<outputdir>`はHTMLファイルの出力先（今回は`docs/_build`）
        以下例
        ```
        $ sphinx-build -b singlehtml ./docs ./docs/_build
        ```
        `-b`の値が`singlehtml`だと単一ページになる。`html`だと複数ページ
        `docs/_build`内に`index.html`が生成される。
        初回時はエラーが発生する場合がある。その場合、`__pycache__`などのキャッシュを削除することで正常に動作する場合がある。
## タイトルの変更
`index.rst`内の
```rst
.. hoge documentation master file, created by
   sphinx-quickstart on Sun Sep  2 18:45:37 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to hoge's documentation!
================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   main

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
```
この中の`Welcome to hoge's documentation!`を書き換え再度`sphinx-build`すると適応される。
### デザインの変更
以下コマンドで`sphinx_rtd_theme`をインストール
```
$ pip install sphinx_rtd_theme
```
`conf.py`の`html_theme`を以下に修正
```
html_theme = 'sphinx_rtd_theme'
```
再度`sphinx-build`すると適用される。