# Vue의 NUXT에서 SASS를 적용해서 사용하는 방법(feat.BULMA)

<br/>

원본글 출처

- [Vue의 NUXT에서 SASS를 적용해서 사용하는 방법(feat.BULMA)](https://ux.stories.pe.kr/147)

<br/>

웹화면은 HTML, CSS, JAVASCRIPT로 구성되어 있습니다.
웹개발자 분은 보통 HTML과 JAVASCIPT로 작업을 하고, 웹디자이너는 HTML과 CSS로 작업을 합니다.
하지만 점차 시간이 지남에 따라 HTML, CSS, JAVASCRIPT, 심지어 백오피스까지 손을 대는 `풀스텍 개발자`가 나오게 되었습니다.
그러면서 자연스럽게 CSS 마저도 마치 프로그램처럼 작성하고 싶은 요구가 생겨나기 시작했습니다.
그런 요구에 따라 CSS도 프로그램처럼 개발하는 언어가 생겼는데 그것을 `CSS 프리프로세서`, 한국말로 `CSS 전처리기`라고 합니다.
CSS 프리프로세서로 코딩을 하고 컴파일을 하면 결과물은 기존의 CSS 형태로 되어져서 나옵니다.
CSS 프리프로세서의 종류는 `LESS, SASS(SCSS), Stylus, PostCSS`가 있으며 사용법이 익숙해지면 상당히 편리하고 대규모의 프로젝트를 할 떄도 유용한 역활을 하게 됩니다.

이러한 CSS 프리프로세서를 Vue.js나 Nuxt에도 적용할 수 있는데 이번에 `NUXT`에 스타일을 작업하기 위해 `BULMA`의 SASS를 사용할 수 있게 세팅하는 방법을 포스팅하려고 합니다.
Nuxt에 대해서는 이전 `Vue의 NUXT 템플릿을 쉽게 설치하는 3가지 방법` 포스팅을 참조하세요.

> **BULMA 란?** <br/>
> BULMA는 Bootstrap처럼 미리 지정한 CSS로 편리하게 웹사이트에 스타일을 설정하는 패키지 입니다.
> 웹퍼블리싱에 있어 이전까지는 거의 대부분 Bootstrap을 사용했었으나 요즘에는 BULMA도 많이 사용하는 추세입니다.
> BULMA의 눈에 띄는 장점은 Bootstrap과 달리 jquery나 javascript파일에 종속되어 있지 않다는 것입니다.

<br/>

## NUXT 설치하기

<br/>

## BULMA 설치하기

<br/>

보통 Node.js의 패키지 매니저는 npm이라고 할 수 있으나 요즘에는 성능이나 속도면에서 yarn을 많이 사용하는 추세입니다.

제가 포스팅하는 내용도 yarn을 기준으로 작성하려고 합니다.

![images](https://t1.daumcdn.net/cfile/tistory/995D82465D0C9D9821)

`부르마(Bulma)와 Nuxt`

Nuxt의 starter를 설치할 때 설치 옵션으로 BULMA를 같이 설치하는 옵션이 있습니다.
이것을 통해서 설치해도 되나 이렇게 설치하면 BULMA의 CSS로만 연결될 뿐이지 우리가 지금 사용하려는 SASS와는 연결되지 않습니다.
물론 설치 후 수동으로 설정을 해 주시면 되긴 합니다.
아래의 설명은 Starter에서 설치하지 않았다는 가정하여 설치하는 것 부터 작성하겠습니다.

```sh
$ yarn add -D bulma
```

bulma를 설치하면 아래와 같이 `css폴더, sass폴더`와 `bulma.sass`파일이 생성됩니다.

![images](https://t1.daumcdn.net/cfile/tistory/995ACE435D0C9D9827)

- css폴더 : 컴파일이 완료된 CSS최종본이 저장되어 있음
- sass폴더 : 컴파일 하기 전의 SASS 파일 소스가 저장되어 있음
- bulma.sass : SASS폴더에 있는 각각 모듈을 하나의 페이지로 불러오기위한 import 파일.

Nuxt 프로젝트에서 sass를 사용하려면 아래의 3가지 패키지를 devDependencies로 설치해야 합니다.

```sh
$ yarn add -D node-sass sass-loader @nuxtjs/style-resources
```

`nuxt.config.js`에 sass를 사용하겠다고 아래와 같이 등록을 해야 합니다.

`modules`에 `style-resources`를 선언하고 `styleResources: {}` 에 사용할 scss파일을 지정하면 됩니다.

```js
// nuxt.config.js
module.exports = {
  // ...
  modules: ["@nuxtjs/style-resources"],
  styleResources: {
    sass: ["@/assets/scss/mystyles.scss"]
  }
  // ...
};
```

<br/>

## NUXT프로젝트에 SASS(BULMA) 연결하기

<br/>

이제 설치된 Bulma의 sass를 Nuxt프로젝트에 연결을 합니다.

연결하기 위해서 Nuxt의 `assets/scss/mystyles.scss` 파일을 생성합니다. mystyles.scss 파일은 bulma를 임포트한 후 프로젝트 전체에 적용될 scss파일입니다.

mystyles.scss 파일명은 원하시는 이름으로 작성하시면 됩니다.

```sh
$ mkdir assets/scss/mystyles.scss
```

생성된 mystyles.scss 파일에 bulma의 sass를 아래와 같이 `import` 합니다.

bulma의 전체 모듈을 한번에 불러올 수도 있고 각 모듈 별로 각각 불러올 수도 있습니다.
전체를 불러올 경우 사용에는 용이하나 나중에 `Production`으로 만들었을 때 상대적으로 css파일의 용량이 크므로 트래픽을 많이 차지할 것이고 각각의 필요한 모듈을 골라서 불러올 경우는 상대적으로 css파일의 용량이 작으므로 트래픽에 이득을 볼 수 있습니다.

```scss
@import "~bulma/bulma";
```

이렇게 불러온 sass는 변수값을 재정의 해서 나만의 스타일을 만들 수 있다는 장점이 있습니다.

변수는 아래와 같이 재정의를 할 수 있습니다.

```scss
// mystyles.scss

//////////////////////////////////////////////////////////////////////////////////
// BULMA 전체 불러오기
@import "~bulma/bulma";
//////////////////////////////////////////////////////////////////////////////////

// 필요한 BULMA 모듈만 불러오기
// @import "~bulma/sass/utilities/_all.sass";
// @import "~bulma/sass/base/_all.sass";
// @import "~bulma/sass/elements/button.sass";
// @import "~bulma/sass/elements/container.sass";
// @import "~bulma/sass/elements/title.sass";
// @import "~bulma/sass/form/_all.sass";
// @import "~bulma/sass/components/navbar.sass";
// @import "~bulma/sass/layout/hero.sass";
// @import "~bulma/sass/layout/section.sass";
//////////////////////////////////////////////////////////////////////////////////

// Import a Google Font
// font-family: "Noto Sans KR";
@import url(http://fonts.googleapis.com/earlyaccess/notosanskr.css);

// Set your brand colors
$purple: #8a4d76;
$pink: #fa7c91;
$brown: #757763;
$beige-light: #d0d1cd;
$beige-lighter: #eff0eb;

// Update Bulma's global variables
$family-sans-serif: BlinkMacSystemFont, -apple-system, "Noto Sans KR", "Malgun Gothic",
  sans-serif;
$grey-dark: $brown;
$grey-light: $beige-light;
$primary: $purple;
$link: $pink;
$widescreen-enabled: false;
$fullhd-enabled: false;

// Update some of Bulma's component variables
$body-background-color: $beige-lighter;
$control-border-width: 2px;
$input-border-color: transparent;
$input-shadow: none;

// ...
```

<br/>

## 사용하기

<br/>

nuxt의 화면에 보여지는 페이지나 컴포넌트의 &lt;style&gt;&lt;/style&gt; 태그 영역에 아래와 같이 적용할 수 있습니다.

&lt;style lang="scss" scoped&gt; 여기에서…

- lang="scss" : 사용언어로 scss를 사용하겠다는 선언입니다.
- scoped : 전역이 아니고 지역에서만 적용된다는 뜻으로 해당 페이지나 컴포넌트에만 적용된다는 의미입니다.

```js
<style lang="scss" scoped>
.title {
  font-family: $family-sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: $primary;
  letter-spacing: 1px;
}
</style>
```

아래의 코드 중 font-family: \$family-sans-serif;는 Bulma에서 정의한 변수를 그대로 사용할 수 있습니다.

위에서 \$family-sans-serif는 BlinkMacSystemFont, -apple-system, "Noto Sans KR", "Malgun Gothic", sans-serif;로 지정되어 있네요..

color도 \$primary bulma변수가 지정되어 있습니다.

위에서 $primary는 $purple 로 지정되어 있고 \$purple 은 그 위에 #8A4D76로 지정되어 있네요. 결국 색상값은 #8A4D76입니다.

이렇게 Bulma의 SCSS를 지정해 놓으면 프로젝트 파일의 어디에서나 bulma의 변수값을 그대로 사용할 수 있습니다.
