# PRECSS(Prefixed CSS)

PRECSS は CSS を役割に応じて下記の 6 つの group に分類し、それぞれについて規則を設けています。

1. Base
2. Layout
3. Module
   a. Block module
   b. Element module
4. Helper
5. Unique
6. Program

それだけでなく、PRECSS は Base group 以外の各 group class において 2 文字の接頭辞がついていれば良いため、開発要件に合わせた徳治の group を作成することも可能です。

## 命名規則まとめ

- 命名規則
  - Element
    - 接頭辞
      - `ly_` (layout)
  - Block
    - 接頭辞
      - `bl_`
  - Element
    - 接頭辞
      - `el_`
  - Modifier
    - 該当の module 名前を継承し、その後に underscore ２つをつけ、modifier 名を続ける形です。
  - Helper
    - 接頭辞
      - `hp_`
  - Unique
    - 接頭辞
      - `un_` (unique の略)
  - Program
    - 接頭辞
      - `js_` (javascript)
        - Javascript にて要素を取得するために class です。
      - `is_` (英語 be 動詞の is から)
        - 要素の状態を管理するための class です。

## 命名規則

- 各 group における 2 文字の接頭辞の後は、 underscore を使用し、その後に class 名を続ける
- また、接頭辞の後だけでなく、各 module の子要素の命名にも underscore を使用します。
- つまり PRECSS において underscore は、構造的な段階を表す役割を狙っています。
- 1 つの段階の中で複数の単語を含んでいる際は、先頭を小文字にする `lowerCamelCase` を使用します。
- 詳細度の管理が複雑になるのを防ぐため、基本的に ID Selector は使用しません。下記が実際のコード例です。

```html
<!-- × 1つの段階内で underscore を使用している -->
<div class="bl_half_media">...</div>

<!-- ○ 1つの段階内では lowerCamelCase を使用する -->
<div class="bl_halfMedia">...</div>
```

また各 module の子要素は、基本的に親の名前のみを継承し、underscore の後に子要素の名前を続けます。**例えば子要素の中に子要素が nest されている際も、nest されている子要素はあくまで親の名前のみを継承します。**

```html
<!-- ○ それぞれの子要素は nest の段階に関わらず 「bl_halfMedia」 のみを継承している -->
<div class="bl_halfMedia">
  <img class="bl_halfMedia_img" src="example.jpg" alt="" />
  <div class="bl_halfMedia_desc">
    <h3 class="bl_halfMedia_ttl">タイトルが入ります</h3>
    <p class="bl_halfMedia_txt">説明文が入ります</p>
  </div>
</div>
```

ただし、

- 親子関係を意図をもって明確に定義したい
- module が大きいため、子要素の名前の重複を避けたい

のいずれかに該当する場合は、nest された子要素の class 名に直近の親要素の名前を含めることも許容します。

```html
<div class="bl_halfMedia">
  <img class="bl_halfMedia_img" src="example.jpg" alt="" />
  <div class="bl_halfMedia_desc">
    <!-- ○  「bl_halfMedia_desc」 を継承 -->
    <h3 class="bl_halfMedia_ttl">タイトルが入ります</h3>
    <!-- ○  「bl_halfMedia_desc」 を継承 -->
    <p class="bl_halfMedia_txt">説明文が入ります</p>
  </div>
</div>
```

### 汎用的に使用可能な単語

- `_wrapper`
- `_inner`
- `_header`
- `_body`
- `_footer`

これらは後述するそれぞれの group いずれにおいても、必要に応じて汎用的に使用できます。何らかの都合があり 「\_wrapper」 class が module の外側に必要な場合は、次のように markeup します。

```html
<div class="bl_halfMedia_wrapper">
  <div class="bl_halfMedia">
    <div class="bl_halfMedia_inner">
      <div class="bl_halfMedia_header">...</div>
    </div>
  </div>
</div>
```

### 単語を省略する場合

