---
title: "[Node.js] 사이트의 보안성 향상하기 - (12)"
categories: Node.js
---

## 서버에 존재하는 파일들의 보안성 향상하기

사이트 관리자 외의 사용자는 서버에 존재하는 파일들 중 접근이 허가된 파일들을 제외하곤 접근할 수 없어야 한다. 하지만 현재는 URL에 `../`나 `./` 등의 폴더 접근 방식을 사용하면 서버에 있는 모든 파일들에 접근할 수 있는 상태이다. 이를 방지하기 위한 조치가 필요하다.

```js
const path = require("path");
```

`path`는 Node.js에서 파일과 경로를 다루기 위해 사용되는 모듈이다. 경로에서 필요한 부분 추출하기, 파일이름 알아내기 등 다양한 기능을 제공한다.

```js
const safeId = id && path.parse(id).base;
//혹은
const safeId = id && path.basename(id);

fs.readFile(`./data/${safeId}`, "utf-8", (err, data) => {});
```

`path.parse(경로)`는 경로에 대한 속성들을 담은 객체를 반환한다. 그리고 그 중 `base`는 경로에서의 파일 이름에 해당한다. `path.basename(경로)`는 마찬가지로 경로에서의 파일 이름을 반환한다. 이렇게 설정해주면 사용자가 URL을 통해 다른 경로로 접근하는 것을 방지할 수 있다. 이제 이처럼 경로로부터 파일을 읽어오는 모든 곳을 똑같이 수정해주면 된다.

## 사용자가 입력하는 내용에서 발생하는 버그 잡기

현재 우리가 입력하는 내용은 HTML페이지에서 표현이 되고 있다. 그렇다는 것은 만약 사용자가 글 생성을 할 때 `<script>`태그를 사용하여 자바스크립트 작동을 유발할 수도 있고, `<h1>`등의 일반적인 태그를 사용할 수도 있는 상태라는 것이다. 이를 방지하기 위한 방법엔 여러가지가 있는데, `<>`와 같은 특수문자들을 HTML Entities로 표현하거나 `sanitize-html`패키지를 사용하는 등 상황에 맞게 사용하면 된다. 여기에선 `sanitize-html` 패키지를 사용해 보려 한다.

### npm을 사용하여 사이트 패키지화

우선 패키지를 사용하려면 환경을 구축해야 한다.  
이는 터미널로 수행할 수 있다.

```
$ npm init
```

패키지 이름, 버전, 설명, 레포지토리 등을 확인하며 엔터를 눌러 `package.json`을 생성해주면 된다.

```
$ npm install sanitize-html
```

이렇게 입력해주면 패키지가 설치된다.

```js
const sanitizeHtml = require("sanitize-html");
```

이후 모듈을 불러오고

```js
req.on("end", () => {
  const post = new URLSearchParams(body);
  const title = post.get("title");
  const content = post.get("content");
  const sanitizedContent = sanitizeHtml(content);
  fs.writeFileSync(`data/${title}`, sanitizedContent);
  res.writeHead(303, { Location: encodeURI(`/?id=${title}`) });
  res.end();
});
```

`create_action` 부분을 이렇게 수정해주면 스크립트 태그는 물론 위험한 내용들은 전부 제거되는 것을 확인할 수 있다.

참고: [Node.js 공식문서](https://nodejs.org/dist/latest-v16.x/docs/api/){:target="\_blank"}  
참고: [mdn 공식문서](https://developer.mozilla.org/ko/docs/Web){:target="\_blank"}  
참고: [sanitize-html](https://www.npmjs.com/package/sanitize-html){:target="\_blank"}
