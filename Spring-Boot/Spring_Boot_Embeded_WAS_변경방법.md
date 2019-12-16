# Spring Boot Embeded WAS 변경방법

<br/>

## Tomcat

Spring Boot 는 기본 내장 서버로 Tomcat을 사용하고 있습니다.

따라서 특별한 설정없이 Web Starter 의존성만 추가해주면 됩니다.

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

<br/>

## Jetty

Spring Boot 의 내장 서버로 Tomcat 대신에 Jetty를 사용해보겠습니디.

먼저 Web Starter에 기본적으로 들어있는 Tomcat Starter를 제외시켜 줍니다.

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

다음으로 Jetty Starter 의존성을 추가해줍니다.

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```

<br/>

## Undertow

Undertow도 Jetty와 동일한 방식으로 설정해줄 수 있습니다.

먼저 Web Starter에 기본적으로 들어있는 Tomcat Starter를 제외시켜 주고,
Undertow Starter 의존성만 추가 후에 스프링 부트 애플리케이션을 실행하면 Undertow 내장 서버가 올라갑니다.

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```
