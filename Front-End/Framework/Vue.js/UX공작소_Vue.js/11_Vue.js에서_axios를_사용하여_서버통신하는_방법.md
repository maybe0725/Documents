# Vue.js에서 axios를 사용하여 서버통신하는 방법

<br/>

원본글 출처

- [Vue.js에서 axios를 사용하여 서버통신하는 방법](https://ux.stories.pe.kr/138)

<br/>

웹 또는 앱을 개발하다 보면 거의 대부분이 서버가 필요하게 됩니다. 서버에 내용을 저장하고 웹이나 앱에서 서버의 저장된 내용을 불러다가 사용자에게 보여주게 되는데요. 이때 javascript에는 `axios`라는 아주 훌륭한 플러그인이 있습니다.

`axios`는 javascript용 플러그인으로 많이 사용하지만 `Vue.js`에서도 매우 요긴하게 사용되어 집니다.

`axios`는 `Promise` 기반의 자바스크립트 비동기 처리방식을 사용합니다. 그래서 요청후 .then()으로 결과값을 받아서 처리를 하는 형식으로 구성되어 있습니다.

```js
axios.get("/api/data").then(res => {
  console.log(res.data);
});
```

/api/data에서 데이터를 불러옵니다. 불러온 데이터는 .then()의 res에 담아서 처리하는 식입니다.

여기서는 간단하게 크롬브라우저의 Console화면에 결과값을 보여주게 처리되어 있습니다.

<br/>

## Vue.js 프로젝트에 axios 설치하기

<br/>

여기까지 찾아오셨다면 아마 nodejs는 이미 설치되어 있을 것이고 그러면 npm이나 yarn을 이용하여 axios를 설치하시면 될 것 같습니다. 설치하는 것은 매우 쉽습니다.
아래의 3가지 방법중 하나만 설치하거나 설정하면 됩니다.

1. npm 으로 설치하는 경우는 아래의 명령어를 Command창에 입력을 하면 됩니다.
   <br/> > npm install --save axios
2. yarn 으로 설치하는 경우는 아래의 명령어를 입력을 하면 됩니다.
   <br/> > yarn add axios
3. 직접 웹페이지의 <HEAD></HEAD> 영역 안에 입력을 해도 됩니다.
   <br/> > &lt;script src="https://unpkg.com/axios/dist/axios.min.js"&gt;&lt;/script&gt;

<br/>

## axios 별칭으로 사용하기

<br/>

axios는 REST을 별칭을 이용해서 쉽게 통신을 할 수 있습니다.

- 불러오기 : axios.get(url[, config])
- 입력하기 : axios.post(url[, data[, config]])
- 수정하기 : axios.patch(url[, data[, config]])
- 삭제하기 : axios.delete(url[, config])

<br/>

### GET (불러오기)

GET은 서버로 부터 데이터를 가져오는데 사용합니다. 아마도 가장많이 사용하는 명령어 일 것입니다.

서버 주소인 /api/data로 부터 값을 불러올 때 사용합니다.

```js
axios.get("/api/data").then(res => {
  // 불러온 값을 Console에 뿌려줍니다.
  console.log(res.data);
});
```

axios 요청 시 파마메터 정보(/api/todos/1)를 같이 입력하여 정보를 얻어 올 수 있습니다. 위의 것이 리스트를 불러온다면 지금 아래의 요청은 하나의 상세정보를 불러온다고 보시면 됩니다.

```js
axios.get("/api/data/1").then(res => {
  console.log(`status code: ${res.status}`);
  console.log(`headers: ${res.headers}`);
  console.log(`data: ${res.data}`);
});
```

axios 요청 시 파라메터 정보가 아니라 메소드의 두 번째 인자인 config 객체로 요청값을 넘길 수 있습니다.

```js
axios
  .get("/api/data", {
    params: { title: "vue.js는 조으다." },
    headers: { "X-Api-Key": "my-api-key" },
    timeout: 1000 // 1초 이내에 응답이 없으면 에러 처리
  })
  .then(res => {
    console.log(res.data);
  });
```

<br/>

### POST (값 입력하기)

/api/data에 값을 입력 할 때 사용합니다.

서버의 데이터 리스트의 마지막에 지금 넘기는 정보를 추가 합니다.

```js
axios.post("/api/data", { title: "vue.js는 조으다." }).then(res => {
  console.log(res.data);
});
```

<br/>

### PATCH (특정 값 수정하기)

/api/data/3에 값을 입력 할 때 사용합니다.

서버의 데이터 리스트 중 3에 해당 하는 값의 title를 수정합니다.

```js
axios.patch("/api/data/3", { title: "vue.js는 조으다." }).then(res => {
  console.log(res.data);
});
```

<br/>

### DELETE (특정 값 삭제하기)

/api/data/3에 값을 삭제 할 때 사용합니다.

서버의 데이터 리스트 중 3에 해당 하는 값을 삭제 합니다.

```js
axios.delete("/api/data/3").then(res => {
  console.log(res.data);
});
```

<br/>

## axios로 파일 업로드 하기

<br/>

axios로 파일도 업로드 할 수 있습니다.

먼저 HTML로 아래와 같이 form문을 작성합니다.

<br/>

### HTML코드

여기에서 `ref="photoimage"`는 중요한 역할을 하니 빼먹으면 안됩니다.

```html
<form method="post" enctype="multipart/form-data" action="/contant/124/photo">
  <input type="file" name="photo" ref="photoimage" />
  <input type="submit" />
</form>
```

<br/>

### JAVASCRIPT 코드

FormData() 객체를 생성하고 this.\$refs.photoimage과 같이 ref옵션을 이용해서 필드에 직접 참조를 하여 이미지파일을 가져오고 업로드를 할 수 있습니다.

```js
var data = new FormData();
var file = this.$refs.photoimage.files[0];
data.append("photo", file);
axios
  .post("/api/data/" + this.no + "/photo", data)
  .then(res => {
    this.result = res.data;
  })
  .catch(ex => {
    console.log("사진업로드 실패", ex);
  });
```
