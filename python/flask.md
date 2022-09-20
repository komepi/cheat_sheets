# flask

## ajax
javascriptからflaskに対してデータを送信する。
javascriptに以下を記述

```javascript
<script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>
<script>
    data = {data1:"data1",data2:"data2"}
    json_data = JSON.stringify(data)
    $.ajax({
        type:"POST",
        url:"/url",
        data:json_data,
        contentType:"application/json",
        success: function(msg){
            console.log(msg)
        },
        error: function(msg) {
            console.log("error")
        }
    })
</script>
```

またflaskには以下を記述

```python
import request
@app.route('/url', methods = 'POST')
def get_data():
    if request.method == 'POST':
        data = request.json
        app.logger.debug(data)
    return data
```

これでjavascriptからpythonのflaskに対して任意にデータを送ることができる。

## requestについて
requestオブジェクトから取得できるパラメータやメソッド、パスは以下の通り
* メソッド・パス情報
    `request.method`:GETやPOSTなどのメソッド
    `request.url`: URL(http://localhost/test?q=1)
    `request.host_url`: ホストURL(http://localhost)
    `request.scheme`: スキーマ(http)
    `request.host`: ホスト(localhost)
    `request.path`: パス名(test)
    `request.query_string`: クエリ文字列(q=1)

* リクエストパラメータ
    `request.args`: クエリ部分を辞書型で取得({"q":"1"})
    `request.form`: formのデータを辞書型で取得
    `request.get_json()`, `request.json`:リクエストボディ。送信時`Content-Type:application/json`での送信が必須

* その他
    `request.accept_charsets`: 受付可能なキャラクタセット
    `request.accept_encoding`: 受付可能なエンコーディング
    `request.accept_languages`: 受付可能な言語
    `request.accept_mimetypes`: 受付可能なMIMEタイプ
    `request.remote_addr`: リモートアドレス
    `request.remote_user`: リモートユーザー
    `request.date`: 日時
    `request.content_type`: コンテントタイプ
    `request.referrer`: 遷移元URL
    `request.get_data()`: ボディ（バイナリ）
    `request.headers`: ヘッダ（辞書型）
    `request.environ`: 環境変数