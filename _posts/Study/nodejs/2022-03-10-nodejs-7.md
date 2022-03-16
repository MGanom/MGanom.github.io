---
title: "[Node.js] 간단한 코드 자동화 구현하기 - (7)"
categories: Node.js
---

## 첫 화면 구현하기

코드 자동화를 구현하기에 앞서, 현재 첫번째, 두번째, 세번째 페이지는 모두 정상적으로 출력되지만 기본 경로인 `'/'`로 이동했을 때는 undefined가 나오는 현상부터 해결해야 한다. 이 현상의 원인은 `fs.readFile`이 쿼리 파라미터를 참조하여 파일을 읽어올 때, 기본 경로에서는 불러오는 파일이 없다는 점이다. 이는 쿼리 파라미터가 없는 경우에 출력해줄 내용을 적음으로써 간단히 해결할 수 있다.

```js
if (pathname === "/") {
  fs.readFile(`data/${id}`, "utf-8", (err, data) => {
    if (id === null) {
      // 추가된 부분
      data = "어서오세요 문가네입니다.";
    }
    // 이전에 작성한 코드
  });
} else {
  // 이전에 작성한 코드
}
```

쿼리 파라미터가 존재하지 않을 때 `id`를 출력해 보면 `null`이 나오는 것을 확인할 수 있다. 즉, `fs.readFile`에서 `id`를 읽어올 때, 만약 `id`가 `null` 이라면 첫 화면에 보여질 내용을 `data`에 할당함으로써 그 내용이 출력되게끔 할 수 있다는 것이다.

## 폴더를 자동으로 읽어와 코드를 생성하도록 구현하기

현재의 코드는 `data` 폴더에 있는 파일들을 하나씩 적어줌으로써 화면에 출력되도록 작성되어 있다. 이렇게 코드가 작성되어 있을 경우 `data` 폴더에 파일이 추가가 된다면 그 파일에 따른 내용을 코드에 직접 추가를 해줘야 한다는 번거로움이 있다. 이러한 번거로움을 해결하기 위해 이 부분을 자동화하면 폴더에 있는 파일이 변동함에 따라 파일이 추가되거나 제거되더라도 자동으로 코드가 추가되고 제거되도록 구현할 수 있다.

```js
let list = "<ol>";
let filelist = fs.readdirSync("./data");
for (let i = 0; i < filelist.length; i++) {
  list = list + `<li><a href="/?id=${filelist[i]}">${i + 1}번째</a></li>`;
}
list = list + "</ol>";
```

`readdirSync(path)`는 `path`에 입력해주는 경로에 있는 파일들의 목록을 배열로 반환 해준다. 이 배열과 반복문을 활용하여 `list`라는 변수에 목록이 생성되도록 구현해줬다.

```js
const template = `
<!DOCTYPE html>
<html>
  <head>
    <title>문가네</title>
    <meta charset="utf-8" />
  </head>
  <body>
    <h1><a href="/">문가네</a></h1>
    ${list}
    ${data}
  </body>
</html>
`;
```

이후 `template` 부분에서 `ol` 태그로 직접 하나씩 입력해줬던 부분을 위에서 선언해준 `list`로 바꿔주면 정상적으로 출력되는 것을 확인할 수 있다. 또한 이 상태에서 `data` 폴더에 파일을 추가하면 자동적으로 `list`의 내용이 변경되고, 해당 페이지의 내용 또한 출력되는 것도 확인할 수 있다.

참고: [Node.js 공식문서](https://nodejs.org/dist/latest-v16.x/docs/api/){:target="\_blank"}
