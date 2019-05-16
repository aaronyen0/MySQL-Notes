# MySQL: Syntax

---
參考資料
- [MySQL 語法匯整](http://note.drx.tw/2012/12/mysql-syntax.html)
- [SQL Teaching](https://www.sqlteaching.com/#!select)
- [服务器脚本教程](http://www.w3school.com.cn/sql/sql_join_inner.asp)
- [Most Used MySQL Commands](http://yuqiaoshen.com/drupal/?q=node/3)
---

## **目錄**

- **DATABASE**
    - [01. CREATE DATABASE \`db_name\`](#DB01):
    - [02. SHOW DATABASES](#DB02):
    - [03. DROP \`db_name\`](#DB03):
    - [04. USE \`db_name\`](#DB04):
    - [05. MYSQLDUMP -u ... -p ... -h ... -P ... --databases ...](#DB05):

- **TABLE**
    - [01. CREATE TABLE ...](#T01)
    - [02. SHOW TABLES FROM \`db_name\`](#T02)
    - [03. DESCRIBE \`tbl_name\`](#T03)
    - [04. ALTER TABLE \`tbl_name\` ADD ...](#T04)
    - [05. ALTER TABLE \`tbl_name\` ALTER COLUMN ...](#T05)
    - [06. ALTER TABLE \`tbl_name\` DROP COLUMN ...](#T06)
    - [07. TRUNCATE TABLE ...](#T07)

- **DATA SELECT**
    - [01. SELECT * FROM \`tbl_name\`](#01): 列出整個表格
    - [02. SELECT \`col_name1\`, \`col_name2\`, ... FROM \`tbl_name\`](#02): 列出表格的特定欄
    - [03. WHERE ... = ](#03): 只列出該欄滿足特定值的數據
    - [04. WHERE ... > ](#04): 
    - [05. WHERE ... >= ](#05): 
    - [06. AND ](#06): 
    - [07. OR ](#07): 
    - [08. IN ](#08): 判別變數有沒有在該欄位內，變數可以多個 (val1,val2,...)
    - [09. DISTINCT ](#09): 抓取有哪些不同的值，而每個值出現的次數並不重要
    - [10. ORDER BY ](#10): 
    - [11. LIMIT # of returned rows ](#11): 
    - [12. COUNT(*) ](#12): 
    - [13. COUNT(*) ... WHERE ](#13): 
    - [14. SUM ](#14): 
    - [15. AVG ](#15): 
    - [16. MAX and MIN ](#16): 
    - [17. GROUP BY ](#17): 需要分group做一些統計，例如sum, avg, max, min, ...
    - [18. Nested queries ](#18): 巢狀結構用小括號將其他敘述整個包起來
    - [19. NULL ](#19): IS NULL, IS NOT NULL
    - [20. Date ](#20): 日期，注意日期格式可以比大小
    - [21. INNER JOIN ... ON ](#21): 雙方都有的欄位才合併
    - [22. Multiple joins ](#22): 
    - [23. Joins with WHERE ](#23): 
    - [24. LEFT JOIN ... ON ](#24): merge進前面的table內，若有值merge不了則填NULL
    - [25. Table alias ](#25): 
    - [26. Column alias ](#26): 
    - [27. Self joins ](#27): 自己join自己，需要用alias區隔同一個變數才辦的到
    - [28. LIKE ](#28): 搜尋
    - [29. CASE ](#29): switch判斷
    - [30. SUBSTR ](#30): 
    - [31. COALESCE ](#31): 順序比對，先被比到的就輸出


## **DATA SELECT**

<h3 id="01">01. SELECT * FROM `tbl_name` </h3>

```SQL
/** 
 * Assuning we have a table called family_members that is shown below.
 * The * means that all of the columns will be returned.
 */

SELECT * FROM family_members;
```


<h3 id="02">02. SELECT `col_name1`, `col_name2`, ...  FROM `tbl_name` </h3>

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

SELECT * FROM friends_of_pickles WHERE species NOT IN ('cat', 'human');
```

![](https://i.imgur.com/jKPX7Yg.png)


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
```

![](https://i.imgur.com/WyZjRrz.png)

```SQL
/**
 * 承上，注意DISTINCT會對各種col組合中都取一組出來
 */

SELECT DISTINCT name, species FROM friends_of_pickles WHERE height_cm > 50;
```
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
```
![](https://i.imgur.com/07VaQd1.png)

```SQL
/*承上*/
SELECT COUNT(*), species FROM friends_of_pickles GROUP BY species;
```
![](https://i.imgur.com/FEcepGM.png)

```SQL
/** 
 * 承上，值得注意的是，即使條件相衝突，還是可以group by出一個表格
 * 要小心group by後表格的同一列，跟原始表格的同一列是不同概念
 * 事實上group by輸出標籤資料本身就有問題，通常都用在統計資料上
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

SELECT character.name, 
       character_actor.actor_name
FROM character 
    INNER JOIN character_actor
        ON character.id= character_actor.character_id;
```

![](https://i.imgur.com/2ZU1Dp7.png)


```SQL
/*承上*/

SELECT character.id, 
       character.name, 
       character_actor.actor_name
FROM character_actor
    INNER JOIN character
        ON character.id = character_actor.character_id;
```

![](https://i.imgur.com/szJ2na5.png)


<h3 id="22">22. Multiple joins </h3>

```SQL
/**
 * 我想輸出一張表如右：(character.name, actor.name)
 * 其中兩者的關聯是透過character_actor:(id, character_id, actor_id)表聯繫的
 * step1: 將character_actor依照character_id併入表character，
 * 此時表內便有了name及actor_id
 * step2: 將actor透過actor_id併入表character，則整張表就完整了
 */

SELECT character.name,  
       actor.name
FROM character 
    INNER JOIN character_actor 
        ON character.id = character_actor.character_id
    INNER JOIN actor
        ON character_actor.actor_id= actor.id;
        
/**
 * 也可以改寫如下，不過要注意，最終輸出表的順序是依照FROM後面的表的順序，
 * 下例也有同樣的輸出順序是因為character的順序和character_actor是相同的
 */

SELECT character.name,
       actor.name
FROM character_actor
    INNER JOIN character
        ON character.id = character_actor.character_id
    INNER JOIN actor
        ON character_actor.actor_id= actor.id;
```

![](https://i.imgur.com/I4H1alS.png)


<h3 id="23">23. Joins with WHERE </h3>

```SQL
/** 
 * We can also use joins with the WHERE clause.
 */
 
SELECT character.name, 
       tv_show.name
FROM character_tv_show
    INNER JOIN character
        ON character.id = character_tv_show.character_id
    INNER JOIN tv_show
        ON tv_show.id = character_tv_show.tv_show_id
WHERE character.name != 'Willow Rosenberg' and tv_show.name != 'How I Met Your Mother'
```

![](https://i.imgur.com/bXbpMC0.png)


<h3 id="24">24. LEFT JOIN ... ON </h3>

```SQL
/** 
 * In the previous exercise, we used joins to match up TV character names with their actors. 
 * When you use INNER JOIN, that is called an "inner join" because 
 * it only returns rows where there is data for both the character name and the actor.
 * 
 * However, perhaps you want to get all of the character names, 
 * even if there isn't corresponding data for the name of the actor. 
 * A LEFT JOIN returns all of the data from the first (or "left") table, 
 * and if there isn't corresponding data for the second table, 
 * it returns NULL for those columns.
 * 
 * Note: Other variants of SQL have RIGHT JOIN and OUTER JOIN, 
 * but those features are not present in SQLite.
 */

SELECT character.name,
       actor.name
FROM character
    LEFT JOIN character_actor
        ON character.id = character_actor.character_id
    LEFT JOIN actor
        ON character_actor.actor_id= actor.id
```

![](https://i.imgur.com/xFhZVHO.png)


<h3 id="25">25. Table alias </h3>

```SQL
/** 
 * In the previous exercise, we ran a query containing the tables character, 
 * tv_show, and character_tv_show. 
 * We can write a shorter query if we used aliases for those tables. 
 * Basically, we create a "nickname" for that table. 
 * 
 * If you want to use an alias for a table, 
 * you add AS *alias_name* after the table name.
 */
 
SELECT c.name, 
       a.name
FROM character AS c
    LEFT JOIN character_actor AS ca
        ON c.id = ca.character_id
    LEFT JOIN actor AS a
        ON ca.actor_id= a.id;
        
/* c:character, ca:character_actor, a:actor */
```

![](https://i.imgur.com/zZmFT3e.png)


<h3 id="26">26. Column alias </h3>

```SQL
/** 
 * n addition to making aliases for tables, 
 * you can also make them for columns. 
 * 
 * This clears up confusion on which column is which. 
 * In the previous exercise, both columns in the result are simply called "name", 
 * and that can be confusing. 
 * 
 * If you want to use an alias for a column, 
 * you add AS *alias_name* after the column name.
 */

SELECT character.name AS character, 
       actor.name AS actor
FROM character 
    LEFT JOIN character_actor
        ON character.id = character_actor.character_id
    LEFT JOIN actor
        ON character_actor.actor_id= actor.id;
```

![](https://i.imgur.com/Hl7Sfe4.png)


<h3 id="27">27. Self joins </h3>

```SQL
/** 
 * Sometimes, it may make sense for you to do a self join. 
 * In that case, you need to use table aliases to determine 
 * which data is from the "first"/"left" table. 
 */

SELECT e1.name AS employee_name, 
       e2.name AS boss_name 
FROM employees AS e1
    INNER JOIN employees AS e2
        ON e1.boss_id= e2.id
```
![](https://i.imgur.com/9QqvhsH.png)

```SQL
SELECT e1.name AS employee_name, 
       e2.name AS boss_name 
FROM employees AS e1
    LEFT JOIN employees AS e2
        ON e1.boss_id= e2.id
```
![](https://i.imgur.com/iy5tQgW.png)

```SQL
SELECT r1.name AS object, 
       r2.name AS beats 
FROM rps AS r1 
    INNER JOIN rps AS r2 
        ON r1.defeats_id = r2.id;
```
![](https://i.imgur.com/LQVVZ9W.png)


<h3 id="28">28. LIKE </h3>

```SQL
/** 
 * In SQL, you can use the LIKE command in order to search through text-based values. 
 * With LIKE, there are two special characters: % and _.
 * 
 * 1. %: The percent sign (%) represents zero, one, or multiple characters.
 * 2. _: The underscore (_) represents one character.
 * 
 * EX:
 * 1. LIKE "SUPER%" would match any value where SUPER is at the beginning
 * 2. LIKE "%SUPER" would match any value where SUPER is at the end
 * 3. LIKE "SUPER _" would match values such as "SUPER 1", "SUPER A",...
 */
 
SELECT * FROM [robots]
WHERE name 
LIKE '%Robot 20%';
```

![](https://i.imgur.com/9Iu8EAc.png)


<h3 id="29">29. CASE </h3>

```SQL
/** 
 * You can use a CASE statement to return certain values 
 * when certain scenarios are true.
 * 
 * CASE "欄位名"
 *     WHEN *first thing is true* THEN *value1*
 *     WHEN *second thing is true* THEN *value2*
 *     ...
 *     ELSE *value for all other situations* 
 * END 
 */
 
SELECT *, CASE species 
    WHEN  'human' THEN "talk" 
    WHEN  'dog' THEN "bark"
    WHEN  'cat' THEN "meow"
    ELSE "else" 
    END AS sound
FROM friends_of_pickles;
```

![](https://i.imgur.com/CV00o1E.png)


<h3 id="30">30. SUBSTR </h3>

```SQL
/** 
 * SUBSTR is used in this format: SUBSTR(column_name, index, number_of_characters)
 * 
 * index is a number that denotes where you would start the substring. 
 * 1 would indicate the first character, 2 would indicated the second character, etc. 
 * The index could also be negative, which means you would count from the end of the string. 
 * -1 would denote the last character, -2 would denote the 2nd to last character, etc. 
 * 
 * number_of_characters is optional; if it is not included, the substring contains the "rest of the string".
 * 
 * Here are some examples:
 * SUBSTR(name, 1, 5) is the first 5 characters of the name. 
 * SUBSTR(name, -4) is the last 4 characters of the name. 
 * 
 * SELECT * FROM robots WHERE SUBSTR(name, -4) LIKE '20__'; 
 * is another way of returning all of the robots that have been released between 2000 and 2099.
 * 
 * Note: In other versions of SQL, you could use RIGHT to do this.
 */

SELECT * 
FROM robots 
WHERE SUBSTR(location, -2) 
LIKE 'NY';
```

![](https://i.imgur.com/Ny3694h.png)


<h3 id="31">31. COALESCE </h3>

```SQL
/** 
 * COALESCE takes a list of columns, 
 * and returns the value of the first column that is not null. 
 * 
 * Suppose we wanted to find the most powerful weapon that a combatant has on hand. 
 * If value of gun is not null, that is the value returned. 
 * Otherwise, the value of sword is returned. Then you would run: 
 * 
 * SELECT name, COALESCE(gun, sword) AS weapon FROM fighters;
 */

SELECT name, COALESCE(tank, gun, sword) AS weapon FROM fighters; 
```

![](https://i.imgur.com/dJsQBUL.png)
