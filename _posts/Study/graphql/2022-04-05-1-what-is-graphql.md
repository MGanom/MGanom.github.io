---
title: "[GraphQL] GraphQL의 개념과 설치 - (1)"
categories: GraphQL
---

## GraphQL이란?

GraphQL(GQL)은 Meta에서 만든 API를 위한 쿼리 언어이다.  
GQL은 Structed Query Language(SQL)과 주 쓰임새가 다른데, GQL은 웹 클라이언트가 서버로부터 데이터를 효율적이 가져오는 것이 주 쓰임새인 반면에 SQL은 백엔드 시스템에서 데이터베이스 시스템에 저장된 데이터를 효율적으로 가져오는 것이 주 쓰임새이다.

### GraphQL의 구조

#### 쿼리와 뮤테이션 (Query & Mutation)

##### 쿼리(Query)

GraphQL은 URL이 아닌 쿼리를 통해 요청이 이루어진다. 또한 이 요청에 따른 응답의 구조는 요청한 쿼리와 유사하기 때문에 굉장히 직관적이다.

<figure>
   <img src="https://user-images.githubusercontent.com/80687334/161761339-859e3b80-da3a-4092-a32f-8e7d4f01b1cf.png" /> 
   <figcaption  figcaption>요청 쿼리문(좌측) & 응답 데이터(우측)</figcaption>
</figure>

이런 식으로 정확하게 요청하는 정보만을 받을 수 있다.  
물론, 쿼리를 더욱 효율적으로 사용하기 위해선 인자(Arguments), 별칭(Aliases), 프래그먼트(Fragments) 등 다양한 기능의 활용법을 익혀야 한다. 대표적으로 작업 이름(Operation Name)이 있는데, 이 기능은 쿼리를 생성하기 위한 함수라고도 볼 수 있다.

<figure>
   <img src="https://user-images.githubusercontent.com/80687334/161781114-8cf7d882-3adb-4b23-b86e-5e6c798a9dd0.png" /> 
   <figcaption>작업 이름(Operation Name)의 예시</figcaption>
</figure>

이렇게 작업 이름을 활용하면 한번의 요청만으로도 원하는 정보를 모두 받을 수 있을 뿐만 아니라, 같은 코드를 반복하여 작성하지 않아도 된다. 그저 작업 이름 안에 있는 변수(Variables)들과 지시어(Directives)만 수정해줌으로써 다른 정보를 손쉽게 요청할 수 있다.

##### Mutation(뮤테이션)

### GraphQL의 장점

1. Overfetching 혹은 underfetching이 발생하지 않는다.
   Overfetching은 내가 필요하거나 요청한 정보보다 많은 정보를 서버로부터 받는 것을 의미하고, underfetching은 그 반대를 의미한다. 예를 들어, 어떠한 유저의 정보 중 이름에 대한 정보가 필요하여 `"/users/1"`이라는 URL로 유저에 대한 정보를 요청하여 응답을 받았는데, 그 안에 이름이라는 정보 말고도 성별, 나이, 닉네임과 같은 다양한 정보가 들어있게 되는 경우가 overfetching이다. 반대로, 특정 유저 3명의 정보가 필요한 상황에서 `"/users/1"`, `"/users/2"`, `"/users/3"`처럼 세 번에 걸친 요청과 응답을 받아야 하는 경우를 underfetching의 예시라고 할 수 있다.  
   GraphQL은 URL이 아닌 Query를 통해 통신이 이루어지기 때문에 Query에 요청하고자 하는 정보를 모두 담아서 서버에 요청하면 서버에선 정확히 이에 대한 정보만을 응답에 담아서 보내기 때문에 상술한 문제가 발생하지 않게 된다.

참고: [Tailwind CSS 공식문서](https://tailwindcss.com/docs/installation){:target="\_blank"}
