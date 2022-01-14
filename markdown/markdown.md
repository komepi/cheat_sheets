# markdown cheat sheet

- [markdown cheat sheet](#markdown-cheat-sheet)
  - [Format Text - テキストの装飾](#format-text---テキストの装飾)
    - [Header - 見出し](#header---見出し)
    - [Emphasis - 強調](#emphasis---強調)
    - [Strikethrough - 打消し線](#strikethrough---打消し線)
    - [Details - 折りたたみ](#details---折りたたみ)
  - [Lists - リスト](#lists---リスト)
    - [Disc型](#disc型)
    - [Decimal型](#decimal型)
    - [Definition型](#definition型)
    - [Checkbox型](#checkbox型)
  - [Blockquotes - 引用](#blockquotes---引用)
  - [Horizontal rules - 水平線](#horizontal-rules---水平線)
  - [Links - リンク](#links---リンク)
  - [Images - 画像埋め込み](#images---画像埋め込み)
  - [テーブル記法](#テーブル記法)
  - [数式](#数式)
  - [コード](#コード)
  - [改ページ](#改ページ)
  - [その他](#その他)
  - [参考資料](#参考資料)

<div style="page-break-before:always"></div>

## Format Text - テキストの装飾

### Header - 見出し

- \# : H1タグ
- \## : H2 タグ
- \##### : H5タグ

\#の数でインデントや文字の大きさが決まる。

### Emphasis - 強調

_か*で囲むとHTMLのemタグになる。_こんな感じ_
\_\_か\*\*で囲むとHTMLのstrongタグになる。要するにbold。**こんな感じ**

### Strikethrough - 打消し線

打消し戦を使うには\~\~で囲む。~~こんな感じ~~

### Details - 折りたたみ

追加情報としたい内容をdetailsタグで囲む。そして、要約として表示したい文章をsummaryタグで記載する。
折りたたんだ部分でMarkdownを使いたい場合は、折りたたまれる部分全体をdivタブで囲む。

<details>
<summary>
ここにたたむ前から表示される文章
</summary>
ここに開いたときに表示される文章.
みたいな
</details>

## Lists - リスト

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
<dl>
    <dt>リンゴ</dt>
    <dd>赤いフルーツ</dd>
    <dt>オレンジ</dt>
    <dd>橙色のフルーツ</dd>
<dl>
こうなる。

加えて、Definition型のリストではMarkdown記法が使えない。例えば

```html
<dl>
    <dt>リンゴ</dt>
    <dd>とても**赤い**フルーツ</dd>
</dl>
```

とすると、

<dl>
    <dt>リンゴ</dt>
    <dd>とても**赤い**フルーツ</dd>
</dl>
こうなる。

Definition型リスト内では代わりにHTMLタグを使わないといけないので

```html
<dl>
  <dt>リンゴ</dt>
  <dd> とても<strong>赤い</strong>フルーツ </dd>
</dl>
```

<dl>
  <dt>リンゴ</dt>
  <dd> とても<strong>赤い</strong>フルーツ </dd>
</dl>

こうなる。
Markdown記法とHTMLタグの対応は以下のようになっている。

|修飾|Markdown|HTML|
|:---:|:---:|:---:|
|ボールド|****|\<strong>\</strong>|
|イタリック|\_ \_|\<em>\</em>|
|コード|\`\`|\<code>\</code>|
|リンク|\[text]\(url)|\<a href="url">text\</a>|

### Checkbox型

Disc型の記述の後ろに\[ ]を入れるとチェックボックスができる。チェックが入った状態のボックスを生成するときは\[x]にする。前後にスペースがいる

- [ ] チェック1
- [ ] チェック2

なぜかできない

## Blockquotes - 引用

>文頭に \> を置くことで引用できる。
>複数行の時は改行のたびにこの記号を置く必要がある。
>引用の中で他のMarkdownを使うこともできる。
>>引用の中で引用もできる。
>
>二重引用を解除するには上みたいに一つ置かないといけない
引用自体はおかなくても行けるけど、視認性のために置いたほうがいい
>一つ空行を置くことで解除できる。

こんな感じに

## Horizontal rules - 水平線

これらは全部水平線になる

```markdown
* * *
***
- - -
-----
```

こんな感じの線になる
***

## Links - リンク

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

## Images - 画像埋め込み

2パターンある

- タイトルなしの画像
\!\[代替テキスト](画像のURL)
- タイトルありの画像
\!\[代替テキスト](画像のURL "画像のタイトル")

>Markdown: \!\[Qiita](https://qiita-image-store.s3.amazonaws.com/0/45617/015bd058-7ea0-e6a5-b9cb-36a4fb38e59c.png "Qiita")
>結果: ![Qiita](https://qiita-image-store.s3.amazonaws.com/0/45617/015bd058-7ea0-e6a5-b9cb-36a4fb38e59c.png "Qiita")

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

プレビューで`<p data-line="232" class="sync-line" style="margin:0;"></p>`と表示されることがあるけど、バグ。PDFに変換するとなくなる。

## 参考資料

- [Markdown記法 チートシート - Qiita](https://qiita.com/Qiita/items/c686397e4a0f4f11683d)

- [Daring Fireball: Markdown Syntax Documentation](https://daringfireball.net/projects/markdown/syntax)
