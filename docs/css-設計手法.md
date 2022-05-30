# CSS 設計手法

- OOCSS
- SMACSS
- BEM
- PRECSS
  - Atomic design 에 친화적이다

## OOCSS

### link

- [oocss](http://oocss.org/)
- [what-is-object-oriented-css](https://www.slideshare.net/stubbornella/what-is-object-oriented-css)
- [object-oriented-css](https://www.slideshare.net/stubbornella/object-oriented-css)

---

## SMACSS

### module 名の接頭辞

- layouts
  - `l-`
- theme
  - `theme-`
- state style
  - `is-`

---

## BEM

### BEM のその他の命名規則

- BEM の標準の命名規則 `block-name__elem-name_mod-name_mod-val` のように
  - 英数字の小文字
  - Element と Modifier はそれぞれ Block のなめを継承する
  - それぞれの区切りの中に複数の単語がある場合は hyphen 1 つ
  - Block と Element の区切りは underscore ２つ
  - Modifier の key の区切りは underscore 1 つ
  - Modifir の値の区切りは underscore 1 つ

### Link

- [BEM](https://en.bem.info/)
- [BEM methodology](https://en.bem.info/methodology/quick-start/)
- [BEM syntax](https://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

---

## PRECSS

### module 名の接頭辞

- layouts
  - `ly_`
- element
  - `el_`
- block
  - `bl_`

### Link

- [precess](https://precss.io/ja/)
- [Google HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.html)
- [idiomatic CSS](https://github.com/necolas/idiomaticcss)
