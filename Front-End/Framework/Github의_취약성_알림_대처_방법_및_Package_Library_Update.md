# Github의 취약성 알림 대처 방법 및 Package Library Update

<br/>

참고
- https://blog.sonim1.com/225

<br/>
 
## 1. npm update

```sh
npm install -g npm
```

<br/>

## 2. npm outdated

많은 의존성 모듈에서 새로운 버전이 나오는 모듈을 일일이 확인하기 어려운데 
`npm outdated` 명령어를 사용하면 의존성 모듈 중에 `업데이트가 필요한 모듈을 확인할 수 있다.`

- Current : 현재 설치된 버전.
- Wanted : package.json에 지정한 버전 범위로 설치되는 최대 범위를 의미.

"Current"는 현재 설치된 버전이고 
"Wanted"는 package.json에 지정한 버전 범위로 설치되는 최대 범위를 의미한다. 

즉, npm update를 실행하면 설치되는 버전이다. 
"Latest"는 모듈의 최신 버전이다. 

"Wanted"와 "Latest"가 같은 모듈이 빨간색으로 표시되고 

"Latest"가 "Wanted"보다 높은 모듈은 노란색으로 표시된다. 

여기서 확인을 한 뒤에 일일이 원하는 모듈을 업데이트해도 되지만 꽤 귀찮은 일이다.

```sh
[npm use]
npm outdated

[yarn use]
yarn outdated
```

<br/>

## 3. npm-check

npm update eslint-utils
npm -D install eslint-utils

<br/>
