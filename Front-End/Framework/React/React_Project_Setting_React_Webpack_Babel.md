React Project Setting - React + Webpack + Babel
===============================================

### 1. 디렉토리 생성
```
[Linux]
  $ mkdir react-setting-test && cd react-setting-test
  $ mkdir -p src public

[Window Visual Studio Code Terminal powershell]
  PS C:\DEV\React> mkdir react-test
  PS C:\DEV\React\react-test> mkdir src
  PS C:\DEV\React\react-test> mkdir public
```

### 2. 프로젝트 관리를 위한 package.json 파일생성.
```
[Linux]
  $ npm init -y

[Window Visual Studio Code Terminal powershell]
  PS C:\DEV\React\react-setting-test> npm init -y
```
```
[Result :: package.json]
  {
    "name": "react-setting-test",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"
  }
```

### 3. Bundler Webpack

Webpack은 Front-end 개발을 할 때 필요한 많은 설정 들을 지원해주는 강력한 도구 입니다. <br/>
React app은 Component라는 모듈 단위로 개발되는데 개발되는 component들을 묶어 주는 역할을 합니다.

```
[Linux]
  $ npm install --save-dev webpack webpack-cli

[Window Visual Studio Code Terminal powershell]
  PS C:\DEV\React\react-setting-test> npm install --save-dev webpack webpack-cli
```

설치가 완료 되면 프로젝트 파일에 있는 `package.json` 파일에 다음과 같은 명령어를 추가해 줍니다.

```
  ...
  "scripts": {
      "build": "webpack --mode production",
  },
  ...
```

Webpack을 제대로 사용하기 위해서는 `webpack.config.js` 파일을 생성해주고 추가적인 설정을 해줘야 합니다.

```
  const path = require("path");

  module.exports = {
          mode: "development",
      entry: {
          main: './src/index.js'
      },
      output: {
          filename: '[name].bundle.js',
          path: path.resolve(__dirname, 'build')
      },
        resolve: { extensions: ["*", ".js", ".jsx"] },
  };
```

위의 설정 들을 간략히 설명하자면 다음과 같습니다.

>● entry   : 파일들을 묶기 위해 Webpack이 바라보는 파일 시작점 <br/>
>● output  : bundle된 파일의 결과물을 위한 설정 <br/>
>● mode    : production or development <br/>
>● resolve : import 될 수 있는 파일 확장자 명

이 설정들은 Webpack의 동작을 정의하는 설정들입니다. 
다음으로 Webpack이 동작할 때 Html과 CSS파일을 인식하기 위한 추가적인 설정을 해보겠습니다.

### 4. html 설정

```
[Linux]
  $ npm install --save-dev html-webpack-plugin clean-webpack-plugin

[Window Visual Studio Code Terminal powershell]
  PS C:\DEV\React\react-setting-test> npm install --save-dev html-webpack-plugin clean-webpack-plugin
```

### 5. CSS 설정

```
[Linux]
  $ npm install --save-dev mini-css-extract-plugin css-loader sass-loader file-loader

[Window Visual Studio Code Terminal powershell]
  PS C:\DEV\React\react-setting-test> npm install --save-dev mini-css-extract-plugin css-loader sass-loader file-loader  
```

위 라이브러리들이 다 설치가 되었으면 `webpack.config.js` 파일에 설정을 추가해 보겠습니다.

```
  const path = require('path');
  const MiniCssExtractPlugin = require('mini-css-extract-plugin');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const { CleanWebpackPlugin } = require('clean-webpack-plugin');
  const webpack = require('webpack');

  module.exports = {
          mode: "development",
      entry: {
          main: './src/index.js'
      },
      output: {
          filename: '[name].bundle.js',
          path: path.resolve(__dirname, 'build')
      },
      module: {
          rules: [
              {
                  test: /\.(sa|sc|c)ss$/,
                  use: [
                      {
                          loader: MiniCssExtractPlugin.loader,
                          options: {
                              hmr: true,
                              reloadAll: true
                          }
                      },
                      'css-loader',
                      'sass-loader'
                  ]
              }, 
              {
                  test: /\.(png|jpg|svg|gif)/,
                  use: [
                      'file-loader'
                  ]
              }
          ]
      },
      resolve: {
          extensions: [ '*', '.js', '.jsx' ]
      },
      plugins: [
          new CleanWebpackPlugin(),
          new HtmlWebpackPlugin({
              title: 'webpack-react-start-kit',
              template: './public/index.html'
          }),
          new MiniCssExtractPlugin({
              filename: '[name].css',
              chunkFilename: '[id].css'
            }),
      ]
  }
```

