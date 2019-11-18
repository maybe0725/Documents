# VS code에서 vue프로젝트에 ESLint recommended, autoFixOnSave 적용

<br/>

원본출처 :: [VS code에서 vue프로젝트에 ESLint recommended, autoFixOnSave 적용](https://velog.io/@skyepodium/VS-code%EC%97%90%EC%84%9C-vue%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-eslint-%EC%A0%81%EC%9A%A9-7xjr4r2on5)

<br/>

## 환경

- vue-cli 3.x 이상
- eslint recommended
- VS code
- ESLint (VS code 플러그인)

<br/>

협업 프로젝트를 진행할때 코딩 컨벤션을 따르지 않으면 타인이 작성한 코드를 쉽게 이해할 수 없는 경우가 발생한다. 
또한, 개인 프로젝트에서도 일정한 규칙이 없으면 한 사람이 작성하더라도 다른 형태의 코드가 작성되는 경우가 있다. 
일관된 코드 작성을 위해 협업 프로젝트에서, ESLint를 적용한 방식은 다음과 같다.

<br/>

## ESLint란?

ES + Linter

- ES는 EcmaScript 자바스크립트를 의미하며
- Linter는 코딩 컨벤션과 문법 체크를 도와주는 툴을 의미한다.

<br/>

## recommended, autofix 사용 이유

### recommended는 제일 높은 강도의 규칙이다.

하위 컴포넌트에서 받는 props가 많아지면 다음과 같이 길어지는 경우가 발생한다. 
이럴때 작성하는 사람마다 개행하는 위치도 다르고 props 작성 순서도 다르다. 
이를 위해 제일 강한 강도의 규칙을 설정한다.

```html
<template>
  <div id="app">
    <HelloWorld v-if="visible" msg="Welcome to Your Vue.js App" :title="info.title" :date="info.date"/>
  </div>
</template>
```

### autoFixOnSave는 저장할 때 마다 설정한 규칙에 맞게 자동으로 코드를 수정해주는 설정이다.

ESLint 에러가 터미널에 출력되는데 귀찮아서 고치지 않으면, 쌓이고 쌓여서 에러가 무진장 많이 출력된다. 
가능한 편하게 고치기 위해 `autoFixOnSave`를 설정해준다.

규칙을 적용해도 수정하는 방식이 다르고, 귀찮아서 고치지 않는 경우가 있기 때문에 `recommended, autoFixOnSave`를 설정해주었다.

<br/>

## 1. ESLint 플러그인 설치

마켓 플레이스에서 ESLint를 검색해서 플러그인을 설치한다.

<br/>

## 2. package.json 설정

vue-cli 3.x 부터는 eslint 설정이 package.json에 들어있다.

```json
//path: 프로젝트 폴더 명/package.json

"eslintConfig": {
...
  "extends": [
    "plugin:vue/recommended", // essential에서 recommended로 변경
    "eslint:recommended"
  ],
...
},
```

<br/>

## 3. VS code 설정
```json
//path: 프로젝트 폴더 명/.vscode/settings.json
//VS code에서 프로젝트 폴더를 열때 작업영역에 폴더를 추가하는 것으로 열어준다.

...
"eslint.validate": [
    {"language": "vue", "autoFix": true}, //vue 체크
    {"language": "javascript", "autoFix": true}, //자바스크립트 체크
    {"language": "html", "autoFix": true}, //HTML 체크 <div></div> -> <div />
],
"eslint.autoFixOnSave": true,
"eslint.alwaysShowStatus": true,
```

<br/>

## 4. 결과

gif 업로드가 안되서 자동으로 변환되는것을 보여줄 수 없지만, 저장 버튼을 꾹 누르면 autoFixOnSave가 수행되어 
recommended에 맞게 코드가 자동으로 수정된다.

<br/>

## 5. 단점

- 의도와는 다르게 코드가 변경됨
- 처음부터 코드를 간결하고, 정확하게 작성하려는 노력이 점점 줄어드는 경향이 있음
- 외부 라이브러리 import 하는 경우 순서가 바뀌어 버리는 경우가 있음
