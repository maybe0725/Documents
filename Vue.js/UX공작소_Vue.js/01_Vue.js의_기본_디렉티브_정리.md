# Vue.js의 기본 디렉티브 정리

<br/>

원본글 출처

- [Vue.js의 기본 디렉티브 정리](https://ux.stories.pe.kr/112)

<br/>

Vue.js를 공부하면서 기억해야 할 것에 대해 요약정보를 정리해 보려고 합니다.
이번에는 Vue.js에 기본으로 포함되어 있는 기본 디렉티브에 대한 정리 입니다.

<br/>

## 디렉티브

<br/>

디렉티브를 우리나라 말로 하면 지시문이라고 할 수 있습니다.
어떻게 어떻게 해라~ 라고 지시하는 것이죠.
Vue.js의 View영역에서 태그의 속성을 지정하는 방식으로 구현이 됩니다.
태그 속성에 간단히 v-text="message" 라고 작성을 하면 javascript단에서 message변수에 할당된 값을 보여주는 식으로 약속된 명령어를 구현합니다.

Vue.js에서 기본으로 제공하고 있는 디렉티브를 **기본 디렉티브**라고 하고 사용자가 스스로 만들어서 사용하는 것을 **사용자 지정 디렉티브** 또는 **커스텀 디렉티브**라고 합니다.

이번에는 몇개 되지는 않지만 요긴하고 빈번하게 사용하는 기본 디렉티브에 대해서 정리를 하겠습니다.

<br/>

## 기본 디렉티브

<br/>

Vue.js에서 기본으로 제공해 주는 기본 디렉티브는 아래와 같습니다.

<table>
  <thead>
    <tr>
      <th>디렉티브</th>
      <th>설명</th>
      <th>예제</th>
      <th>비고</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>v-text</td>
      <td>innerText와 동일한 기능을 수행하며 태그를 인코딩하여 문자그대로 보여줌</td>
      <td>&ltp v-text="desc"&gt&lt/p&gt</td>
      <td></td>
    </tr>
    <tr>
      <td>v-html</td>
      <td>innerHTML과 동일한 기능을 수행하며 태그를 파싱하여 화면에 구현함</td>
      <td>&ltp v-html="desc"&gt&lt/p&gt</td>
      <td>XSS(Cross Site Scripting) 공격에 주의</td>
    </tr>
    <tr>
      <td>v-bind</td>
      <td>태그와 태그사이의 값을 다루는 것이 아니라 태그의 속성을 변경할 때 사용함</td>
      <td>&ltimg v-bind:src="imgPath" /&gt</td>
      <td></td>
    </tr>
    <tr>
      <td>v-model</td>
      <td>양방향 데이터를 공유할 때 사용함</td>
      <td>&ltinput type="text" v-model="name" /&gt&ltp v-text="name"&gt&lt/p&gt</td>
      <td>lazy, number, trim 속성이 있음</td>
    </tr>
    <tr>
      <td>v-show</td>
      <td>css의 display 와 동일한 기능</td>
      <td>&ltimg v-bind:src="imgPath" v-show="true"/&gt</td>
      <td></td>
    </tr>
    <tr>
      <td>v-if</td>
      <td>if 조건문을 구현함</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>v-else</td>
      <td>else 조건문을 구현함</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>v-else-if</td>
      <td>else if 조건문을 구현함</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>v-for</td>
      <td>for 반복문을 구현함</td>
      <td>&lttr v-for="image in images"&gt ... &lt/tr&gt</td>
      <td></td>
    </tr>
    <tr>
      <td>v-pre</td>
      <td>vuejs가 컴파일하지 않게 함. {{}}도 그대로 보여짐</td>
      <td>&ltp v-pre&gt {{name}} &lt/p&gt</td>
      <td></td>
    </tr>
    <tr>
      <td>v-once</td>
      <td>vuejs가 처음 한번만 렌더링을 수행함. 데이터가 변경되도 그냥 처음값만 보여줌</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>v-clock</td>
      <td>vuejs가 복잡할 경우 컴파일되지 않은 값이 순간 나오는 경우가 있는데 이런것을 막아서 결과값만 안전하게 보여주게 함</td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
  </table>
