# Java에서 JSON(GSON)사용

[Link :: Maven dependency](https://mvnrepository.com/artifact/com.google.code.gson/gson)

```xml
<dependency>
	<groupId>com.google.code.gson</groupId>
	 <artifactId>gson</artifactId>
	 <version>x.x.x</version>
</dependency>
```

### 1. Object to Json

```java
public class Person {
    private String name;
    private int age;
    private String gender;
}

public void objectToJson() {
    Person person = new Person();
    person.setName("kim");
    person.setAge(20);
    person.setGender("M");

    Gson gson = new Gson();

    String json = gson.toJson(person);
    System.out.println(json);
}
```

### 2. JsonObject 를 이용한 Json 만들기

```java
public void simpleJson() {
    Gson gson = new Gson();

    JsonObject object = new JsonObject();
    object.addProperty("name", "park");
    object.addProperty("age", 22);
    object.addProperty("success", true);

    String json = gson.toJson(object);
    System.out.println(json);
}
```

### 3. Json Parsing

```java
public void jsonStringParsing() {
    String json = "{\"name\":\"kim\",\"age\":20,\"gender\":\"M\"}";

    JsonParser parser = new JsonParser();
    JsonElement element = parser.parse(json);

    String name = element.getAsJsonObject().get("name").getAsString();
    System.out.println("name = "+name);

    int age = element.getAsJsonObject().get("age").getAsInt();
    System.out.println("age = "+age);
}
```

```java
public void uiScriptParser(String jsonString) throws Exception {

    System.out.println("==================================================");
    System.out.println(jsonString);
    System.out.println("==================================================");

    Gson gson = new Gson();

    JsonParser  jsonParser  = new JsonParser();
    JsonElement jsonElement = jsonParser.parse(jsonString);

    JsonObject headerJsonObject = jsonElement.getAsJsonObject().get("header")  .getAsJsonObject();
    JsonArray functionJsonArray = jsonElement.getAsJsonObject().get("function").getAsJsonArray();
    JsonArray objectJsonArray   = jsonElement.getAsJsonObject().get("object")  .getAsJsonArray();

    Map<String, String>       headerMap    = (Map<String, String>)       gson.fromJson(headerJsonObject,  HashMap.class);
    List<Map<String, String>> functionList = (List<Map<String, String>>) gson.fromJson(functionJsonArray, List.class);
    List<Map<String, String>> objectList   = (List<Map<String, String>>) gson.fromJson(objectJsonArray,   List.class);

    System.out.println("==================================================");
    System.out.println(headerMap);
    System.out.println("--------------------------------------------------");
    System.out.println(functionList);
    System.out.println("--------------------------------------------------");
    System.out.println(objectList);
    System.out.println("==================================================");
}
```

### 4. Json to Object

```java
public void jsonToObject() {
    String json = "{\"name\":\"kim\",\"age\":20,\"gender\":\"M\"}";
    Gson gson = new Gson();
    Person person = gson.fromJson(json, Person.class);

    System.out.println("name = " + person.getName());
    System.out.println("age = " + person.getAge());
    System.out.println("gender = " + person.getGender());
}
```
