# Step One

Maven

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

Jackson有三個JAR檔\(core/annotations/databind\)，其中只要引入databind，其他兩個就會一併被引入，因為databind相依其他兩個library。



