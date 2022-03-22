---
title: "[Node.js] 글 수정 기능 구현하기 - (10)"
categories: Node.js
---

## 수정 버튼 만들기

글을 수정하는 버튼은 글을 읽는 화면에서만 나타나야 한다. 다시 말해 홈 화면이나 글 생성 화면에서는 보여선 안된다는 의미이다. 이를 처리하려면 `htmlTemplate`함수를 조금 수정하고 조건문을 활용하면 된다.

```js
const htmlTemplate = (content, update) => {
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
        ${update ? update : ""}<br/>
        ${content}
      </body>
    </html>
    `;
};
```

두번째 인자로 `update`를 만들어주고, 글 생성 버튼 아래에 추가해준다. 또한 조건문으로 `update`인자에 값이 입력될 경우에는 `update`에 대한 내용을 보여주지만 그렇지 않을 경우에는 아무 것도 보여주지 않도록 적어준다.

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
    const template = htmlTemplate(data, update);
    res.writeHead(200);
    res.end(template);
  });
}
```

이제 첫번째 if문으로 와서 `update`인자에 들어갈 버튼의 내용을 `update` 변수에 적어주고, `htmlTemplate(data, update)`라고 적어주면 버튼이 정상적으로 출력되는 것을 볼 수 있다. 이 때, 홈 화면과 글 생성 화면에선 수정 버튼이 보이지 말아야 하므로 글을 읽을 때에만 쿼리 파라미터에 id가 존재한다는 점을 활용해 조건문을 만들어주면 된다.

```js
const update = id
  ? `
      <a href="/update?id=${id}">글 수정</a>
      `
  : null;
```

글 수정 화면에서 어떤 글을 수정하려는지 판별할 수 있어야하므로 쿼리파라미터로 id를 넘겨줬다.

## 글 수정 페이지 구현하기

글 생성 때처럼 수정 버튼을 누르면 글 수정 화면이 나와야 한다.

```js
else if (pathname === "/update") {
    fs.readFile(`./data/${id}`, "utf-8", (err, data) => {
      const update = `
      <a href="/?id=${id}">수정 취소</a>
      `;

      const content = `
      <form action="${url.origin}/update_action" method="post">
        <input type="hidden" name="filename" value="${id}"
        <p><input type="text" name="title" placeholder="제목" value=${id} /></p>
        <p>
          <textarea name="content" placeholder="내용" style="resize:none">${data}</textarea>
        </p>
        <p>
          <input type="submit" />
        </p>
      </form>
      `;
      const template = htmlTemplate(content, update);
      res.writeHead(200);
      res.end(template);
    });
  }
```

else 문 전에 else if 문을 하나 추가하여 글 수정 화면을 구현하였다. 글 수정화면에선 글 생성 화면처럼 사용자가 입력하는 `input`과 `textarea`가 있어야 하고, 작성된 글에 대한 내용이 그 안에 미리 들어가있어야 한다. 그렇기 때문에 수정하고자 하는 글의 내용을 불러오기 위해 `fs.readFile`이 필요하다.

```js
const update = `
<a href="/?id=${id}">수정 취소</a>
`;
```

글 수정 화면에선 글 수정 버튼이 수정 취소 버튼으로 바뀌어야 하고, 그 버튼을 눌렀을 때 수정하려던 글 화면으로 넘어가야하므로 이렇게 구현해줬다.

```js
const content = `
<form action="${url.origin}/update_action" method="post">
  <input type="hidden" name="filename" value="${id}"
  <p><input type="text" name="title" placeholder="제목" value=${id} /></p>
  <p>
    <textarea name="content" placeholder="내용" style="resize:none">${data}</textarea>
  </p>
  <p>
    <input type="submit" />
  </p>
</form>
`;
```

글 수정 화면에선 수정하고자 하는 글의 내용이 미리 입력되어 있어야 하므로 `input`태그에선 `value` 속성에 제목을 미리 넣어줬고, `textarea`태그에선 태그 안에 `data`를 적어줬다. 여기에서 주의할 점은 글 수정 시 작성자가 제목을 바꾸게 되면 POST 요청이 존재하는 파일을 수정하는 것이 아닌 새로운 파일을 생성하게 될 것이기 때문에 이에 대한 처리를 해줘야한다는 점이다.

```js
  <input type="hidden" name="filename" value="${id}"
```

이렇게 화면에는 보이지 않는 `input`태그를 생성하여 POST요청 시 이 태그를 활용한다면 문제 없이 이미 존재하는 파일을 지목하여 수정할 수 있다.

## POST를 통해 글 수정 완료하기

```js
else if (pathname === "/update_action") {
    let body = "";
    req.on("data", (data) => {
      body = body + data;
    });
    req.on("end", () => {
      const post = new URLSearchParams(body);
      const filename = post.get("filename");
      const title = post.get("title");
      const content = post.get("content");
      fs.renameSync(`data/${filename}`, `data/${title}`);
      fs.writeFileSync(`data/${title}`, content);
      res.writeHead(303, { Location: encodeURI(`/?id=${title}`) });
      res.end();
    });
  }
```

else if문을 하나 더 추가해주고 POST 요청을 하기 위한 페이지를 구현하면 된다. 글 생성 때와 유사하게 작성하면 되지만 수정해야 하는 부분이 몇 가지 존재한다.

```js
const filename = post.get("filename");
```

기존에 존재하는 파일을 지목하기 위한 변수이다.

```js
fs.renameSync(`data/${filename}`, `data/${title}`);
```

`fs.renameSync(oldpath, newpath)`는 기존에 존재하는 파일의 이름을 새로 입력한 이름으로 변경해주는 코드이다.

이렇게 작성하고 글을 수정하면 정상적으로 `data`폴더의 파일이 변경되는 것을 확인할 수 있다.

참고: [Node.js 공식문서](https://nodejs.org/dist/latest-v16.x/docs/api/){:target="\_blank"}  
참고: [mdn 공식문서](https://developer.mozilla.org/ko/docs/Web){:target="\_blank"}
