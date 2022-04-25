---
title: "[Tailwind CSS] Tailwind CSS의 작동 방식과 동적 스타일링 작성 방법 - (2)"
categories: Tailwind_CSS
---

## Tailwind CSS의 작동 방식

Tailwind CSS는 정규표현식을 통해 HTML 상에서 class일 수 있는 모든 단어들을 가져온 뒤 해당 단어가 Tailwind CSS에서 인식되면 그것을 CSS로 변환하는 방법으로 작동한다. 즉, class에 작성된 내용을 코드적으로 분석하거나 파싱하여 작동하는 것이 아니고, 단어의 있는 그대로 이해하고 적용된다는 의미이다. 예를 들어,

```jsx
<div class="text-{{ error ? 'red' : 'green' }}-600"></div>
```

는 조건문에 따라 'text-red-600' 혹은 'text-green-600'으로 변동하지만, Tailwind CSS는 이를 인식하지 못하고 건너뛰어버리고 만다. 즉,

```jsx
<div class="{{ error ? 'text-red-600' : 'text-green-600' }}"></div>
```

와 같이 작성을 해줘야 완전한 두 단어를 인식하여 CSS로 변환해준다.

## 동적 스타일링 작성 방법 (in React)

Tailwind CSS를 사용할 수 있는 언어나 프레임워크는 다양하지만 이 글에선 React를 기준으로 작성하도록 하겠다.
위에서 설명한 개념을 이해했다면 동적 스타일링을 작성할 때 어떻게 해야할 지에 대한 감이 왔을 것이다.  
핵심은 **'반드시 완전한 상태의 단어를 적는다'**이다.

### 1. HTML의 className 속성에서 조건문 활용하기

위에서 든 예시처럼 작성해주면 된다.

```jsx
<div
  className={`${isImportant ? "text-red-600 text-lg font-bold" : "text-black"}`}
></div>
```

이 예시는 `isImportant`가 참이라면 앞에 적어준 3가지의 CSS가, 그리고 거짓이라면 뒤에 적어준 1가지의 CSS가 적용될 것이다.

### 2. JavaScript를 사용하여 className에 추가하기

자바스크립트로 HTML 태그의 className에 추가해주는 방식으로도 적용할 수 있다.

```js
if (isImporant) {
  document
    .querySelector("#div")
    .classList.add("text-red-600 text-lg font-bold");
} else {
  document.querySelector("#div").classList.add("text-black");
}
```

참고: [Tailwind CSS 공식문서](https://tailwindcss.com/docs/installation){:target="\_blank"}
