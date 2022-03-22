---
title: "[Node.js] POST로 데이터 통신하여 글 생성하기 - (9)"
categories: Node.js
---

## POST로 보낸 데이터에서 쿼리 파라미터 가져오기

```js
    else if (pathname === "/create_action") {
    let body = "";
    req.on("data", (data) => {
      body = body + data;
      console.log(body);
    });
    req.on("end", () => {
      const post = new URLSearchParams(body);
      const title = post.get("title");
      const content = post.get("content");
    });

    res.writeHead(200);
    res.end("success");
  }
```

글 생성 페이지인 `/create`에서 내용을 입력하고 제출을 했을 때 도달하게 되는 `/create_action`페이지에서 쿼리 파라미터를 활용할 수 있도록 하는 코드를 만들어줬다.

```js
let body = "";
req.on("data", (data) => {
  body = body + data;
});
```

`req.on()`은 요청의 전송이 이루어질 때의 작동을 정의한다. 첫번째 인자에는 이벤트가, 그리고 두번째 인자에는 그 이벤트의 작동을 정의하게 된다. 이 코드의 경우 `data`이벤트가 발생했을 때, 그 `data`를 body에 할당해주고 있다. 여기에서 `data`는 `Buffer`로써, 이것이 어떤 형태를 갖추고 있는지에 따라 정제를 해줘야 하는데, 지금의 경우는 문자열인 것을 알기 때문에 string인 body에 합쳐줌으로써 문자열로 변환한 것이다.

```js
req.on("end", () => {
  const post = new URLSearchParams(body);
  const title = post.get("title");
  const content = post.get("content");
});
```

여기에선 `end`이벤트, 즉 요청이 끝날 때의 작동을 정의하고 있다. 현재 `body`는 문자열의 형태로 쿼리 스트링이 담겨있으므로, 그것을 활용하여 `new URLSearchParams(body)`를 통해 쿼리 파라미터 객체를 생성해줬다. 그리고 각 파라미터에 대한 값은 이전에 했던 것처럼 `get()`으로 가져올 수 있다.

## 가져온 데이터로 파일 생성하기

데이터를 가져오기만 해선 아무 쓸모가 없다. 이 데이터를 출력하기 위해선 `fs`를 사용해야 한다.

```js
req.on("end", () => {
  const post = new URLSearchParams(body);
  const title = post.get("title");
  const content = post.get("content");
  fs.writeFileSync(`data/${title}`, content);
  res.writeHead(200);
  res.end("success");
});
```

`fs.writeFileSync()`는 첫번째 인자로 파일을 생성할 경로와 그 파일의 이름을, 두번째 인자로 그 파일의 내용을 전달하면 원하는 경로에 원하는 파일을 생성해주는 메서드이다. 이렇게 작성을 해주고 작동을 해보면 `data`폴더에 파일이 생성되는 것을 확인할 수 있다.

## 파일 생성이 완료되면 주소를 리다이렉션 하기

파일이 만들어지고 나서 화면이 그대로 "success"만 뜬 상태로 멈춰있다면 의미가 없다. 또한 이 화면에서 새로고침을 하게 되면 내가 보낸 요청이 다시 발생하게 되는데, 이는 중복 요청이기 때문에 잘못된 작동이라고 볼 수 있다. 따라서 생성이 완료되면 리다이렉션을 통해 다른 페이지로 이동해줘야 한다.

```js
res.writeHead(303, { Location: `/?id=${title}` });
res.end();
```

이렇게 작성을 해주면 303에 의해 리다이렉션이 이루어지고, 내가 작성한 글로 페이지가 넘어가게 된다. 왜 하필 303을 사용하는 지에 대해선 나중에 정리하도록 하겠다.

### 이로써 구현이 끝난 듯 하지만 사실은 그렇지 않다.

만약 `title`에 한글을 작성하게 되면 오류가 발생하는 것을 확인할 수 있다. 이는 URI에서의 한글은 UTF-8로 인코딩이 되어 표현이 되야 하는데 지금 상태에서는 인코딩이 이루어지지 않고 있기 때문이다. 따라서 이를 해결 하기 위해선 한글을 인코딩하도록 하는 메서드를 사용해줘야 한다.

```js
res.writeHead(303, { Location: encodeURI(`/?id=${title}`) });
```

이렇게 `encodeURI()`를 사용하면 한글이 정상적으로 인코딩되어 오류가 발생하지 않는다.

참고: [Node.js 공식문서](https://nodejs.org/dist/latest-v16.x/docs/api/){:target="\_blank"}  
참고: [mdn 공식문서](https://developer.mozilla.org/ko/docs/Web){:target="\_blank"}