- [4.1.3 ID and Class Name Style](https://google.github.io/styleguide/htmlcssguide.html#ID_and_Class_Name_Style)

2 語以上で一つのまとまりを表す語群は、それぞれの頭文字の大文字のみで表現することも推奨します。ただし、ある程度一般的であったり、連続する pattern があることが望ましいでしょう。次に省略後の例をいくつか提示します。

- 一般的な 2 語以上の例

| 省略前     | 省略後 |
| ---------- | ------ |
| mainVisual | MV     |

- 連続する pattern の 2 語以上の例

| 省略前       | 省略後 |
| ------------ | ------ |
| northEurope  | NE     |
| northAmerica | NA     |
| southAmerica | SA     |

- その他、よく使われる省略後

| 省略前        | 省略後  |
| ------------- | ------- |
| category(ies) | cat(s)  |
| column        | col     |
| content(s)    | cont(s) |
| level         | lv      |
| version       | v       |
| selection     | sect    |
| description   | desc    |
| button        | btn     |
| clearfix      | cf      |
| image         | img     |
| title         | ttl     |
| text          | txt     |
| left          | l       |
| right         | r       |
| small         | sm      |
| medium        | md      |
| large         | lg      |
| reverse       | rev     |

### Series を形成する場合

module の class 名は基本的に意味のある、または、目的や挙動が読み取れる命名を推奨しますが、似たような module が続く場合は、連番をつけて管理することも PRECSS では許容します。ただしその場合、一つ目のものには連番をつけません。

```html
<!-- × 1つ目に連番がついている -->
<div class="bl_halfMedia1">...</div>
<div class="bl_halfMedia2">...</div>
<div class="bl_halfMedia3">...</div>

<!-- ○ 1つ目に連番がついていない -->
<div class="bl_halfMedia">...</div>
<div class="bl_halfMedia2">...</div>
<div class="bl_halfMedia3">...</div>
```

## Group

### Base Group

Reset CSS の rule set や、その他 project において標準となる styling を行う。

- 命名規則
  - 接頭辞なし

```css
/* project において標準となる styling 例 */
html {
  font-family: serif;
}

a {
  color: #1565c0;
  text-decoration: none;
}

img {
  max-width: 100%;
  verticla-align: top;
}
```

また、PRECESS では、特定の scope ないにおける限定的な base style の運用も許容します。例えば、「header ないの link は全て白色だが、footer 内は青色に統一したい」 という場合に、限定的な base style を使用します。ただし詳細度を高めてしまう行為であるため、使用する際は十分注意してください。

```css
/* 定期的な base style の例 */
.ly_header a {
  color: white;
}

.lf_footer a {
  color: blue;
}
```

### Layout Group

header, body area, media area, side area, footer などの大きな layout を形成する要素に使用します。

- 制限事項

  - layout に関わる styling しか行いません
    - ex. width, margin, pading, float など
  - ただし、header 背景色は黒、など湧くと「あしらい」の粒度が一致している場合は、必要に応じて layout 以外の styling を行うことも許容します

- 命名規則
  - 接頭辞
    - `ly_` (layout)

### Module group

PRECESS では再利用性の高い code を module と呼び、管理します。module は大きさよって

- block module
- element module

#### Block module

block module は、その module 特有のいくつかの子要素を持ち、また後述する element module や、他の block module を含むこともできませう。BEM で例えるならば 「複数の Element を持つ Block」 と言い換えることができます。

- 命名規則

  - 接頭辞
    - `bl_`

- [block-module example 01](../chapter03/PRECSS/block-module.html)
- [block-module example 02](../chapter03/PRECSS/block-module-02.html)

##### Block module に layout に関わる styling しない

block module には他の要素に影響を及ぼさない style のみを適用します。すなわち float や width などの layout に関わるものは、block module 自体には styling しません。

つまり block module の幅は、なるべく初期値のまま(多くは block level 要素であるため、親よその横いっぱいに広がる)であることが望ましいと言えます。

Layout に関わる指定が必要な場合は、BEM と同様に block module が使用される context の Element として style を適用します。

- [block-module example 03](../chapter03/PRECSS/block-module-03.html)

##### Block module における概念・命名の粒度

Block module は layout のために高次の moduyle を作成することもあるため、以下に block module の命名に役に立つ方針を小空きします。

| item      | explain                                                                          |
| --------- | -------------------------------------------------------------------------------- |
| Block     | block module の基本単位。その module 特有の複数の子要素や、element module を含む |
| Unit      | Block の集まり (上記の例の 「bl_3colCardUnit」)                                  |
| Container | Unit の集まり                                                                    |

#### Element Module

button, label, 見出し等の最小単位の module で、どこにでも埋め込むことが可能な module です。命名な次の code ように、極力汎用的なものを推奨します。

```html
<!-- × 名前が汎用的でない -->
<span class="el_newsLabel">News</span>
<button class="el_submitBtn">送信</button>

<!-- ○ 名前は汎用的である -->
<span class="el_label">News</span>
<button class="el_btn">送信</button>
```

- 命名規則
  - Element
    - 接頭辞
      - `el_`
  - Modifier
    - 該当の module 名前を継承し、その後に underscore ２つをつけ、modifier 名を続ける形です。
      - ex. `<span class="el_label el_label__red">News</span>`

##### Element module の layout に関わる styling

Block module と同様に、element module に対しても基本的に layout に関わる styling が行いません。ただし、block module に比べ element module は、variation に限りがある場合が多いのです。例えば button の大きさの違いは site ないで 10 数個もなく、たいていは 5,6 個程度でしょう。そのため、element module に直接 width を指定すること、および modifier で size の変更を制御することは許容します。
しかし、modifier 名には十分気をつける必要があります。例えば小さい size の button を 200px として、次のように code を書いたとしましょう。

```html
<button class="el_btn">送信</button>
<!-- × modifier 名に具体的な固定値を入れている -->
<button class="el_btn el_btn__w200">送信</button>

<!-- ○ modifir 名に汎用的な keyword を使用している -->
<button class="el_btn el_btn__s">送信</button>
```

```css
.el_btn {
  width: 300px;
}

/* modifier で width property を上書き */
/* × modifier 名に具体的な固定値を入れている */
.el_btn.el_btn__w200 {
  width: 200px;
}

@media screen and (max-width: 768px) {
  .el_btn.el_btn__w200 {
    width: 100%;
  }
}

/* ○ modifier 名に汎用的な keyword を使用している */
.el_btn.el_btn__s {
  width: 200px;
}

@media screen and (max-width: 768px) {
  .el_btn.el_btn__s {
    width: 100%;
  }
}
```

#### Block module か、Element module か

block module と element module の境界を画一的に定義することは残念ながらできません。ただしわかりやすい指針として、**「他のいろいろな module の中に埋め込まれるかどうか?」**があります。

- IF 他の module の中に埋め込まれることが多いのであれば、
  - Elemenet module
    - 取り回しをし訳すしておく
    - ex. button, label など
- ELSE
  - Block module

#### Modifier

##### Modifier を使う場合

- あしらいが変わる
- 大きさが変わる
- 一定の規則に従って振る舞いが変わる (カラム等)

##### Modifier 名の付け方

「何をする modifier なのか」 を m 根威嚇にするために、 modifier 名は `__backgroundClolorRed` ように 「**keyValue」 の形を基本としますが、「el_btn**red」 ようにおおよそ想像がづくものであれば、 key の省略が可能です。また、名前が長くあんるのを避けたい場合は、Emmet の short hand に準じて `__backgroundColorRed` を `__bgcRed` のように省略することも可能です。

また BEM の場合は命名に 「見た目」 よりも 「意味」 を重視するため、特に色に関しては 「theme」 という単語を含むことを推奨しています。例えば赤色の場合は警告色とみなして 「btn_theme_caution」 という命名をします。

しかし現実として、全ての色に意味を持たせた命名は混乱です。そのため、PRECSS では見た目通りに `el_btnred` と modifier 名を作成することを許容します。もちろん、例えば赤がその site における警告色である場合は、`el_btn__cautionColor` というように意味を密 modifier 名をつけか t も推奨します。

#### Block module に対する modifier の例

Block module において子要素に modifier による変更を適用する際

- 対象の**子要素のみ**に modifier を適用する
- block module の root 要素に modifier を適用する

#### LINK

- [Emmet Document](https://docs.emmet.io/)
  - HTML と CSS を効率よく開発するための toolkit です。editor に plugin として追加することで使える
  - [Emmet cheat-sheet](https://docs.emmet.io/cheat-sheet)

### Helper group

基本的に１つ style のみで、「ここの style だけ調整したい」 というような場合に用いる group です。helper class による上書きはとても意図的であり確実に適用されてほしいため、 `!important` を付加することを推奨します。

- 命名規則
  - 接頭辞
    - `hp_`

命名規則に関しては modifier と同様 「keyValue」 の形を取り、省略する場合は Emmet の short hand に準ずることを推奨します

- Emmet の short hand に準じた命名の例
  - ｀ hp_marginBottom20`→`hp_mb20`

その他の規則として

- px 以外の単位の場合は Emmet の short hand で表現 (Emmet にない場合は、わかりやすい頭文字を使用)
- 素数点は underscore で表現
- Negative 名値は key を大文字で表現

Helper class は基本的に 1 style のみのため、CSS の property と値を**一行で記載する**ことを許容します。また挙動が限定的 & 明確な場合のみ、１つ以上の style であっても helper class で処理することが可能です。

```css
/* px 以外の単位の場合は Emmet の short hand で表現 */
.hp_mt2e {
  margin-top: 2em !important;
}

/* 素数点は underscore で表現 */
.hp_mt2_5e {
  margin-top: 2.5em !important;
}

/* Negative 名値は key を大文字で表現 */
.hp_MT2e {
  margin-top: -2em !important;
}
```

#### clearfix について

PRECSS では float の解除に clearfix の使用を推奨しています。親要素に `overflow: hidden;` を設定する方法もありますが、 `overflow` property は本来の用途で使われることもしばしばあります。code だけ見た時に float 解除のためなのか、overflow 本来の用途としているのか判別がつかない状態は好ましくありません。

#### helper の拡張 group を作成する

```css
/* desktop 幅だけで表示 */
.lg_only {
  display: block !important;
}

@media screen and (max-width: 768px) {
  .lg_only {
    display: none !important;
  }
}

/* Tablet 幅以下だけで表示 */
.md_only {
  display: none !important;
}

@media screen and (max-width: 768px) {
  .md_only {
    display: block !important;
  }
}
```

#### module の上下間の余白を実装する helper class

例えば、「card module の次に text が続く場合は余白を 20px にしたいが、他の module が続く場合は、詰まって見えてしまうため 40px にしたい」 という design 上の都合は、至極もっともです(당연한 이야기이다)。
その場合、module 事態に余白のための styling を行うのは現実的ではありませんので、helper class を用いて実装することを筆者はよく行われてっています。

```html
<!-- desktop 幅, tablet 幅 以下どちらにも同じ値を適用する場合 -->
<div class="bl_card hp_mb20">...</div>

<!-- それぞれ異なる値を適用する場合 -->
<div class="bl_card lg_mb40 mb_mb20">...</div
```

```css
/* desktop 幅, tablet 幅 以下どちらにも同じ値を適用する場合 */
.hp_mb20 {
  margin-bottom: 20px !important;
}

/* それぞれ異なる値を適用する場合 */
.lg_mb40 {
  margin-bottom: 40-px !important;
}

@media screeen and (max-width: 768px) {
  .md_mb20 {
    margin-bottom: 20 @x !important;
  }
}
```

### Unique group

ある 1 page でしか使用されていないことを明示する group です。その page でしか使われていないため、改修や運用の際に影響官位を気にせずに style を編集して良い目標になっています。module の大きさも自由です。小さくても大きくてもかまいません。ECSS 以外の設計手法で、不要になれば迷わず削除できる CSS の目印を用意しているのは、筆者の知る限り他にありません。

Unique group は、あらゆる irregular のための万能な回避策です。ただし使い過ぎると再利用性に欠けるため、あくまで irregular のための措置であることは有意してください。

Unique group を styling している CSS には、どの page で使用しているか comment を残しておくとより良いでしょう。

- 命名規則
  - 接頭辞
    - `un_` (unique の略)

```html
<!-- PRECSS document の扉 page -->
<div class="un_siteRoot_wrapper">
  <section class="un_siteRoot">
    <figure class="un_siteRoot_logo">
      <img src="images/icon.svg" alt="PRECSS logo" />
    </figure>

    <h1 class="un_siteRoot_ttl">PRECSS</h1>
    <p class="un_siteRoot_link">
      <a href="/en/">English</a> /
      <a href="/ja/">日本語</a>
    </p>
  </section>
</div>
```

```css
.un_siteRoot_wrapper {
  position: relative;
  top: 33vh;
  text-align: center;
}

.un_siteRoot {
  display: inline-block;
}

.un_siteRoot_logo {
  width: 100px;
  margin: 0 auto;
}
```

### Program group

PRECSS では Javscript 等の proram で要素に touch する際、または状態を管理する際、専用の class を付加し、module としての styling とは分離することを推奨します。program group は少し特殊で、２つの接頭辞が存在します。

- 命名規則
  - 接頭辞
    - `js_` (javascript)
      - Javascript にて要素を取得するために class です。
    - `is_` (英語 be 動詞の is から)
      - 要素の状態を管理するための class です。

状態の命名は `is_active` と simple に記述することが可能ですが、他の箇所にも影響を及ぼさないよう必ず selector は複数の class にする必要があります。また対応 browser や状況によっては、Javascript 用の class ではなく、custom data 即成や WAI-ARIA を使用して状態を管理することも許容します。

```html
<dl class="bl_accordion js_accordion">
  <dt>
    <a class="bl_accordion_ttl js_accordion_ttl" href="#"> Accordion Title </a>
  </dt>

  <!-- Javascript によって、 is_active が付加される -->
  <dd class="bl_accordion_txt js_accordion_body is_active"></dd>
</dl>
```

```css
.js_accordion_body {
  display: none;
}

/* × 他の箇所にまで影響を及ぼす可能性がある */
.is_active {
  display: block;
}

/* ○ 幅数 class で適用箇所を絞っている */
.js_accordion_body.is_active {
  display: block;
}
```

module によっては、「表示/非表示の状態によって、icon も変わる」 など、複雑な styling が必要なこともあるでしょう。そういった場合は、`.js_` class と`ls_` class の組み泡 s ではなく、 `.bl_` class と `.is_` class の組み合わせで styling を行うことも可能です。
