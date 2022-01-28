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