# Spring Boot 공식 지원 내장 WAS 인 Undertow 를 사용합시다.

<br/>

출처 :: [Spring Boot 공식 지원 내장 WAS 인 Undertow 를 씁시다.](https://zepinos.tistory.com/35?category=797552)

<br/>

Java 가 Web 개발에서 두각을 나타내면서 WAS(Web Application Server) 라는 용어를 널리 사용하게 만들었습니다.
처음의 의도와 달리 WAS 를 지칭하는 의미는 점차 확대 되었고, Java에서는 Apache Tomcat(이하 Tomcat) 이라는 WAS 가 널리 보급되면서 온라인 상에 유통되는 대부분의 WAS에 관한 것은 Tomcat 일 정도가 되었습니다.

Tomcat 은 Java 로 만들어져 이식성이 좋고, Full Spec 의 J2EE 를 지원하진 않지만 Spring Framework 의 탄생으로 인해 J2EE Servlet 지원 만으로도 Enterprise 급의 프로그램을 개발할 수 있게 되면서 현재까지 널리 사용되고 있습니다.
그래서 Java 에서 Web Page 개발에 대한 온라인 상의 글들(심지어 책에서 조차)은 Tomcat 을 설치하고 거기서 운영하도록 작성되어져 있습니다.

<br/>

서비스 측면에서 Tomcat 은 어떤 수준일까요?

먼저, 다른 개발언어인 ASP.NET 의 경우 대부분 IIS 를 기반으로 대부분의 문서가 작성되어져 있습니다.
그리고 서비스 시에도 대부분 IIS 기반으로 서비스를 운영합니다.
이는 Microsoft 에서 만들어낸 언어라는 특수성이 반영된 결과 입니다.
그리고 사실 IIS 가 아닌 다른 환경에서 운영할 수 있도록 Web Server(혹은 WAS) 가 존재하는지조차 모르는 사람들이 대부분일 것입니다.

거기에 반해 Java 는 WebLogic, WebSphere, JBoss(Wildfly), Glassfish, Jetty 등 수많은 대안이 존재 합니다.
그리고 각 WAS 들의 특징이나 주요 사용처가 다릅니다.
이러한 많은 WAS 들 중 Tomcat 이 가지는 위상은 어떤 것일까요?

<br/>

Java 를 이용해 오래 개발한 분들이라면 Tomcat 을 Service 에 사용하기에는 좀 무리가 있다고 생각하시는 분들이 있으실 것입니다.
저의 경우 마지막 사용해본 Tomcat 버전이 Tomcat 8.5 인데, 많이 좋아졌다고는 하나, 시스템에 따라 원인도 모르게 Tomcat 이 동작하지 않는 문제가 한달 정도 안에 발생하는 서버가 많아서 주기적으로 모니터링 하면서 재시작을 해주곤 했습니다.
현재에는 그렇게까지 불안하진 않은 것으로 보입니다만, 여전히 오래된 방식의 내부 구현으로 인해 많은 사용 요청이 올 결우 제대로 된 응답을 보내주지 못하는 경우가 발생하기도 합니다.

거이에 반해 WebLogic 이나 WebSphere 등의 상용 WAS 들은 아주 오래 전부터 꽤나 안정적인 내구성을 자랑했고, 그 결과 기관이나 큰 업체에서는 J2EE 로 개발되지 않았음에도 거금을 들여 상용 WAS 로 서비스를 운영했습니다. 저의 경우도 특정 드라마로 인해 개발 후 운영중이던 서버가 부하로 인해 계속 장애가 발생했는데, WebLogic 도입 후 장애 없이 느려지기만 할 뿐 서비스가 이루어지는 것을 보고 왜 큰 비용을 들이는지 납득하기도 했었습니다.

<br/>

하지만 Spring Boot 가 나오면서 굉장한 옵션이 생겼습니다.

기존에는 WAS 를 설치한 뒤 개발한 소스를 war 파일로 혹은 디렉토리 전체를 특정 위치에 복사한 뒤 Deploy(배포) 하는 형태로 WAS 를 운영하거나 Jetty 와 같은 라이브러리를 추가하고 소스 코드 내부에서 WAS 를 기동시켜 Embeded WAS 형태로 사용하였습니다.
하지만 Embeded 형태롤 나온 Jetty 는 Tomcat 이상으로 대량의 부하에 취약한 편이라 가볍게 서버를 구성하는 환경이 아니고서는 잘 사용되지 않았습니다.
그러나 Undertow 라는 내장 WAS 가 나오면서 상황이 바뀌었다고 생각 합니다.

JBoss 는 J2EE 규격을 모두 만족하는 WAS 를 제공하고 있었는데, 기존에는 Tomcat 을 Servlet Container 로 포함하는 형태 였습니다.
하지만 최근에 Tomcat 을 버리고 Netty 라고 아주 뛰어난 Java Network Library 를 이용해 새로운 Servlet Container 를 만들었습니다.
그것이 바로 Undertow 입니다.
이 Undertow 는 JBoss 나 Wildfly 라는 WAS 에 포함되어 사용 되고 있고, Jetty 와 마찬가지로 내장 WAS 로 별도 사용이 가능한 형태입니다.
그리고 Spring Boot 는 Tomcat, Jetty 와 더불어 Undertow 를 내장 WAS 로 쉽게 사용할 수 있도록 지원하였습니다.
물론 Tomcat 역시 Jetty 나 Undertow 처럼 자신들이 스스로 내장 WAS 로 동작할 수 있도록 기존 Tomcat 코드를 이용해 사실상 동일한 성능을 낼 수 있는 결과물을 만들었고 Spring Boot 에서는 이를 이용하고 있습니다.

<br/>

실제 ab 와 같은 단순한 부하 테스트 도구를 이용해 간단한 테스트를 진행해보면 Undertow 는 Tomcat 보다 훨씬 빠르고 안정적인 모습을 보여줍니다. 그리고 Spring Boot 에서는 Undertow 로 변경하기 위해서 Maven 혹은 Gradle 에서 몇 줄만 수정 하기만 하면 적용이 완료됩니다.
그래서 사실 Spring Boot 2 에서 Undertow 를 기본 WAS 로 해두지 않았는지 의문이 들 정도 입니다.

혹자는 이런 말을 합니다.

<br/>

> **"외장 Tomcat 이 내장 Tomcat 보다 성능이나 안정성이 더 뛰어난 거 아닌가요?"**

<br/>

네. 오해입니다. 이건 Tomcat 개발자들이 이미 답을 한 내용입니다.
사실 Undertow 가 JBoss 나 Wildfly 에 이미 적용되어 있기 때문에 외장 WAS 에 소스를 배포하는 식으로 사용하고 싶다면 언제라도 무료인 Wildfly 를 설치하면 됩니다.
하지만 오히려 사용하지 않을 J2EE Spec 을 준수하기 위해 몸집이 더 비대한(성능에 영향을 주지 않을지라도) WAS 를 사용할 필요가 있을까요?
왜 Spring 이 J2EE 를 이용하지 않고도 Enterprise 한 프로그램을 만들 수 있도록 제공되는 Framework 을 표방하고 나왔으며, 굳이 Spring 을 쓰고 있는데 J2EE 를 지원하는 WAS 를 별도로 사용할 필요가 있을까요? 결국 WAS 의 선택 기준은 성능에 의해 Tomcat 이냐 Undertow 냐가 될 뿐이지, 내장 WAS 냐 외장 WAS 냐는 성능과 무관하고 외장 WAS 에서만 처리할 수 있는 기능이 필요할 때에만 외장 WAS 를 고려해도 된다는 것입니다. 여러 도메인을 하나의 WAS 에서 처리해야 한다는 조건 같은 것도 각각의 내장 WAS 를 띄우고 앞에 haproxy 등 L7 을 붙이는 것만으로도 충분히 해결이 가능하니 대부분의 경우는 내장 WAS 를 서비스로 이용해도 괜찮다고 생각 합니다.

<br/>

그러고보니, 새롭게 제공되는 Spring WebFlux 에서는 Netty 를 기본으로 제공하니, 사실 Tomcat 의 시대는 끝나가고 있는게 아닐지 조심스레 추측해봅니다. 그래도 지금까지 Java 진영의 발전에 혁혁한 공을 세운 Tomcat 을 영원히 기억할 것 같습니다.
