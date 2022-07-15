---
title: "[JavaScript] 웹 브라우저의 작동 원리"
categories: JavaScript
---

## 웹 브라우저(Web Browser)란?

웹 브라우저란 웹 서버 상에서 쌍방향으로 통신하고 HTML 문서나 파일을 출력하는 GUI 기반의 응용 소프트웨어를 의미한다. 브라우저의 주요 기능으로는 사용자가 선택한 자원을 서버에 요청하고 그것을 화면에 표시하는 것이 있다.

## 웹 브라우저의 기본 구조

브라우저의 주요 구성 요소는 사용자 인터페이스, 브라우저 엔진, 렌더링 엔진, 통신, UI 백엔드, 자바스크립트 엔진, 그리고 자료 저장소까지 총 7가지가 있다.

### 사용자 인터페이스(User Interface)

사용자 인터페이스는 요청한 페이지를 보여주는 부분을 제외한 주소 표시줄이나 북마크 메뉴 등의 나머지 모든 부분을 의미한다.

### 브라우저 엔진(Browser Engine)

브라우저 엔진은 웹 브라우저의 핵심이 되는 구성 요소이며, 주된 역할은 HTML 문서와 기타 자원의 웹 페이지를 사용자가 상호작용할 수 있도록 시각적으로 변환하는 것이다. 그리고 이를 통해 사용자 인터페이스와 렌더링 엔진 간의 동작을 제어하는 엔진이다.

### 렌더링 엔진(Rendering Engine)

렌더링 엔진은 요청한 콘텐츠에 대한 자원을 화면에 표시하는 역할을 한다.

### 통신(Networking)

통신이란 HTTP 요청과 같은 네트워크 호출을 의미한다.

### UI 백엔드 (UI Backend)

UI 백엔드란 콤보 박스와 창 같은 기본적인 장치를 그리는 역할을 한다.

### 자바스크립트 엔진(JavaScript Engine)

자바스크립트 엔진은 자바스크립트 코드를 해석하고 실행하는 역할을 한다.

### 자료 저장소(Data Storage)

자료 저장소란 말 그대로 자료를 저장하는 계층으로써, 쿠키, 세션, 그리고 토큰 등 모든 종류의 자원이 저장되는 곳을 의미한다.

참고: [PoiemaWeb](https://poiemaweb.com/js-browser){:target="\_blank"}
참고: [브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361){:target="\_blank"}
참고: [Render-tree Construction, Layout, and Paint](https://web.dev/critical-rendering-path-render-tree-construction/){:target="\_blank"}