이어서 처음 생성한 `public` 폴더에 `index.html` 파일을 만들도록 하겠습니다.

```
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <title>React A to Z</title>
</head>

<body>
  <div id="root"></div>
  <noscript>
    You need to enable JavaScript to run this app.
  </noscript>
</body>

</html>
```

지금 설정한 Webpack 설정들은 다음과 같습니다.

#### HTML
>● html-webpack-plugin : Webpack이 실행 될 때 public 파일에 설정한 html 파일을 기준으로 결과물을 만들어줍니다. <br/>
>● clean-webpack-plugin : Webpack이 실행될 때 이전에 나온 결과물을 제거합니다.<br/>
(최신 결과물 만을 유지하기 위해)

#### CSS
>● Sass-loader , css-loader : Webpack을 실행할 때 CSS와 Sass 파일을 적용해 주는 패키지 입니다. <br/>
>● mini-css-extract-plugin : css 결과물을 여러 개의 chunk 파일로 분리 시켜주는 라이브러리 입니다.

### 6. Compiler Babel

React component 들은 JavaScript ES6+ 문법과 JSX 문법으로 작성됩니다. 
이 코드를 그대로 쓰는 경우 지원하지 않는 브라우저에서는 코드가 동작하지 않으므로 Babel을 사용하여 Transpiling 을 해줘야 합니다.

```
[Linux]
  $ npm install --save-dev @babel/core babel-loader @babel/preset-env @babel/preset-react

[Window Visual Studio Code Terminal powershell]
  PS C:\DEV\React\react-setting-test> npm install --save-dev @babel/core babel-loader @babel/preset-env @babel/preset-react
```

설치된 라이브러리들에 대한 설명을 해드리자면 아래와 같습니다.

>● @babel/core : babel 사용을 위한 코어 라이브러리 <br/>
>● babel-loader : Webpack을 사용할 때 babel을 적용하기 위한 라이브러리 <br/>
>● @babel/preset-env : JavaScript ES6 코드를 ES5로 compiling 해주는 라이브러리 <br/>
>● @babel/preset-react : JSX 코드를 JavaScript 코드로 변환시켜 주는 라이브러리

설치가 다 되셨으면 Babel 적용을 위한 설정 파일을 만들도록 하겠습니다.

`.babelrc` 파일을 만들고 다음과 같이 입력 해주시면 됩니다.

```
{
    "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

### 7. Babel과 Webpack 연동

React 프로젝트를 위한 필수 항목을 다 설치하였습니다. 마지막으로 Babel과 Webpack을 연동해 보겠습니다.

우선 Webpack 설정 파일 `webpack.config.js` 파일을 만들고 아래와 같은 설정을 입력해주세요.

```
module.exports = {
    ...,
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/,
        exclude: /node_modules/,
                use: {
                    loader: "babel-loader"
                }
            }
        ]
    }
};
```

이제 필요한 도구를 모두 설정하였습니다.

### 8. Creating React Components

이제 React 라이브러리를 설치하고 컴포넌트를 만들어 보도록 하겠습니다.

```
[Linux]
  $ npm install react react-dom

[Window Visual Studio Code Terminal powershell]
  PS C:\DEV\React\react-setting-test> npm install react react-dom
```

```
[src/Index.js]

import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(<div>Hello React</div>, document.getElementById('root'));
```

파일을 저장하고 나서 `npm run build` 를 실행시켜 보시면 `build` 파일이 생성되어 있고 내부에

`index.html` 과 `main.bundle.js` 파일이 생성되어있습니다. html 파일을 열어보시면 결과는 다음과 같습니다.

이로써 필수 요구사항을 모두 만족한 React 프로젝트가 생성되었습니다.

### 9. 선택사항 - Live Dev Server

다음으로 선택 사항 중 실시간 개발 서버를 적용해 보도록 하겠습니다.

```
[Linux]
  $ npm install webpack-dev-server

[Window Visual Studio Code Terminal powershell]
  PS C:\DEV\React\react-setting-test> npm install webpack-dev-server
```
<br/><br/>

>● 원본 글 링크 :: [내맘대로 리액트 A to Z - 1](https://velog.io/@yesdoing/%EB%82%B4%EB%A7%98%EB%8C%80%EB%A1%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8-A-to-Z-1-9pjwz1o6ai)
