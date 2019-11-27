### 1. Atoms Design이란

* Atoms Design 이란 UI 컴포넌트들을 더 조직화할 수 있도록 하는 목적을 둔 방법론입니다.

* atoms, molecules, organisms, pages ,templates 의 형식으로 하여 컴포넌트를 분리하여 적용할수 있습니다.

![Atoms Design이란](images/20190730-1407-01.png)

### 2. Atoms Design 구성 요소들

Atoms(원자)

* Atom은 주로 native hmtl 태그가 대상이 된다. 그리고 컬럼 팔레트와 애니메이션 같은 추상적인 요소도 포함된다.

![Atoms(원자)](images/20190730-1407-02.png)

Molecules(분자)

* Molecules는 atoms의 그룹, 원자에서 분자를 만들때는 단순한 한가지 사용에 근거한 조합을 권장한다. <br/>( 예시 ) 검색 Input box 와 검색 버튼의 조합 )

![Molecules(분자)](images/20190730-1407-03.png)

Organisms(유기체)

* organism 은 Atom들의 그룹 또는 Molecule 그리고 다른 organism 까지도 포함하는 개념<br/>
(예) Header 같이 인터페이스로 구분되는 것들의 조합)

![Organisms(유기체)](images/20190730-1407-04.png)

Templates

* Templates은 Pages 들에서 사용되는 Layout 들

![Templates](images/20190730-1407-05.png)

Pages

* 페이지는 템플릿의 특정 인스턴스가 된다.

![Pages](images/20190730-1407-06.png)

### 3. Atomic Design 구현 예시

-atomic design를 쉽게 할수 있도록 한 react-start 프로젝트(https://arc.js.org/)

<br/><br/>

---

## References

* https://byeonggi.github.io/blog/atoms-design-%EC%9D%B4%EB%9E%80
* https://github.com/diegohaz/arc/wiki/Atomic-Design
* http://bradfrost.com/blog/post/atomic-web-design/
* https://brunch.co.kr/@ultra0034/63
* https://arc.js.org/
