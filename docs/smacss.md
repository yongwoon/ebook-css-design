# SMACSS (Scalable and Modular Architecture for CSS)

CSS Code を役割に応じて category 分けしたのが特徴で、以下 5 つの category についてそれぞれ規則が設けられています

- base
- layout
- module
- state
- theme

OOCSS で語られていた範囲は、SMACSS における module におおよそ該当します。OOCSS がほぼ module のみにしか言及していないのに対し、SMACSS はもっと拡大し、実際に Web site を構築する上では欠かせない base や layout コードの扱い法にまで言及しています。

## Category

### Base rule

- base rule における selector は主に要素系 selector(`body {}` など) となります。
- 注意: base rule にあまり多く定義してしまうと、影響範囲が広くなってしまう
- 「project 内において、各要素が標準としてどのように振る舞うか」 を定義するための rule であるため、**特定の状況下での使用がそうてされる ID selector や class selector は使用できません**
- 同様の理由で、base rule においては `!important` 使用されることもありません。
- SMACSS では、reset css も base rule として含みます。chapter 2 で紹介したような web に公開されている既存の reset css を使用する場合は、その分だけ行数は多くなるでしょう。
- SMACSS では **body の背景色を base rule で設定することを強く推奨さています。**

```css
/* 要素系 Selector の例 */
body {
  background-color: #fff;
}

/* 子結合子(child combinator) の例 */
a > img {
  transition: 0.25s;
}

/* 子孫 Selector の例 */
ul li {
  margin-botton: 10px;
}

/* 擬似 Selector の例 */
a:hover {
  text-decoration: underline;
}
```

### Layout rule

Layout rule は header, main area, sidebar, footer などの web site の大枠を構成する大きな module に対する rule です。 Layout を構成するものの多くは page 内でしか一度しか使用されないことが多いため、ID selector による styling が許容されています。Layout に関わり繰り返し使用される module については、class selector を利用します。
下記が layout rule の code 例です。

```css
#header {
  width: 1080px;
  margin-right: auto;
  margin-left: auto;
  background-colot: #fff;
}

#main {
  width: 1080px;
  margin-right: auto;
  margin-left: auto;
  background-color: #fff;
}

#footer {
  width: 1080px;
  margin-right: auto;
  margin-left: auto;
  background-color: #eee;
}

/* `l-` は　SMACSS　で layout という意味 */
.l-section {
  padding-top: 80px;
  padding-bottom: 80px;
}
```

### Module rule

module rule に該当する module は、先述の layout module 内に配置されることを想定しているものです。以下は例になります。

- 見出し
- button
- card
- navigation
- carousel

module は他の page に移動したり別の layout の中に埋め込んでも、見た目が崩れたりせず、変わらず使用できることが求められます。CSS 設計の肝となる部分であるため、実装の際には 「無駄な Code はないか」, 「この code は他の layout に移動した時に影響しないか」 など、より一層の注意が必要になります。

- 実装する時に気をつけるところ
  - なるべく 「要素系 selector」 を使用しない
  - 特定の context にみだりに依存しない

#### なるべく 「要素系 selector」 を使用しない

- 要素を semantic にする (semantic な要素のみ要素系 selector を使う)
  - [semantic](https://developer.mozilla.org/ja/docs/Glossary/Semantics)
    - 「意味や目的を持たせる」という意味
- 要素系 selector を使用する際は、子 selector を使用する
- div と span 要素には class をつける

[CODE](03-module-rule)

SMACSS の考える semantic 性は、下記の式が成り立つといえます。

div, span 要素などの汎用的な要素 < 見出しなどの意味を持つ要素(semantic) < class 属性がついた要素

#### 特定の context にみだりに依存しない

- 要素系 selector を使用する際は、子孫 selector(ex. `div p`) ではなく 子 selector(ex. `div > p`)

```css
.media {
  display: flex;
  align-items: center;
  font-size: 16px;
  line-height: 1.5;
}

.media > figure {
  flex: 1 1 25%;
  margin-right: 3.33333%;
}

/* この styling は汎用的なので、意図的に子孫 selector のままにします */
.media img {
  width: 100%;
  vertical-align: top;
}

.media-body {
  flex: 1 1 68.33333%;
}

.media > h2 {
  margin-top: 10px;
  font-size: 18px;
  font-weight: bold;
}

.media > h3 {
  margin-top: 10px;
  margin-bottom: 10px;
  font-size: 16px;
  border-bottom: 1px solid #555;
}

.media-sub-text {
  color: #555;
  font-size: 14px;
}
```

### State rule

state とは状態のことで、既存の style を上書き・拡張するために使用されます。「既存の style を上書き・拡張」と聞くと、「先述の module rule 内の sub class と同じなのでは？」と思われるかもしれません。しかし module rule の sub class と state rule の状態 style を明確に区別するために

1. state style は layout や module に割り当てることができ
2. state style は javascript に依存するという意味を持つ

と定義されており、特に重要なのが 2 つです。
state rule の state style は全て 「is-」 の接頭辞が付き、また既存の style を全て上書きして state style が反映されることが期待されるため、必要な場合は `!important` の使用も推奨されています。

### Theme rule

Theme rule は site 内の layout や色、text 処理などを一定の法則に従い上書きするもので、既存のあらゆる styling が上書きの対象となり得ます。
