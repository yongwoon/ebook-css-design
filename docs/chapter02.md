# Chapter 2

## Selector 種類 (2-1)

### 単純 Selector

- 要素系 Selector(Type Selector)
  - ex. `p {}`
- 全称 Selector
  - ex. `* {}`
- 属性 Selector
  - ex. `a[href="http://www.w3.org/"]`
- Class Selector
  - ex. `.my-class {}`
- ID Selector
  - ex. `#my-id {}`
- 擬似(ぎじ) Class
  - 擬似 Class 内でさらに分類がありますが、本書の解説の本節ではないので Colon(:) を使用したものは全て 「擬似 Class」 と呼ぶことにします。
  - ex. `a:visited {}`

### 擬似要素

ex. `a::before {}`

### [結合子](http://lovee7.blog.fc2.com/blog-entry-28.html)

- 子孫(しそん)結合子
  - セレクタ A で選択される要素の子孫に、セレクタ B を適用。
  - ex. `div p`
- 子結合子(child combinator)
  - セレクタ A で選択される直接の子要素に、セレクタ B を適用。
  - ex. `div > p`
- 隣接(りんせつ)兄弟接合子
  - セレクタ A で選択される要素の直後の隣接している要素に、セレクタ B を適用。
  - ex. `div + p`
- 一般兄弟結合子
  - セレクタ A で選択される要素と共通の親要素を持ち、かつその要素より後の要素に、セレクタ B を適用。
  - ex. `div ~ p`

---

## CSS の Cascading 仕組み (2-1)

Selector が示す同じ要素の同じ property に対して異なる値が設定されていた場合、最終的にはどれを適応するかという規則のことで

1. 重要度
   - `!important;`
2. 詳細度
   - ex. `.my-class` と `p {}` では `.my-class` が詳細度が高い
3. コードの順序
   - 最後に書いたコードが適用される

のゆうせインド順で適用する style が決定されます

### 詳細度

優先順位はいかになります。(1 が一番優先順位が高い。つまり適用される Style は 1 になる)

1. ID Selector
2. Class Selector, 属性 Selector, 義理 class
3. 要素系 Select

