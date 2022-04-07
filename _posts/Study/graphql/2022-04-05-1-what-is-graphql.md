---
title: "[GraphQL] GraphQL의 개념 - (1)"
categories: GraphQL
last_modified_at: 2022-04-07
---

## GraphQL이란?

GraphQL(GQL)은 Meta에서 만든 API를 위한 쿼리 언어이다.  
GQL은 Structed Query Language(SQL)과 주 쓰임새가 다른데, GQL은 웹 클라이언트가 서버로부터 데이터를 효율적이 가져오는 것이 주 쓰임새인 반면에 SQL은 백엔드 시스템에서 데이터베이스 시스템에 저장된 데이터를 효율적으로 가져오는 것이 주 쓰임새이다.

### GraphQL의 장점

1. Overfetching이 발생하지 않는다.
   Overfetching은 내가 필요하거나 요청한 정보보다 많은 정보를 서버로부터 받는 것을 의미한다. 예를 들어, 어떠한 유저의 정보 중 이름에 대한 정보가 필요하여 `"/users/1"`이라는 URL로 유저에 대한 정보를 요청하여 응답을 받았는데, 그 안에 이름이라는 정보 말고도 성별, 나이, 닉네임과 같은 다양한 정보가 들어있게 되는 경우가 overfetching이다.

2. Underfetching이 발생하지 않는다.
   Underfetching은 내가 필요한 정보를 받기 위해 서버에 여러 번 요청해야 하는 상황을 의미한다. 예를 들어, 특정 유저 3명의 정보가 필요한 상황에서 `"/users/1"`, `"/users/2"`, `"/users/3"`처럼 세 번에 걸친 요청과 응답을 받아야 하는 경우가 underfetching이다.

3. 확실하게 정의된 타입과 스키마가 존재하기에 서버와의 통신에서 오류가 발생할 일이 적다.
   GraphQL은 URL이 아닌 Query를 통해 통신이 이루어지기 때문에 Query에 요청하고자 하는 정보를 모두 담아서 서버에 요청하면 서버에선 정확히 이에 대한 정보만을 응답에 담아서 보낸다.

4. 어플리케이션이 REST API를 사용하고 있다면 QraphQl도 적용할 수 있다.
   GraphQL은 어플리케이션 아키텍쳐를 새로이 만들거나 변화시키지 않기 때문에 REST API가 사용되고 있는 아키텍쳐라면 문제 없이 녹아들어 API 관리 패키지들과 충돌이 발생하지 않는다.

### GraphQL의 단점

1. 처음 접하는 사람은 다루기 힘들다.
   REST API가 익숙한 사람들은 쿼리, 뮤테이션, 스키마, 타입 등을 활용하여 통신하는 GraphQL이 친숙하지 않을 것이다. 이 내용들을 이해해야 GraphQL을 다룰 수 있기 때문에 이에 대한 학습이 필요하다.

2. 요청의 크기가 커질 수 있다.
   쿼리를 활용하여 요청을 하다 보니 요청 자체의 크기가 커질 수 있다. 이는 서버, 데이터베이스, API 등의 유지 보수 측면에서 비용이 높아질 수 있음을 의미하기도 한다.

참고: [GraphQL 공식문서](https://graphql.org/learn/){:target="\_blank"}  
참고: [Red Hat - What is GraphQL?](https://www.redhat.com/en/topics/api/what-is-graphql){:target="\_blank"}
