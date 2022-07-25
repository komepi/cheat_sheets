# websocket
httpにおいて双方向通信を行う仕組み。チャットとかに使われているみたい。
基本的にサーバーとクライアント間でコネクションをつなぎ、接続が閉じられるまで通信を行う。
## Flaskでの実装

Flaskでwebsocketを扱う拡張機能のflask-socketioを使用
以下、テキストボックスの入力が同期されているwebブラウザのサンプルコード

* pythonサンプルコード
    ```python
    # flaskのwebsocketを用いたサンプルコード
    # アイコンのパスが通ってない

    from flask import Flask, render_template, request,url_for
    from flask_socketio import SocketIO, send, emit

    app = Flask(__name__)
    app.config['SECRET_KEY'] = 'secret!'

    # cors_allowed_originは本来適切に設定するべき
    socketio = SocketIO(app, cors_allowed_origins='*')

    # ユーザー数
    user_count = 0
    # 現在のテキスト
    text = ""

    @app.route('/')
    def index():
        return render_template('index.html')

    # ユーザーが新しく接続すると実行
    @socketio.on('connect')
    def connect(auth):
        global user_count, text
        user_count += 1
        # 接続者数の更新（全員向け）
        emit('count_update', {'user_count': user_count}, broadcast=True)
        # テキストエリアの更新
        emit('text_update', {'text': text})


    # ユーザーの接続が切断すると実行
    @socketio.on('disconnect')
    def disconnect():
        global user_count
        user_count -= 1
        # 接続者数の更新（全員向け）
        emit('count_update', {'user_count': user_count}, broadcast=True)


    # テキストエリアが変更されたときに実行
    @socketio.on('text_update_request')
    def text_update_request(json):
        global text
        text = json["text"]
        # 変更をリクエストした人以外に向けて送信する
        # 全員向けに送信すると入力の途中でテキストエリアが変更されて日本語入力がうまくでき  ない
        emit('text_update', {'text': text}, broadcast=True, include_self=False)

    @socketio.on("send_notification_request")
    def send_notification(json):
        msg = json["text"]
        num = json["num"]
        emit("send_notification",
             {"title":msg,
              "body":"this is push notification.",
              "icon":"images/icon.png",
              "tag":"",
              "data1":num
              },
             broadcast=True)

    if __name__ == '__main__':
        # 本番環境ではeventletやgeventを使うらしいが簡単のためデフォルトの開発用サーバー    を使う
        socketio.run(app, debug=True)
    ```
* htmlサンプル
    ```html
    <html>
      <head>
        <title>FlaskでWebSocket</title>
      </head>
      <body>
        <p>現在の接続者数<span id="user_count"></span>人</p>
        <textarea id="text" name="text" rows="10" cols="60"></textarea>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.0.1/socket. io.js" integrity="sha512-q/  dWJ3kcmjBLU4Qc47E4A9kTB4m3wuTY7vkFJDTZKjTs8jhyGQnaUrxa0Ytd0ssMZhbNua9hE   +E7Qv1j+DyZwA==" crossorigin="anonymous"></script>
        <script src="https://code.jquery.com/jquery-3.6.0.min.js"   integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4="   crossorigin="anonymous"></script>
        <input id = "notification" type="button" value="通知送信"/>
        <script type="text/javascript" charset="utf-8">
          var socket = io();

          // 接続者数の更新
          socket.on('count_update', function(msg) {
            $('#user_count').html(msg.user_count);
          });

          // テキストエリアの更新
          socket.on('text_update', function(msg) {
            $('#text').val(msg.text);
          });

          // テキストエリアが変更されると呼び出される
          $('#text').on('change keyup input', function() {
            socket.emit('text_update_request', {text: $(this).val()});
          });

          $('#notification').click(function(){
            socket.emit("send_notification_request",{text: $("#text").val(),num: $  ("#user_count").val()})
          })
          socket.on("send_notification", function(msg) {
            Notification.requestPermission()

            var notify = new Notification(msg.title,{
                body:msg.body,
                icon:msg.icon,
                tag:msg.tag,
                data:{
                    data1:msg.data1
                }
            })
          })
        </script>

      </body>
    </html>
    ```

emit関数を用いてjavascript、flask間の通信を行う。(send関数というのもある。）
>```javascript
>$('#text').on('change keyup input', function() {
>  socket.emit('text_update_request', {text: $(this).val()});
>});
>```
\#textが変更されたとき上の関数が発火し、`socket.emit('text_update_request', ～)`が起こる。これによってapp.pyのtext_update_request関数が発火。text_update_request関数の引数としてemit関数の第二引数が渡される。

>```python
>@socketio.on('text_update_request')
>    def text_update_request(json):
>        global text
>        text = json["text"]
>        # 変更をリクエストした人以外に向けて送信する
>        # 全員向けに送信すると入力の途中でテキストエリアが変更されて日本語入力がうまくできない
>        emit('text_update', {'text': text}, broadcast=True, include_self=False)
>```

`@socketio.on`でjavascriptの関数と一致させる。emit関数でまたjavascriptのtext_update関数を発火させる。broadcastはすべてのクライアントに対して送信するオプション、include_selfはbroadcastがTrueの時に送信者を含めるかを定めるオプション

>```javascript
>socket.on('text_update', function(msg) {
>  $('#text').val(msg.text);
>});
>```
pythonからtext_updateでemitされたためjavascriptのこの関数が発火。

以上のように、基本的には以下のような順序になると思う
1. javascriptでボタンを押されるなどの動作を検知
2. flaskに対してリクエストの送信
3. flaskで処理を行い、htmlに対して引数付きで送信
4. javascriptがflaskからの引数を受け取り、それに応じて処理

場合によっては1. 2. が省かれることもありそう。

また、注意点として必ずhtml(javascript)でSocket.ioに接続する必要がある。サンプルコードでそれを担っているのは以下の部分
>```html
><script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/4.0.1/socket. io.js" integrity="sha512-q/  
>```

### 他サーバーからsocketサーバーへの接続
socketサーバーに対して他のサーバーから接続することもできる。

* Notificationについて
プロパティは以下の通り

|プロパティ名|説明|
|:--|:--|
|title|通知のタイトル|
|body|通知の本文|
|icon|通知のアイコン。パスを指定|
|tag|同じタグが複数短期間に送られた場合、その中で最も新しいものを表示する|
|data|データ|

* 通知を閉じる
    close()を使って通知を消去できる。
    ただしすでにその通知を見ているときなどの場合のみ。4sたったから、みたいな使い方は違う。
* イベント
Notificationでは以下のイベントを定義している
    * click
        ユーザーが通知をクリックしたときに発生
    * close
        通知が閉じられたときに発生
    * error
        通知でエラーが発生したときに発生。通常は何らかの理由で通知が表示されなかったため。
    * show
        通知がユーザーに表示されたとき発生

    これらは`onclick`、`onclose`などのハンドラを使って追跡できる。


### 検討事項

サーバーの起動がsocketio.runで行うものしか見つけられない。多分普段と違う通信（双方交通）を行っているからだと思う。

### 参照
[FlaskでWebSocketを使う - kivantium活動日記](https://kivantium.hateblo.jp/entry/2021/10/18/110509)
[通知 API の使用 - Web API | MDN](https://developer.mozilla.org/ja/docs/Web/API/Notifications_API/Using_the_Notifications_API)
[Flask-SocketIO — Flask-SocketIO documentation](https://flask-socketio.readthedocs.io/en/latest/)