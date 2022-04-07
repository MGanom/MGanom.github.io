---
title: "[GraphQL] 스키마와 타입 (Schema & Type) - (3)"
categories: GraphQL
last_modified_at: 2022-04-07
---

## 스키마와 타입 (Schema & Type)

### 스키마 (Schema)

Gql은 요청 쿼리문과 응답 결과의 형태가 거의 일치하기 때문에 서버가 어떠한 구조를 이루고 있든 요청자는 반활 될 결과를 예측할 수 있다. 하지만 이는 요청에 성공했을 경우의 이야기이기 때문에 애초에 요청을 원활하게 이루어지기 위해선 서버에 데이터가 어떠한 구조로 저장되어 있고, 요청자는 어떠한 필드를 선택할 수 있으며, 어떤 종류의 객체를 반환할 수 있는지 등을 알 수 있어야 한다. 이러한 구조, 제약 조건, 그리고 관계 등에 대해 전반적인 명세를 기술한 것을 스키마라고 한다. Gql은 쿼리 가능한 데이터들에 대한 타입들을 정의하고, 요청 쿼리문이 들어오면 이에 대한 유효성을 스키마를 통해 검사한 후 실행된다.

### 타입 (Type)

#### 객체 타입(Object types)

Gql 스키마의 가장 기본적인 구성 요소는 객체 타입이며 이는 서버에서 가져올 수 있는 객체의 종류와 필드를 나타낸다.

<figure>
   <img src="https://user-images.githubusercontent.com/80687334/161997828-9005d7a2-3f6a-4ec9-9cd5-c62c39fe9015.png
" /> 
   <figcaption>객체 타입의 예시</figcaption>
</figure>

`type`은 타입을 정의함을 나타낸다. `Character`는 객체 타입의 이름이다. `name`과 `appearsIn`은 객체 타입의 필드이며 이는 `Character` 타입인 쿼리에선 이 두 개의 필드만 접근 가능하다는 의미이다. `String`은 Gql의 내장 스칼라 타입 중 하나이다. `String` 뒤의 `!`는 이 필드가 `non-nullable`, 즉 이 필드는 쿼리 요청 시에 반드시 반환된다는 의미이다. `[Episode]!` 또한 `non-nullable`이기에 비어있는 배열일지라도 항상 반환이 된다는 의미이다. 객체 타입에도 쿼리와 마찬가지로 인자를 활용할 수 있다.

#### 쿼리 타입 & 뮤테이션 타입 (Query types & Mutation types)

대부분의 스키마는 객체 타입이지만 쿼리 타입과 뮤테이션 타입이라는 타입 또한 존재한다.

<figure>
   <img src="https://user-images.githubusercontent.com/80687334/162165841-17caf22c-f131-4481-950c-046baa82f8f2.png
" /> 
   <figcaption>쿼리 타입과 뮤테이션 타입의 예시</figcaption>
</figure>

모든 Gql 서비스는 `query` 타입을 갖고 있으며 `mutation` 타입은 선택적으로 갖고 있다. 이 두 가지 타입은 모든 쿼리의 entry point를 정의하며, 만약 어떤 서비스에 이 타입들이 정의되어 있다면 그 서비스를 이용하기 위해선 쿼리 요청에 반드시 그 타입들을 담아야 한다. 참고로, 이 두 타입의 기본 형태 자체는 객체 타입과 같기 때문에 작동 방식 또한 객체 타입과 동일하긴 하다.

#### 스칼라 타입 (Scalar types)

스칼라 타입은 쿼리의 끝을 나타낸다.

<figure>
   <img src="https://user-images.githubusercontent.com/80687334/162172169-64d9992d-c3fc-4b0d-96d8-e53a2a60e380.png
" /> 
   <figcaption>스칼라 타입의 예시</figcaption>
</figure>

예시에서 `name`과 `id`는 하위 필드가 없으며 둘은 쿼리의 끝을 의미하는 스칼라 타입이다. 기본 제공되는 스칼라 타입에는 `Int`, `Float`, `String`, `Boolean`, `ID`가 있으며 `scalar 이름`을 통해 커스텀 스칼라 타입을 생성할 수도 있다.

참고: [GraphQL 공식문서](https://graphql.org/learn/){:target="\_blank"}
