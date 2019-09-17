# Step One

## Maven

{% code-tabs %}
{% code-tabs-item title="pom.xml" %}
```markup
<properties>
  ...

  <!-- Use the latest version whenever possible. -->
  <jackson.version>2.9.8</jackson.version>
  ...
</properties>

<dependencies>
  ...
  <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>${jackson.version}</version>
  </dependency>
  ...
</dependencies>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Jackson有三個JAR檔\(core/annotations/databind\)，其中只要引入databind，其他兩個就會一併被引入，因為databind相依其他兩個library。

## Jackson使用ObjectMapper來處理json格式的Serialize序列化與Deserialize反序列

```java
ObjectMapper mapper = new ObjectMapper(); // create once, reuse
```

## Deserialize反序列化 \(json to POJOs\)

```java
public class MyValue {
	public String name;
	public Integer age;
}
```

```java
// 從檔案讀取
MyValue value = mapper.readValue(new File("data.json"), MyValue.class);
// 從網路讀取
MyValue value = mapper.readValue(new URL("http://some.com/api/entry.json"), MyValue.class);
// 從字串讀取
MyValue value = mapper.readValue("{\"name\":\"Bob\", \"age\":13}", MyValue.class);
```

## Serialize序列化 \(POJOs to json\)

```java
MyValue myResultObject = new MyValue();
myResultObject.name = "Batty";
myResultObject.age = 25;

// 寫到檔案
mapper.writeValue(new File("result.json"), myResultObject);
// 變成 byte array
byte[] jsonBytes = mapper.writeValueAsBytes(myResultObject);
// 變成字串
String jsonString = mapper.writeValueAsString(myResultObject);
```

## Generic collections

```java
String jsonSource = "{\"Andy\":89, \"Batty\":99}";

// 直接傳回Map object
Map<String, Integer> scoreByName = mapper.readValue(jsonSource, Map.class);
scoreByName.forEach((k, v) -> log.info("{}:{}", k, v));

// 直接傳回List object
jsonSource = "[\"Andy\", \"Batty\"]";
List<String> names = mapper.readValue(jsonSource, List.class);
names.forEach(x -> log.info("{}", x.toString()));
		
// 原本接到什麼就寫入什麼
mapper.writeValue(new File("names.json"), names);
```

{% code-tabs %}
{% code-tabs-item title="names.json" %}
```text
["Andy","Batty"]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

