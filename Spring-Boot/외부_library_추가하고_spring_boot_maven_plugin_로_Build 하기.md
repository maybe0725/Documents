Spring Boot 에서 외부 library 추가하고 spring-boot-maven-plugin 로 Build 하기
============================================================================

## Spring boot에서 pom.xml에 외부 library 추가는 다음과 같이 한다.

`````````````````````````````````````````````````````````````````````````````````````````````````
<dependency>
	<groupId>DaouCrypto-20180824</groupId>
	<artifactId>DaouCrypto-20180824</artifactId>
	<version>1.0</version>
	<scope>system</scope>
	<systemPath>${basedir}/src/main/resources/external-lib/DaouCrypto-20180824.jar</systemPath>
</dependency>
 `````````````````````````````````````````````````````````````````````````````````````````````````

scope가 system일때 systemPath property를 사용 가능한데 반드시 절대경로여야한다.

classpath 안됨.

외부 jar 추가하고 build까지는 잘 됐는데 runtime에서 NoClassDefFoundError를 뱉었다.

아래는 해결~
 
````````````````````````````````````````````````````````
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <includeSystemScope>true</includeSystemScope>
    </configuration>
</plugin>
```````````````````````````````````````````````````````` 

plugin에

``````````````````````````````````````````````````
<configuration>
    <includeSystemScope>true</includeSystemScope>
</configuration>
 ``````````````````````````````````````````````````

를 추가해주면 빌드도 잘 되고 java -jar ... 로 spring boot server를 올려도 아주 잘 작동된다.

## References

>● [https://sup2is.tistory.com/53](https://sup2is.tistory.com/53)

>● [stackoverflow.com](https://stackoverflow.com/questions/30207842/add-external-library-jar-to-spring-boot-jar-internal-lib)
