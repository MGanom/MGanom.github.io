---
title: "[Node.js] 함수로 재사용성 향상하기 및 Form으로 글 생성 UI 만들기 - (8)"
categories: Node.js
---

## 함수를 사용하여 재사용성 향상하기

현재 목록을 생성하는 코드는 정해진 경로만을 사용하고 있다. 즉, 다른 종류의 목록을 생성하려면 똑같은 코드를 경로만 다르게 하여 작성해줘야 하는 상황인 것인데, 이를 피하기 위해 함수를 활용하면 된다.

```js
const dataList = fs.readdirSync("./data");

const createList = (filelist) => {
  let list = "<ol>";
  for (let i = 0; i < filelist.length; i++) {
    list = list + `<li><a href="/?id=${filelist[i]}">${i + 1}번째</a></li>`;
  }
  list = list + "</ol>";
  return list;
};
```

우선 경로에 존재하는 파일들을 담는 `fs.readdirSync()` 메서드를 `dataList`에 할당해준다. 이후 사용자가 인자로 전달해주는 `filelist`에 따라 목록을 생성하는 함수인 `createList`를 만들어준다. 이렇게 하면 호출할 때 `createList(filelist)`의 `filelist`에 전달해주는 경로에 따라 목록이 생성되는 것을 확인할 수 있다.

```js
const htmlTemplate = (content) => {
  return `
  <!DOCTYPE html>
  <html>
    <head>
      <title>문가네</title>
      <meta charset="utf-8" />
      </head>
    <body>
        <h1><a href="/">문가네</a></h1>
      ${createList(dataList)}
      ${content}
    </body>
  </html>
  `;
};
```

그 다음 `template`에 해당하는 부분도 함수화해줬다. `content`에 전달해주는 인자에 따라 내용만 바뀌고 나머지는 모양이 그대로 유지되도록 해줬다. 이렇게 하는 이유는 이후 글 생성 페이지를 만들 때나 다른 페이지를 만들 때 같은 템플릿을 사용하는 것에서 발생하는 중복 코드를 피하기 위해서이다.

```js
fs.readFile(`./data/${id}`, "utf-8", (err, data) => {
  if (id === null) {
    data = "어서오세요 문가네입니다.";
  }
  const template = htmlTemplate(data);
  res.writeHead(200);
  res.end(template);
});
```

`template` 부분이 이렇게나 간결해진다. 앞으로도 이런 식으로 앞으로도 중복되는 코드가 생기거나, 재사용해야 하는 부분이 바뀐다면 함수를 적극적으로 활용하면 된다.

## 글 생성 UI 만들기

현재의 사이트에선 글을 작성하거나 삭제하는 것이 불가능하다. 이 기능을 추가하기 위해 HTML의 `form`을 사용해 볼 수 있다.

```js
`
      <body>
        <h1><a href="/">문가네</a></h1>
        ${createList(dataList)}
        <a href="/create">글 생성</a>
        ${content}
      </body>
`;
```

글 생성 페이지로 넘어갈 `a`태그를 만들어줬고, 그 주소는 `"/create"`로 결정했다.

```js
if (pathname === "/") {
  // 코드
} else if (pathname === "/create") {
  const content = `
    <form action="${url.host}/create_action" method="post">
      <p><input type="text" name="title" placeholder="제목" /></p>
      <p>
        <textarea name="content" placeholder="내용" style="resize:none"></textarea>
      </p>
      <p>
        <input type="submit" />
      </p>
    </form>
    `;
  const template = htmlTemplate(content);
  res.writeHead(200);
  res.end(template);
} else {
  // 코드
}
```

주소에 따라 나타나는 내용이 다르도록 해주기 위해 `else if`문을 사용하였고, 경로가 `"/create"`면 `htmlTemplate(content)`에서 `content`에 들어가게 될 내용을 변수에 할당해줬다.

```js
<form action="${url.host}/create_action" method="post">
  <p>
    <input type="text" name="title" placeholder="제목" />
  </p>
  <p>
    <textarea name="content" placeholder="내용" style="resize:none"></textarea>
  </p>
  <p>
    <input type="submit" />
  </p>
</form>
```

`form`에서의 `action`은 `submit`이 이루어졌을 때 이동할 주소 및 통신을 할 주소를 의미한다. 또한 `method`는 서버와 통신할 때 사용하는 http 메서드로써, 데이터를 작성하거나 업데이트할 때 사용되는 메서드이다. 이렇게 `action`을 통해 통신을 하게 되면, `form`안에 있는 `input`과 `textarea`의 `name` 속성에 할당된 내용이 쿼리 파라미터로의 변수로, 그리고 작성한 내용이 쿼리 파라미터의 내용으로 넘어가게 된다. 즉, 이 경우에는 `http://localhost:3000/http://localhost:3000/create_action?title=작성된내용&content=작성된내용`으로 넘어가게 된다는 뜻이다.

참고: [Node.js 공식문서](https://nodejs.org/dist/latest-v16.x/docs/api/){:target="\_blank"}  
참고: [mdn 공식문서](https://developer.mozilla.org/ko/docs/Web){:target="\_blank"}
