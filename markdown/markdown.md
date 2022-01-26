# markdown cheat sheet

- [markdown cheat sheet](#markdown-cheat-sheet)
  - [テキストの装飾](#テキストの装飾)
    - [見出し](#見出し)
    - [強調](#強調)
    - [位置の調整](#位置の調整)
    - [打消し線](#打消し線)
    - [下線](#下線)
    - [折りたたみ](#折りたたみ)
  - [空行](#空行)
  - [リスト](#リスト)
    - [Disc型](#disc型)
    - [Decimal型](#decimal型)
    - [Definition型](#definition型)
    - [Checkbox型](#checkbox型)
  - [引用](#引用)
  - [水平線](#水平線)
  - [リンク](#リンク)
  - [画像埋め込み](#画像埋め込み)
  - [テーブル記法](#テーブル記法)
    - [テーブル内の改行](#テーブル内の改行)
    - [テーブルのセル結合](#テーブルのセル結合)
      - [横方向](#横方向)
      - [縦方向](#縦方向)
      - [組み合わせ](#組み合わせ)
  - [数式](#数式)
  - [コード](#コード)
  - [改ページ](#改ページ)
  - [その他](#その他)
    - [markdown enhance preview のエラーについて](#markdown-enhance-preview-のエラーについて)
    - [HTMLについて](#htmlについて)
  - [参考資料](#参考資料)

<div style="page-break-before:always"></div>

## テキストの装飾

### 見出し

- \# : H1タグ
- \## : H2 タグ
- \##### : H5タグ

\#の数でインデントや文字の大きさが決まる。

### 強調

_か*で囲むとHTMLのemタグになる。_こんな感じ_
\_\_か\*\*で囲むとHTMLのstrongタグになる。要するにbold。**こんな感じ**

### 位置の調整
Markdownには位置の調整（中央寄せ、右寄せ、左寄せ）する記法はない。そのため、自分でHTMLを記述する。
```html
<div style="text-align: center">
中央寄せする文章
</div>
```
><div style="text-align: center">
>中央寄せする文章
></div>

text-alignのプロパティは以下がある。
|値|表示|
|:-:|:-:|
|center|中央寄せ|
|right|右寄せ|
|left|左寄せ|

### 打消し線

打消し戦を使うには\~\~で囲む。~~こんな感じ~~

### 下線

markdownには記法がない。そのためhtmlでの記述が必要
```html
<u>テキスト</u>
```
<u>テキスト</u>

### 折りたたみ

追加情報としたい内容をdetailsタグで囲む。そして、要約として表示したい文章をsummaryタグで記載する。


<details>
<summary>
ここにたたむ前から表示される文章
</summary>
ここに開いたときに表示される文章.
みたいな
</details>

折りたたんだ部分でMarkdownを使いたい場合は、折りたたまれる部分全体をdivタブで囲む。

<details><summary>すごく長い文章とかプログラムとか</summary><div>

```python
test
```
</div></details>
必ず\<div>の後に空行を入れること

## 空行
htmlの<br />を使う
```html
1行目
<br />
<br />
4行目
```
1行目
<br />
<br />
2行目

## リスト

箇条書きをする

### Disc型

- 文頭に「\*」「\+」「\-」のどれかを入れるとDisc型になる
- 記号の次に空白を忘れないように
- リストを挿入するときは、リストの上下に空行がないと正しく表示されないかもしれない

### Decimal型

1. 文頭に「数字.」を入れるとDecimal型になる。
1. Markdown上では1. 1. 1.でも行ける。表示では1. 2. 3.となる。
1. 「数字.」の次の空白を忘れずに

### Definition型

HTMLのdlタグをそのまま使う。

```html
<dl>
    <dt>リンゴ</dt>
    <dd>赤いフルーツ</dd>
    <dt>オレンジ</dt>
    <dd>橙色のフルーツ</dd>
<dl>
```

みたいにすると

<div>
<dl>
    <dt>リンゴ</dt>
    <dd>赤いフルーツ</dd>
    <dt>オレンジ</dt>
    <dd>橙色のフルーツ</dd>
<dl>
</div>

こうなる。

加えて、Definition型のリストではMarkdown記法が使えない。例えば

```html
<dl>
    <dt>リンゴ</dt>
    <dd>とても**赤い**フルーツ</dd>
</dl>
```

とすると、

<div>
<dl>
    <dt>リンゴ</dt>
    <dd>とても**赤い**フルーツ</dd>
</dl>
</div>

こうなる。

Definition型リスト内では代わりにHTMLタグを使わないといけないので

```html
<dl>
  <dt>リンゴ</dt>
  <dd> とても<strong>赤い</strong>フルーツ </dd>
</dl>
```

<div>
<dl>
  <dt>リンゴ</dt>
  <dd> とても<strong>赤い</strong>フルーツ </dd>
</dl>
</div>

こうなる。
Markdown記法とHTMLタグの対応は以下のようになっている。

|修飾|Markdown|HTML|
|:---:|:---:|:---:|
|ボールド|****|`<strong> </strong>`|
|イタリック|\_ \_|`<em> </em>`|
|コード|\`\`|`<code> </code>`|
|リンク|\[text]\(url)|`<a href="url"> text </a\>`|

### Checkbox型

Disc型の記述の後ろに\[ ]を入れるとチェックボックスができる。チェックが入った状態のボックスを生成するときは\[x]にする。前後にスペースがいる

- [ ] チェック1
- [x] チェック2

なぜかできない

## 引用

>文頭に \> を置くことで引用できる。
>複数行の時は改行のたびにこの記号を置く必要がある。
>引用の中で他のMarkdownを使うこともできる。
>>引用の中で引用もできる。
>
>二重引用を解除するには上みたいに一つ置かないといけない
引用自体はおかなくても行けるけど、視認性のために置いたほうがいい
>一つ空行を置くことで解除できる。

こんな感じに

## 水平線

これらは全部水平線になる

```markdown
* * *
***
- - -
-----
```

こんな感じの線になる
***

## リンク

- リンク付きテキスト
\[リンクテキスト](URL)
これでクリックするとURLに飛ぶテキストが作れる
例：

>Markdown: \[Qiita](http://qiita.com)
>結果: [Qiita](http://qiita.com)

- タイトル付きのリンクを作れる
\[リンクテキスト](URL "タイトル")
この時文面に出てくるのはリンクテキストで、タイトルはマウスホバーすると表示される。
例：

>Markdown: \[Quita](http://qiita.com "Qiita Home")
>結果: [Quita](http://qiita.com "Qiita Home")

- 同じURLのリンクを複数設置
\[リンクテキスト][名前]
\[名前]:URL
これで同じURLへのリンクを複数設置できる
例：
Markdown:
\[ここ][link-1]と\[この][link-1]は同じ。
\[link-1][]もできる
\[link-1]:http:qiita.com
結果：
[ここ][link-1] と [この][link-1] は同じ。
[link-1][] もできる
[link-1]:http:qiita.com
なぜかできないけど

## 画像埋め込み

2パターンある

- タイトルなしの画像
\!\[代替テキスト](画像のURL)
- タイトルありの画像
\!\[代替テキスト](画像のURL "画像のタイトル")

>Markdown: \!\[Qiita](https://qiita-image-store.s3.amazonaws.com/0/45617/015bd058-7ea0-e6a5-b9cb-36a4fb38e59c.png "Qiita")
>結果: ![Qiita](https://qiita-image-store.s3.amazonaws.com/0/45617/015bd058-7ea0-e6a5-b9cb-36a4fb38e59c.png "Qiita")

画像のリサイズをしたい場合は、htmlで記述しないといけない
> \<img src="画像のURL" width=画像のサイズ>

## テーブル記法

```table
| Left align | Right align | Center align |
|:-----------|------------:|:------------:|
| This       | This        | This         |
| column     | column      | column       |
| will       | will        | will         |
| be         | be          | be           |
| left       | right       | center       |
| aligned    | aligned     | aligned      |
```

これがこれになる
| Left align | Right align | Center align |
|:-----------|------------:|:------------:|
| This       | This        | This         |
| column     | column      | column       |
| will       | will        | will         |
| be         | be          | be           |
| left       | right       | center       |
| aligned    | aligned     | aligned      |
二段目の「:」の位置で左寄せ右寄せ中央が決まる。

### テーブル内の改行
テーブルの一つのセル内で改行を行うには`<br>`タグを入れればいい
```markdown
|name|
|:---|
|foobar <br> baz|
```
|name|
|:---|
|foobar <br> baz|

### テーブルのセル結合

markdownのテーブルでもセル結合を行うことができる。
vscodeの「Markdown Preview Enhanced」を使う場合、設定の変更が必要。「Markdown-preview-enhanced: Enable Extended Table Syntax」にチェックを付ける（デフォルトだと向こうになっている)

#### 横方向

横方向に結合するには、何も設定しないか`>`を設定する。文字の位置を指定する場合には、表示する文字のセル位置に注意が必要。

```markdown
|header1|header2|header3|
|:------|:-----:|------:|
|hoge   |fuga   |piyo   |
|hoge   |       |piyo   |
|>      |fuga   |piyo   |
|hoge   |fuga   |       |
|hoge   |>      |piyo   |
|hoge   |       |       |
|>      |fuga   |       |
|>      |>      |piyo   |
```
|header1|header2|header3|
|:------|:-----:|------:|
|hoge   |fuga   |piyo   |
|hoge   |       |piyo   |
|>      |fuga   |piyo   |
|hoge   |fuga   |       |
|hoge   |>      |piyo   |
|hoge   |       |       |
|>      |fuga   |       |
|>      |>      |piyo   |

#### 縦方向

縦方向に結合する場合は`^`を設定する。
```markdown
|header1|header2|header3|
|:------|:-----:|------:|
|hoge   |fuga   |piyo   |
|hoge   |^      |^      |
|hoge   |fuga   |^      |
```
|header1|header2|header3|
|:------|:-----:|------:|
|hoge   |fuga   |piyo   |
|hoge   |^      |^      |
|hoge   |fuga   |^      |

#### 組み合わせ

縦と横を組み合わせることもできる。

```markdown
|header1|header2|header3|
|:------|:-----:|------:|
|hoge   |>      |piyo   |
|hoge   |^      |^      |
|hoge   |^      |^      |
|hoge   |       |piyo   |
|^      |^      |piyo   |
```
|header1|header2|header3|
|:------|:-----:|------:|
|hoge   |>      |piyo   |
|hoge   |^      |^      |
|hoge   |^      |^      |
|hoge   |       |piyo   |
|^      |^      |piyo   |

## 数式

コードブロックに「math」を付けるとTex記法を使って数式をかける。
>\```math
>\left( \sum_{k=1}^n a_k b_k \right)^{!!2} \leq
>\left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
>\```

```math
\left( \sum_{k=1}^n a_k b_k \right)^{!!2} \leq
\left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
```

\$2\^3\$みたいに文中に埋め込むこともできる。すると$2^3$となる。

## コード

コードみたいに埋め込むこともできる。
\`printf()\`みたいにラップすると`printf()`となる。
``これでも行ける``
ブロックで入れることもできるその場合は\`\`\`で囲む。

>\`\`\`ruby:qiita.rb
>puts 'The best way to log and share programmers knowledge.'
>\`\`\`

これが

```ruby:qiita.rb
puts 'The best way to log and share programmers knowledge.'
```

こうなる。

## 改ページ
PDFに変換したときに改ページするには以下のcssを改ページしたいか所に挿入する。

```css
<div style="page-break-before:always"></div>
```
## その他

### markdown enhance preview のエラーについて
プレビューで`<p data-line="232" class="sync-line" style="margin:0;"></p>`と表示されることがあるけど、バグ。PDFに変換するとなくなる。

### HTMLについて
Markdown内でHTMLタグを使うときは、その範囲をdivタグで囲まないとそれ以降が正常に動いてくれない可能性がある。

## 参考資料

- [Markdown記法 チートシート - Qiita](https://qiita.com/Qiita/items/c686397e4a0f4f11683d)

- [Daring Fireball: Markdown Syntax Documentation](https://daringfireball.net/projects/markdown/syntax)
