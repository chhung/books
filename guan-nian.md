# 觀念

## \[Access Right\] Modifier

interface不能用final  
enum不能用abstract class只能用public, abstract, final, default來修飾  
protected: 在不同的package, 需要繼承才能存取  
default: 只要在同一個package, 就可以存取

## \[interface\]

interface裡面宣告的方法隱含public修飾，所以即便不加，也是代表public，因此, 在實作此interface的類別，該方法也必需主動加上public修飾。  
interface也可以含有資料成員，不過這些成員都會自動被加入final static修飾。

## OCA

* 物件可以是實體或是抽象的，如「提款機」是實體的，「付款」則比較抽象。
* 物件導向分析思考
  1. 訪談，記錄需求項目；同時建立user case flow。
  2. 開始分類，定義有哪些類別。
  3. 把類別所需的屬性及行為定義出來，同時若屬性有關聯到其他類別則在該屬性名稱前加上星號\(\*\)。
  4. 用UML來塑模類別之間的關係
* 程式執行時，每個產生的物件都會在記憶體裡佔據一塊空間，具有獨立記憶體位址，稱為「實例\(instance\)」。
* 方法宣告的修飾詞如public、static是形容詞，其順序先後不影響方法的意義。
* final用在class，表示此class無法被繼承\(inhertance\)；用在方法，表示無法被覆寫\(override\)；用在屬性，表示無法被更改值。
* 實例變數（又稱類別屬性或成員），若在使用前沒有給初值，則java會自行給預設值。
* 區域變數（方法內的），若在使用前沒有給初值，則無法通過編譯。
* 「宣告型別（reference type）」與所參照的「物件型態（object type）」實際上並沒有一定要相同的限制。
* JVM記憶體分類
  * Global
    * static variable
  * Stack
    * Primitive type variable
    * Primitive value
    * Reference type variable
  * Heap
    * Reference type instance
* 記住，String字串是immutable，所以只要對String字串做變動，就會在String Pool產生新的字串物件；而StringBuffer and StringBuilder則不會，因為是mutable。
* 陣列「array」是一種「容器物件\(container object\)」，可以裝載「多個」且「單一型態」的「基本型別/參考型別」。
* 基本上，基本型別/參考型別宣告的變數，都是放在stack記憶體裡。而基本型別的值也是放在stack，參考型別產生的物件實體\(instance\)則放在heap。
* ArrayList類別只存放參考型別的物件，不接受基本型別； 但可以改用基本型別的包覆類別。
* 用static修飾的的方法稱為類別方法，而變數則稱為類別變數，在記憶體中只會有一份。
* 類別方法內的使用的變數只能是該方法所傳入的參數或是方法內宣告的變數及類別變數/方法。

## JAVA字串剖析

在Java中為了效率考量，以""包括的字串，只要內容相同（序㓚、大小寫相同），無論在程式碼中出現幾次，JVM都只會建立一個String實例，並在字串池（String pool）中維護。 PS. 前提是不能使用到new關鍵字，一但用了new，即便內容一樣，但是就是有另一個獨立的實例在記憶體中。

所以要比較字串實際字元內容是否相同時，要使用equals\(\)，記住==用在物件就只是比較是否參考到同一物件。

用+來連接字串會產生新的實例，所以避免在重覆性的迴圈中做字串串接；可使用StringBuilder或StringBuffer來做。 StringBuilder用在單機非多執行緒情況下較佳，因為它不處理同步問題。 StringBuffer會處理同步問題，適用在多執行緒下。

## 基礎語法

byte, char —&gt; 1 byte short —&gt; 2 byte int, float —&gt; 4 byte long, double —&gt; 8 byte boolean —&gt; true ^ false

字面常量從java se 7開始整數或浮點數可以用底線表示   
Ex.  
int a = 113\_567;   
double b = 3.1\_4159\_26;   
int c = 0b1011\_0010\_0011;

對於類別型態物件，使用＝＝比較時，是比較兩個名稱是否參考至同一個物件。

邏輯運算: &&\(AND\), \|\|\(OR\), !\(NOT\)

位元運算： &\(AND\), \|\(OR\), ~\(補數\), ^\(XOR\), &lt;&lt;\(左移\), &gt;&gt;\(右移\), &gt;&gt;&gt;\(右移\)

&lt;&lt;左移位元，右邊補上0  
&gt;&gt;右移位元，左邊原本是1就補1，是0就補0  
&gt;&gt;&gt;右移位元，左邊一律補0

浮點數宣告在字面常量，編譯器預設用double型態；而整數宣告，預設是int型態。

如果運算式中包括不同型態數值，則運算時以長度最長的型態為主，其他數值自動提昇（Promote）型態。 如果運算元都是不大於int的整數，則自動全部提昇為int型態後進行運算。

switch條件式，可用於比對整數，字元、enum及字串。   
Ex.  
switch\(變數或運算式）{   
case 整數、字元、字串、enum:   
          陳述句；   
          Break;  
case 整數、字元、字串、enum:   
           陳述句；   
           Break; ….   
default:   
         陳述句；   
}

## java & javac

執行用java，編譯用javac，兩者在用到其他自訂類別或是jar檔時，都要指定-cp參數，否則若在不同目錄下則會找不到類別；其中jar檔可以看成特別的資料夾。

Javac中有一個參數為-sourcepath，其目的是要指定原始檔來源，可是，我覺得要搭配-cp一起用；因為，如果編譯的檔案有用到自訂類別（但是也是原始檔呈現），那麼沒有指定-cp，則每次編譯時，其他被用到的自訂類別都會重新被編譯一次，但是，有指定-cp，則編譯時會去看看-cp指定的目錄下有沒有該類別檔\(.class），如果用，而他的原始檔又沒有被更改過，就不會再編譯一次，只會針對有更改過的類別原始檔再編譯，而指定編譯的原始檔是無論如何都會被重新編譯。總之，-sourcepath和-cp搭一起用，會省編譯的時間，尤其是專案非常大的時候。

-d參數是指明編譯後的class檔要放的位置 -sourcepath參數是指在編譯時期到某些目錄去找原始檔 -cp參數是指在編譯或執行時間到某些目錄去找類別檔

有一點要特別注意，如果編譯的檔案不在當前目錄下，則需把原始檔的路徑附加在原始檔前面（絕對或相對路徑皆可）；而相同情形換作在執行時，則只需給要執行的類別檔案名稱即可，類別的位罝就交給-cp參數去處理就行了。

如果有相同的類別原始檔案，但是分別在不同的目錄下，而此時main class都呼叫了相同名稱的類別，請問，此時執行起來的話，會是呼叫到哪一個類別？答案是，得看當時編譯時的-sourcepath參數把哪一個目錄先寫在前頭，就會先把那個類別編譯進去，所以執行就會呼叫到那個類別。因此，要避免此情形或是不同coder同時寫了相同的類別名稱會衝突或䨱蓋，需用package來管理檔案目錄。

MacOS的jdk安裝路徑在/Library/Java/JavaVirtualMachines



