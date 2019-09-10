# json名稱開頭大小寫通吃

json syntax:  
{"name":"Andy"} java class:

```java
public class JsonModel {
    private String name; 
    public void setName(String s) { 
    this.name = s;
    }
    public String getName() {
        return this.name;
    }
}
```

在java裡創建一個class, 用來接上面的json, 使之成為一個object供程式直接來使用, 這個過程稱反序列化 如果現在把上面那段json轉成java object時, 可用下面方式, 不用加任何Jackson annonation

```java
ObjectMapper jsonOmp = new ObjectMapper();
jsonOmp.readValue([json_string_source], JsonModel.class);
```

重點來了, json syntax中的name, 對於有些人來說, name與Name的意思是相同的, 但是這在做反序列化可行不通 若要兩者都可以吃下來, 則在class要多加annonation和method

例如:

```java
public class JsonModel { 
    private String name;
    public void setName(String s) { 
        this.name = s; 
    }
    public String getName() { 
        return this.name; 
    }
} 
```

如果原本就是接受json name屬性是小寫, 突然有一天, 全部都要改大寫的話, 就直接在java class 屬性加上@JsonProperties\("Name"\) 如此一來, 就只能接受開頭大寫

