React IE Web Browser 설정.
==========================

create-react-app v2 부터는 IE 지원을 아예 빼 버렸기 때문에 IE 에서도 돌아가게 하려면 polyfill을 추가해야 한다.

적용하고 싶은 프로젝트 루트에서 아래 명령어로 polyfill을 추가하자.

### Step 1. Install these two packages.
```
npm install babel-polyfill
npm install react-app-polyfill
```

### Step 2. This must be the first line in src/index.js
```java script
import "react-app-polyfill/ie9"
import "react-app-polyfill/ie10"
import "react-app-polyfill/ie11"
import "react-app-polyfill/stable"
import 'raf/polyfill';
```

### Step 3. package.json Edit.
> Type 1.
```json
  "browserslist": [
    ">0.2%",
    "not dead",
    "ie >= 9" 
  ]
```
> Type 2.
```json
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all",
      "ie >= 9"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version",
      "last 1 ie version"
    ]
  }
```  

간혹, 적용이 안되면 node_modules/.cache directory 를 삭제 후 다시 실행 한다.

<br/><br/>

> 참조
>* https://stackoverflow.com/questions/54451945/react-production-build-not-working-with-ie-11
>* https://facebook.github.io/create-react-app/docs/supported-browsers-features
>* https://browserl.ist/?q=%3E+0.2%25%2C+not+dead%2C+not+op_mini+all%2C+ie+%3E%3D+9
