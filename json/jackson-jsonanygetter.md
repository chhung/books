# Jackson - @JsonAnyGetter

這個註解是把Map資料型能屬性當成一般的資料型態屬性 舉例來說, json語法為name:value的組合, 而Map也是key:value的組合, 兩者很像 但是, Map是一種集合, 所以不加任何註解在Jackson解析下, 會保留屬性名稱, 而內容值則為Map的所有集合

```java
public class JsonPojo { 
    public String name; private Map properties;
    @JsonAnyGetter(enabled = true)
    public Map getProperties() {
        return properties;
    }

    public void add(String k, String v) {
        if (properties == null)
            properties = new TreeMap();

        properties.put(k, v);
    }
}
```

Test:

```java
public class JsonPojoTest { 
    @Test 
    public void test() { 
        JsonPojo jp = new JsonPojo();
        jp.name = "Betty";
        jp.add("attr1", "val1");
        jp.add("attr2", "val2");

        String result = null;
        try {
            result = new ObjectMapper().writeValueAsString(jp);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }

        assertThat(result, containsString("attr1"));
        assertThat(result, containsString("val1"));
        System.out.println(result);
        jp.getProperties().forEach((k, v) -> {System.out.println("key:" + k + "\tvalue:" + v);});
    }
}
```

有加JsonAnyGetter的輸出

`{"name":"Betty","attr1":"val1","attr2":"val2"}   
key:attr1 value:val1   
key:attr2 value:val2`

沒有加 JsonAnyGetter的輸出

`{"name":"Betty","properties":{"attr1":"val1","attr2":"val2"}}   
key:attr1 value:val1   
key:attr2 value:val2`

