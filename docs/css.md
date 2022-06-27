# CSS

## currentColor

- その要素事態に color property の値が設定されていれば、その値
- なければ、直近の親要素の color property の値

を継承します。今回は hover 時の color propert 「#e25c00」を指定していますので、hover color もこの 「#e25c00」 が適用されます

```css
.el_btn:focus,
.el_btn:hover {
  background-color: #fff;
  border-color: currentColor;
  color: #e25c00;
}
```

## rem

- `font-size` の時によく使われる

### よく使う

- `1 rem` = `16px`
- `1.125 rem` = `18px`

## em

### rem がよく使われている css attributes

- width
- padding

## position

### `position: relative`

after 擬似要素で実装する icon に `position: absolute;` を使用するため、こちらで `position: relative` を使用し after 擬似要素の起点とします。

```css
.btn--arrow-right {
  position: relative;
  padding-right: 2em;
  padding-left: 1.38em;
}

/* 
https://fontawesome.com/icons/arrow-right?style=solid&s=solid 
*/
.btn--arrow-right::after {
  content: "\f061";
  position: absolute;
  top: 50px;
  right: 0.83em;
  font-family: "Front Awesome 5 Free";
  font-weight: 900;
  transform: translateY(-50%);
}
```

## object-fit

img 要素や video 要素に適用が可能で、5 つの値があります。

- contain
  - aspect 比を維持したまま、表示領域に収まるように拡大・縮小される
- cover
  - aspect 比を維持したまま、表裏領域全体を埋めるように拡大・縮小される
- fill
  - aspect 比を無視して、表示領域全体を埋めるように引き伸ばされる
- none
  - 拡大・縮小を行わない
- scale-down
  - contain または none から、実際の size が小さくなる方を採用する

![over-fit](./imgs/ch6-overfit.png)

## background-size

背景画像の siz を調整するための property です。

- 表示したい画像を破池画像として設定する必要があるため、HTML の構造に変更が必要になります。
- また、背景画像として設定する、即ち img 要素を使わないため、 machine readable ではありません。
  - machine readable ??
    - 検索エンジンの crawer や、browser が 「content」 として認識できる状態のことです。
- SEO や accessibility の担保においては若干 minus となるため、`background-size` property の手法を採用する際には「content として認識されなくても問題ないか」をよく確認しましょう。

### property

- contain
  - aspect 比を維持したまま、表示領域に収まるように拡大・縮小される
- cover
  - aspect 比を維持したまま、表裏領域全体を埋めるように拡大・縮小される
- auto
  - aspect 比が維持されるように、適切は方向に拡大・縮小される
- `<length>`
  - 指定された長さになるように拡大・縮小される値が一つだけの場合は横幅に対する指定となり、高さは auto となる。
  - 値が２つの場合は、横幅・高さの順に値が適用される。複数の値は半額 space で区切る
- `<percentage>`
  - 指定された割合になるように拡大・縮小される。適用ルールは `<length>` と同様

![background-size](./imgs/ch06-background-size.png)
