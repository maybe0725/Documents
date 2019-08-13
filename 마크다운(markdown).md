마크다운 markdown 작성법
=======================

### 마크다운 

> 마크다운(markdown)은 일반 텍스트 문서의 양식을 편집하는 문법이다. <br/>
> README 파일이나 온라인 문서, 혹은 일반 텍스트 편집기로 문서 양식을 편집할 때 쓰인다. <br/>
> 마크다운을 이용해 작성된 문서는 쉽게 HTML 등 다른 문서형태로 변환이 가능하다.

# 1. 문단

문단과 문단 사이는 빈 줄(엔터키 두 번)로 구분한다.

```
하나의 문단.

다른 문단.
```

하나의 문단.

다른 문단.

# 2. 제목 Headers

``h1`` 부터 ``h6`` 까지 제목을 표현할 수 있다.

```
# h1
## h2
### h3
#### h4
##### h5
###### h6  
```

# h1
## h2
### h3
#### h4
##### h5
###### h6  

``h1``과 ``h2``는 다음과 같이 표혈할 수 있다.

```
h1
=====

h2
-----
```

h1
=====

h2
-----

# 3. 인용문

``>`` 블럭인용문자를 이용한다.

```
> 인용문
>> 중첩된 인용문
>>> 중중첩된 인용문
```

> 인용문
>> 중첩된 인용문
>>> 중중첩된 인용문

# 4. 목록(리스트)

* 4.1. 순서있는 목록(번호)

```
1. 첫 번째
2. 두 번째
3. 세 번째
```

1. 첫 번째
2. 두 번째
3. 세 번째

* 4-2. 순서없는 목록(글머리 기호)

```
* 첫 번째
    * 두 번째
        * 세 번째

+ 첫 번째
    + 두 번째
        + 세 번째

- 첫 번째
    - 두 번째
        - 세 번째
```

* 첫 번째
    * 두 번째
        * 세 번째

+ 첫 번째
    + 두 번째
        + 세 번째

- 첫 번째
    - 두 번째
        - 세 번째        

# 5. 코드 블럭     

`` ``` ``혹은 `` ~~~ ``를 첫 줄과 마지막 줄에 삽입하고 그 사이에 글 작성.

```
이것은
코드 블록
입니다
```

~~~
이것은
코드 블록
입니다
~~~

# 6. 인라인 코드

```  `` 로 감싼 텍스트

```
`인라인 코드 블럭`

``인라인 코드 블럭``
```

`인라인 코드 블럭`

``인라인 코드 블럭``

# 7. 수평선

`-`또는 `*` 또는 `_`를 3개 이상 작성 

(단, `-`를 사용할 경우 header로 인식할 수 있으니 이 전 라인은 비워두어야한다.) 

아래 줄은 모두 수평선을 만든다.

```
* * *
***
*****
- - -
____________
```

* * *
***
*****
- - -
____________

# 8. 강조

기울여 쓰기, 굵게 쓰기, 밑줄 쓰기, 취소선

```
*기울여 쓰기*
_기울여쓰기_
**굵게 쓰기**
__굵게 쓰기__
~~취소선~~
```

*기울여 쓰기*

_기울여쓰기_

**굵게 쓰기**

__굵게 쓰기__

~~취소선~~

# 9. 링크(Links)

* 9.1. 인라인 링크

`[링크](https://예시.com "링크 제목")` 인라인 링크

```
[Google](https://www.google.com "구글")
```

[Google](https://www.google.com "구글")

* 9.2. url 링크

`<예시.com/>` `<예시@예시.com>` url 링크

```
<https://www.google.com/>
<abc1@gmail.com/>
```

<https://www.google.com/>
<abc1@gmail.com/>

# 10. 이미지

* 10.1. 이미지 삽입

`![설명](이미지 링크)`

```
![vue image](https://vuejs.org/images/logo.png)
```

![vue image](https://vuejs.org/images/logo.png)

* 10.2. 이미지 링크

마크다운 이미지 코드를 링크 코드로 묶어준다.

`[![설명](이미지 링크)](https://예시.com "링크 설명")`

```
[![vue image](https://vuejs.org/images/logo.png)](https://kr.vuejs.org/ "vue 이미지")
```

[![vue image](https://vuejs.org/images/logo.png)](https://kr.vuejs.org/ "vue 이미지")

# 11. 블록접기


<details>
<summary>CLICK ME - show hidden blocks</summary>
<p>

yes, even hidden code blocks!

```python
print("hello world!")
```

</p>
</details>

<details>
  <summary>CLICK ME - show hidden table blocks</summary>
  <table>
  <thead>
    <tr>
      <th>key</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>x-api-token</td>
      <td>XA/Xxa/FAASDASDxxxxXXXXXXX/xxxxxxxxxx=</td>
    </tr>
    <tr>
      <td>x-user-lang</td>
      <td>en_US</td>
    </tr>
    <tr>
      <td>Content-Type</td>
      <td>application/json</td>
    </tr>
  </tbody>
  </table>
</details>
