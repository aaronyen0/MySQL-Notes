# MySQL: Syntax

---
參考資料
- [MySQL 語法匯整](http://note.drx.tw/2012/12/mysql-syntax.html)
- [SQL Teaching](https://www.sqlteaching.com/#!select)
---

## **目錄**
- [01. SELECT* ](#01): 列出整個表格
- [02. SELECT specific columns ](#02): 列出表格的特定欄
- [03. WHERE ... = ](#03): 只列出該欄滿足特定值的數據
- [04. WHERE ... > ](#04): 
- [05. WHERE ... >= ](#05): 
- [06. AND](#06): 
- [07. OR](#07): 
- [08. IN](#08): 
- [09. DISTINCT](#09): 
- [10. ORDER BY](#10): 
- [11. LIMIT # of returned rows](#11): 
- [12. ](#12): 
- [13. ](#13): 
- [14. ](#14): 
- [15. ](#15): 
- [16. ](#16): 
- [17. ](#17): 
- [18. ](#18): 
- [19. ](#19): 

## **SQL Teaching**

<h3 id="01">01. SELECT * </h3>

```SQL
/** 
 * Assuning we have a table called family_members that is shown below.
 * The * means that all of the columns will be returned.
 */

SELECT * FROM family_members;
```


<h3 id="02">02. SELECT specific columns </h3>

```SQL
/* If we only want to see the name and num_books_read, we would type */

SELECT name, num_books_read FROM family_members;
```


<h3 id="03">03. WHERE ... = </h3>

```SQL
/** 
 * In order to select particular rows from this table, 
 * we use the WHERE keyword. 
 */

SELECT * FROM family_members WHERE species = 'human';
```


<h3 id="04">04. WHERE ... > </h3>

```SQL
/** 
 * If we want to select family members with a num_books_read, 
 * we would type, we would type"
 */

SELECT * FROM family_members WHERE num_books_read > 99;
```


<h3 id="05">05. WHERE ... >= </h3>

```SQL
/** 
 * SQL accepts various inequality symbols, including: 
 * =  "equal to"
 * >  "greater than"
 * <  "less than"
 * >= "greater than or equal to"
 * <= "less than or equal to"
 */

SELECT * FROM family_members WHERE num_books_read >= 99;
```


<h3 id="06">06. AND </h3>

```SQL
/** 
 * In the WHERE part of a query, 
 * we can search for multiple attributes by using the AND keyword.
 */

SELECT * FROM friends_of_pickles WHERE height_cm > 25 AND species = 'cat';
```


<h3 id="07">07. OR </h3>

```SQL
/** 
 * Search for rows that match any of multiple attributes 
 * by using the OR keyword.
 */

SELECT * FROM friends_of_pickles WHERE height_cm > 25 OR species = 'cat';
```


<h3 id="08">08. IN </h3>

```SQL
/** 
 * Using the WHERE clause, 
 * we can find rows where a value is in a list of several possible values. 
 * Following would return the friends_of_pickles that are either a cat or a human.
 * 
 * To find rows that are not in a list, you use NOT IN instead of IN. 
 */

SELECT * FROM friends_of_pickles WHERE species IN ('cat', 'human');
```

<h3 id="09">09. DISTINCT </h3>

```SQL
/** 
 * By putting DISTINCT after SELECT, you do not return duplicates. 
 * Even though there are multiple row satisfies the conditions.
 *  
 * For example, if you run :
 * SELECT DISTINCT gender, species FROM friends_of_pickles WHERE height_cm < 100;
 * , you will get the gender/species combinations of the animals less than 100cm in height. 
 */

SELECT DISTINCT name, species FROM friends_of_pickles WHERE height_cm > 50;
```
![](https://i.imgur.com/8mSqYdM.png)

![](https://i.imgur.com/VtWiOft.png)


<h3 id="10">10. ORDER BY </h3>

```SQL
/** 
 * If you want to sort the rows by some kind of attribute, 
 * we can use the ORDER BY keyword.
 * 
 * In order to put the names in descending order, 
 * we would add a DESC at the end of the query.
 */

SELECT * FROM friends_of_pickles ORDER BY name;
SELECT * FROM friends_of_pickles ORDER BY height_cm DESC;
```


<h3 id="11">11. LIMIT # of returned rows </h3>

```SQL
/** 
 * Often, tables contain millions of rows, 
 * and it can take a while to grab everything. 
 * If we just want to see a few examples of the data in a table, 
 * we can select the first few rows with the LIMIT keyword. 
 * If you use ORDER BY, you would get the first rows for that order.
 * 
 * Note: 
 * - Some variants of SQL do not use the LIMIT keyword.
 * - The LIMIT keyword comes after the DESC keyword.
 */

SELECT * FROM friends_of_pickles ORDER BY height_cm LIMIT 2;
SELECT * FROM friends_of_pickles ORDER BY height_cm DESC LIMIT 1;
```


<h3 id="12">12.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="13">13.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="14">14.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="15">15.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="16">16.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="17">17.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="18">18.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="19">19.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="20">20.  </h3>

```SQL
/** 
 * 
 */


```
