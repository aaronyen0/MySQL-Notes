# MySQL: DataType

----
資料來源
- [MySQL 所支援的各種欄位型態](https://blog.xuite.net/javax/programmer/11946942-MySQL+%E6%89%80%E6%94%AF%E6%8F%B4%E7%9A%84%E5%90%84%E7%A8%AE%E6%AC%84%E4%BD%8D%E5%9E%8B%E6%85%8B)

- [MySQL數據庫中CHAR與VARCHAR之爭](http://inspirr.pixnet.net/blog/post/70870607-mysql%E6%95%B8%E6%93%9A%E5%BA%AB%E4%B8%ADchar%E8%88%87varchar%E4%B9%8B%E7%88%AD)
----
## **數字型態**

|型態|空間需求|範圍|
|--|--|--|
|TINYINT(M)|1 byte|signed: (-128 to 127) <br> unsigned: (0 to 255)|
|SMALLINT(M)|2 bytes|signed: (-2<sup>15</sup> to 2<sup>15</sup>-1) <br> unsigned: (0 to 2<sup>16</sup>-1)|
|MEDIUMINT(M)|3 bytes|signed: (-2<sup>23</sup> to 2<sup>23</sup>-1) <br> unsigned: 0 to 2<sup>24</sup>-1|
|INT(M)<br>INTEGER(M)|4 bytes|signed: -2<sup>31</sup> to 2<sup>31</sup>-1 <br> unsigned: 0 to 2<sup>32</sup>-1|
|BIGINT(M)|8 bytes|signed: -2<sup>63</sup> to 2<sup>63</sup>-1 <br> unsigned: 0 to 2<sup>64</sup>-1|
|FLOAT(precision)|4 or 8|precision &le; 24: FLOAT(單精度)<br>25 &le; precision &le; 53: DOUBLE(倍精度)|
|FLOAT(M,D)|4 bytes|[單精度浮點數範圍](https://zh.wikipedia.org/wiki/%E5%96%AE%E7%B2%BE%E5%BA%A6%E6%B5%AE%E9%BB%9E%E6%95%B8)|
|DOUBLE(M,D)<br>REAL(M,D)|8 bytes|[倍精度浮點數範圍](https://zh.wikipedia.org/wiki/%E9%9B%99%E7%B2%BE%E5%BA%A6%E6%B5%AE%E9%BB%9E%E6%95%B8)|
|DECIMAL(M[,D])<br> DEC(M[,D])<br> NUMERIC(M[,D])|M + 2|依 M 與 D 的值決定|

- **M 代表「最大顯示寬度」，其值不得大於 255。** 無論將欄位的型態設定為「INT(4)」或「INT(5)」，都不影響它儲存數值的能力；但在顯示時，就可以發現其差異了，令以下兩個欄位存入數字9，ZEROFILL表空白處補0：
    - INT(4) ZEROFILL = 0009
    - INT(5) ZEROFILL = 00009
    
-  **D 代表「小數位數」，其值不得大於 30，也不能大於 M-2。**
    -  FLOAT(5, 3)

- 如果將欄位型態設定為 UNSIGNED 的話，對整數型態而言，可以儲存較大的正整數；對浮點數型態而言，則可以避免被存入負的數值。

- 若存入的數值超過欄位的範圍時，MySQL 只會取其所能處理的最大值。例如：
    - TINYINT 中存入 300 = 127


## **日期與時間型態**

|型態|空間需求|範圍|
|--|--|--|
|DATE|3 bytes|'1000-01-01' to '9999-12-31'|
|TIME|3 bytes|'-838:59:59' to '838:59:59'|
|DATETIME|8 bytes|'1000-01-01 00:00:00' to '9999-12-31 23:59:59'|
|TIMESTAMP(M)|4 bytes|自 1970 年起，至 2037 年的某時|
|YEAR(2 or 4)|1 byte|2-digit format: 1970 to 2069<br>4-digit format: 1901 to 2155|

- TIMESTAMP(M) 可以指定顯示長度如下，但不影響儲存的記憶體空間：

    |型態|顯示格式|
    |--|--|
    |TIMESTAMP(14)|YYYYMMDDhhmmss|
    |TIMESTAMP(12)|YYMMDDhhmmss|
    |TIMESTAMP(10)|YYMMDDhhmm|
    |**TIMESTAMP(8)**|**YYYYMMDD**|
    |TIMESTAMP(6)|YYMMDD|
    |TIMESTAMP(4)|YYMM|
    |TIMESTAMP(2)|YY|

## **字串型態**

|型態|空間需求|範圍|
|--|--|--|
|CHAR(M)|M bytes|M bytes|
|VARCHAR(M)|L+1 bytes|M bytes|
|TINYBLOB, TINYTEXT	|L+1 bytes|2<sup>8</sup>-1 bytes|
|BLOB, TEXT	|L+2 bytes|2<sup>16</sup>-1 bytes|
|MEDIUMBLOB, MEDIUMTEXT|L+3 bytes|2<sup>24</sup>-1 bytes|
|LONGBLOB, LONGTEXT	|L+4 bytes|2<sup>32</sup>-1 bytes|
|ENUM('value1','value2',...)|1 or 2 bytes|65535 個成員|
|SET('value1','value2',...)	|1, 2, 3, 4, or 8 bytes|64 個成員|

- **CHAR 與 VARCHAR**

    - CHAR 及 VARCHAR 型態時，必須宣告「最大儲存長度」，就是上表中的 M。M的最大長度都是 255 bytes。(MySQL v5.0 以上變成65535)
    
    - 兩者異之處在於 **CHAR 是固定長度的型態，而 VARCHAR 是長度可變的型態**。如下表：

        |字串內容    |CHAR(4)                  |空間需求|VARCHAR(4)|空間需求 |
        |----------|--------------------------|-------|----------|-------|
        |''        |'&nbsp;&nbsp;&nbsp;&nbsp;'|4 bytes|''        |1 byte |
        |'ab'      |'ab&nbsp;&nbsp;'          |4 bytes|'ab'      |3 bytes|
        |'abcd'    |'abcd'                    |4 bytes|'abcd'    |5 bytes|
        |'abcdefgh'|'abcd'                    |4 bytes|'abcd'    |5 bytes|


    - 使用 CHAR 型態儲存資料時，MySQL 會以空白字元填補至最大儲存長度

    - 當資料內容少於最大儲存長度時，VARCHAR 將可以有效地節省空間，這是它最大的優點；檢索時擁有較佳的效率，則是 CHAR 的長處。

    - VARCHAR 和各類 TEXT 與 BLOB 都是長度可變的型態。當一資料表中同時選用 CHAR 和這類長度可變的型態時，CHAR 會被自動改為 VARCHAR，除非它的最大儲存長度少於 4。
    
    - VARCHAR 欄位的最大儲存長度少於 4 時，其型態也會被自動改為 CHAR。

- **TEXT 與 BLOB**

    - BLOB 的全名是「binary large object」，與 TEXT 一樣，都是用來儲存長度較長的字串或是二元資料。兩者大同小異，唯一的差別在於，**TEXT 是有區分大小寫的，而 BLOB 不分**。
    
    
- **ENUM 與 SET**

    - **ENUM 與 SET 是特別的字串型態，這兩種欄位的值只能從固定的項目中挑選。** 下面設定使用者的性別與年齡，若輸入「男性，32 歲」，則分別以 sex 的第 1 個選項與 age 的第 3 個選項存入。：
    
        - sex ENUM("M", "F")
        
        - age ENUM("0-20", "21-30", "31-40", "41-50", "51-100")
    
    - **ENUM 型態最多可以建立 65535 個不同的成員，而 SET 只能建立 64 個不同的成員。** 
    
    - 不過SET 型態可以從中挑選一組以上的值，而 ENUM 只能選其一。**也就是ENUM 型態是單選，而 SET 型態則是複選。**
        - subject SET("Chinese", "English", "Math", "Music", "Art", "Sport")


## **補充**

- 注意建表的編碼規則：1個英文字是佔1byte，1個中文字佔2bytes。 若用**UTF8**或**BIG5**來建立B，那麼VARCHAR中的每個字元，就可以用中文字來表示；若以歐美拉丁語系建立的，那MySQL就會將每個中文字看成兩個英文語系的字元。


- SET資料儲存的樣式

    |SET成員  |十進位表達     |二進位表達       |
    |--------|--------------|---------------|
    |'white' |1             |00000...0000001|
    |'yellow'|2             |00000...0000010|
    |'green' |4             |00000...0000100|
    |'black' |2<sup>63</sup>|10000...0000000|
    
    - 由於每個選項獨自占用一個bit，因此可以透過bit的0/1切換複選多個選項。

    - 例如字段上存入5，我們知道5 = 1 + 4，也就是同時選擇了white和green。


- **ENUM內的成員是從1開始編號，所以上限為65535個**。(0被MySQL用作錯誤成員，如果以字串的形式表示就是空字串: '' )


- **正確的選擇 CHAR 或 VARCHAR**

    VARCHAR往往用來保存可變長度的字串。簡單的說，我們只是給其固定了一個最大值，然后系統會根據實際存儲的數據量來分配合適的存儲空間。

    由於長度是可變的，例如：VARCHAR(50)的欄位，更改前長度是10位，此時系統就分配10個存儲的位置。更改後數據量達到了20位。還沒有超過最大50位的限制，為此數據庫還是允許其存儲的。但原先的存儲位置已經無法滿足其存儲的需求。此時系統就需要進行額外的操作。如根據存儲引擎不同，有的會采用**拆分機制**，而有的則會采用**分頁機制**。(沒特別查過，從名字來看都像是linkedlist的應用)
    
    - VARCHAR 欄位是將實際內容單獨儲存在聚簇索引之外，內容開頭用1到2個位元組表示實際長度(長度超過255時需要2個位元組)，因此最大長度不能超過65535。 

    - 如果某個字段其長度雖然比較長，但是其長度總是近似的，如一般在90個到100個字符之間，甚至是相同的長度。此時比較適合采用CHAR字符類型。比較典型的應用就是MD5哈希值。

    - **使用VARCHAR設定的最大長度不能太過慷慨。** 對VARCHAR(200)數據類型來說，硬盤上的存儲空間雖然是根據實際字符長度(例如50)來分配的，但是對于內存來說則不是。其使用固定大小的內存塊來保存值。也就是使用字符類型中定義的長度──200個字符空間。
