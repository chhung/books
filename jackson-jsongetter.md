# Jackson – @JsonGetter

@JsonGetter annonation, 用來表示一個json pojo model的某個屬性getter method是哪一個mothod

亦即, jackson解析時, 預設會先去找setter/getter method, 即便我們不需要給任何的annonation

意思是, 只要是get/set開頭的method, jackson都會掃描, 而預設屬性名稱為去掉get/set後, 第一個字母變小寫

ex.

private String Name;

public String getName\(\) {return Name;}  –&gt;屬性名稱為name

public String getAbcd\(\) {return Name;}  –&gt;屬性名稱為abcd

而且這兩個屬性都會被輸出

回到JsonGetter上面來, jackson都可以自己解析了, 何需用此註解

有一種情況會需要, 就是取得屬性的方法名稱不會命名為getter方式, 則用此註解來告訴Jackson, 這個方法需要輸出成json的一部份

ex.

public String printName\(\) {return Name;} –&gt;; Jackson parser時不會理會這個方法

而加上註解後

@JsonGetter\(“name"\)

public String printName\(\) {return Name;} –&gt;;json syntax is {“name" : xxxx}

@JsonGetter\(\)

public String printName\(\) {return Name;} –&gt;;json syntax is {“printName" : xxxx}

