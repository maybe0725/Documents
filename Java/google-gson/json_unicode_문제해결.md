# [Gson] json unicode 문제해결

Gson을 사용하여 json변환 작업시, = 문자가 \u003d로 변환

```java
Map<String,String> map = new HashMap<String,String>();
map.put("id", "asd12sdnwe==");
Gson gson = new Gson();
String json = gson.toJson(map);
```

asd12sdnwe\u003d 이런식으로 출력된다.

아래와 같이 gson 을 사용하면 유니코드로 변환되지않는다.

```java
Map<String,String> map = new HashMap<String,String>();
map.put("id", "asd12sdnwe==");
Gson gson = new GsonBuilder().disableHtmlEscaping().create();
String json = gson.toJson(map);
```

```xml
<bean id="gsonBuilder" class="com.google.gson.GsonBuilder"/>
<bean class="org.springframework.beans.factory.config.MethodInvokingFactoryBean">
    <property name="targetObject" ref="gsonBuilder"/>
    <property name="targetMethod" value="disableHtmlEscaping"/>
</bean>
<bean id="gson" class="com.google.gson.Gson" factory-bean="gsonBuilder" factory-method="create"/>
```
