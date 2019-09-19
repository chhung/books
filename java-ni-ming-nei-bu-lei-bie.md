# Java匿名內部類別

在JAVA 8有新的語言特性出現，原本在interface都只限定宣告方法，不能有具體的實作。 但是現在已經可以在interfcace中實作方法，分別是static method與default method。

```java
public interface NewFeature {
    static void produce() {
        System.out.println("Vehicles");
    }
 
    public default void linux() {
        System.out.println("LINUX");
    }
 
    void botherm();
}
```

```java
public class App {
	public static void main(String[] args) {
		NewFeature.produce(); //直接使用 interface 的 static method
		new NewFeature() { //匿名內部類別寫法
			@Override
			public void linux() {
				//覆寫 interface 的 default method, 此時不需再加上 default
				//因為只有一般類別才能建立實體, 因此一般類繼承了 interface 時, 覆寫 default method
				// 也不用加上 default, 反之界面繼承界面若要覆寫 default method時, 必需要加上 default
				System.out.println("new linux");
			}

		@Override
			public void botherm() {} //直接在匿名類別裡實作界面方法
		}.linux();

		NewFeature intfaceInstance = new NewFeature() { 
			//匿名內部類別寫法
			@Override
			public void botherm() {
				System.out.println("匿名內部類別");
			}};

		intfaceInstance.linux();
		intfaceInstance.botherm();

		// interface只有一個方法宣告時, 可以直接用Lambda語法來實作
		NewFeature nf = () -> { System.out.println("Lambda implement"); };
		nf.botherm();
	}
}
```

## Inner static class

```java
class Sauvage {
	int age;
	Sauvage.Ville ville;
	
	public static class Ville {
		String name;
		double price;
		static int total;	// 就算是在 inner class裡，這個變數也只會有一份
	}
}

public class App {
	public static void main(String[] args) {
		Sauvage sau = new Sauvage();
		sau.ville = new Sauvage.Ville();
		sau.ville.name = "money";
		sau.ville.price = 458.236;
		sau.ville.total = 954;
		
		Sauvage sau2 = new Sauvage();
		sau2.ville = new Sauvage.Ville();
		
		System.out.println("name:" + sau2.ville.name);
		System.out.println("price:" + sau2.ville.price);
		System.out.println("total:" + sau2.ville.total);
	}
}
```

{% code-tabs %}
{% code-tabs-item title="console output:" %}
```text
name:null
price:0.0
total:954
```
{% endcode-tabs-item %}
{% endcode-tabs %}



