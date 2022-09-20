- [schema定義： marshmallow](#schema定義-marshmallow)
  - [model定義](#model定義)
  - [schema定義](#schema定義)
    - [Metaクラス](#metaクラス)
    - [fields](#fields)
  - [読み込み/書き込み/検証](#読み込み書き込み検証)
    - [シリアル化： dump](#シリアル化-dump)
    - [読み込み](#読み込み)
    - [検証](#検証)
    - [その他](#その他)
  - [検証](#検証-1)
  - [フィールドのカスタム](#フィールドのカスタム)


# schema定義： marshmallow
公式:[marshmallow: 簡素化されたオブジェクトのシリアル化 — marshmallow 3.17.0 ドキュメント](https://marshmallow.readthedocs.io/en/stable/index.html)
marshmallowはschema定義用のpythonライブラリ
使用することでschemaを定義してpython以外のデータ（jsonなど）を読み込める
以下のようにインストールすれば使用可能`
```cmd
$ pip install marshmallow
```
marshmallow配下の順序で使用する
1. model定義
2. schema定義
3. 読み込み/書き出し


## model定義
シリアライズ、デシリアライズする対象となる。
例：
```python
class User:
  def __init__(self,name,age,email):
    self.name=name
    self.age=age
    self.email=email
    self.created=created=datetime.datetime.now()
```


## schema定義
```python
from marshmallow import Schema, fileds, post_load, validates, ValidationError

class UserSchema(Schema):
  name=fields.Str()
  age=fields.Int()
  email=fields.Email()
  created=fields.DateTime()
  
  @post_load
  def make_user(self,data,**kwargs): # デシリアライズする際に使用
    return User(**data)
  
  @validates("age") # 検証用。必須ではない
  def validate_age(self,value):
    if value < 0:
      raise ValidationError("age must be greater than 0.")
    if value > 100:
      raise ValidationError("age must not be greater than 100.")
```
もしくは`from_dict`メソッドによって辞書型で作成
```python
UserSchema = Schema.from_dict(
  {
    "name": fileds.Str(),
    "age": fields.Int(),
    "email": fields.Email(),
    "created": fields.DateTime()
  }
)
```
基本的には`filed名=fields.{クラス}`で定義する。クラスの一覧は[こちら](https://marshmallow.readthedocs.io/en/stable/marshmallow.fields.html#api-fields)
またフィールドに他のスキーマを使用したい時などは以下のように
```python
class SubSchema(Schema):
  sub_fields1=fields.Str()
  sub_fields2=fields.Bool()

class MainSchema(Schema):
  main_fields1=fields.Float()
  sub_fields=fields.Nested(SubSchema())
```
例えば以下のように、クラス`Blog`が`User`を使用する場合
```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
        self.created_at = dt.datetime.now()

class Blog:
    def __init__(self, title, author):
        self.title = title
        self.author = author  # A User object

class UserSchema(Schema):
    name = fields.String()
    email = fields.Email()
    created_at = fields.DateTime()


class BlogSchema(Schema):
    title = fields.String()
    author = fields.Nested(UserSchema)

user = User(name="Monty", email="monty@python.org")
blog = Blog(title="Something Completely Different", author=user)
result = BlogSchema().dump(blog)
pprint(result)
# {'title': u'Something Completely Different',
#  'author': {'name': u'Monty',
#             'email': u'monty@python.org',
#             'created_at': '2014-08-17T14:58:57.600623+00:00'}}
```
### Metaクラス
フィールドをそれぞれ定義せずに、Metaクラス内で指定することでmarshmallowが自動で型を作成する
```python
class UserSchema(Schema):
    uppername = fields.Function(lambda obj: obj.name.upper())

    class Meta:
        fields = ("name", "email", "created_at", "uppername")
```
このとき`name`は`String`で、`created_at`は`DateTime`で自動でフォーマットされる。
`fields`での指定ではフィールドで指定しているものも含む必要があるが、`additional`ではフィールドで定義しているものに加えるものを指定できる。
```python
class UserSchema(Schema):
    uppername = fields.Function(lambda obj: obj.name.upper())

    class Meta:
        # No need to include 'uppername'
        additional = ("name", "email", "created_at")
```

デフォルトではスキーマ内で一致しないキーに遭遇しloadがされると`ValidationError`が発生する。
以下の`unknown`にこれらオプションを渡すことでこの挙動を変更できる。
* RAISEValidationError:デフォルト。ValidationErrorを投げる
* EXCLUDE:不明なフィールドを除外
* INCLUDE:不明なフィールドを受け入れて含める
`Meta`クラスでこのオプションのデフォルトを指定できる
```python
class TESTSchema(Schema):
  class Meta:
    unknown = INCLUDE
```
なおこのオプションはインスタンス化やロード時に`schema=TESTSchema(unknown=EXCLUDE)`のようにしてオーバーライドできる。

またフィールドの順序を維持するには、`orderd`オプションを`True`に設定する。
これにより`dump`などをした際に`collections.OrderedDict`で返される。
```python
class UserSchema(Schema):
    first_name = fields.String()
    last_name = fields.String()
    email = fields.Email()

    class Meta:
        ordered = True


u = User("Charlie", "Stones", "charlie@stones.com")
schema = UserSchema()
result = schema.dump(u)
assert isinstance(result, OrderedDict)
pprint(result, indent=2)
#  OrderedDict([('first_name', 'Charlie'),
#              ('last_name', 'Stones'),
#              ('email', 'charlie@stones.com')])
```


### fields
それぞれのフィールドに設定を付け加えることができる。
1. 必須
  `required=True`で必須項目として設定できる。これにより`Schema.load`した際に不足しているとエラーが出る。
  `error_messages`でそのエラー文をカスタマイズできる。辞書型でエラーメッセージ、もしくはエラーコードなどを設定する。
    ```python
    field1=fields.String(required=True)
    field2=fields.Integer(required=True, error_messages={"required":"this is required."})
    field3=fields.String(required=True, error_messages={"required":{"message":"this is required.","code":400}})
    ```
2. デフォルト
  `load_default`でデフォルトの逆シリアル化の値を指定、`dump_default`でデフォルトのシリアル化の値を指定する。
    ```python
    field1=fileds.UUID(load_default=uuid.uuid1)
    field2=fileds.DateTime(dump_default=datetime.datetime(2020,1,1))
    ```
3. 読み取り、書き込み専用
  `dump_only`で読み取り専用、`load_only`で書き込み専用フィールドにできる。
    ```python
    password=fields.Str(load_only=True)
    created=fields.DateTime(dump_only=True)
    ```
4. シリアライズ、デシリアライズ時のキー
    ```python
    class UserSchema(Schema):
       name = fields.String()
       email = fields.Email(data_key="emailAddress")
    s = UserSchema()
    data = {"name": "Mike", "email": "foo@bar.com"}
    result = s.dump(data)
    # {'name': u'Mike',
    # 'emailAddress': 'foo@bar.com'}

    data = {"name": "Mike", "emailAddress": "foo@bar.com"}
    result = s.load(data)
    # {'name': u'Mike',
    # 'email': 'foo@bar.com'}
    ```
5. 検証
  引数に`validate`を渡すことでフィールドの検証ができる。
  この時渡すものは`marshmallow.validate`やオリジナルのメソッド。検証の種類は[こちら](https://marshmallow.readthedocs.io/en/stable/marshmallow.validate.html#api-validators)
    ```python
    def validate_quantity(n):
      if n < 0:
          raise ValidationError("Quantity must be greater than 0.")
      if n > 30:
          raise ValidationError("Quantity must not be greater than 30.")
    class UserSchema(Schema):
      name = fields.Str(validate=validate.Length(min=1))
      permission = fields.Str(validate=validate.OneOf(["read", "write", "admin"]))
      age = fields.Int(validate=validate.Range(min=18, max=40))
      quantity = fields.Integer(validate=validate_quantity)
    ```


## 読み込み/書き込み/検証
以下使用するモデルとスキーマは以下とする。
```python
import datetime as dt
from marshmallow import Schema, fields
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email
        self.created_at = dt.datetime.now()

    def __repr__(self):
        return "<User(name={self.name!r})>".format(self=self)

class UserSchema(Schema):
    name = fields.Str()
    email = fields.Email()
    created_at = fields.DateTime()
```


### シリアル化： dump
オブジェクトのスキーマをメソッドに渡してシリアル化する。

```python
user = User(name="Monty", email="monty@python.org")
schema = UserSchema()
result = schema.dump(user)
pprint(result)
# {"name": "Monty",
#  "email": "monty@python.org",
#  "created_at": "2014-08-17T14:54:16.049594+00:00"}
```
`dumps`によってJSONでエンコードされた文字列にもできる。
```python
json_result = schema.dumps(user)
pprint(json_result)
# '{"name": "Monty", "email": "monty@python.org", "created_at": "2014-08-17T14:54:16.049594+00:00"}'
```
`only`でフィールドを指定することで出力するフィールドを制限できる。
```python
summary_schema = UserSchema(only=("name", "email"))
summary_schema.dump(user)
# {"name": "Monty", "email": "monty@python.org"}
```


### 読み込み
`load`を使用することで辞書型をモデルに変換する。
```python
from pprint import pprint

user_data = {
    "created_at": "2014-08-11T05:26:03.869245",
    "email": "ken@yahoo.com",
    "name": "Ken",
}
schema = UserSchema()
result = schema.load(user_data)
pprint(result)
# {'name': 'Ken',
#  'email': 'ken@yahoo.com',
#  'created_at': datetime.datetime(2014, 8, 11, 5, 26, 3, 869245)},
```
辞書型で返していて、`created_at`が文字列から`datetime.datetime`型になっている。
またこの時デコレータに`post_load`を付けたメソッドを追加することで、それに従って処理を行う。
```python
class UserSchema(Schema):
    name = fields.Str()
    email = fields.Email()
    created_at = fields.DateTime()

    @post_load
    def make_user(self, data, **kwargs):
        return User(**data)
user_data = {"name": "Ronnie", "email": "ronnie@stones.com"}
schema = UserSchema()
result = schema.load(user_data)
print(result)  # => <User(name='Ronnie')>
```


### 検証
`load`や`loads`を行ったときにフィールドが存在しないなどの無効なデータが渡されるとエラーが出る。
デフォルトだと`ValidationError`。`ValidationError`は`message`でメッセージ、`valid_data`で対象のデータが取得できる。
```python
from marshmallow import ValidationError

try:
    result = UserSchema().load({"name": "John", "email": "foo"})
except ValidationError as err:
    print(err.messages)  # => {"email": ['"foo" is not a valid email address.']}
    print(err.valid_data)  # => {"name": "John"}
```


### その他
1. 反復処理
  読み込み、書き込み対象が複数の場合、`many`を指定することでリスト化された対象すべてに対して処理ができる。
    ```python
    user1 = User(name="Mick", email="mick@stones.com")
    user2 = User(name="Keith", email="keith@stones. com")
    users = [user1, user2]
    schema = UserSchema(many=True)
    result = schema.dump(users)  # OR UserSchema(). dump(users, many=True)
    pprint(result)
    # [{'name': u'Mick',
    #   'email': u'mick@stones.com',
    #   'created_at': '2014-08-17T14:58:57.600623 +00:00'}
    #  {'name': u'Keith',
    #   'email': u'keith@stones.com',
    #   'created_at': '2014-08-17T14:58:57.600623 +00:00'}]
    ```


## 検証
`dump`や`load`なしに入力データの検証のみが必要な場合は`Schema.validate()`で検証できる。
```python
errors = UserSchema().validate({"name": "Ronnie", "email": "invalid-email"})
print(errors)  # {'email': ['Not a valid email address.']}
```

## フィールドのカスタム
スキーマで定義するフィールドはいかの3種類のカスタムの仕方がある。
* カスタムFieldクラスの作成
* Methodフィールドの使用
* Functionフィールドの使用
1.  カスタムFieldクラスの作成
  `fields.Field`を継承したサブクラスを作成し、`_serialize`メソッド、`_deserialize`メソッドを定義する。
  また`default_error_message`を定義することでデフォルトのエラーメッセージを設定できる。
  またこれは`fields.Date`や`fields.Int`などを継承してもいい。
    ```python
    class PinCode(fields.Field):
        """Field that serializes to a string of numbers and deserializes
        to a list of numbers.
        """
        default_error_message = {"invalid": "Please provide a valid date"}

        def _serialize(self, value, attr, obj, **kwargs):
            if value is None:
                return ""
            return "".join(str(d) for d in value)

        def _deserialize(self, value, attr, data, **kwargs):
            try:
                return [int(c) for c in value]
            except ValueError as error:
                raise ValidationError("Pin codes must contain only digits.") from error
    ```
2. Methodフィールド
  スキーマのメソッドによって返される値にシリアル化される。`obj`はその受け取ったパラメータを指す。
    ```python
    class UserSchema(Schema):
      name = fields.String()
      email = fields.String()
      created_at = fields.DateTime()
      since_created = fields.Method("get_days_since_created")

      def get_days_since_created(self, obj):
          return dt.datetime.now().day - obj.created_at.day
    ```
3. Functionフィールド
  引数に一つのメソッドを受け取る。`obj`は受け取ったパラメータを指す。
    ```python
    class UserSchema(Schema):
      name = fields.String()
      email = fields.String()
      created_at = fields.DateTime()
      uppername = fields.Function(lambda obj: obj.name.upper())
    ```

Method, Functionフィールドではシリアル化する際のメソッドを指定するが、`deserialize`を使用することで、デシリアライズに使用するメソッドを指定することもできる。
```python
class UserSchema(Schema):
    # `Method` takes a method name (str), Function takes a callable
    balance = fields.Method("get_balance", deserialize="load_balance")

    def get_balance(self, obj):
        return obj.income - obj.debt

    def load_balance(self, value):
        return float(value)

schema = UserSchema()
result = schema.load({"balance": "100.00"})
result["balance"]  # => 100.0
```

`context`を用いてスキーマに状態を持たせることができる。
```python
class UserSchema(Schema):
    name = fields.String()
    # Function fields optionally receive context argument
    is_author = fields.Function(lambda user, context: user == context["blog"].author)
    likes_bikes = fields.Method("writes_about_bikes")

    def writes_about_bikes(self, user):
        return "bicycle" in self.context["blog"].title.lower()


schema = UserSchema()

user = User("Freddie Mercury", "fred@queen.com")
blog = Blog("Bicycle Blog", author=user)

schema.context = {"blog": blog}
result = schema.dump(user)
result["is_author"]  # => True
result["likes_bikes"]  # => True
```
