# Spring Boot pom.xml 파일의 line 1번 Unknown Error

<br/>

## 에러

<br/>

[STS > Problems window]


| Description | Reourse | Path | Location | Type |
|---|---|---|---|---|
| Errors(1 item) | | | | |
| Unknown | pom.xml | /BootTest | line 1 | Maven Configuration Problem |


<br/>

## 해결방법

<br/>

### 1. Update Project

```
Project Explorer > 해당 프로젝트 마우스 우클릭 > Maven > Update Project...

해당 방법은 STS가 최신버전일때 가능한 해결방법이다.
STS가 최신버전이 아니라면 아래 방법으로 조치 한다.
```

### 2. pom.xml 에서 maven plugin 변경 (property 추가)

```xml
<properties>
    <java.version>1.8</java.version>
    <maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>
</properties>
```

### 3. pom.xml 에서 spring-boot-starter-parent 변경

변경전
```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.8.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

변경후
```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.4.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```
