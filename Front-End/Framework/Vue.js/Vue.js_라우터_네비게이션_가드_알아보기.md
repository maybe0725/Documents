# Vue.js 라우터 네비게이션 가드 알아보기

<br/>

## 네비게이션 가드란?

네비게이션 가드(navigation guard)란 뷰 라우터로 특정 URL에 접근할 때 해당 URL의 접근을 막는 방법을 말합니다. 예를 들어, 사용자의 인증 정보가 없으면 특정 페이지에 접근하지 못하게 할 때 사용하는 기술입니다.

<br/>

## 네비게이션 가드의 종류

네비게이션 가드의 종류는 아래와 같이 3가지가 있습니다.

- 애플리케이션 전역에서 동작하는 전역 가드
- 특정 URL에서만 동작하는 라우터 가드
- 라우터 컴포넌트 안에 정의하는 컴포넌트 가드

<br/>

## 전역 가드

전역 가드는 라우터 인스턴스를 참조하는 객체로 설정할 수 있습니다. 그러면 전역 가드 설정 방법을 알아보겠습니다.

먼저, 전역 가드 설정을 위해 먼저 아래와 같이 라우터 인스턴스를 생성합니다.

```javascript
var router = new VueRouter();
```

다음으로 `router` 변수에 아래와 같이 `.beforeEach()` API를 호출합니다.

```javascript
router.beforeEach(function(to, from, next) {
  // to : 이동할 url
  // from : 현재 url
  // next : to에서 지정한 url로 이동하기 위해 꼭 호출해야 하는 함수
});
```

또 다른 방법으로는 아래와 같이 사용합니다.

```javascript
export default new Router({
  mode: "history",
  base: process.env.BASE_URL,
  routes: [
    {
      path: "/",
      name: "home",
      beforeEnter: (to, from, next) => {
        console.log(":: router.js :: to: ", to, "/ from: ", from);
        next();
        // next("/"); // Home 으로 이동
        // next("/about"); // About 으로 이동
      },
      component: Home
    },
    {
      ...
    },
    {
      ...
    }
  ]
});
```

여기서 `beforeEach()`를 호출하면 다음과 같이 3개의 인자를 받습니다.

- to : 이동할 url 정보가 담긴 라우터 객체
- from : 현재 url 정보가 담긴 라우터 객체
- next : to에서 지정한 url로 이동하기 위해 꼭 호출해야 하는 함수

`router.beforeEach()`를 호출하고 나면 모든 라우팅이 대기 상태가 됩니다. 원래 url이 변경되고 나면 해당 url에 따라 화면이 자연스럽게 전환되어야 하는데 전역 가드를 설정했기 때문에 화면이 전환되지 않습니다. 여기서 해당 url로 라우팅 하기 위해서는 next()를 호출해줘야 합니다. next()가 호출되기 전까지 화면이 전환되지 않습니다.

<br/>

## 전역 가드 동작 예제

앞에서 설명한 전역 가드의 동작 방식을 이해하기 위해 아래와 같은 라우터 코드를 준비하였습니다.

```javascript
// 라우터 컴포넌트
var Login = { template: "<p>Login Component</p>" };
var Home = { template: "<p>Home Component</p>" };

// 라우팅 정보
var router = new VueRouter({
  routes: [
    { path: "/login", component: Login },
    { path: "/home", component: Home }
  ]
});
```

여기서 전역 가드를 설정하는 코드를 아래와 같이 추가합니다.

```javascript
router.beforeEach(function(to, from, next) {
  console.log("every single routing is pending");
});
```

이제 ‘/login’이나 ‘/home’으로 이동하더라도 라우팅이 되지 않고 아래와 같이 로그만 출력됩니다.

만약 원하는 url로 이동하고 싶으면 아래와 같이 next()를 호출하면 됩니다.

```javascript
router.beforeEach(function(to, from, next) {
  next();
});
```

<br/>

## 전역 가드로 페이지 인증하기

전역 가드를 실제 애플리케이션 로직에 어떻게 적용할 수 있는지 살펴보겠습니다. 앞에서 살펴본 예제의 Login 컴포넌트에 다음과 같이 meta 정보를 추가하였습니다.

```javascript
var router = new VueRouter({
  routes: [
    // meta 정보에 authRequired라는 Boolean 값 추가
    { path: "/login", component: Login, meta: { authRequired: true } },
    { path: "/home", component: Home }
  ]
});
```

그리고 `beforeEach()`의 콜백 함수에 사용자 인증 여부를 체크하는 로직을 추가합니다.

```javascript
router.beforeEach(function(to, from, next) {
  // to: 이동할 url에 해당하는 라우팅 객체
  if (
    to.matched.some(function(routeInfo) {
      return routeInfo.meta.authRequired;
    })
  ) {
    // 이동할 페이지에 인증 정보가 필요하면 경고 창을 띄우고 페이지 전환은 하지 않음
    alert("Login Please!");
  } else {
    console.log("routing success : '" + to.path + "'");
    next(); // 페이지 전환
  }
});
```

위 코드는 이동하려는 페이지에 만약 인증 정보가 필요하면 경고 창을 띄우고 화면은 전환하지 않는 코드입니다. 뷰 라우터 인스턴스에서 ‘/login’에 해당하는 라우터 객체에만 `authRequired` 값을 설정해놨기 때문에 ‘/home’ 페이지로 이동할 때는 `next()`로 페이지를 이상 없이 전환합니다.

<br/>

## 라우터 가드와 컴포넌트 가드

앞에서 살펴본 전역 가드와 마찬가지로 라우터 가드와 컴포넌트 가드도 같은 원리로 동작합니다. 다만 URL 이동을 막기 위해 사용하는 API만 조금 다릅니다.

### 라우터 가드

전체 라우팅이 아니라 특정 라우팅에 대해서 가드를 설정하는 방법은 아래와 같습니다.

```javascript
var router = new VueRouter({
  routes: [
    {
      path: "/login",
      component: Login,
      beforeEnter: function(to, from, next) {
        // 인증 값 검증 로직 추가
      }
    }
  ]
});
```

### 컴포넌트 가드

라우터로 지정된 특정 컴포넌트에 가드를 설정하는 방법은 다음과 같습니다.

```javascript
const Login = {
  template: "<p>Login Component</p>",
  beforeRouteEnter(to, from, next) {
    // Login 컴포넌트가 화면에 표시되기 전에 수행될 로직
    // Login 컴포넌트는 아직 생성되지 않은 시점
  },
  beforeRouteUpdate(to, from, next) {
    // 화면에 표시된 컴포넌트가 변경될 때 수행될 로직
    // `this`로 Login 컴포넌트를 접근할 수 있음
  },
  beforeRouteLeave(to, from, next) {
    // Login 컴포넌트를 화면에 표시한 url 값이 변경되기 직전의 로직
    // `this`로 Login 컴포넌트를 접근할 수 있음
  }
};
```

<br/>
<br/>

> 출처 :: [Vue.js 라우터 네비게이션 가드 알아보기](https://joshua1988.github.io/web-development/vuejs/vue-router-navigation-guards/#%EB%9D%BC%EC%9A%B0%ED%84%B0-%EA%B0%80%EB%93%9C%EC%99%80-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EA%B0%80%EB%93%9C)
