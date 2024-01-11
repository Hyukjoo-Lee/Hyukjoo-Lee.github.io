---
title: "{ SCSS } Features"
date: 2022-08-31 14:00:00 +07:00
tags: [CSS, SCSS, Responsive design]
description: "SCSS Features Reviews"
---

**SCSS Features**

### Nesting

- SCSS allows you nest CSS selectors.
- SCSS reduces the parent selector and allows you to write code easily depending on the HTML structure

- e.g.

![nesting source code image](https://github.com/Hyukjoo-Lee/Hyukjoo-Lee.github.io/blob/main/_posts/images/SCSS_Nesting.png?raw=true "nesting_source")

### Mixins

- SCSS allows you make css styles like a function.
- Defined @mixins can be reused throughout the website.
  (css 스타일을 함수처럼 만들 수 있음.)

- e.g.

```scss
@mixins function($param);
```

![mixins source code image](https://github.com/Hyukjoo-Lee/Hyukjoo-Lee.github.io/blob/main/_posts/images/SCSS_Mixins.png?raw=true "mixins_source")

### Extends

- It allows you to share styles.
- Extends and mixins are both ways of encapsulizaion and reusable styles.

- Souce code

![extends_source code image](https://github.com/Hyukjoo-Lee/Hyukjoo-Lee.github.io/blob/main/_posts/images/SCSS_Extends.png?raw=true "extends_source")

### Include & Content

- When a mixin is included, arguments can be passed by name in addition to passing them.
  (@mixin 함수 에 variable(argument) 를 전달 함으로써 재활용 가능하게 해줌)

- e.g.

```scss
@include function(argument) </em>;
```

#### @content

- It allows you add css style to another css style.
- By calling with @include {...}, you can write additional style codes inside {...}.
  ( @include로 호출해서 { .... } 내에 추가적인 스타일 코드 입력 하면 content 에 더해짐.)

- source code

```scss
@include function("iPhone") {
  color: blue;
}

@mixin function($device) {
  font-size: 28px;
  @if $device == "iPhone" @content ; // color: blue;
}
```

![Include&Content source code image](https://github.com/Hyukjoo-Lee/Hyukjoo-Lee.github.io/blob/main/_posts/images/SCSS_Include&content.png?raw=true "include&content_source")
