# Vue.js 템플릿 구문 작성시 주의할 점

<br/>

원본글 출처

- [Vue.js 템플릿 구문 작성시 주의할 점](https://ux.stories.pe.kr/132)

<br/>

Vue.js의 꽃인 컴포넌트에서 DOM을 구성하기 위한 Template을 만들때 주의할 점 몇가지를 포스팅합니다.

<br/>

## is 특성 사용하기

컴포넌트를 등록하고 HTML에서 이것을 사용하려면 보통 아래와 같이 사용합니다.

```html
<div id="app">
  <component-a></component-a>
</div>
```

하지만 컴포넌트가 기존 HTML테그의 하위로 들어갈 경우 정상적으로 렌더링이 되지 않는 경우가 있는데 이럴때 is특성을 사용하면 정상적으로 렌더링이 됩니다.
아래와 같이 작성할 경우 &lt;select&gt;라는 HTML태그에 컴포넌트가 들어가 있으므로 실제로 렌더링을 하면 &lt;option&gt;이 나오지 않습니다.

```html
<div id="app">
  <select>
    <component-option></component-option>
    <component-option></component-option>
  </select>
</div>
```

<br/>

## 컴포넌트의 템플릿 작성시 루트요소는 하나여야 합니다.

<br/>

컴포넌트 안에 옵션을 작성하는 요소중에 Template가 있습니다.
DOM을 형성할 때 어떻게 나와야 하는지 HTML형태로 작성을 하는데요.
이 떄도 주의해야 할 점이 있습니다.
루트요소는 하나만 작성을 해야 오류가 나지 않습니다.

```html
<template id="testTemplate">
  <div>Test</div>
  <div>Template입니다.</div>
</template>
```

위와 같이 &lt;template&gt; 안에 루트 요소가 2개 이상이면 오류가 발생합니다.
이럴경우는 아래와 같이 수정해야 합니다.

```html
<template id="testTemplate">
  <div>
    <div>Test</div>
    <div>Template입니다.</div>
  </div>
</template>
```

위와 같이 <DIV> 하나 감싸서 루트요소를 하나로 만들면 정상적으로 작동합니다.
