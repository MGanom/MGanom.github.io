---
title: "[Next.js] _app.js와 Layout 다루기 - (4)"
categories: Next.js
---

## \_app.js란?

`_app.js`는 최상위 컴포넌트의 역할을 하는 컴포넌트이다. 프로젝트를 처음 생성했을 때는 존재하지 않기 때문에 직접 추가를 해줘야 하며, 이를 활용하면 모든 페이지에 공통적으로 들어가야 하는 코드나 스타일링을 정의할 수 있다.

### \_app.js의 사용법

```
.
├─ components
│  └─ NavBar.js
├─ pages
│  ├─ _app.js   // 추가된 파일
│  ├─ about.js
│  └─ index.js
├─ public
├─ .eslintrc.json
├─ .gitignore
├─ next.config.js
├─ package-lock.json
├─ package.json
└─ README.md
```

이렇게 `pages`폴더에 추가해주면 된다.

```js
// _app.js
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
```

이것이 `_app.js`의 기본 형태이다. `props`로 `Component`와 `pageProps`를 받으며 이를 반환한다. `Component` 프롭스는 페이지들 중 현재 선택된 페이지이며, `pageProps`는 기본적으로는 빈 객체이지만 만약 페이지 컴포넌트에 프롭스를 전달했다면 그 프롭스를 의미한다.

```js
// _app.js
import NavBar from "../components/NavBar";

export default function App({ Component, pageProps }) {
  return (
    <>
      <NavBar />
      <Component {...pageProps} />
      <style jsx global>
        {`
          a {
            color: grey;
          }
        `}
      </style>
    </>
  );
}
```

만약 모든 페이지에 보여야 할 컴포넌트가 있다면 `NavBar` 컴포넌트처럼 넣어주면 된다. 또한 `global.css`로 전역에 해줄 스타일링이 있다면 여기에 import 해주면 되고, 또는 예시처럼 style-jsx를 활용할 수도 있다.

## Layout이란?

상술한 것처럼 `_app.js`에서 페이지 전역에 사용될 내용을 정의하는 방법도 있지만 `Layout` 컴포넌트를 만들어 활용하는 것이 유용한 경우도 많다. 왜냐하면 `_app.js`에는 다양한 라이브러리와 서비스들도 import 될 것인데 만약 이 페이지 하나에서 모든 것을 작성하게 된다면 굉장히 지저분해지고 가독성이 저하될 수 있기 때문이다.

뿐만 아니라, Layout은 페이지별로도 사용도 할 수 있는데, 만약 각 페이지마다 공통적으로 관리하고 싶은 Layout이 있다면 `getLayout`을 활용하면 된다.

### Layout 기본 사용법

```
.
├─ components
│  ├─ Layout.js  // 추가된 파일
│  └─ NavBar.js
├─ pages
│  ├─ _app.js
│  ├─ about.js
│  └─ index.js
├─ public
├─ .eslintrc.json
├─ .gitignore
├─ next.config.js
├─ package-lock.json
├─ package.json
└─ README.md
```

우선 `Layout.js`를 만들어 준다.

```js
// Layout.js
import NavBar from "./NavBar";

export default function Layout({ children }) {
  return (
    <>
      <NavBar />
      <div>{children}</div>
    </>
  );
}
```

여기에서 프롭스인 `children`은 `Layout` 컴포넌트가 감싸고 있는 다른 컴포넌트를 의미한다.

```js
// _app.js
import Layout from "../components/Layout";

export default function App({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
      <style jsx global>
        {`
          a {
            color: grey;
          }
        `}
      </style>
    </Layout>
  );
}
```

이런식으로 `<Layout>` 컴포넌트로 감싸주면 모든 페이지에서 `Layout.js`에 적어준 코드가 적용되는 것을 확인할 수 있다.

참고: [Next.js 공식문서](https://nextjs.org/docs){:target="\_blank"}
