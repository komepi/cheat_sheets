# 1. pytest
- [1. pytest](#1-pytest)
  - [1.1. ディレクトリ構造](#11-ディレクトリ構造)
  - [1.2. テストの設定](#12-テストの設定)
    - [1.2.1. fixture](#121-fixture)
      - [1.2.1.1. スコープ](#1211-スコープ)
      - [1.2.1.2. 終了処理](#1212-終了処理)
        - [1.2.1.2.1. yieldを使うパターン](#12121-yieldを使うパターン)
        - [1.2.1.2.2. addfinalizerを使うパターン](#12122-addfinalizerを使うパターン)
      - [1.2.1.3. ファクトリ・フィクスチャ](#1213-ファクトリフィクスチャ)
      - [1.2.1.4. パラメタライズド・フィクスチャ](#1214-パラメタライズドフィクスチャ)
    - [1.2.2. パラメータ化したテスト](#122-パラメータ化したテスト)
    - [テストの検証](#テストの検証)
      - [条件： assert](#条件-assert)
      - [エラー： pytest.raises](#エラー-pytestraises)
  - [1.3. オプション](#13-オプション)
  - [1.4. mock, patch](#14-mock-patch)
    - [1.4.1. 基本](#141-基本)
    - [1.4.2. 名前空間の注意](#142-名前空間の注意)
    - [1.4.3. 置き換え](#143-置き換え)
      - [1.4.3.1. メソッド・クラス](#1431-メソッドクラス)
      - [1.4.3.2. 辞書・環境変数](#1432-辞書環境変数)
      - [1.4.3.3. datetime](#1433-datetime)
      - [1.4.3.4. requestsライブラリ](#1434-requestsライブラリ)
    - [1.4.4. mockの実行結果の検証](#144-mockの実行結果の検証)
      - [1.4.4.1. 実行回数](#1441-実行回数)
      - [1.4.4.2. 引数](#1442-引数)

---
## 1.1. ディレクトリ構造
テストは`test_`で始まるpythonファイルに記述する
複数ファイルにテストを分ける際は、`tests`ディレクトリに全テストを格納し、`__init__.py`ファイルも作成する

## 1.2. テストの設定
conftest.pyにテストの設定やfixtureなどを記述する
### 1.2.1. fixture
テストの前処理（DBの登録など）を行うもの
実行するテストの引数にそのフィクスチャ名を記述しているとそのテストの事項前にfixtureが実行される。
fixtureで返り値を設定していれば、テストメソッドでその返り値を使用することもできる
```python
@pytest.fixture()
def make_db():
    # 事前処理
    return db

def test_one(db):
    assert db[0] = "bob
```
fixtureで他のfixtureを使用することもできるし、１つのテストで複数のfixtureを使用したい場合は列挙すればいい
```python
@pytest.fixture()
def make_db():
    # dbのセッティング

@pytest.fixture()
def register_db(make_db):
    # dbにデータの登録

@pytest.fixture()
def create_data()
    # データの作成
    return data

def test_one(register_db, create_data):
    assert create_data == 1
```

また、fixtureはconftest.pyで定義している場合テスト内で共有できる
インポートなしでconftest.pyで定義したfixtureを他のテストpythonプログラムで呼び出すことができる。
#### 1.2.1.1. スコープ
スコープを設定することでそれぞれのfixtureの実行粒度を制御できる。
スコープの種類は以下の４つ。
|スコープ名|実行粒度|
|:--|:--|
|function|テストケースごとに１回実行（デフォルト）|
|class|テストクラス全体で１回実行|
|module|テストファイル全体で１回実行|
|session|テスト全体で１回実行|

以下のように設定する。
```python
@pytest.fixture(scope='module')
def fixture_method():
    # 処理
```

#### 1.2.1.2. 終了処理
##### 1.2.1.2.1. yieldを使うパターン
`yield`を使用することで、テスト終了後の処理を定義できる。
```python
@pytest.fixture()
def db():
    # 1. db作成・データ登録
    yield
    # 2. db削除

def test_db(db):
    # 3. テスト
```
この場合、1⇒3⇒2の順に処理される。
この2の処理はテストの成否や例外の有無に関係なく実行されるため、フィクスチャ内での例外キャッチ等のエラーハンドリングは必要ない（フィクスチャ内での例外は別）
また、function以外のスコープでは、そのフィクスチャを使うテストがすべて実行されてから終了処理が実行される。
##### 1.2.1.2.2. addfinalizerを使うパターン
yieldと比較して特徴は以下の３つ
* 終了処理をコールバック関数として登録
* コールバック関数は複数登録できる
* フィクスチャ内で例外が起きても必ず実行される
```python
class MailClient:
    def close(self):
        print(f'Close smtp connection:', self)

@pytest.fixture
def clients(request):
    ret = []
    for i in range(3):
        client = MailClient()
        request.addfinalizer(client.close)
        ret.append(client)
    return ret

def test_one(clients):
    pass
```
これを実行すると、clientsで設定して分3回の出力が起きる。

#### 1.2.1.3. ファクトリ・フィクスチャ
基本fixtureは固定的なデータを返すものだが、動的なものを返すものが欲しい時がある。
その際はデータを生成するための関数（ファクトリ）を返す方法がある。
```python
@pytest.fixture
def list_factory():
    def factory(lens):
        return [i for i in range(lens)]
    return factory
```

#### 1.2.1.4. パラメタライズド・フィクスチャ
複数の入力値による網羅的なテストを行いたい場合はパラメトライズ・フィクスチャというものがある。
pytest.fixtureのデコレータのparams引数にlistなどのiterableを渡すと、各要素をパラメータとしたfixtureが自動で生成される。
```python
@pytest.fixture(params=[('Tom',10), ('Bob',15), ('Alice',12)])
def student(request):
    return Student(request.param[0], request.param[1])
```

### 1.2.2. パラメータ化したテスト
１つのテストのある変数などをパラメータ化してテストを行うこともできる
```python
@pytest.mark.parametrize(('number', 'expected'), [
    (1, False),
    (2, True),
    (3, False)
])
def test_param(number, expected):
    assert is_prime(number) == expected
```
parametrizeの第一引数で引数名を設定し、それぞれのパラメータを第二引数でlist(tuple)で設定する。
### テストの検証
####  条件： assert
検証したい条件を`assert`で比較する。`if a == b`で`True`だとテストが通り、`False`だと通らないみたいな感じ
```python
def test1():
    assert 1 + 2 == 3
```
#### エラー： pytest.raises
`with pytest.raises(Exception)`でプログラム内でそのエラーが発生したかどうかを検証できる。`raises`の引数に起こってほしいエラーを記述する。
また、以下のようにするとエラーメッセージのテストも可能
```python
def sum_values(a, b):
    return a + b

def test_error_sum():
    with pytest.raises(Exception) as e:
        _ = sum_values(1, 'moji')

    assert str(e.value) == "unsupported operand type(s) for +: 'int' and 'str'"
```
## 1.3. オプション
pytestのオプションは以下の通り
|オプション名|意味|概要|
|:--|:--|:--|
|-v|詳細表示|詳細を表示する|
|-collect-only|テスト一覧|実行されるテストの一覧を表示する|
|-x|終了|テストが失敗したタイミングでテストを終了する|
|-maxfaul={num}|終了（回数）|num回テストが失敗したら終了する|
|-s|標準出力|テスト実行中にprint文の出力を標準出力に書き出す|
|-lf|失敗テスト|失敗しているテストのみ実行する|

## 1.4. mock, patch
参考：[【pytest】モックの使い方まとめ](https://zenn.dev/re24_1986/articles/0a7895b1429bfa)
pytest-mockというモジュールがある。
pytest-mockはunittest.mockのラッパー。私はこっちのが書き方がすっきりしてて好き

### 1.4.1. 基本

mocker.patchを使ってコード内の任意の関数をモックし、返り値を設定する。

```python
def func_main():
    return func_hoge()

def func_hoge():
    return [x for x in range(10)]
```
上のようなコードがあったとする
func_hogeをモックしてfunc_mainをテストする

```python
def test_func_main(mocker):
    mocker.patch("main.func_hoge", return_value=[1, 2])
    print(main.func_main()) # [1,2]
```
mockerはpytest-mockをインストールするとfixtureで使えるようになる（インポートする必要はなし）

### 1.4.2. 名前空間の注意
モジュールのインポートの仕方によって指定方法が変わる
```python
# main.py
# 1. import ～の場合
import os
def func_main():
    return os.getcwd()
```
```python
# main.py
# 2. from ～ import ～の場合
from os import getcwd
def func_main():
    return getcwd()
```

```python
def test_func_main(mocker):
    mocker.patch("os.getcwd", return_value="/var/app")
    mocker.patch("main.getcwd", return_value="/var/app")
```
importのみでのインポートだとそのまま、from込みのインポートだと、そのモジュール内のgetcwdといったイメージ
参考：[Python の mock.patch のハマりやすい挙動についてまとめる - Qiita](https://qiita.com/Chanmoro/items/69f401ddbe41e818a8cf)


### 1.4.3. 置き換え
#### 1.4.3.1. メソッド・クラス
```python
# main.py
def  func_main():
    s = Sample()
    print("")
    print("ライブラリ呼び出し:{}".format(os.getcwd()))
    print("関数呼び出し:{}".format(func_sample()))
    print("インスタンスメソッド呼び出し:{}".format(s.half_value()))
    print("プロパティ呼び出し:{}".format(s.value))
    print("クラスメソッド呼び出し:{}".format(Sample.get_cvalue()))
    print("スタティックメソッド呼び出し:{}".format(Sample.get_svalue()))
    print("")

def func_sample():
    return 0

class Sample:
    __cvalue = 30
    
    def __init__(self):
        self.__value = 20
    
    def half_value(self):
        return int(self.value / 2)
    
    @property
    def value(self):
        return self.__value
    
    @classmethod
    def get_cvalue(cls):
        return cls.__cvalue
    
    @staticmethod
    def get_svalue():
        return 40
```
```python
# test_main.py
def test_func_main(mocker):
    m1 = mocker.patch("os.getcwd", return_value="/var/app")
    m2 = mocker.patch("main.func_sample", return_value = 1)
    m3 = mocker.patch("main.Sample.half_value", return_value=11)
    m4 = mocker.patch("main.Sample.value", return_value=21, new_callable=mocker.PropertyMock)
    m5 = mocker.patch("main.Sample.get_cvalue", return_value=31)
    m6 = mocker.patch("main.Sample.get_svalue", return_value=41)
    main.func_main()
```

また各パッチの返り値は以下のように設定する。

1. 常に同じ値
    ```python
    mocker.patch("main.func_sample", return_value=1)
    ```
2. 実行回数によって変化
    ```python
    mocker.patch("main.func_sample", side_effect=[10, 20, 30])
    ```
    1回目は10、2回目は20、3回目は30といった形でリスト内の値が順番に返される。
3. 引数によって変わる
    ```python
    mocker.patch("main.func_sample", side_effect=lambda x: False if x % 2 else True)
    ```
    side_effectはラムダ式も指定できる。
4. 例外
    ```python
    mocker.patch("main.func_sample", side_effect=Exception("new exception"))
    ```
    side_effectでエラーを返すことができる。

####  1.4.3.2. 辞書・環境変数
mocker.patch.dictを使うことで変数に代入された辞書を差し替えることができる
```python
d = {"a": 10, "b": 20, "c": 30}
def func_main():
    print(d)
```
```python
def test_func_main(mocker):
    # 一部書き換え
    mocker.patchdict("main.d", {"c": 40}))
    main.func_main() # {"a": 10, "b": 20, "c": 40}

    # 既存データをすべて書き換え
    mocker.patch.dict("main.d", {"c": 40}, clear=True)
    main.func_main() # {"c": 40}
```
また同様に環境変数をパッチすることもできる
環境変数は`os.environ`に辞書型で格納されていると考え、以下のようにパッチする
```python
mocker.patch.dict("os.environ", {"NEW_KEY":"newvalue"})
```

#### 1.4.3.3. datetime

datetimeは他と同じようにpatchしようとするとエラーが出る
1. freezegunを使う
    パッケージの一つ
    `pytest-freezegun`をインストールして以下のように記述すると、`datetime.datetime.now()`で取得できる日付を固定できる
    ```python
    def func():
        return datetime.datetime.now()

    @pytest.mark.freeze_time(datetime.datetime(2000, 1, 2, 3, 4))
    def test_func(mocker):
        assert func() == datetime.datetime(2000, 1, 2, 3, 4)
    ```
2. datetimeオブジェクト自体を差し替え、各オブジェクトを設定する
    ```python
    def func():
        return datetime.datetime.now()
    
    def test_func(mocker):
        # 差し替える前に予測値を取得しておく
        expect = datetime.datetime(2000, 1, 2, 3, 4)
        mocker.patch("datetime.datetime", **{
            "now.return_value": datetime.datetime(2000, 1, 2, 3, 4)
        })
        assert func() == expect
    ```
3. 日付返却用のラッパー関数を作成する
    このためだけにパッケージを増やすのもいや、2の記述を毎回書くのも汚い、といった場合はこういった方法もある。
    ただ、本番環境のコードの書き方に関わってくるため、他人が作ったコードをテストする際などには使用できない
    ```python
    # main.py
    def get_now():
        return datetime.datetime.now()
    
    def func():
        return get_now()
    ```
    ```python
    # test_main.py
    def test_func_main(mocker):
        mocker.patch("main.get_now", return_value=datetime.datetime(2000, 1, 2, 3, 4))
        assert main.func() == datetime.datetime(2000, 1, 2, 3,4)
    ```

#### 1.4.3.4. requestsライブラリ
```python
# main.py
def func(url):
    res = requests.get(url)
    ras.raise_for_status()

    return res.text
```
```python
# test_main.py
def mock_requests_get(*args, **kwargs):
    class MockResponse:
        def __init__(self, text, status_code):
            self.__text = text
            self.__status_code = status_code
        @property
        def text(self):
            return self.text
        def raise_for_status(self):
            if self.__status_code != 200:
                raise Exception("requests error")
    
    if args[0] == 'url1':
        return MockResponse("Hello World!", 200)
    elif args[0] == 'url2':
        return MockResponse("goodbye World!", 200)
    
    return MockResponse(None, 404)

def test_func(mocker):
    mocker.patch("requests.get", side_effect=mock_requests_get)
    print(main.func('url1')) # Hello World!
    print(main.func('url2')) # goodbye World!
    print(main.func("")) # 例外が発生
```

### 1.4.4. mockの実行結果の検証
モックが呼ばれたかどうかなど、モックに関する実行結果を確かめることができる。
確認をしたいモックを以下のように定義し、テスト対象のメソッドをよびだしているとする。
```python
m = mocker.patch("main.func_sample", return_value=1)
main.func_main()
```
モックの実行結果の確認をするには、対応するメソッドを`m.method(param)`の形で呼び出す。
#### 1.4.4.1. 実行回数
```python
assert m.call_count == 3 # m.call_count:呼ばれた回数
m.assert_called() # 少なくとも１回呼ばれた
m.assert_called_once() # 1回だけ呼ばれた
m.assert_not_called() # 1回も呼ばれていない
```
#### 1.4.4.2. 引数
```python
# 最後に実行したモックの引数
# main(1, "test", [2,3,4])で呼び出したか確認する場合
m.assert_called_with(1, "test", [2,3,4])
# 最後に実行したモックの引数（キーワード混在）
# main(1, x="test", y=3)で呼び出したかの確認
m.assert_called_with(1, x="test", y=3)
# 指定した引数で1度だけ呼ばれたか
m.assert_called_once_with(1,2,3)
# 指定した引数で1度でも呼ばれたか
m.assert_any_call(2)

# 複数実行時に、それぞれの引数で順に実行したかどうかを検証
# 1回目：1,2,3   2回目:4,5,6   3回目:7,8,9で呼び出されたかどうかの確認
# ※順に引数をチェックしているだけなので、実行回数はチェックしていない。
#   上に加え4回目何かで呼ばれていても通る
m.assert_has_calls(
    [mocker.call(1,2,3),
    mocker.call(4,5,6),
    mocker.call(7,8,9)]
)
# any_order=Trueを指定すると順不同になる
m.assert_has_calls(
    [mocker.call(1,2,3),
    mocker.call(4,5,6),
    mocker.call(7,8,9)],
    any_order=True
)

# 最後に実行した引数を取得（市引数、キーワード引数それぞれのタプルで取得）
args, kwargs = m.call_args
# 実行した順に引数リストを取得
args_list = m.call_args_list
for args, kwargs in args_list:
    pass
```
