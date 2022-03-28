---
title: "[Next.js] 라우팅 심화 (useRouter, Link, catch all)- (7)"
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
    router.push(
      // url
      {
        pathname: "/details/[id]",
        query: { id: movie.id, name: movie.name },
      },
      // as
      "/details/[id]"
    );
  };
  return (
    <div>
      <button onClick={toAbout}>About</button>
    </div>
  );
}
```

가장 대표적으로 사용되는 `router.push`의 예시이다.  
`pathname`에는 이동하고자 하는 주소를, `query`에는 쿼리파라미터에 담고 싶은 정보를 적어주면 된다. 참고로 여기에서 `pathname`의 `[id]`는 `query`의 `id`에 연결되어 `/details/${moive.id}`로 이동하게 된다. `router.push`의 두 번째 인자로는 `as`에 관한 내용을 적어줄 수도 있는데, 이것이 뜻하는 것은 사용자가 이동하는 주소는 `pathname`+`query`에 해당하는 주소이지만 브라우저에 표시되는 주소는 `as`에 적어주는 주소가 된다는 의미이다.

이 외에도 히스토리 스택에 새로운 주소를 추가하지 않고 대체하는 `router.replace`, 페이지를 미리 불러옴으로써 클라이언트 측에서의 페이지 이동 속도를 향상 해주는 `router.prefetch`, 히스토리 스택 상의 이전 페이지로 돌아가는 `router.back` 등 다양한 기능이 존재한다.

### Link

`next/link`에서 제공하는 `Link` 컴포넌트를 사용하면 페이지를 이동할 수 있다.

#### `Link`를 활용하는 방법

```jsx
import Link from "next/link";

export default function Details({ movie }) {
  const urlObject = {
    pathname: "/about/[id]",
    query: { id: movie.id, name: movie.name },
  };

  return (
    <div>
      <Link href={urlObject}>
        <a>About</a>
      </Link>
    </div>
  );
}
```

`useRouter`와 사용 방법은 유사하다. `pathname`에는 이동하고자 하는 주소를, `query`에는 쿼리파라미터에 담고 싶은 정보를 적어주면 되며, `useRouter`의 `as`를 `Link`에서 사용하려면 프롭스로 `as`에 원하는 주소를 전달하면 된다. 만약 push말고 replace 를 원한다면 프롭스로 `replace`를 적어주면 된다.

이 외에도 프롭스로 앞서 설명한 `prefetch`, 주소 이동 후 스크롤을 최상단으로 이동해주는 `scroll`, 정적으로 페이지를 생성하는 `getStaticProps`, SSR로 페이지를 생성하는 `getServerSideProps` 등 다양하게 전달하여 사용할 수 있다.

## Catch All Routes

페이지에서 모든 `path`를 가져와 쿼리파라미터화 하는 방법이다. 이를 활용하면 URL에 원하는 데이터를 출력하는 것은 물론 그 데이터를 활용할 수 있게 되어 유용하다.

### 예시

`pages`폴더에 영화에 대한 정보를 출력해주는 페이지가 있다고 예시로 들어보자.  
폴더에 `pages/movies/[...params].js`라는 경로의 파일을 생성하면, 브라우저에서 `/movies`이후에 몇 개의 `path`가 붙든 `[...params].js` 파일로 연결된다. 또한 `/movies` 이후에 오는 `path`들은 모두 파라미터화가 되어 `useRouter`의 `query`객체 안에 `params` 배열에서 확인이 가능하다.

다시 말해, 만약 경로가 `/movies/movies/Spider-Man:%20No%20Way%20Home/634649`이라면, `useRouter`의 `query`에는

```jsx
{
  params: ["Spider-Man: No Way Home", "634649"];
}
```

가 존재하게 된다는 것이다. 이를 활용한다면,

```jsx
export default function MovieDetail() {
  const router = useRouter();
  const [title, id] = router.query.params || [];
  return (
    <div>
      <h4>ID: {id}</h4>
      <h4>Movie Name:{title}</h4>
    </div>
  );
}
```

처럼 코드를 작성하여 화면에 영화의 아이디와 이름을 출력할 수 있는 것이다.

참고: [Next.js 공식문서](https://nextjs.org/docs){:target="\_blank"}
