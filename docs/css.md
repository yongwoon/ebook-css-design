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
