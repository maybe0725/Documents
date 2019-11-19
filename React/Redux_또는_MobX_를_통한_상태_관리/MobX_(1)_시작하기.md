# MobX (1) 시작하기

<br/>

MobX 는 또 다른, 하나의 인기있는 리액트 상태 관리 라이브러리입니다. 저는 MobX 는, 사실상 라이브러리 그 이상의 가치를 하는, 리액트의 개발 흐름 자체를 많이 바꿔주는 강력한 도구라고 생각합니다. "MobX 는 최소한의 공수로 여러분들의 상태관리 시스템을 설계 할 수 있게 해줍니다. [" The curious case of mobx state tree"](https://codeburst.io/the-curious-case-of-mobx-state-tree-7b4e22d461f)

<br/>

### 1-1. MobX 의 주요 개념들

MobX 의 주요 개념들은 다음 4가지가 있습니다. 미리 알아두고 진행하면 MobX 를 이해하는것에 도움이 됩니다.

<br/>

### 1. Observable State (관찰 받고 있는 상태)

MobX 를 사용하고 있는 앱의 상태는 Observable 합니다. 이를 직역하자면 이 상태는 관찰 할 수 있는 상태인데요, 어쩌면 관찰 받고 있는 상태라고 이해하는게 조금 더 쉬울 수도 있겠습니다. 우리의 앱에서 사용하고있는 상태는, 변할 수 있으며, 만약에 특정 부분이 바뀌면, MobX 에서는 정확히 어떤 부분이 바뀌었는지 알 수 있습니다. 그 값이, 원시적인 값이던, 객체이던, 배열 내부의 객체이던 객체의 키이던 간에 말이죠.

### 2. Computed Value (연산된 값)

연산된 값은, 기존의 상태값과 다른 연산된 값에 기반하여 만들어질 수 있는 값입니다. 이는 주로 성능 최적화를 위하여 많이 사용됩니다. 어떤 값을 연산해야 할 때, 연산에 기반되는 값이 바뀔때만 새로 연산하게 하고, 바뀌지 않았다면 그냥 기존의 값을 사용 할 수 있게 해줍니다.

이를 이해하기 위해 간단한 예시를 들어보겠습니다. 우리가 편의점에서 800원짜리 물을 네병 샀는데 이게 얼마나오지? 하고 체크하는 함수 `total()` 이라는 함수가 있다고 가정하겠습니다. 우리가 처음 머릿속으로 계산할때 암산으로 4 \* 8 에 32! 라면서 3,200 원이군, 하고 간단히 계산을 하겠죠. 잠시 후에 친구가 그거 다 얼마냐고 또 물어봅니다. 이 때 우리는 머릿속에서 별 생각안하고 응 3,200원이야 라고 말합니다. 친구가, 나도 한병 사줘! 하면 이때 다시 우리는 무의식중에 800원을 더해서 우리가 내야 할 돈이 4,000원인걸 연산합니다.

### 3. Reactions (반응)

Reactions 는 Computed Value 와 비슷한데요, Computed Value 의 경우는 우리가 특정 값을 연산해야 될 때 에만 처리가 되는 반면에, Reactions 은, 값이 바뀜에 따라 해야 할 일을 정하는 것을 의미합니다. 예를 들어서 Observable State 의 내부의 값이 바뀔 때, 우리가 `console.log('ㅇㅇㅇ가 바뀌었어!')` 라고 호출해 줄 수 있습니다.

### 4. Actions (액션; 행동)

액션은, 상태에 변화를 일으키는것을 말합니다. 만약에 Observable State 에 변화를 일으키는 코드를 호출한다? 이것은 하나의 액션입니다. - 리덕스에서의 액션과 달리 따로 객체형태로 만들지는 않습니다.

<br/>

### 1-2. 리액트 없이 MobX 사용해보기

리덕스와 마찬가지로, MobX 는 리액트 종속적인 라이브러리가 아닙니다. 그냥 따로 쓸 수도 있어요. UI 프레임워크 / 라이브러리 없이 쓰셔도 되고, Vue, Angular 등이랑 써도 전혀 무방합니다.

MobX 를 자세하게 이해하려면, 라이브러리 없이 사용을 해보는게 가장 좋습니다!

<p><a target="_blank" href="https://codesandbox.io/s/jl12r55265"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit Vanilla JS MobX Boilerplate"></a></p>

이번 실습은 CodeSandbox 에서 진행하겠습니다.

이번에는 딱히 HTML 은 건들이지 않고 JavaScript 만 수정해보도록 할게요!

