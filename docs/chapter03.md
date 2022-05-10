# Chapter 03

## OOCSS(Oritented Object CSS)

### Thinking

- lego ように自由に組み合わせが可能な module の集まりを作ろう
- その module の組み合わせで page を作成しよう
- そのため新規 page を作るときも、基本的に追加の CSS を書く必要がない

### Lego のような module を実現するための手法

- structure と skin の分離
  - OOCSS には明確に決められているわけではないが、以下のように分けられる
  - structure にあたる property 例
    - width
    - height
    - padding
    - margin
  - skin にあたる property 例
    - color
    - font
    - background
    - box-shadow
    - text-shadow
- container(area) と contents の分離
  - module をなるべく特定の area に依存させない

### sample code

- `01-split-structure-skin`
- `02-split-container-content`

### link

- [oocss](http://oocss.org/)
- [what-is-object-oriented-css](https://www.slideshare.net/stubbornella/what-is-object-oriented-css)
- [object-oriented-css](https://www.slideshare.net/stubbornella/object-oriented-css)

## SMACSS (Scalable and Modular Architecture for CSS)

CSS Code を役割に応じて category 分けしたのが特徴で、以下 5 つの category についてそれぞれ規則が設けられています

- base
- layout
- module
- state
- theme

OOCSS で語られていた範囲は、SMACSS における module におおよそ該当します。OOCSS がほぼ module のみにしか言及していないのに対し、SMACSS はもっと拡大し、実際に Web site を構築する上では欠かせない base や layout コードの扱い法にまで言及しています。

### Category

#### Base rule

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

#### Layout rule

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

#### Module rule

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

##### なるべく 「要素系 selector」 を使用しない

- 要素を semantic にする (semantic な要素のみ要素系 selector を使う)
  - [semantic](https://developer.mozilla.org/ja/docs/Glossary/Semantics)
    - 「意味や目的を持たせる」という意味
- 要素系 selector を使用する際は、子 selector を使用する
- div と span 要素には class をつける

[CODE](03-module-rule)

SMACSS の考える semantic 性は、下記の式が成り立つといえます。

div, span 要素などの汎用的な要素 < 見出しなどの意味を持つ要素(semantic) < class 属性がついた要素

##### 特定の context にみだりに依存しない

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

#### State rule

state とは状態のことで、既存の style を上書き・拡張するために使用されます。「既存の style を上書き・拡張」と聞くと、「先述の module rule 内の sub class と同じなのでは？」と思われるかもしれません。しかし module rule の sub class と state rule の状態 style を明確に区別するために

1. state style は layout や module に割り当てることができ
2. state style は javascript に依存するという意味を持つ

と定義されており、特に重要なのが 2 つです。
state rule の state style は全て 「is-」 の接頭辞が付き、また既存の style を全て上書きして state style が反映されることが期待されるため、必要な場合は `!important` の使用も推奨されています。

#### Theme rule

Theme rule は site 内の layout や色、text 処理などを一定の法則に従い上書きするもので、既存のあらゆる styling が上書きの対象となり得ます。

## BEM (Block, Element, Modifier)

User interface を独立した block に落とし込んでいくことで、複雑な page であっても開発を簡単かつ、素早く行うことを目的としています。

OOCSS のように基本的に module を base にした方法論であるものの、その内容は他の設計手法に比べ厳格・強力です。そのため世界的にも OOCSS に匹敵するほど名前が知られ、利用されています。本性をお手に取られた方でも、名前だけは聞いたことがある人は多いのではないでしょうか。

BEM では名前の通り、module を

- Block
- Element
- Modifier

という単位で分解し、定義しています。また**これら Block・Element・Modifier をまとめて 「BEM Entity」 と呼びます。**

### Rule

- 要素系 Selector や ID Selector の使用は推奨されません。 Class selector の使用が基本になります。
- 詳細度を均一にする
- Block, Element, Modifier の命名
  - Block と Element は class の名前が主に 「それが何であるか」 ということを重視している
  - Modifier は 「それがどうであるか」 を重視する
- Blockå
  - Layout に関する style(周りに影響を及ぼす position や float, margin など)をしてはいけません。
  - Class 名は半角英語数字の小文字で、複数の単語は hyphen でつなぐ
  - Block の nest は許容する
- Element
  - BEM では Element の中に Element が nest された命名を推奨していません
  - Element は必ず Block 内に配置する
  - Element の nest は推奨しない
  - Element はなくても良い
    - Element はあくまで 「Block を構成する option 要素」 の位置付けであるので、必ずしも Element を持たなければいけないわけではありません acti
- Modifier
  - Modifier は 「Block もしくは Element の見た目や状態、ふりまいを定義するもの」 と定義されていませう。また Block、もしくは Element に対する Option 要素という位置付けであるため、必ず必要なものではありません。

### Block の基本

BEM における Block は 「論理的 & 機能的に独立した page module」 と定義されています。要は 「特定の context に依存していない、どこでも使いまわせる parts」 と捉えて良いでしょう

「どこでも使いまわせる」 という状態を担保するために、Block 事態に Layout に関する style(周りに影響を及ぼす position や float, margin など)をしてはいけません。 Layout に関する指定が必要な場合は、御述する Mix というテクニックを用いて実装します。

![block](./imgs/ch03-bem-block.png)

Block の名目規則

- 単語が 1 つの場合
  - format: block
  - ex. `menu`
- 単語が複数の場合(hyphen で繋ぐ)
  - format: `block-name`
  - ex. `global-nav`

### Element の基本

Block の次の単位となる Elemnt は 「Block を構成し、Block の外で独立して使用できないもの」 と定義されています。先ほど Block の例として挙げた menu block は以下の図のように 4 つの Element で構成されているということになります。

![element](./imgs/ch03-bem-element.png)

Element class 名は、Block の名前を継承し、under score ２つを記述した後に Element の名前をつけます。

Element の名目規則

- 単語が 1 つの場合
  - foramt: `block__element`
  - ex. `menu__item`
- 単語が複数の場合(underscore ２つをで繋ぐ)
  - foramt: `block-name__element-name`
  - ex. `global-nav__link-item`

#### Element の nest

BEM では Element の中に Element が nest された命名を推奨していません。これは

- Block 内で Element が移動することがある
- いくつかの Element がない状態で、使われることがある
- Element を後から追加することがある

など、Block ないの構造が変わる可能性があるからです。

### Modifier の基本

Modifier は 「Block もしくは Element の見た目や状態、ふりまいを定義するもの」 と定義されていませう。また Block、もしくは Element に対する Option 要素という位置付けであるため、必ず必要なものではありません。

Modifier の名目規則

- 単語が 1 つの場合
  - foramt: `block__element_modifier`
  - ex. `menu__item_actived`
- 単語が複数の場合(underscore 1 つをで繋ぐ)
  - foramt: `block-name__element-name_modifier-name`
  - ex. `global-nav__link-item_actived-and-focused`

Modifier の名目規則(key と値の組み合わせ)

- 単語が 1 つの場合

  - ex. 「text_large」 の場合 `text` が key, `large` が value
    - `menu__item_text_large`

- 単語が複数の場合
  - ex. 「color-theme_caution」 は `color-theme` が key, `caution` が value
    - `global-nav__link-item_color-theme_caution`

#### Modifier の type

- 審議値
  - disabled, focused, actived など
- key と value の pair
  - ex. `text_large`
- 振る舞い
  - ex. `directions_right-to-left`

### Block の Nest

Block は、他のあらゆる Block の中に Nest して設置することができます。この nest の数に特に上限はなく、いくらでも nest して良いことになっています。 図のように、一番大きな枠として Header block がある、その中に menu, logo, 検索, 認証など各種 Block を埋め込むのは BEM が推奨している手法です。

![3-18 Head block の中にさまざまな Block が nest されている様子](imgs/ch03-bem-block-nested.png)

### Mix

Mix は 「単一の DOM node に、異なる BEM Entity が複数付加された」 と定義されており、誤解を恐れずに噛み砕いて言えば**「一つの HTML 要素に、役割の異なる複数の Class がついている状態」** ということができます。

Mix を行うことにより、

- Code を複製することなく、複数の BEM Entity の振る舞いや Style を組み合わせる
- 既存の BEM Entity から、新しい module を作成する。
  - 意味を持つ interface conponent となる

ことができるようになります。

### その他

#### Mix か Modifier か

Block (または Element) の振る舞いや style を変更する場合、 「Mix にすべきか Modifier にすべきか」 と悩むこともあるでしょう。先に結論から言ってしまうと、これは変更する CSS の property である程度方針を分けることができます。

- Mix を使用する場合
  - Position や Margin など、「Layout (他の要素との位置関係を整理する)に絡む」 変更の場合
- Modifire を使用する場合
  - Layout ではなく、その Block(または Element) 内で完結する変更の場合

#### Group selector の代わりに Mix

- そもそも最初から group selector を使わず、無理に Mix も行わない
- Mix をするのであれば、上書きは Mix をした Block も Modifier で行う

```css
/* group selector */
.header,
.footer {
  font-familty: Arial, sans-serif:
  font-size: 14px;
  color: #000
}

/* そもそも最初から group selector を使わず、無理に Mix も行わない */
.header{
  font-familty: Arial, sans-serif:
  font-size: 14px;
  color: #000
}

.footer {
  font-familty: Arial, sans-serif:
  font-size: 14px;
  color: #000
}
```

#### Mix では対処できない場合

- Element の中に他の Block を nest する
- Context に依存する styling を行う
  - ex. CMS の WYSIWYG(What You See Is What You Get) Editor

#### 名前が衝突することにより、Modifier の詳細度が増す

#### どの class に対する Modifier か見分けがづかない

#### Code が検索しづらい

#### BEM のその他の命名規則

- BEM の標準の命名規則 `block-name__elem-name_mod-name_mod-val` のように
  - 英数字の小文字
  - Element と Modifier はそれぞれ Block のなめを継承する
  - それぞれの区切りの中に複数の単語がある場合は hyphen 1 つ
  - Block と Element の区切りは underscore ２つ
  - Modifier の key の区切りは underscore 1 つ
  - Modifir の値の区切りは underscore 1 つ

#### MindBEMding

```html
<!-- BEM 本来の Modifier の書き方 -->
<a class="button button_size_s" href="#">button</a>

<!-- MindBEMding で紹介されている Modifier の書き方 -->
<a class="button button--s" href="#">button</a>
```

#### BEM を成功させるコツ

- DOM model ではなく、Block という単位を Base に考えましょう
- ID Selector と要素系 Selector は使用しないようにしましょう
- 子(孫) selector で nest される Selector の数は、なるべく少なくしましょう
- 名前の衝突を避けるために、また code から情報が読み取れるように、命名規則にきちんと従った Class 目をつけましょう
- Block なのか、Element なのか、Modifier なのかを常に意識しましょう
- Block または Element で変更が頻繁に起こりそうな Style の property は、Modifier に移しておきましょう
- Mix を積極的に使用しましょう
- 管理性を高めるために、Block は 1 つ 1 つがなるべく小さくなるように分割しましょう
- Block を積極的に再利用しましょう

### Link

- [bem](https://en.bem.info/)
- [BEM syntax](https://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)
