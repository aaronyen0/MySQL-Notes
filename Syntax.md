# MySQL: Syntax

---
參考資料
- [MySQL 語法匯整](http://note.drx.tw/2012/12/mysql-syntax.html)
- [SQL Teaching](https://www.sqlteaching.com/#!select)
---

## **目錄**
- [01. SELECT * ](#01): 列出整個表格
- [02. SELECT specific columns ](#02): 列出表格的特定欄
- [03. WHERE ... = ](#03): 只列出該欄滿足特定值的數據
- [04. WHERE ... > ](#04): 
- [05. WHERE ... >= ](#05): 
- [06. AND ](#06): 
- [07. OR ](#07): 
- [08. IN ](#08): 
- [09. DISTINCT ](#09): 
- [10. ORDER BY ](#10): 
- [11. LIMIT # of returned rows ](#11): 
- [12. COUNT(*) ](#12): 
- [13. COUNT(*) ... WHERE ](#13): 
- [14. SUM ](#14): 
- [15. AVG ](#15): 
- [16. MAX and MIN ](#16): 
- [17. GROUP BY ](#17): 
- [18. Nested queries ](#18): 
- [19. NULL ](#19): 
- [20. Date ](#20): 
- [21. Date ](#21): 
- [22. Date ](#22): 
- [23. Date ](#23): 
- [24. Date ](#24): 
- [25. Date ](#25): 
- [26. Date ](#26): 
- [27. Date ](#27): 
- [28. Date ](#28): 
- [29. Date ](#29): 

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
 * SELECT DISTINCT gender, species FROM friends_of_pickles WHERE height_cm < 100;,
 * you will get the gender/species combinations of the animals less than 100cm in height.
 * 
 * Compare two statements below:
 */

SELECT DISTINCT species FROM friends_of_pickles WHERE height_cm > 50;

SELECT DISTINCT name, species FROM friends_of_pickles WHERE height_cm > 50;
```

![](https://i.imgur.com/WyZjRrz.png)

![](https://i.imgur.com/OnOs0jt.png)


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


<h3 id="12">12. COUNT(*) </h3>

```SQL
/** 
 * Another way to explore a table is to check the number of rows in it. 
 * For example, if we are querying a table states_of_us, 
 * we'd expect 50 rows, or 500 rows in a table called fortune_500_companies.
 */

SELECT COUNT(*) FROM friends_of_pickles;
```


<h3 id="13">13. COUNT(*) ... WHERE </h3>

```SQL
/** 
 * We can combine COUNT(*) with WHERE to return the number of rows that matches the WHERE clause.
 */

SELECT COUNT(*) FROM friends_of_pickles WHERE species = 'dog';
```

![](https://i.imgur.com/m7SnLBZ.png)


<h3 id="14">14. SUM </h3>

```SQL
/** 
 * We can use the SUM keyword in order to find the sum of a given value. 
 */

SELECT SUM(num_books_read) FROM family_members;
```

![](https://i.imgur.com/XU1ghPf.png)


<h3 id="15">15. AVG </h3>

```SQL
/** 
 * We can use the AVG keyword in order to find the average of a given value.
 * 
 * Note: 
 * Because of the way computers handle numbers, 
 * averages will not always be completely exact.
 */

SELECT AVG(num_books_read ) FROM family_members;
```

![](https://i.imgur.com/fIPjPnk.png)



<h3 id="16">16. MAX and MIN </h3>

```SQL
/** 
 * We can use the MAX and MIN to find the maximum or minimum value of a table.
 */

SELECT MAX(num_books_read ) FROM family_members; 
```

![](https://i.imgur.com/yW8dsNS.png)



<h3 id="17">17. GROUP BY </h3>

```SQL
/** 
 * You can use aggregate functions such as COUNT, SUM, AVG, 
 * MAX, and MIN with the GROUP BY clause. 
 * 
 * When you GROUP BY something, 
 * you split the table into different piles based on the value of each row.
 * 
 * 簡單來說，先依據指定的欄將變數分group後，再將滿足條件的選擇列出來，
 * 而且每個group只會產出一個結果
 */

SELECT MAX(height_cm), species  FROM friends_of_pickles GROUP BY species;

SELECT COUNT(*), species FROM friends_of_pickles GROUP BY species;

SELECT MAX(height_cm), MIN(height_cm), species  FROM friends_of_pickles GROUP BY species;
```

![](https://i.imgur.com/07VaQd1.png)

![](https://i.imgur.com/FEcepGM.png)

![](https://i.imgur.com/DlIOuBy.png)

```SQL
/** 
 * 值得注意的是，即使條件相衝突，還是可以group by出一個表格
 * 要小心這張表格的同一列，跟原始表格的同一列已經是不同概念了
 * 
 * 承上例：
 */

SELECT name, gender, MAX(height_cm), MIN(height_cm), species  FROM friends_of_pickles GROUP BY species;
```

![](https://i.imgur.com/o4EclGD.png)


<h3 id="18">18. Nested queries </h3>

```SQL
/** 
 * In SQL, you can put a SQL query inside another SQL query.
 */

SELECT * FROM family_members WHERE num_legs = (SELECT MIN(num_legs) FROM family_members);

/**
 * The SELECT query inside the parentheses is executed first, 
 * and returns the minimum number of legs. 
 * Then, that value (2) is used in the outside query, 
 * to find all family members that have 2 legs.
 */
 
SELECT * FROM family_members WHERE num_books_read = (SELECT MAX(num_books_read) FROM family_members); 
```

![](https://i.imgur.com/vakVsYI.png)


<h3 id="19">19. NULL </h3>

```SQL
/** 
 * In order to find the rows where the value for a column is or is not NULL, 
 * you would use IS NULL or IS NOT NULL.
 */

SELECT * FROM family_members WHERE favorite_book IS NOT NULL;
```

![](https://i.imgur.com/zAdZI15.png)


<h3 id="20">20. Date </h3>

```SQL
/** 
 * Sometimes, a column can contain a date value. 
 * The first 4 digits represents the year, 
 * the next 2 digits represents the month, 
 * and the next 2 digits represents the day of the month. 
 * 
 * You can compare dates by using < and >.
 */

SELECT * FROM celebs_born WHERE birthdate > '1980-09-01';
```

![](https://i.imgur.com/9sWbDwV.png)


<h3 id="21">21. INNER JOIN ... ON </h3>

```SQL
/** 
 * Different parts of information can be stored in different tables, 
 * and in order to put them together, we use INNER JOIN ... ON. 
 * Joining tables gets to the core of SQL functionality, 
 * but it can get very complicated.
 * 
 * As we can see below, there are 3 tables:
 * 1. character: Each character is a row and is represented by a unique identifier (id), e.g. 1 is Doogie Howser
 * 2. character_tv_show: For each character, which show is he/she in
 * 3. character_actor: For each character, who is the actor?
 * 
 * See that in character_tv_show, instead of storing both the character 
 * and TV show names (e.g. Willow Rosenberg and Buffy the Vampire Slayer), 
 * it stores the character_id as a substitute for the character name. 
 * This character_id refers to the matching id row from the character table. 
 * 
 * SELECT col_name(s)
 * FROM table1
 * INNER JOIN table2
 * ON table1.col_name=table2.col_name;
 */

SELECT character.name, character_actor.actor_name
FROM character 
INNER JOIN character_actor
ON character.id= character_actor.character_id;
```

![](https://i.imgur.com/2ZU1Dp7.png)


```SQL
/*承上*/

SELECT character.id, character.name, character_actor.actor_name
FROM character_actor
INNER JOIN character
ON character.id= character_actor.character_id;
```

![](https://i.imgur.com/szJ2na5.png)



<h3 id="22">22. Multiple joins </h3>

```SQL
/** 
 * 
 */


```


<h3 id="23">23.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="24">24.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="25">25.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="26">26.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="27">27.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="28">28.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="29">29.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="30">30.  </h3>

```SQL
/** 
 * 
 */


```


<h3 id="31">31.  </h3>

```SQL
/** 
 * 
 */


```
