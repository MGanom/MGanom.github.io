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

   `node_modules`로부터 스타일을 적용한다는 것은 Bootstrap과 같은 라이브러리나 프레임워크를 사용한다는 것이다. 사용 방법은 각 패키지마다 다르므로 공식 문서와 블로그들을 참고하면 된다.

3. ### 컴포넌트 단위로 CSS Modules 적용하기

   `CSS Modules`를 활용하면 각 컴포넌트 마다 `[name].module.css`와 같은 파일로 스타일링을 할 수 있다. 이 파일은 컴파일 과정에서 고유한 클래스 이름을 생성하기 때문에 여러 파일에서 클래스 이름이 중복이 되어도 상관이 없다는 장점이 있다.

   ```css
   /* ./Navbar.module.css */
   .link {
     color: crimson;
   }
   ```

   우선 이렇게 css 파일을 만들어준다.

   ```js
   import Link from "next/link";
   import styles from "./NavBar.module.css";

   export default function NavBar() {
     return (
       <nav>
         <Link href="/">
           <a className={styles.link}>HOME</a>
         </Link>
         <Link href="/about">
           <a className={styles.link}>ABOUT</a>
         </Link>
       </nav>
     );
   }
   ```

   그리고 컴포넌트에 `styles`로 import 해주고 `className`에 `styles.[클래스이름]`을 해주면 스타일링이 적용된다.

   4. ### Sass

      Sass를 설치하고 3번처럼 `[name].module.scss` 혹은 `[name].module.sass`로 사용하면 된다. 만약 Sass 컴파일러를 수정하고 싶다면 `next.config.js`에서 `sassOptions`를 추가하여 수정하면 된다.

   5. ### CSS-in-JS
      CSS-in-JS를 활용한 모든 스타일링을 사용할 수 있다. 예를 들어, inline 스타일링을 활용해도 되고 Styled Components를 사용해도 되며 styled-jsx를 사용해도 된다.

참고로, CSS는 JavaScript가 비활성화 되어 있어도 적용된다.

참고: [Next.js 공식문서](https://nextjs.org/docs){:target="\_blank"}
