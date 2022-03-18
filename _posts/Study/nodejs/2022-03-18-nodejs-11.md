---
title: "[Node.js] 글 삭제 기능 구현하기 - (11)"
categories: Node.js
---

## 삭제 버튼 만들기

삭제 버튼의 경우 생성이나 업데이트 버튼처럼 화면이 존재하지 않는다. 그렇기 때문에 버튼을 누르는 순간 바로 서버와 통신을 해야하므로 바로 `form`태그를 활용해주면 된다.

```js
const htmlTemplate = (content, update, remove) => {
  return `
    <!DOCTYPE html>
    <html>
      <head>
        <title>문가네</title>
        <meta charset="utf-8" />
      </head>
      <body>
        <h1><a href="/">문가네</a></h1>
        ${createList(dataList)}<br />
        <a href="/create">글 생성</a>
        ${update ? update : ""}
        ${remove ? remove : ""}<br />
        ${content}
      </body>
    </html>
    `;
};
```

우선 글 수정 때처럼 `htmlTemplate`에 `remove`를 추가해준다. 이제 이 인자에 내용을 전달해주는 페이지에서는 그 내용이 보일 것이다.

```js
if (pathname === "/") {
  fs.readFile(`./data/${id}`, "utf-8", (err, data) => {
    if (id === null) {
      data = "어서오세요 문가네입니다.";
    }
    const update = id
      ? `
      <a href="/update?id=${id}">글 수정</a>
      `
      : null;
    const remove = id
      ? `
      <form action="delete_action" method="post">
        <input type="hidden" name="title" value="${id}" />
        <input type="submit" value="글 삭제" />
      </form>
      `
      : null;
    const template = htmlTemplate(data, update, remove);
    res.writeHead(200);
    res.end(template);
  });
}
```

글을 읽고 있을 때만 삭제 버튼이 보여야하므로 `id`가 존재한다면 삭제 버튼 역할을 할 `form`이 보이고, 그렇지 않으면 보이지 않도록 적어줬다. 이전에도 설명했듯이 `form`이 submit이 이루어지면 `input`의 `name` 속성과 `value`속성의 값이 함께 쿼리파라미터로 넘어간다. 그렇기에 이 파일의 이름을 확인해 줄 `title`을 `name`으로 가진 `input`을 `hidden`으로 추가해주었다.

## POST를 통해 파일 삭제 완성하기

```js
else if (pathname === "/delete_action") {
    let body = "";
    req.on("data", (data) => {
      body = body + data;
    });
    req.on("end", () => {
      const post = new URLSearchParams(body);
      const title = post.get("title");
      fs.unlinkSync(`data/${title}`);
      res.writeHead(303, { Location: "/" });
      res.end();
    });
  }
```

주소가 `'/delete_action'`일 때의 작동을 정의해주면 된다. `'/create_action'`때와 대부분 유사하지만 다른 점이라면 `fs.unlinkSync`를 사용하여 파일을 삭제한다는 점과 리다이렉션 경로가 `'/'`이라는 점이다. 이렇게 코드를 작성하고 삭제 버튼을 누르면 정상적으로 파일이 삭제되는 것을 확인할 수 있다.

참고: [Node.js 공식문서](https://nodejs.org/dist/latest-v16.x/docs/api/){:target="\_blank"}  
참고: [mdn 공식문서](https://developer.mozilla.org/ko/docs/Web){:target="\_blank"}
