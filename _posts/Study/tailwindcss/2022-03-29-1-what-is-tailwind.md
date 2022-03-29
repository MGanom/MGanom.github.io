---
title: "[Tailwind CSS] Tailwind의 개념과 설치 - (1)"
categories: Tailwind_CSS
---

## Tailwind CSS란?

Tailwind CSS는 'utility-first'라는 목적 하에 만들어진 CSS 프레임워크이다. 별도의 CSS 파일이나 `style` 태그를 사용하지 않고 HTML의 `class` 속성에 입력되는 내용에 따라 스타일링이 적용되기 때문에 굉장히 간단하고 직관적으로 코드를 작성할 수 있다.

### 장점

- HTML의 `class` 속성에 직접 입력되기 때문에 별도의 CSS 파일을 생성하여 임포트 하지 않아도 된다.
- 작성해야 하는 코드의 길이가 확연하게 짧아진다.
- 반응형 스타일링을 적용하기 용이하다.
- 클래스 이름에 대해 고민하거나 중복을 걱정할 일이 없다.

### 단점

- HTML 코드가 지저분해진다.
- `class` 속성에 작성하는 코드들이 각각 어떤 동작을 하는지 익혀야 하고, 그 코드들의 이름을 외워야 하기 때문에 이 모든 것을 익히려면 시간이 오래 걸린다.
- CSS에 익숙하지 않다면 각 코드들의 동작 방식을 이해하기 힘들다.
- 완성형 컴포넌트가 다른 프레임워크들에 비해 적기 때문에 사용자가 대부분 직접 만들어야 한다.
- CDN 사용 시 웹팩과 컴파일러를 통한 최적화가 이루어지지 않을 경우 기본적인 CSS 파일의 크기가 무거워진다.

### 설치 방법

설치하는 방법의 경우 다양한 방법이 존재하기 때문에 공식 문서를 참고하여 자신에게 맞는 설치법을 참고하는 게 좋다.

대표적인 설치 방법으로, Tailwind CLI를 활용하여 설치해보자.

1. 패키지를 설치한다.

   ```
   npm install -D tailwindcss
   ```

2. `tailwind.config.js` 파일을 생성한다.

   ```
   npx tailwindcss init
   ```

3. `tailwind.config.js` 파일에 Tailwind CSS를 사용할 템플릿들이 존재하는 경로를 적어준다.

   ```js
   // tailwind.config.js
   module.exports = {
     content: ["./src/**/*.{html,js}"],
     theme: {
       extend: {},
     },
     plugins: [],
   };
   ```

4. 최상위 혹은 글로벌 CSS 파일에 `@tailwind` 지시어들을 추가한다.

   ```css
   /* global.css */
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

5. CLI가 입력되는 코드들을 CSS로 컴파일 및 빌드하도록 실행한다.

   ```
   npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch
   ```

   참고로, --watch를 작성할 시 파일의 변화를 감지하여 그 때마다 빌드를 해준다.

6. HTML의 `<head>`에 컴파일된 Tailwind CSS 파일을 추가하고, 스타일링 코드를 작성한다.

   ```html
   <!DOCTYPE html>
   <html>
     <head>
       <meta charset="UTF-8" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0" />
       <link href="/dist/output.css" rel="stylesheet" />
     </head>
     <body>
       <h1 class="text-3xl font-bold underline">Hello world!</h1>
     </body>
   </html>
   ```

이 외에도 PostCSS 플러그인을 활용하여 설치하는 방법, Next.js나 Create React App 등 다양한 프레임워크에 맞춰 설치하는 방법 등 다양하게 존재한다.

참고: [Tailwind CSS 공식문서](https://tailwindcss.com/docs/installation){:target="\_blank"}
