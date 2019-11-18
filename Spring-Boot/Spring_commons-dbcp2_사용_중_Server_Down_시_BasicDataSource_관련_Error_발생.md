Spring commons-dbcp2 사용 중 Server Down 시 BasicDataSource 관려 Error 발생
==========================================================================

DB Conntion Pool 로 commons-dbcp2 사용시 Server Down 중에 아래와 같은 에러가 발생

`````````````````````````````````````````````````````````````````````````````````````````````````````
[2019-06-11] [13:48:23.303] [WARN] [o.a.c.d.BasicDataSource] [BasicDataSource.java:1919 [Failed to unregister the JMX name: org.apache.commons.dbcp2:name=dataSource,type=BasicDataSource]
javax.management.InstanceNotFoundException: org.apache.commons.dbcp2:name=dataSource,type=BasicDataSource
	at 블라블라...
`````````````````````````````````````````````````````````````````````````````````````````````````````

### 원인

Spring 이 먼저 DataSource 객체의 JMX 정보를 unregist 하고 나서 BasicDataSource 에서도 close 메소드가 호출 되면서 
이미 내려간 JMX 정보를 내리려 하기 때문에 에러가 발생.

### 해결책

1) Spring JMX 기능을 커스터마이징 하여 DataSource 에 대해서는 제외 한다.
2) BasicDataSource 의 close 메소드를 호출하지 않도록 한다.

이 두가지 방법 중 제일 쉬운 것인 '2' 해결책 이다.

대부분 BasicDataSource 의 Spring 예제 코드는, 아래와 같이 되어 있다.

``````````````````````````````````````````````````````````````````
@Primary
@Bean(name = "escrowMasterDataSource", destroyMethod = "close")
public DataSource dataSource() {
    BasicDataSource dataSource = new BasicDataSource();
    ...
    ...
    return dataSource;
}
``````````````````````````````````````````````````````````````````

이것을 아래와 같이 수정한다.

``````````````````````````````````````````````````````````````````````````
@Primary
@Bean(name = "escrowMasterDataSource", destroyMethod = "postDeregister")
public DataSource dataSource() {
    BasicDataSource dataSource = new BasicDataSource();
    ...
    ...
    return dataSource;
}    
``````````````````````````````````````````````````````````````````````````

위와 같이 수정하면 JMX unregist 에 대한 에러가 발생하지 않는다.

## References
>● [https://windwolf.tistory.com/32](https://windwolf.tistory.com/32)
