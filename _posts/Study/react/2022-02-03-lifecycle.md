---
title: "Lifecycle (생명주기)"
categories: React
---

## Lifecycle(생명주기)란?

모든 컴포넌트에는 여러 종류의 '생명주기 메서드'가 있다. 이 메서드들을 활용하면 컴포넌트가 DOM 트리에 마운트 될 때부터 마운트 해제 될 때까지 컴포넌트 내의 코드들을 원하는 시점에 실행되도록 설정할 수 있다. 생명주기 메서드는 클래스형 컴포넌트에서만 사용이 가능하기 때문에 함수형 컴포넌트에선 이러한 작동을 할 수 없었으나, React Hook이 소개되면서 State Hook과 Effect Hook 등을 통해 생명주기 메서드와 유사한 작동이 가능해졌다.

## 대표적인 생명주기 메서드

### 1. constructor()

constructor(생성자)는 컴포넌트가 마운트 되기 전에 호출되며, 보통 this.state에 객체를 할당하여 state를 초기화하고 인스턴스에 이벤트 처리 메서드를 바인딩하기 위해 사용된다.

### 2. render()

클래스 컴포넌트에서 반드시 구현돼야 하는 메서드로써, 주로 this.props와 this.state 값을 활용하여 React 엘리먼트를 반환하기 위해 사용된다.

### 3. componentDidMount()

컴포넌트가 DOM 트리에 마운트 된 직후 호출되는 메서드로써, 보통 네트워크 요청 등을 통해 외부에서 데이터를 불러올 때 사용된다. 만약 여기에서 setState()가 호출될 경우 render()가 다시 발생하게 되는데, 이 경우 브라우저가 화면을 갱신하기 전에 이루어져서 사용자는 그 과정을 볼 수 없다. 하지만 결과적으로 render()가 여러 번 발생하는 것이기 때문에 초기 state는 constructor()에서 할당해주는게 좋다.

### 4. componentDidUpdate()

최초 렌더링에서는 호출되지 않고 갱신이 일어난 직후에 호출되는 메서드로써, 컴포넌트가 갱신됐을 때 DOM을 조작하기 위해 활용하면 된다. 만약 이 안에서 setState()를 호출할 경우 무한 반복이 이루어지거나 과도하게 추가적인 렌더링이 유발될 수 있으므로 꼭 조건문 등을 활용해야 한다.

### 5. componentWillUnmount()

컴포넌트가 DOM 트리에서 마운트 해제되어 제거되기 직전에 호출되는 메서드로써, 네트워크 요청 취소와 같은 정리 작업을 수행할때 활용하면 된다. 이후 이 컴포넌트는 다시 렌더링되지 않으므로 이 안에서 setState()를 호출하면 안된다.

## 생명주기 메서드의 작동 흐름

![image](https://user-images.githubusercontent.com/80687334/152369566-489bd19d-b9fb-4111-a6e7-d077423451ab.png)
_출처: https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/_

1. ReactDOM.render()로 컴포넌트가 전달되면 마운트가 되기 전에 constructor()가 호출된다. 동시에 this.state가 초기화 된다.
2. render()가 호출되어 화면에 표시되어야 할 내용을 바탕으로 DOM을 업데이트한다.
3. 컴포넌트가 마운트 되어 componentDidMount()가 호출된다.
4. 만약 3번에서 setState()가 호출됐을 경우 render()가 한번 더 이루어지게 된다.
5. 컴포넌트에서 갱신이 이루어질 때마다 componentDidUpdate()가 호출된다.
6. 만약 5번에서 setState()가 호출됐을 경우 render()가 한번 더 이루어지게 된다.
7. 컴포넌트를 DOM 트리에서 마운트 해제하려 하면 해제 직전에 componentWillUnmount()가 호출되고 이후 마운트 해제가 이루어진다.
