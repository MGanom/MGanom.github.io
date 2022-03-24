---
title: "[Next.js] 페이지와 라우팅 - (2)"
categories: Next.js
---

## 페이지 생성과 그 페이지의 주소

React에서는 페이지를 이동하기 위해선 React-Router-DOM 등을 함께 사용하여 각 페이지마다 주소를 설정해줘야 했다. 하지만 Next.js에선 파일을 생성만 해도 자동적으로 그 페이지에 대한 주소가 생성된다.

```
.
├─ pages
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

이렇게 `pages` 폴더에 파일을 생성하면 그 파일의 이름이 곧 그 파일의 주소가 된다.

```js
// index.js
export default function Home() {
  return <h1>THIS IS HOME</h1>;
}
```

단, `index.js`는 자동적으로 메인 화면으로 인식되어 주소가 `'/'`로 생성된다.

```js
// about.js
export default function About() {
  return <h1>THIS IS ABOUT</h1>;
}
```

`/about`으로 이동하면 정상적으로 출력되는 것을 확인할 수 있다.

참고로 페이지의 주소는 `pages` 폴더 내에 있는 **파일의 이름**에 의해 생성되는 것이기 때문에 파일 내에 있는 컴포넌트의 이름은 무엇으로 하든 상관없다.

## 페이지 간 라우팅

Next.js에선 페이지 간 이동을 할 때 단순히 `<a>`태그만 사용하면 안된다. 왜냐하면 `<a>`태그만 사용하게 될 경우 새로고침이 일어나면서 페이지 전체가 다시 로드 되기 때문에 페이지 전환 속도가 느려질 뿐만 아니라 Next.js의 CSR, SSR 기능을 제대로 활용하지 못하기 때문이다. 그렇기에 반드시 `<Link>` 컴포넌트를 사용해야 한다.

```js
// NavBar.js
// bad
export default function NavBar() {
  return (
    <nav>
      <a href="/">HOME</a>
      <a href="/about">ABOUT</a>
    </nav>
  );
}

// good
import Link from "next/link";

export default function NavBar() {
  return (
    <nav>
      <Link href="/">
        <a>HOME</a>
      </Link>
      <Link href="/about">
        <a>ABOUT</a>
      </Link>
    </nav>
  );
}
```

참고로 `<Link>` 컴포넌트에는 `className`이나 `styles` 등의 프롭스를 전달할 수 없기 때문에 만약 스타일링을 하고 싶다면 컴포넌트 내부의 `<a>` 태그 등을 활용해야 한다.
참고로 `<Link>` 컴포넌트에는 `className`이나 `styles` 등의 프롭스를 전달할 수 없기 때문에 만약 스타일링을 하고 싶다면 컴포넌트 내부의 `<a>` 태그 등을 활용해야 한다.

참고: [Next.js 공식문서](https://nextjs.org/docs){:target="\_blank"}