```
import { observable, reaction, computed, autorun } from 'mobx';
```

우선, mobx 에서 함수 몇가지들을 불러옵니다. 이 함수들은 하나하나 사용해보면서 설명을 드리도록 하겠습니다.

<br/>

### observable

[observable](https://mobx.js.org/refguide/reaction.html) 함수는 Observable State 를 만들어줍니다.

한번, 덧셈을 해주는 계산기 객체를 만들어보겠습니다.

```javascript
import { observable, reaction, computed, autorun } from "mobx";

// **** Observable State 만들기
const calculator = observable({
  a: 1,
  b: 2
});
```

이렇게 Observable State 를 만들고나면 MobX 가 이 객체를 "관찰 할 수" 있어서 변화가 일어나면 바로 탐지해낼수있습니다.

<br/>

### reaction

특정 값이 바뀔 때 어떤 작업을 하고 싶다면 reaction 함수를 사용합니다.

한번 a 나 b 가 바뀔 때 console.log 로 바뀌었다고 알려주도록 코드를 작성해보겠습니다.

```javascript
import { observable, reaction, computed, autorun } from "mobx";

// Observable State 만들기
const calculator = observable({
  a: 1,
  b: 2
});

// **** 특정 값이 바뀔 때 특정 작업 하기!
reaction(
  () => calculator.a,
  (value, reaction) => {
    console.log(`a 값이 ${value} 로 바뀌었네요!`);
  }
);

reaction(
  () => calculator.b,
  value => {
    console.log(`b 값이 ${value} 로 바뀌었네요!`);
  }
);

calculator.a = 10;
calculator.b = 20;
```

콘솔쪽을 보시면 다음과 같이 나타날 것입니다.

```
a 값이 10 로 바뀌었네요!
b 값이 20 로 바뀌었네요!
```

<br/>

### computed

computed 함수는 연산된 값을 사용해야 할 때 사용됩니다. 특징은, 이 값을 조회할 때 마다 특정 작업을 처리하는것이 아니라, 이 값에서 의존하는 값이 바뀔 때 미리 값을 계산해놓고 조회 할 때는 캐싱된 데이터를 사용한다는 점 입니다.

```javascript
import { observable, reaction, computed, autorun } from "mobx";

// Observable State 만들기
const calculator = observable({
  a: 1,
  b: 2
});

// **** 특정 값이 바뀔 때 특정 작업 하기!
reaction(
  () => calculator.a,
  (value, reaction) => {
    console.log(`a 값이 ${value} 로 바뀌었네요!`);
  }
);

reaction(
  () => calculator.b,
  value => {
    console.log(`b 값이 ${value} 로 바뀌었네요!`);
  }
);

// **** computed 로 특정 값 캐싱
const sum = computed(() => {
  console.log("계산중이예요!");
  return calculator.a + calculator.b;
});

sum.observe(() => calculator.a); // a 값을 주시
sum.observe(() => calculator.b); // b 값을 주시

calculator.a = 10;
calculator.b = 20;

//**** 여러번 조회해도 computed 안의 함수를 다시 호출하지 않지만..
console.log(sum.value);
console.log(sum.value);

// 내부의 값이 바뀌면 다시 호출 함
calculator.a = 20;
console.log(sum.value);
```

결과를 확인해볼까요?

```
계산중이예요!
a 값이 10 로 바뀌었네요!
계산중이예요!
b 값이 20 로 바뀌었네요!
계산중이예요!
30
30
a 값이 20 로 바뀌었네요!
계산중이예요!
40
```

<br/>

### autorun

[autorun](https://mobx.js.org/refguide/autorun.html) 은 reaction 이나 computed 의 observe 대신에 사용 될 수 있는데, autorun 으로 전달해주는 함수에서 사용되는 값이 있으면 자동으로 그 값을 주시하여 그 값이 바뀔 때 마다 함수가 주시되도록 해줍니다. 여기서 만약에 computed 로 만든 값의 .get() 함수를 호출해주면, 하나하나 observe 해주지 않아도 됩니다.

```javascript
import { observable, reaction, computed, autorun } from "mobx";

// Observable State 만들기
const calculator = observable({
  a: 1,
  b: 2
});

// computed 로 특정 값 캐싱
const sum = computed(() => {
  console.log("계산중이예요!");
  return calculator.a + calculator.b;
});

// **** autorun 은 함수 내에서 조회하는 값을 자동으로 주시함
autorun(() => console.log(`a 값이 ${calculator.a} 로 바뀌었네요!`));
autorun(() => console.log(`b 값이 ${calculator.b} 로 바뀌었네요!`));
autorun(() => sum.get()); // su

calculator.a = 10;
calculator.b = 20;

// 여러번 조회해도 computed 안의 함수를 다시 호출하지 않지만..
console.log(sum.value);
console.log(sum.value);

calculator.a = 20;

// 내부의 값이 바뀌면 다시 호출 함
console.log(sum.value);
```

결과를 확인해볼까요?

```
a 값이 1 로 바뀌었네요!
b 값이 2 로 바뀌었네요!
계산중이예요!
a 값이 10 로 바뀌었네요!
계산중이예요!
b 값이 20 로 바뀌었네요!
계산중이예요!
30
30
a 값이 20 로 바뀌었네요!
계산중이예요!
40
```

<p><a target="_blank" href="https://codesandbox.io/s/y62jylzz9"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit Vanilla JS MobX Boilerplate"></a></p>

<br/>

### class 문법을 사용해서 조금 더 깔끔하게

ES6 의 class 문법을 사용하면 조금 더 깔끔하게 코드를 작성 할 수 있습니다. 기존의 코드를 날리고, 이번엔 편의점 장바구니를 만들어보겠습니다. class 로 장바구니를 구현 후, decorate 함수를 통하여 MobX 를 적용해주겠습니다.

```javascript
import { decorate, observable, computed, autorun } from "mobx";

class GS25 {
  basket = [];

  get total() {
    console.log("계산중입니다..!");
    // Reduce 함수로 배열 내부의 객체의 price 총합 계산
    // https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce
    return this.basket.reduce((prev, curr) => prev + curr.price, 0);
  }

  select(name, price) {
    this.basket.push({ name, price });
  }
}

// decorate 를 통해서 각 값에 MobX 함수 적용
decorate(GS25, {
  basket: observable,
  total: computed
});

const gs25 = new GS25();
autorun(() => gs25.total);
gs25.select("물", 800);
console.log(gs25.total);
gs25.select("물", 800);
console.log(gs25.total);
gs25.select("포카칩", 1500);
console.log(gs25.total);
```

결과는 다음과 같습니다.

```
계산중입니다..!
계산중입니다..!
800
계산중입니다..!
1600
계산중입니다..!
3100
```

<br/>

### action

우리가 이전에 상태에 변화를 일으키는것을 action 이라고 부른다고 언급했습니다. 만약에, 이 변화를 일으키는 함수에 MobX 의 action 을 적용하면 무엇을 할 수 있는지 알아보겠습니다.
우선, 코드 상단에서 action 함수를 불러오고, decorate 쪽에 select 가 action 이라는 것을 명시해줄게요.

```javascript
// **** 액션 불러옴
import { decorate, observable, computed, autorun, action } from "mobx";

class GS25 {
  basket = [];

  get total() {
    console.log("계산중입니다..!");
    // Reduce 함수로 배열 내부의 객체의 price 총합 계산
    // https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce
    return this.basket.reduce((prev, curr) => prev + curr.price, 0);
  }

  select(name, price) {
    this.basket.push({ name, price });
  }
}

decorate(GS25, {
  basket: observable,
  total: computed,
  select: action // **** 액션 명시
});

const gs25 = new GS25();
autorun(() => gs25.total);
gs25.select("물", 800);
console.log(gs25.total);
gs25.select("물", 800);
console.log(gs25.total);
gs25.select("포카칩", 1500);
console.log(gs25.total);
```

이 action 을 사용함에 있어서의 이점은 나중에 개발자도구에서 변화의 세부 정보를 볼 수 있고, 변화를 한꺼번에 일으켜서 변화가 일어날 때 마다 reaction 들이 나타나는것이 아니라, 모든 액션이 끝나고 난 다음에서야 reaction 이 나타나게끔 해줄 수 있습니다.

액션을 한꺼번에 일으키는건, [transaction](https://mobx.js.org/refguide/transaction.html) 을 통해 할 수 있습니다.

우선 다음 예제의 콘솔 결과를 확인해보겠습니다.

```javascript
import {
  decorate,
  observable,
  computed,
  autorun,
  action,
  transaction // *** transaction 불러옴
} from "mobx";

class GS25 {
  basket = [];

  get total() {
    console.log("계산중입니다..!");
    // Reduce 함수로 배열 내부의 객체의 price 총합 계산
    // https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce
    return this.basket.reduce((prev, curr) => prev + curr.price, 0);
  }

  select(name, price) {
    this.basket.push({ name, price });
  }
}

decorate(GS25, {
  basket: observable,
  total: computed,
  select: action
});

const gs25 = new GS25();
autorun(() => gs25.total);
// *** 새 데이터 추가 될 때 알림
autorun(() => {
  if (gs25.basket.length > 0) {
    console.log(gs25.basket[gs25.basket.length - 1]);
  }
});

gs25.select("물", 800);
gs25.select("물", 800);
gs25.select("포카칩", 1500);

console.log(gs25.total);
```

```
계산중입니다..!
계산중입니다..!
Object {name: "물", price: 800}
계산중입니다..!
Object {name: "물", price: 800}
계산중입니다..!
Object {name: "포카칩", price: 1500}
3100
```

계산의 경우, 가장 처음 한번 호출이 되고, 데이터가 추가 될 때마다 계산이 되고 있습니다.
한번 이걸 transaction 으로 감싸보겠습니다.

```javascript
import {
  decorate,
  observable,
  computed,
  autorun,
  action,
  transaction
} from "mobx";

class GS25 {
  basket = [];

  get total() {
    console.log("계산중입니다..!");
    // Reduce 함수로 배열 내부의 객체의 price 총합 계산
    // https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce
    return this.basket.reduce((prev, curr) => prev + curr.price, 0);
  }

  select(name, price) {
    this.basket.push({ name, price });
  }
}

decorate(GS25, {
  basket: observable,
  total: computed,
  select: action
});

const gs25 = new GS25();
autorun(() => gs25.total);
// 새 데이터 추가 될 때 알림
autorun(() => {
  if (gs25.basket.length > 0) {
    console.log(gs25.basket[gs25.basket.length - 1]);
  }
});

transaction(() => {
  gs25.select("물", 800);
  gs25.select("물", 800);
  gs25.select("포카칩", 1500);
});

console.log(gs25.total);
```

```
계산중입니다..!
계산중입니다..!
Object {name: "포카칩", price: 1500}
3100
```

transaction 을 통하여 계산 작업은 가장 처음 한번, 그리고 transaction 끝나고 한번 호출이 되었고, 새 데이터 추가 될 때마다 알리는 부분도 3개를 다 추가하고 나서야 딱 한번 콘솔에 마지막 객체가 나타났습니다.

액션을 사용하면, 이렇게 성능 개선도 이뤄낼 수 있고 나중에 MobX 개발자 도구를 사용하게 될 때 변화에 대한 더 자세한 정보를 알 수 있게 해줍니다.

<p><a target="_blank" href="https://codesandbox.io/s/j1v1qw34wv"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit Vanilla JS MobX Boilerplate"></a></p>

<br/>

### decorator 문법으로 더 편하게!

[decorator](https://www.sitepoint.com/javascript-decorators-what-they-are/) 문법은 일종의, 자바스크립트 사투리 라고 생각하시면됩니다. 정규 문법은 아니지만, babel 플러그인을 통하여 사용 할 수 있는 문법입니다. 이 문법을 사용하면 decorate 함수가 더 이상 필요하지 않고, 다음과 같이 작성 해 줄 수 있답니다.

```javascript
import { observable, computed, autorun, action, transaction } from "mobx";

class GS25 {
  @observable basket = [];

  @computed
  get total() {
    console.log("계산중입니다..!");
    // Reduce 함수로 배열 내부의 객체의 price 총합 계산
    // https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce
    return this.basket.reduce((prev, curr) => prev + curr.price, 0);
  }

  @action
  select(name, price) {
    this.basket.push({ name, price });
  }
}

const gs25 = new GS25();
autorun(() => gs25.total);
// 새 데이터 추가 될 때 알림
autorun(() => {
  if (gs25.basket.length > 0) {
    console.log(gs25.basket[gs25.basket.length - 1]);
  }
});

transaction(() => {
  gs25.select("물", 800);
  gs25.select("물", 800);
  gs25.select("포카칩", 1500);
});

console.log(gs25.total);
```

결과는 아까와 똑같습니다.

<p><a target="_blank" href="https://codesandbox.io/s/v3lprkm9p5"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit Vanilla JS MobX Boilerplate"></a></p>

MobX 를 사용하는 대부분의 예제는 이 Decorator 를 사용합니다. 아무래도, 사용하는 편이 훨씬 편하긴 하지만 없이도 사용 하는 것에는 지장이 안갑니다 ([참고](https://mobx.js.org/best/decorators.html)).

<br/>
<br/>

> 퍼온글
>
> - [velog :: MobX (1) 시작하기](https://velog.io/@velopert/MobX-1-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-9sjltans3p)
