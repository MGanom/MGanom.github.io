---
title: "[Next.js] 페이지를 완전한 SSR로 전환하기(getServerSideProps) - (5)"
categories: Next.js
---

## getServerSideProps란?

Next.js는 기본적으로 SSR을 통해 pre-render를 한 html과 js파일을 클라이언트에게 전송하고 클라이언트 측에서 그 pre-render된 html에 js를 활용하여 hydration과 같은 CSR 과정을 통해 페이지 렌더를 완료한다. 이렇게 하면 클라이언트 측에서 렌더가 완료되기 전까지 빈 html을 보고 있지 않아도 되기에 UX적인 측면에서 좋은 점도 있지만, pre-render된 html에 hydration이 이루어지는 동안 로딩 화면이나 상호 작용이 불가능한 태그들을 보고 있어야 한다는 점도 존재한다. 이러한 상황을 싫어하는 사람들을 위해 아예 모든 과정을 SSR로 전환함으로써 클라이언트 측에서는 완성된 html을 받아볼 수 있는 방법이 있고, 그것이 바로 getServerSideProps가 하는 역할이다.

### getServerSideProps 사용 예시

참고: [Next.js 공식문서](https://nextjs.org/docs){:target="\_blank"}
