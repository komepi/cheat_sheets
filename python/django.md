# django
webページなどを作成するフレームワーク
[公式ドキュメント](https://docs.djangoproject.com/ja/4.0/)

## プロジェクトの作成
まず以下コマンドを実行
```
$ django-admin startproject [project name]
```
するとproject nameのディレクトリが作成される

## サーバーの起動
以下コマンドを打ち込んでサーバーを起動させる
```
$ python manage.py runserver
```

## アプリケーションの作成
以下コマンドを実行
```
$ python manage.py startapp [app name]
```

## models
データベースのモデルを設定する
共通オプションは以下
|オプション名|説明|デフォルト|
|:--|:--|:--|
|verbose_name|入力欄のタイトル|なし|
|blank|ブランク（空白）入力|False(不可)|
|null|ヌル制約|False(Not null)|
|default|デフォルト値|なし|
|editable|フォームでの表示|True(表示する)|
|max_length|最大文字長|フィールド別|
|help_text|ツールチップの内容|なし|