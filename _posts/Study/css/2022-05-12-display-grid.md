---
title: "[CSS] display: grid"
categories: CSS
---

## display: grid란?

```css
.container {
  display: grid;
}
```

Grid는 단어의 의미 그대로 바둑판처럼 영역을 나누어 크기와 위치를 정해 화면을 구성할 수 있게 해주는 CSS 기법이다. Table과 유사한 점이 많지만 그리드는 자식 요소의 크기, 위치, 계층 등을 컨테이너에서 정해주는 등 더 다양한 방법으로 레이아웃을 구성할 수 있기 때문에 효율적이다.

## 관련 주요 프로퍼티

모든 프로퍼티들은 사용할 수 있는 파라미터가 다양하므로 이에 대해 숙지하고 사용하는 것이 중요하다.

### grid-template-columns & grid-template-rows

각각의 방향으로 요소가 몇 개만큼, 얼만큼의 크기로 정렬이 될 지를 정한다.

```css
.container {
  display: grid;
  grid-template-columns: 60px 40px;
}
```

열이 2개가 되고, 각각의 열이 60px과 40px이 된다.

```css
.container {
  display: grid;
  grid-template-rows: 1fr 2fr 3frf;
}
```

행이 3개가 되고, 각각의 행이 1:2:3의 비율이 된다.

### grid-auto-columns & grid-auto-rows

각각의 방향으로 요소를 지정해준 크기로 정렬을 한다.

```css
.container {
  display: grid;
  grid-auto-columns: 50px;
}
```

각 열의 크기가 50px이 되고, 이것으로 아래 방향으로 채워 나간다.

```css
.container {
  display: grid;
  grid-auto-rows: 50px;
}
```

각 행의 크기가 50px이 되고, 이것으로 우측 방향으로 채워 나간다.

### grid-auto-flow

요소가 어느 방향으로 정렬이 될 지를 결정한다.

```css
.container {
  display: grid;
  grid-auto-flow: row;
}
```

요소들이 행 방향으로 정렬된다.

```css
.container {
  display: grid;
  grid-auto-flow: column;
}
```

요소들이 열 방향으로 정렬된다.

### grid-gap

행과 열 간의 간격을 정한다. 이는 `grid-row-gap`과 `grid-column-gap`을 합친 것으로 필요에 따라 나눠써도 무방하다.

```css
.container {
  display: grid;
  grid-gap: 1rem;
}
```

행과 열의 간격을 1rem 씩 띄운다.

```css
.container {
  display: grid;
  grid-gap: 30px 40px;
}
```

행 간에는 30px 씩 띄우고, 열 간에는 40px 씩 띄운다.

참고: [MDN 공식 문서](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout){:target="\_blank"}