- Link
  - [Specificity Calculator](https://specificity.keegan.st/)

---

## Reset CSS (2-2)

Reset は CSS 設計に直接は関係ありませんが、Rest CSS の選択を誤ると Module 作成時の Cost が無駄に増加していしまいます。

### Reset CSS が必要になってきた背景

browser はそれぞれ default sytle と呼ばれるものを持っており、Styling が全くされていない HTML でも、指定源の見た目を田んぼするようにできています。これを統一しないまま CSS を書くと、「ある browser ではきちんと意図通りに表示できるが、他の browser では意図通りの表示にならない」ということが起こります。

この問題を解消するために Base style を定義する必要があり、そのための手法として現在一般的に用いられているのが 「hard reset」 という手法と 「Normalize」 という手法です

- Hard reset
  - 各要素の余白を取り去る & Font size を統一する
  - ex
    - [HTML5 Doctor Reset CSS](http://html5doctor.com/html-5-reset-stylesheet/)
    - [css-wipe](https://github.com/stackcss/css-wipe)
- Normalize 系 CSS
  - Normalize 系 Css は」 browser 間の差異や bug をなくしつつも、有用な Default Style を生かすという方針のため、Default style に近い形で Style を定義がされていることが特徴です。
  - またそれだけでなく、細かいな Usability (유용성) の向上のための Style 定義もされています。
  - ex
    - [Normalize.css](https://necolas.github.io/normalize.css/)
    - [sanitize.css](https://csstools.github.io/sanitize.css/)

### Reset CSS は CSS 設計にどのような影響を及ぼすか

Reset CSS について、Hard reset 系 CSS, normalize rest 系 CSS を紹介しました。CSS 設計においてはどちらを利用することもできますが、重要なのは

- 選択を誤ると、module 作成時の code と開発 cost が増加する
- 途中からどちらかに変更することは現実的ではない

---

## 英単語結合する方式の名前 (2-3)

- hyphen case (ex. `sub-title)
- snake case (ex.`sub_title`)
- camel case (ex.`subTitle`)
- upper camel cas (ex.`SubTitle`)

---

## 良い CSS 設計が目指す 4 つの Goal (2-4)

- 予測できる
- 再利用できる
  - 例えば既存の parts を別の箇所でも使用したいとなったとき、Code をいちいち書き直したり、上書きする手間がない状態が「再利用できる」状態です。そのために、 Styling はきちんと抽象化されており、また適性つ分離されている必要があります。
- 保守できる
  - 新しい module や機能を追加・更新、あるいは配置換えしたとき、既存の CSS を refactoring する必要がない状態が 「保守できる」 状態です。
- 拡張できる
  - CSS に係る人が 1 人であっても複数からなる team であっても、問題なく管理できる状態を指します。

### Link

- [CSS Atchitecture](https://philipwalton.com/articles/css-architecture/)

---

## CSS 設計の実績と 8 つの Point

- 特性に応じて CSS を分類する
  - この方法は `SMACSS`, `PRECSS` で採用されています
- Content と Styling が疎結合である
- 影響範囲がみだりに広すぎない
- 特定の context にみだりに依存していない
- 詳細度がみだりに高くない
- Class 名から影響範囲が想像できる
- Class 名から見た目・機能・役割が想像できる
- 拡張しやすい

### 特性に応じて CSS を分類する

- Base group
  - Site 全体に適用される style
- Layout group
  - Layout に関する Style
- Module group
  - site 内全体で使いまわしたいもの
  - **module 事態には layout に関する指定は基本的に行わないことが、best practice になります**
    - 「layout に関する指定」 とは、具体的に以下のようなものがあります。
      - `position` (static, relative を除く)
      - `z-index`
      - `top` / `right` / `bottom` / `left`
      - `float`
      - `width`
      - `margin`

### Content と Styling が疎結合である

- HTML の要素名をなるべく selector にとして使用しないようにするのが best practice です。例えば今までが div 洋装だったものが、何らかの都合で p 要素になったりすると、div 要素に対して設定していた style が適用されなくなってしまうためです。

### 影響範囲がみだりに広すぎない

### 特定の context にみだりに依存していない

### 詳細度がみだりに高くない

詳細度が高い CSS は

- Selector の見通しが悪くなりがち
- 他の要素(親要素など)に対する依存が多くなりがち
- 上書きが難しい
- ゆえに maintenance の工数が増える

実際に詳細度を低くするためのコツとしては、

- Selector は class selector を使用する

### Class 名から影響範囲が想像できる

すなわち Class 名から影響範囲が想像できるようにするにはどうすれば良いかというと、

「Module の子要素には、Module の root 要素の class 名を継承させる」 ようにする。

### Class 名から見た目・機能・役割が想像できる

使い回しを前提とした module に対する最適な命名とはつまり

1. context ではなく、見た目・機能・役割を Base とし
2. media, accordion, slider など、一般的な名称を使用する

### 拡張しやすい

- 拡張しやすい Class 設計を行う(multi-class 設計を採用する)
- 拡張用として作成した class は、機能・役割に応じて適切な粒度・影響範囲も持つ

#### Single class VS multi class

- single class の demerit
  - それぞれの button の box-shadow がないバターン
  - shadow はあり、文字色が白黒反転した pattern
  - shadow はなく、文字色も判定した pattern

---

## module の粒度を考える (2-6)

- 「使いまわすことを前提としたひとかたまりの単位」を考える

### module の粒度のばらつき(고르지 못한 분포)起こす問題

### module 粒度の指針

- 最小 module
  - button, label, title などの simple な要素
- 複合 module
  - 幾つかの子要素をもつ、ほとかたまりの要素
