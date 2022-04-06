---
title: "[GraphQL] 쿼리와 뮤테이션 (Query & Mutation) - (2)"
categories: GraphQL
---

## 쿼리와 뮤테이션 (Query & Mutation)

### 쿼리(Query)

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

이렇게 작업 이름을 활용하면 한번의 요청만으로도 원하는 정보를 모두 받을 수 있을 뿐만 아니라, 같은 코드를 반복하여 작성하지 않아도 된다. 그저 작업 이름 안에 있는 변수(Variables)들과 지시어(Directives)만 수정해줌으로써 다른 정보를 손쉽게 요청할 수 있다. 게다가, 정보를 가져오기 위한 이러한 함수를 프론트엔드단에서 생성할 수 있다는 점은 백엔드단에서 정의하여 전달해줘야만 했던 REST의 request / response 형식에서 자유로워질 수 있다는 점에서 큰 메리트가 있다.

### 뮤테이션(Mutation)

서버에서 데이터를 가져올 수 있다면 서버의 데이터도 수정할 수 있어야 한다. 즉, REST에서의 POST와 같은 역할을 할 수 있어야 한다는 뜻이다. Gql에서는 이를 위해선 `mutation`을 사용함으로써 데이터를 fetch하는 것이 아니라 수정하는 것임을 명시해주는 것이 좋다.

<figure>
   <img src="https://user-images.githubusercontent.com/80687334/161788966-36b26c43-70ed-4fb1-8060-e74d030c5d66.png" /> 
   <figcaption>뮤테이션(Mutation)의 예시</figcaption>
</figure>

예시에서 볼 수 있듯이 새로 생성된 리뷰에 대한 정보가 반환된다.

참고: [GraphQL 공식문서](https://graphql.org/learn/){:target="\_blank"}
