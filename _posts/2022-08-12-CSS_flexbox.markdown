---
title: "{ CSS } Flex Box"
date: 2022-08-19 12:30:00 +07:00
tags: [CSS, Flex Box, Display, Responsive design]
description: "CSS Concepts Reviews"
---

**FlexBox**

#### life Before Flexbox

**Display values**

- block: It does not allow locate any elements next to it.
- inline - Loses all of attributes in a block = there is no width & height.
- inline-block - Keep being a block but elements can be next each other
  problem: it creates unexpected margin between elements = you should calculate the layout.
- flex - Flexbox, it solves a problem of inline-block

#### first Rules of Flexbox

- Flex box do not talk to children.
- If you want to move something in a flexbox, you have to create flexbox container.
- box 들의 parent 가 container 이고, they should be direct children (adjusted).

```html
...
<div class="wrapper">
  <div class="child">1</div>
  <div class="child">2</div>
  <div class="child">3</div>
  <div class="child">4</div>
  <div class="child">5</div>
  <div class="child">6</div>
</div>
```

```css
.wrapper {
  display: flex;
}

.box {
  width: 200px;
  height: 200px;
  background: peru;
  color: white;
}
```

**Note that we are not coding anything about boxes.**

#### main axis & Cross Axis

- default flex-direction = row.
- If, flex-direction is row,
  - justify-content: changes the location of children with 'main-axis' direction.
  - align-items: changes the location of children with 'cross-axis' direction.
- flex-direction is column,
  - main-axis => vertical, cross-axis => horizontal.

![main&cross axis](https://user.oc-static.com/upload/2018/06/14/15289918022085_1.png)

**justify-content values**

- space-between - Distribute items evenly
  The first item is flush with the start,
  The last is flush with the end
- space-around - Distribute items evenly
  Items have a half-size space

**align-items values**

- stretch: fill whole full height of an element til wrapper height.

#### align-self and order

**nties for children**

- align-self : cross-axis but only for a children.
- order : default order is '0', useful when you are not allowing to modify html due to some reasons.

#### wrap, nowrap, reverse, align-content

<em>wrap & nowrap</em>

- Even if a browser size is shrinked, flex property allows you to align children in the same line.
- It means flex property changes the width of children automatically.
- flex-wrap determines how you deal with the width of child.

  **flex-wrap values**

  - wrap: Respect the width of child.
  - nowrap: Disregard the width of child.

<em>reverse</em>

**flex-direction values**

- row-reverse: reverse row order (right side is starting with 1)
- column-reverse: reverse column order (bottom side is starting with 1)
- flex-wrap: wrap-reverse;
  wrap but, cross-start and cross-end are permuted.

<em>align-content & justify-content</em>

- Both plays a role in adjusting spaces between elements.
- align-content: Align lines within the flex container when there is 'extra space' in the cross-axis.
- justify-content: Align individual elements within the main-axis.

#### flex-grow & flex-shrink

- flex-grow, flex-shrink are properties that we can give to child
- They are useful when we do responsive design.
- flex-shrink : When the width OR height of elements is shrinked, an element is shrinked more than other elements by 'given value'.
- e.g. a box is shrinked 'two times more' than other elements if given value is '2' (Default value: 1)
- flex-grow : When there is some remaining space between elements, the space is taken by an element (Default value: 0)

#### flex-basis

- flex basis is the initial size for elements 'before' an element is expanding or growing or shrinking.
- flex is changing the size of elements on main-axis.
- width when flex-direction is row.
- height when flex-direction is column.

### Review (Korean)

<em>justify-content</em>

- flex-start: 요소들을 컨테이너의 왼쪽으로 정렬.
- flex-end: 요소들을 컨테이너의 오른쪽으로 정렬.
- center: 요소들을 컨테이너의 가운데로 정렬.
- space-between: 요소들 사이에 동일한 간격.
- space-around: 요소들 주위에 동일한 간격.

<em>aligh-items</em>

- flex-start: 요소들을 컨테이너의 꼭대기로 정렬.
- flex-end: 요소들을 컨테이너의 바닥으로 정렬.
- center: 요소들을 컨테이너의 세로선 상의 가운데로 정렬.
- baseline: 요소들을 컨테이너의 시작 위치에 정렬.
- stretch: 요소들을 컨테이너에 맞도록 늘림.

<em>flex-direction</em>

- row: 요소들을 텍스트의 방향과 동일하게 정렬.
- row-reverse: 요소들을 텍스트의 반대 방향으로 정렬.
- column: 요소들을 위에서 아래로 정렬.
- column-reverse: 요소들을 아래에서 위로 정렬.

<em>reverse 를 사용하면 start 와 end 의 순서도 바뀐다.</em>

```css
.boxes {
  flex-direction: row-reverse;
  justify-content: flex-end; // 오른쪽이 아닌 왼쪽 정렬이 된다.
}

.boxes_2 {
   flex-direction: column-reverse;
   justify-content: flex-start; // 위가 아닌 아래 정렬이 됨.
}
```

[Game for practice](https://flexboxfroggy.com/#ko)
