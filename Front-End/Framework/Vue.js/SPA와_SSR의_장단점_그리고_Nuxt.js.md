# SPA와 SSR의 장단점 그리고 Nuxt.js

<br/>

원본글 링크

- [아하 프론트 개발기(1) — SPA와 SSR의 장단점 그리고 Nuxt.js](https://medium.com/aha-official/%EC%95%84%ED%95%98-%ED%94%84%EB%A1%A0%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EA%B8%B0-1-spa%EC%99%80-ssr%EC%9D%98-%EC%9E%A5%EB%8B%A8%EC%A0%90-%EA%B7%B8%EB%A6%AC%EA%B3%A0-nuxt-js-cafdc3ac2053)

<br/>

지난 편에서 프론트엔드 프레임워크를 `Vue.js` 로 결정하고 본격적으로 개발에만 집중하면 될 줄 알았지만, 한 가지 더 고민할 부분이 있었습니다. 
`Vue-CLI`로 만든 앱은 검색엔진최적화(SEO)가 어렵다는 점이죠. 
웹서비스를 하는 저희 회사 입장에서는 검색엔진에 적절하게 노출되는 것이 아주 중요합니다. 
일반적인 `SPA` 앱을 검색 로봇 입장에서 보면 모든 페이지의 소스가 아래와 같습니다.

```html
<html>
<head>
  <title>Single Page Application</title>
  <link rel="stylesheet" href="app.css" type="text/css">
</head>
<body>
  <div id="app"></div>
  <script src="app.js"></script>
</body>
</html>
```

로딩시점에서는 검색 엔진이 색인을 할 만한 콘텐츠가 존재하지 않는 것이죠. 
**SPA** 는 **어떻게** 렌더링되는 걸까요? 
그리고 **SSR**은 왜 하는거죠? 
**SPA**와 **SSR**의 장단점을 살펴본 후, **Vue.js** 에서 **SSR**을 하는 여러 옵션의 특징을 분석해보겠습니다.

<br/>

## SPA는 어떻게 렌더링하길래?

<br/>

다들 아시겠지만 **SPA(Single Page Application)**은 Client Side Rendering (혹은 Client Side Navigation) 방식으로 
어플리케이션 생명 주기 중에서 단 한 번만 리소스(HTML, CSS, JavaScript) 를 로딩하고, 그 후에는 데이터를 받아올 때만 서버와 통신합니다. 
정리하자면 **첫 요청시 딱 한 페이지만 불러오고 페이지 이동시 기존 페이지의 내부를 수정해서 보여주는 방식**입니다.

이를 사용자(클라이언트) 관점에서 다시 말하자면 최초 페이지를 로딩한 시점부터는 페이지 리로딩 없이 필요한 부분만 서버로 부터 받아서 화면을 갱신하는 것 입니다. 
필요한 부분만 갱신하기 때문에 네이티브 앱에 가까운 자연스러운 페이지 이동과 사용자 경험(UX)을 제공할 수 있습니다.

이전 포스트에서도 언급했지만 **Angular**를 필두로 **React, Vue.js** 등 걸출한 프론트엔드 기술들이 나오면서 크게 유행하고 있습니다.

![images](https://miro.medium.com/max/2220/1*8yXsJ6-NcpEBT4--WLlalQ.png)

👍 **SPA 장점**
1. 자연스러운 사용자 경험(UX)
2. 필요한 리소스만 부분적으로 로딩(성능)
3. 서버의 탬플릿 연산을 클라이언트로 분산(성능)
4. 컴포넌트별 개발 용이(생산성)
5. 모바일 앱 개발을 염두에 둔다면 동일한 API를 사용하도록 설계 가능(생산성)

👎 **SPA 단점**
1. JavaScript 파일을 번들링해서 한 번에 받기 때문에 초기 구동 속도 느림(webpack 의 code splitting으로 해결)
2. 검색엔진최적화(SEO)가 어려움 (SSR 로 해결)
3. 보안 이슈 (프론트엔드에 비즈니스 로직 최소화)

중복된 리소스 요청없이 정확히 필요한 요청만 하기 때문에 대부분의 경우 퍼포먼스 상 이득이 있고, 
사용자에서 네이티브 앱과 비슷한 수준의 향상된 사용자 경험(UX)을 제공할 수 있어서 최근 spa 가 대세가 되고 있습니다.

장점이 많은 만큼 해결해야할 단점들도 있는데요. 
그 중에서 검색엔진최적화(SEO)를 위해 SSR 을 적용하는 방법에 대해 살펴보겠습니다.

<br/>

## SSR이 렌더링하는 방식

<br/>

웹의 시작부터 **MPA(Multiple Page Application)**이 있었습니다. 
사실 `MPA`라는 용어도 `SPA`와 비교를 위해 등장한 인상이 강합니다. 
웹의 초기부터 `SPA` 에 대한 구현체들이 나오기 전까지 전통적인 웹사이트들은 모두 `MPA`형태로 서비스해 왔습니다.

`MPA`는 페이지를 이동할 때마다 새로운 페이지를 요청합니다. 
모든 탬플릿은 서버 연산을 통해서 렌더링하고 완성된 페이지 형태로 응답합니다.
이 과정을 **서버사이드 렌더링(SSR)**이라고 부릅니다.

![images](https://miro.medium.com/max/2220/1*7yuDF5swqDOd6TlIhsdxTg.png)

👍 **서버사이드 렌더링(SSR)의 장점**
1. 검색 엔진 최적화
2. Search Engine Optimization
3. SEO

다 같은 말입니다. 
전통적인 `MPA` 의 경우 브라우저에서 **JavaScript** 코드가 동작하기 전에도 완성된 형태의 탬플릿(HTML에 데이터가 삽입된 상태)을 서버로 부터 전달받습니다. 
이 때문에 **JavaScript** 를 구동하지 않는 모르는 검색 로봇이 페이지를 크롤링하기에 매우매우 적합합니다.

👎 **서버사이드 렌더링(SSR)의 단점**
1. 페이지 이동시 화면 깜빡임(UX)
2. 페이지 이동시 불필요한 탬플릿도 중복해서 로딩(성능)
3. 서버 렌더링에 따른 부하(성능)
4. 모바일 앱 개발시 추가적인 백엔드 작업 필요(생산성)

단점으로는 매 페이지 요청마다 페이지 리로딩(새로고침)이 발생하며, `SPA` 와는 정반대로 서버의 리스폰스에 의존해서 페이지를 이동해야하기 때문에 
퍼포먼스, 사용자 경험(UX) 측면에서 `SPA`에 비해 떨어진다고 볼 수 있습니다.

두가지 렌더링 방식에 대해서 살펴봤는데요. 
개인적으로 어떤 것이 더 좋으냐하는 논쟁은 의미가 없다고 생각합니다. 😘

<br/>

**사실 서비스나 콘텐츠에 어떤 방식이 더 적합한가.. 를 고민해야 합니다.**

<br/>

어떤 방식을 사용하든 트레이드 오프는 존재한다고 생각합니다. 
어느 한 쪽이 더 우월하다고 말하기는 어렵고 제공하고자하는 웹 서비스의 특성에 맞게 선택하시면 좋을 것 같습니다.

<br/>

## Vue.js 에서 SSR 적용하기

<br/>

서버 환경이 **node.js** 일 때, **Vue.js** 에서 **SSR**하는 방법으로 세 가지 옵션이 있습니다.

1. Prerendering (webpack 의 `prerender-spa-plugin` 사용)
2. [Vue SSR Guide](https://ssr.vuejs.org/)를 참조해서 직접 구현 (`vue-server-renderer`사용)
3. [Nuxt.js](https://nuxtjs.org/) 프레임워크를 사용

첫 번째, **Prerendering** 은 검색엔진 노출이 필요한 페이지들을 배포시점에서 렌더링해서 정적인 파일(`.html`)을 생성해 두는 방식입니다. 
가장 손쉽게 **SSR**을 적용하는 방법이지만, 동적인 페이지에는 적용이 불가능하다는 단점이 있습니다.

두 번째, **Vue SSR**은 `vue-server-renderer` 를 활용해서 **SSR**하는 방식입니다. 
서버와 클라이언트에서 모두 안정적으로 동작하는 JavaScript 코드 작성(window , document 등 브라우저 전용 전역변수 사용 주의 등) 해야하고 
데이터 프리 패칭, **css, font, image** 등의 `assets` 를 관리하기 위한 **webpack** 설정 등을 한 땀 한 땀 해줘야 합니다. 
신경써줘야할 것이 굉장히 많고 시행착오도 많이 겪어야 했습니다. 
**다만 이미 Vue.js 로 동적인 앱을 운영하고 계시다면 고려할 만한 거의 유일한 옵션인 듯 합니다.**

세 번째, **Nuxt.js 는 Vue.js 기반의 애플리케이션을 새로 작성하신다면, 그리고 SSR이 필요하시다면 무조건 추천드리고 싶습니다.** 
위에 서술한 많은 작업들을 시행착오를 줄이고, 일관된 스타일로 코드를 작성할 수 있도록 도와줍니다. 
**새로운 프로젝트를 시작하신다면 꼭꼭!! Nuxt.js 를 고려해보시기 바랍니다!**

<br/>

## Nuxt.js 의 SSR

<br/>

> `universal` : Isomorphic application (Server Side Rendering + Client Side Navigation)

Nuxt.js 는 `spa` 모드와 SSR 이라고 할 수 있는 `universal` 모드를 지원합니다. 
**universal 은 Isomorphic application (server-side rendering + client-side navigation)** 이라고 설명하고 있습니다.

그렇다면 서버 사이드 렌더링 + 클라이언트 사이드 네비게이션은 무슨 말일까요? 
아래 Nuxt.js lifecycle 을 도식화한 그림을 보면 이해하기 편합니다.

![images](https://miro.medium.com/max/914/1*mLL-1t7_0Ne3JN6On0t_vA.png)

처음 리퀘스트가 도착하면 서버사이드에서 `nuxtServeInit` , `middleware` , `validate()` , `asyncData()` , `fetch()` 등의 과정을 거쳐서 
렌더링한 뒤 완성된 페이지를 리스폰스로 보냅니다. 
그 이후에 페이지 이동은 페이지 갱신없이 클라이언트에서 `nuxt-link`(Vue router 의 `router-link`)를 통해 
필요한 리소스만 ajax 통신으로 받아서 렌더링합니다. 
첫 요청후 페이지 이동은 클라이언트 사이드 렌더링을 하는거죠.

말 그대로 server-side rendring + client-side navigation 두 가지 모두 사용합니다.

<br/>

## Isomorphic application

<br/>

생소한 단어가 눈에 밟힙니다.. **Isomorphic application** 이란 무엇일까요? 
보통 **Isomorphic JavaScript** 라는 말로 많이 쓰이는대요. **React** 의 **SSR** 프레임워크인 `Next.js`에서 먼저 사용한 개념인 듯 합니다. 
`Nuxt.js` 가 `Next.js` 에서 영감을 받아서 만들었다고 공식 문서에 밝히고 있는 만큼 동일한 의미로 쓰입니다. 
일반적으로 동형 자바스크립트라고 번역합니다. 
서버와 클라이언트에서 동일한 코드가 동작한다는 의미로 생각하시면 됩니다.

**Nuxt.js** 로 예시를 들자면. `nuxtServeInit` , `middleware` , `validate()` , `asyncData()` , `fetch()` 등의 과정을 
최초 요청에서는 서버사이드에서 처리한다고 말씀드렸었는데요. 
그 이후 페이지를 이동할 때는 클라이언트 사이드에서 동일한 로직을 통해서 페이지를 렌더링합니다. 
서버와 클라이언트에서 같은 코드로 동작하는 것이죠.

**Node.js** 어플리케이션은 서버와 클라이언트가 동일하게 **JavaScript** 로 이루어져 있기 때문에 **Isomorphic JavaScript**가 가능한 것입니다. 
**다만 주의해야할 점은 실제 코딩할 때 해당 코드가 서버/클라이언트 양쪽에서 모두 실행될 수 있다는 걸 항상 염두에 두고 작업해야 합니다.**
