---
title: "[Next.js] 라우팅 심화 (useRouter, next/router, catch all, Link)- (7)"
categories: Next.js
---

## 페이지를 이동하는 방법

페이지를 이동하는 방법엔 여러가지가 있다.

### useRouter

next/router에서 제공하는 `useRouter`를 사용하면 페이지를 이동할 수 있다.

#### router.push

```jsx
import { useRouter } from "next/router";

export default function Details({ movie }) {
  const router = useRouter();

  const toAbout = () => {
    router.push({
      pathname: "/details/[id]",
      query: { id: movie.id },
    });
  };
  return (
    <div>
      <button onClick={() => router.push("/about")}>About</button>
    </div>
  );
}
```

가장 대표적으로 사용되는 `router.push`의 예시이다.

2. Link

3. window.location

참고: [Next.js 공식문서](https://nextjs.org/docs){:target="\_blank"}
