---
title: "[Next.js] Style을 적용하는 다양한 방법(CSS) - (3)"
categories: Next.js
---

## Next.js에 내장되어 지원되는 CSS들

Next.js는 Built-in CSS Support가 존재한다. 즉, 처음부터 내장되어 지원하는 CSS 기법들이 존재한다는 뜻이다.

1. ### 전역 스타일시트 (Global Stylesheet)

   전역으로 적용되는 스타일시트를 추가하려면 `pages/_app.js`와 `styles.css`를 만들어 적용해주면 된다.

   ```js
   // pages/_app.js
   import "../styles.css";

   export default function App({ Component, pageProps }) {
     return <Component {...pageProps} />;
   }
   ```

   ```css
   /* styles.css */
   body {
     width: 100vw;
     height: 100vh;
   }
   ```

이렇게 작성해주면 모든 컴포넌트의 `body`에는 작성한 스타일이 적용된다. 여기서 주의할 점은 `styles.css` 파일은 반드시 `pages/_app.js`파일에만 import 해야 다른 곳에서 충돌이 발생하지 않는다는 점이다.

2. ### `node_modules`로부터 스타일 적용

참고: [Next.js 공식문서](https://nextjs.org/docs){:target="\_blank"}  
 참고: [Nomad Coders](https://nomadcoders.co/){:target="\_blank"}
