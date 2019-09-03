# Vuejs의 컴포넌트 사용 시 알아두면 좋은 6가지

<br/>

원본글 출처

- [Vuejs의 컴포넌트 사용 시 알아두면 좋은 6가지](https://ux.stories.pe.kr/137)

<br/>

Vue.js의 컴포넌트를 사용할때 알아두면 유용한 6가지에 대해서 포스팅을 해보겠습니다. 이것을 알게계시면 소스코드도 많이 줄일 수 있고 보기에도 좋은 코딩을 할 수 있습니다.

<br/>

## 케밥표현과 카멜표현

<br/>

컴포넌트의 &lt;template&gt;&lt;/template&gt; 영역에서 다른 컴포넌트 명을 작성할 때 2개 이상의 단어가 조합된 이름 일 경우 꼭 `케밥(-)` 형식으로 작성해야 합니다. HTML에서는 대소문자를 구별하지 않기 떄문에 `hongGilDong` 과 `honggildong`을 동일하게 처리하기 때문입니다.

컴포넌트의 &lt;script&gt;&lt;/script&gt;영역에서 이름을 카멜형식(`hongGilDong`)으로 작성했더라도 &lt;template&gt;&lt;/template&gt;영역에 작성할 때는 케밥형식으로 변경하여 작성하면 됩니다. Vue.js가 자동으로 같은 컴포넌트로 인식을 합니다.

> 케밥 형식 : 단어와 단어 사이를 '-'로 연결시키는 표현 (예 : good-man, hong-gil-dong ) <br/>
> 카멜 형식 : 단어와 단어 사이를 대문자로 시작해서 연결시키는 표현 (예 : goodMan, hongGilDong )

<br/>

## 컴포넌트에서 Style(CSS)을 모듈화하여 사용하기

<br/>

컴포넌트에서 화면을 꾸며주는 CSS를 모듈형태로 지정해서 사용할 수 있습니다.

보통 컴포넌트 작성 시 CSS 부분을 작성하기 위해 &lt;style&gt;&lt;/style&gt; 태그 안에 작성을 합니다. 이럴경우 CSS는 정적형태로만 사용 되어질 수 밖에 없습니다.
하지만 Vue.js에서는 &lt;style module&gt;&lt;/style&gt; 처럼 모듈화 하면 동적 형태로도 CSS를 사용할 수 있게 됩니다.
이렇게 모듈화된 style은 \$style이라는 계산형 속성을 통해 사용할 수 있습니다.

```html
<template>
  <div><button v-on:class="$style.hand">버튼</button></div>
</template>
<script></script>
<style module>
  .hand {
    cursor: pointer;
    background-color: #f5f5f5;
    color: #333333;
  }
</style>
```

<br/>

## 해당 컴포넌트에만 Style(CSS)이 적용되게 하기

<br/>

단일페이지(SPA)로 화면을 구성하는 Vue.js 특성 상 컴포넌트로 페이지를 구성한다고 해도 컴파일을 해서 화면을 보면 기본적으로 Style은 공통으로 적용되게 되어 있습니다. 그래서 컴포넌트가 달라도 Class명이 같으면 동일한 Style의 영향을 받습니다.
하지만 해당 컴포넌트에만 Class가 적용되기를 원하는 경우도 있을 것입니다. 그럴 경우 scoped를 적시 해 주시면 해당 컴포넌트에만 Style이 반영 됩니다.

```html
<style scoped>
  .hand {
    color: #333333;
  }
</style>
```

<br/>

## 슬롯

<br/>

슬롯은 부모 컴포넌트에서 자식 컴포넌트로 HTML마크업을 전달할 수 있게 해주는 기능입니다.

`Props`와 `Event`로 부모, 자식 간 컴포넌트 사이에서 정보를 교환한다는 것은 많이 들어봤을 것입니다.

하지만 HTML 마크업을 전달하기는 쉽지 않은데요. 이것을 쉽게 해주는 것이 슬롯이라고 보면 됩니다.

```html
<!-- 자식 컴포넌트 파일명 : Child.vue -->
<template>
  <div>
    <p>만나면 이렇게 인사하세요</p>
    <slot></slot>
  </div>
</template>
```

```html
<!-- 부모 컴포넌트 파일명 : Parents.vue -->
<template>
  <div>
    <child>
      <!-- 자식컴포넌트의 <slot></slot> 영역에 아래의 내용이 포함되어 출력됨 -->
      <div>{{AA}}</div>
      <!-- slot영역에 나올 내용 끝 -->
    </child>
    <child>
      <!-- 자식컴포넌트의 <slot></slot> 영역에 아래의 내용이 포함되어 출력됨 -->
      <div>{{BB}}</div>
      <!-- slot영역에 나올 내용 끝 -->
    </child>
  </div>
</template>

<script>
  import Child from "./Child.vue";

  export default {
    components: { Child },
    data: function() {
      return {
        AA: "안녕하세요",
        BB: "반갑습니다."
      };
    }
  };
</script>
```

자식 컴포넌트에 한개가 아닌 여러개의 슬롯을 사용할 경우 슬롯에 이름을 붙혀서 사용하는 방법도 있습니다.

```html
<!-- 자식 컴포넌트  -->
<slot name="header"></slot>
<slot name="body"></slot>
<slot name="footer"></slot>
```

```html
<!-- 부모 컴포넌트  -->
<div slot="header">해더</div>
<div slot="body">본문</div>
<div slot="footer">푸터</div>
```

<br/>

## 동적 컴포넌트

동적 컴포넌트는 동일한 위치에 상황에 따라 다른 컴포넌트를 동적으로 변경하여 보여주기 위한 기능입니다.

마치 웹사이트에서 메뉴명을 클릭하면 본문 내용이 바뀌는 것을 생각하시면 됩니다.

보통 컴포넌트 태그에 v-bind:is 조건으로 컴포넌트 명을 변경해서 적용하는 방법을 사용합니다.

```html
<component v-bind:is="compName"></component>
```

컴포넌트가 나오기 원하는 위치에 위와 같이 작성하고 compName변수명에 컴포넌트 name을 매칭시켜서 변경시키면 됩니다.

적용되어지는 컴포넌트에는 반드시 name: 'Header' 이런 식으로 컴포넌트 이름을 지정해야 합니다.

<br/>

### &lt;keep-alive&gt;&lt;/keep-alive&gt;

동적 컴포넌트를 사용할 경우 자주 사용되는 것이 keep-alive 태그입니다. keep-alive는 컴포넌트가 정적인 화면일 경우 매번 랜더링을 하지 않도록 화면을 캐싱해 주는 태그입니다.

```html
<keep-alive include="home, intro, about">
  <component v-bind:is="compName"></component>
</keep-alive>
```

위와 같이 include="home, intro, about" 처럼 include에 적시하면 해당 컴포넌트는 캐싱이 되어 화면이 바뀔때마다 랜더링을 다시하지 않고 캐싱되어 있는 내용을 다시 보여줍니다.

exclude="item"이렇게 exclude에 적시된 컴포넌트는 해당 화면이 호출될때마다 새로 랜더링을 다시 합니다. 제품 상세 정보나 회원정보를 보여주는 컴포넌트 인 경우에 적당합니다.

<br/>

## 재귀 컴포넌트

<br/>

재귀 컴포넌트는 본인 컴포넌트의 템플릿(template)에서 자기 자신의 컴포넌트를 불러와 사용하는 컴포넌트를 말합니다. 이때 꼭 name 옵션을 지정해야 합니다.

보통은 Tree메뉴같은 Tree구조를 표현할때 많이 사용됩니다.
