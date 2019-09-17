# SimpleDate

給字串轉成date物件\(要先設定好接收的時間字串的格式\)

```java
// format likes this 8/12/2010 9:32:33 PM
String pattern = "M/d/yyyy h:m:s a"; 

// Locale.{} will change the display am/pm by localization
SimpleDateFormat strDateTime = new SimpleDateFormat(pattern, Locale.ENGLISH); 
System.out.println(strDateTime.format(new Date()));

try {
    Date dt = strDateTime.parse("8/12/2010 9:32:33 PM");
    System.out.println(strDateTime.format(dt));
} catch (ParseException e) {
    e.printStackTrace();
}
```

