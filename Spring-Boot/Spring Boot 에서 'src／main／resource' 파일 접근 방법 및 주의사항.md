# Spring Boot에서 src/main/resource 파일 접근 방법 및 주의사항.

<br/>

출처 :: [Spring Boot에서 src/main/resource 파일 접근 방법 및 주의사항.](https://wedul.site/431)

<br/>

Spring Boot를 이용해서 빠르게 프로젝트를 제작해야 할일이 있어서 작업을 하던 도중에 `src/main/resource` 위치에 파일에 접근이 필요했다.

그래서 ClassLoader를 사용해서 resource를 획득한 후 해당 경로를 얻어 Paths 객체를 만들고 파일을 읽었다. 로컬에서 개발할 때는 정상적으로 읽어졌다.

```java
String jsonTxt = new String(Files.readAllBytes(Paths.get(getClass().getClassLoader().getResource("course.txt").toURI())));
cs
```

**하지만 war로 빌드하고 서버에 올리고 나서 문제가 발생했다. 해당 파일 자체를 읽지를 못했다. 왜그럴까? 한참 고민하다가 인터넷 검색해서 한가지 글을 보았다.**

```
"resource.getFile() expects the resource itself to be available on the file system, i.e. it can't be nested inside a jar file.
This is why it works when you run your application in STS but doesn't work once you've built your application and run it from the executable jar.
Rather than using getFile() to access the resource's contents, I'd recommend using getInputStream() instead.
That'll allow you to read the resource's content regardless of where it's located. "
```

```
resource.getFile ()은 리소스 자체가 파일 시스템에서 사용 가능할 것으로 예상합니다. 즉, jar 파일 내에 중첩 될 수 없습니다.
그렇기 때문에 STS에서 응용 프로그램을 실행할 때 작동하지만 응용 프로그램을 빌드하고 실행 가능한 jar에서 실행하면 작동하지 않습니다.
getFile ()을 사용하여 리소스 내용에 액세스하는 대신 getInputStream ()을 대신 사용하는 것이 좋습니다.
그러면 리소스의 위치에 관계없이 리소스의 내용을 읽을 수 있습니다.
```

이 내용을 읽고나서 바로 코드를 수정했다.

```java
ClassPathResource cpr = new ClassPathResource("course.txt");
byte[] bdata = FileCopyUtils.copyToByteArray(cpr.getInputStream());
String jsonTxt = new String(bdata, StandardCharsets.UTF_8);
```

etFile()을 사용하지 않고 getInputStream()을 사용하니 정상적으로 읽어졌다.

사용할 때 조심해야겠다.
