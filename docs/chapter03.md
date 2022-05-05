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
