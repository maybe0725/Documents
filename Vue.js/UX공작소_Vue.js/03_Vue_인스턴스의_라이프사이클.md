# Vue 인스턴스의 라이프사이클

<br/>

원본글 출처

- [Vue 인스턴스의 라이프사이클](https://ux.stories.pe.kr/118)

<br/>

보통 프레임워크나 라이브러리의 경우 처리 순서에 따라 라이프사이클이 존재합니다.
사실 사람이 인식하기도 전에 순식간에 라이프사이클이 지나가 버리기 때문에 육안으로는 관찰이 거의 어렵습니다.
하지만 컴포넌트를 개발하거나 일반 개발 과정에서 라이프사이클은 중요한 요소로 작용을 합니다.
예를 들면, 분명 내 생각에는 A _ ( B + C ) = D가 나와야 하는데 E가 나오는 경우가 발생을 합니다.
확실하게 D가 나와야 하는데 말이죠.. 이런 경우 십중팔구는 라이프사이클 때문 일 확률이 높습니다.
아직 순서 상으로( B + C )가 계산 되기도 전 인데 먼저 A _ B를 계산해 버리는 경우 이런 문제가 발생할 수 있다는 것입니다.
그래서 라이프사이클을 이해하는 것은 나중에 심도있게 개발 할 때 중요한 요소 일 수 있습니다.

Vue에서도 라이프사이클 중간 중간에 끼여들 수 있는 **훅(hook)**을 제공하고 있습니다. 적절한 훅에 개발 코드를 넣어야 원하는 데로 작동을 할 수 있습니다.

<br/>

## Vue 인스턴스의 라이프 사이클

Vuejs.org에서 라이프사이클과 Hook을 정리한 다이어그램이 있습니다.

![images](https://t1.daumcdn.net/cfile/tistory/9989E6475C472C0D1C)

<br/>

## Vue 인스턴스의 라이프 사이클 훅

<table>
  <thead>
    <tr>
      <th>라이프사이클 훅</th>
      <th>설명</th>
      <th>비고</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>beforeCreate</td>
      <td>Vue인스턴스가 생성되었지만 데이터관찰, 이벤트 감시가 설정 되기 전에 호출 됨</td>
      <td></td>
    </tr>
    <tr>
      <td>created</td>
      <td>Vue인스턴스가 생성되고 데이터관찰, 계산형 속성, 메서드, 감시의 설정이 끝나면 호출 됨</td>
      <td></td>
    </tr>
    <tr>
      <td>beforeMount</td>
      <td>DOM인 el 요소에 vue 인스턴스가 마운트 되기 전에 호출 됨</td>
      <td></td>
    </tr>
    <tr>
      <td>mounted</td>
      <td>DOM인 el 요소에 vue 인스턴스가 마운트 된 후에 호출 됨</td>
      <td></td>
    </tr>
    <tr>
      <td>beforeUpdate</td>
      <td>가상 DOM이 생성되기 전에 데이터가 변경될때 호출 됨</td>
      <td></td>
    </tr>
    <tr>
      <td>updated</td>
      <td>데이터의 변경으로 인해 가상 DOM이 다시 렌더링된 후에 호출 됨. DOM에 종속성이 있는 계산은 이 단계에서 수행해야 함</td>
      <td></td>
    </tr>
    <tr>
      <td>beforeDestroy</td>
      <td>Vue 인스턴스가 제거 되기 전에 호출 됨</td>
      <td></td>
    </tr>
    <tr>
      <td>destroyed</td>
      <td>Vue 인스턴스가 제거 된 후에 호출 됨</td>
      <td></td>
    </tr>
  </tbody>
  </table>
