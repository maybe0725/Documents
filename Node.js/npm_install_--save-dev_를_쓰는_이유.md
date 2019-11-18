npm install --save-dev 를 쓰는 이유
===================================

### —save-dev 옵션은 왜 넣을까?

Node.js의 npm 으로 모듈을 설치할 때 아래처럼 --save-dev 옵션을 넣는 경우가 있습니다.  

이 옵션은 어떤 의미일까요?

```````````````````````````````````
$npm install --save-dev babel-cli
```````````````````````````````````

Node.js에서는 보통 npm으로 모듈을 설치합니다. `$npm install [모듈이름]` 이런 식으로요.

이러면 명령을 실행한 디렉토리에 node_modules라는 이름의 디렉토리가 생기고,

해당 디렉토리 안에는 모듈이 설치됩니다.

그런데 다른 프로젝트에서도 똑같은 모듈이 필요하면 어떻게 해야 할까요?

똑같이 `$npm install [모듈이름]`을 해당 프로젝트의 폴더에서 해야겠죠. 

한 두번이면 괜찮은데 이런 작업이 100개 1000개이면?? 또 추가해야 할 모듈이 100개 1000개이면?

뭔가 모듈을 한번에 추가해주는 쉬운 방법이 필요합니다.

그래서 npm은 모듈의 추가, 삭제 뿐만아니라 프로젝트의 관리도 해주는데요.

`$npm init` 을 하면 프로젝트를 관리하기 위한 `package.json` 파일이 생성됩니다.

그 다음에 `$npm install --save [모듈이름]`으로 모듈을 설치하면 package.json에는 설치한 모듈과 버전이 기록됩니다.

`dependencies`라는 항목이 바로 그녀석인데요, 이 프로젝트에 연관된 모듈들의 목록이 들어있죠.

나중에 다른 곳에 해당 프로젝트를 가져오더라도 모든 모듈을 일일이 설치할 필요없이 `$npm install`만 입력하면 dependencies 항목을 기반으로 필요한 모듈을 모두 자동으로 설치합니다. 

그럼 이제 하나 남았네요. `$npm install --save-dev [모듈이름]`으로 설치하는것은 

모듈을 설치할 때 `package.json 내의 devDependencies` 항목에 설치한 모듈과 버전을 넣는 것을 뜻합니다.

dev로 시작되는 이름에서 볼 수 있듯이 `개발용으로 쓸 경우 devDependencies에 기록합니다`. 

가령 이런 경우겠죠. 제품의 릴리즈나 구동시 꼭 필요한 모듈의 경우 `--save 옵션으로 dependencies` 항목에 기록하고

제품의 개발시에 테스트를 위해서 필요한 모듈이긴 한데 실제 릴리즈시에는 필요없는 모듈의 경우 `--save-dev로 devDependencies` 항목에 넣는게 맞겠네요.

자. 마지막으로 이렇게 기록된 모듈 항목들을 설치할 때는 어떻게 쓸까요? 
그냥 `$npm install`이라 하면 될까요?

`$npm install`이라고 쓰면 `dependencies, devDependencies` 모든 모듈을 설치합니다.

dependencies만 설치하려면 `$npm install --only=prod[uction]`

devDependencies만 설치하려면 `$npm install --only=dev[elopment]` 로 씁니다.

매번 그냥 무의식중에 썼는데 알고보니 재밌지 않나요?

<br/>

>● 원본 글 링크 :: [잉고래의 잇다이어리](https://ingorae.tistory.com/1754)
