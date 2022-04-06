---
title: "[GraphQL] GraphQL의 개념 - (1)"
categories: GraphQL
last_modified_at: 2022-04-06
---

## GraphQL이란?

GraphQL(GQL)은 Meta에서 만든 API를 위한 쿼리 언어이다.  
GQL은 Structed Query Language(SQL)과 주 쓰임새가 다른데, GQL은 웹 클라이언트가 서버로부터 데이터를 효율적이 가져오는 것이 주 쓰임새인 반면에 SQL은 백엔드 시스템에서 데이터베이스 시스템에 저장된 데이터를 효율적으로 가져오는 것이 주 쓰임새이다.

### GraphQL의 장점

1. Overfetching 혹은 underfetching이 발생하지 않는다.
   Overfetching은 내가 필요하거나 요청한 정보보다 많은 정보를 서버로부터 받는 것을 의미하고, underfetching은 그 반대를 의미한다. 예를 들어, 어떠한 유저의 정보 중 이름에 대한 정보가 필요하여 `"/users/1"`이라는 URL로 유저에 대한 정보를 요청하여 응답을 받았는데, 그 안에 이름이라는 정보 말고도 성별, 나이, 닉네임과 같은 다양한 정보가 들어있게 되는 경우가 overfetching이다. 반대로, 특정 유저 3명의 정보가 필요한 상황에서 `"/users/1"`, `"/users/2"`, `"/users/3"`처럼 세 번에 걸친 요청과 응답을 받아야 하는 경우를 underfetching의 예시라고 할 수 있다.  
   GraphQL은 URL이 아닌 Query를 통해 통신이 이루어지기 때문에 Query에 요청하고자 하는 정보를 모두 담아서 서버에 요청하면 서버에선 정확히 이에 대한 정보만을 응답에 담아서 보내기 때문에 상술한 문제가 발생하지 않게 된다.

참고: [GraphQL 공식문서](https://graphql.org/learn/){:target="\_blank"}
