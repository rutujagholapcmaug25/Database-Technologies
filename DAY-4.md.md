# MySQL Functions & Joins - Complete Reference Guide

## 📝 String Functions

String functions manipulate text data (names, roles, emails, etc.).

### Case Conversion

#### UPPER(str) - Converts string to uppercase
```sql
SELECT UPPER(full_name) FROM student LIMIT 10;
```
**Output:**
```
+-----------------------------+
| upper(full_name)            |
+-----------------------------+
| AAKASH ASHOK KHARADE ( TL ) |
| AASIF JAMAL                 |
| ABHISHEK ANILKUMAR BORSE    |
| ABHISHEK DASONDHI ( AL )    |
| ABHISHEK NARAYAN JAGTAP     |
| ABHISHEK VILAS GAIKWAD      |
| ADARSH KUMAR CHANDEL ( CL ) |
| ADARSH KUSHWAH              |
| ADITYA ABHIJEET ADHIKARI    |
| ADITYA SACHIN KORDE         |
+-----------------------------+
```

#### LOWER(str) - Converts string to lowercase
```sql
SELECT LOWER(full_name) FROM student LIMIT 5;
```

---

### Length Functions

#### LENGTH(str) - Returns length of string in bytes
```sql
SELECT LENGTH(full_name) FROM student LIMIT 1;
```
**Output:**
```
+-------------------+
| length(full_name) |
+-------------------+
|                27 |
+-------------------+
```

#### CHAR_LENGTH(str) - Returns number of characters
```sql
SELECT CHAR_LENGTH(full_name) FROM student LIMIT 1;
```
**Output:**
```
+------------------------+
| char_length(full_name) |
+------------------------+
|                     27 |
+------------------------+
```

---

### String Concatenation

#### CONCAT(str1, str2, ...) - Combines multiple strings
```sql
SELECT CONCAT(full_name, dob) FROM student LIMIT 1;
```
**Output:**
```
+---------------------------------------+
| concat(full_Name , dob)               |
+---------------------------------------+
| Aakash Ashok Kharade ( TL )2002-07-09 |
+---------------------------------------+
```

```sql
SELECT CONCAT(full_name, " ", dob) FROM student LIMIT 1;
```
**Output:**
```
+----------------------------------------+
| concat(full_Name ," ",  dob)           |
+----------------------------------------+
| Aakash Ashok Kharade ( TL ) 2002-07-09 |
+----------------------------------------+
```

#### CONCAT_WS(separator, str1, str2, ...) - Combines with separator
```sql
SELECT CONCAT_WS('-', YEAR(dob), MONTH(dob), DAY(dob)) FROM student LIMIT 1;
```

---

### Substring Operations

#### SUBSTRING(str, position, length) - Extracts part of string
```sql
SELECT SUBSTRING(full_name, 5, 6) FROM student LIMIT 1;
```
**Output:**
```
+--------------------------+
| substring(full_name,5,6) |
+--------------------------+
| sh Ash                   |
+--------------------------+
```

```sql
SELECT SUBSTRING(full_name, 5) FROM student LIMIT 1;
```
**Output:**
```
+-------------------------+
| substring(full_name,5)  |
+-------------------------+
| sh Ashok Kharade ( TL ) |
+-------------------------+
```

#### LEFT(str, n) - Returns first n characters
```sql
SELECT LEFT(full_name, 4) FROM student LIMIT 1;
```
**Output:**
```
+--------------------+
| left(full_name, 4) |
+--------------------+
| Aaka               |
+--------------------+
```

```sql
SELECT LEFT(dob, 4) FROM student LIMIT 1;
```
**Output:**
```
+-------------+
| left(dob,4) |
+-------------+
| 2002        |
+-------------+
```

#### RIGHT(str, n) - Returns last n characters
```sql
SELECT RIGHT(full_name, 4) FROM student LIMIT 1;
```
**Output:**
```
+---------------------+
| right(full_name, 4) |
+---------------------+
| TL )                |
+---------------------+
```

---

### String Manipulation

#### TRIM(str) - Removes leading and trailing spaces
```sql
SELECT TRIM(full_name) FROM student LIMIT 2;
```
**Output:**
```
+-----------------------------+
| trim(full_name)             |
+-----------------------------+
| Aakash Ashok Kharade ( TL ) |
| Aasif Jamal                 |
+-----------------------------+
```

#### REPLACE(str, from_str, to_str) - Replaces substring
```sql
SELECT REPLACE(full_name, ") LT (", "( TL )") FROM student LIMIT 1;
```
**Output:**
```
+----------------------------------------+
| replace(full_name, ") LT (", "( TL )") |
+----------------------------------------+
| ( TL ) edarahK kohsA hsakaA            |
+----------------------------------------+
```

```sql
SELECT REPLACE(full_name, ") LT (", "***********") FROM student LIMIT 10;
```
**Output:**
```
+---------------------------------------------+
| replace(full_name, ") LT (", "***********") |
+---------------------------------------------+
| *********** edarahK kohsA hsakaA            |
| Aasif Jamal                                 |
| Abhishek Anilkumar Borse                    |
| Abhishek Dasondhi ( AL )                    |
| Abhishek Narayan Jagtap                     |
| Abhishek Vilas Gaikwad                      |
| Adarsh Kumar Chandel ( CL )                 |
| Adarsh Kushwah                              |
| Aditya Abhijeet Adhikari                    |
| Aditya Sachin Korde                         |
+---------------------------------------------+
```

```sql
SELECT REPLACE(full_name, "a", "*") FROM student LIMIT 10;
```
**Output:**
```
+------------------------------+
| replace(full_name, "a", "*") |
+------------------------------+
| ) LT ( ed*r*hK kohsA hs*k*A  |
| A*sif J*m*l                  |
| Abhishek Anilkum*r Borse     |
| Abhishek D*sondhi ( AL )     |
| Abhishek N*r*y*n J*gt*p      |
| Abhishek Vil*s G*ikw*d       |
| Ad*rsh Kum*r Ch*ndel ( CL )  |
| Ad*rsh Kushw*h               |
| Adity* Abhijeet Adhik*ri     |
| Adity* S*chin Korde          |
+------------------------------+
```

#### LOCATE(substring, str) - Finds position of substring
```sql
SELECT LOCATE("LT", full_name) FROM student LIMIT 1;
```
**Output:**
```
+-------------------------+
| Locate("LT", full_name) |
+-------------------------+
|                       3 |
+-------------------------+
```

```sql
SELECT LOCATE("a", full_name) FROM student LIMIT 10;
```
**Output:**
```
+------------------------+
| Locate("a", full_name) |
+------------------------+
|                     10 |
|                      1 |
|                      1 |
|                      1 |
|                      1 |
|                      1 |
|                      1 |
|                      1 |
|                      1 |
|                      1 |
+------------------------+
```

```sql
SELECT LOCATE(SUBSTRING(full_name, 5, 5), full_name) FROM student LIMIT 1;
```
**Output:**
```
+---------------------------------------------+
| Locate(substring(full_name,5,5), full_name) |
+---------------------------------------------+
|                                           5 |
+---------------------------------------------+
```

#### REVERSE(str) - Reverses the string
```sql
SELECT REVERSE(full_name) FROM student LIMIT 1;
```
**Output:**
```
+-----------------------------+
| reverse(full_name)          |
+-----------------------------+
| ) LT ( edarahK kohsA hsakaA |
+-----------------------------+
```

```sql
UPDATE student SET full_name = REVERSE(full_name) WHERE prn = 250840320001;
-- Query OK, 1 row affected (0.01 sec)

SELECT full_name FROM student LIMIT 1;
```
**Output:**
```
+-----------------------------+
| full_name                   |
+-----------------------------+
| ) LT ( edarahK kohsA hsakaA |
+-----------------------------+
```

---

## 📅 Date Functions

Date functions operate on DATE, DATETIME, and TIMESTAMP data types.

### Date Format Rules
- **00-69** → Years 2000 to 2069
- **70-99** → Years 1970 to 1999

### Current Date/Time Functions

#### NOW() / CURRENT_TIMESTAMP - Returns current date and time
```sql
SELECT NOW();
```
**Output:**
```
+---------------------+
| now()               |
+---------------------+
| 2025-10-04 10:15:05 |
+---------------------+
```

```sql
SELECT NOW() FROM DUAL;
```
**Output:**
```
+---------------------+
| now()               |
+---------------------+
| 2025-10-04 10:15:18 |
+---------------------+
```

#### CURDATE() / CURRENT_DATE - Returns today's date
```sql
SELECT CURDATE();
```
**Output:**
```
+------------+
| curdate()  |
+------------+
| 2025-10-04 |
+------------+
```

#### CURTIME() - Returns current time
```sql
SELECT CURTIME();
```
**Output:**
```
+-----------+
| curtime() |
+-----------+
| 10:17:56  |
+-----------+
```

#### CURRENT_TIMESTAMP() - Returns current timestamp
```sql
SELECT CURRENT_TIMESTAMP();
```
**Output:**
```
+---------------------+
| current_timestamp() |
+---------------------+
| 2025-10-04 10:28:57 |
+---------------------+
```

---

### Date Component Extraction

#### YEAR(date) - Extracts year
```sql
SELECT YEAR(dob) FROM student LIMIT 1;
```
**Output:**
```
+-----------+
| year(dob) |
+-----------+
|      2002 |
+-----------+
```

```sql
SELECT YEAR(DATE("700101")) FROM DUAL;
```
**Output:**
```
+----------------------+
| year(date("700101")) |
+----------------------+
|                 1970 |
+----------------------+
```

#### MONTH(date) - Extracts month (1-12)
```sql
SELECT MONTH(dob) FROM student LIMIT 1;
```
**Output:**
```
+------------+
| month(dob) |
+------------+
|          7 |
+------------+
```

#### DAY(date) - Extracts day of month
```sql
SELECT DAY(dob) FROM student LIMIT 1;
```
**Output:**
```
+----------+
| day(dob) |
+----------+
|        9 |
+----------+
```

```sql
SELECT DAY('20250707') FROM DUAL;
```
**Output:**
```
+-----------------+
| day('20250707') |
+-----------------+
|               7 |
+-----------------+
```

```sql
SELECT DAY('20251307') FROM DUAL;
```
**Output:**
```
+-----------------+
| day('20251307') |
+-----------------+
|            NULL |
+-----------------+
1 row in set, 1 warning (0.00 sec)
```

#### DAYNAME(date) - Returns day name
```sql
SELECT DAYNAME(LAST_DAY(NOW())) FROM DUAL;
```
**Output:**
```
+--------------------------+
| dayname(last_day(now())) |
+--------------------------+
| Friday                   |
+--------------------------+
```

#### MONTHNAME(date) - Returns month name
```sql
SELECT MONTHNAME(DATE('700101')) FROM DUAL;
```
**Output:**
```
+---------------------------+
| monthname(date('700101')) |
+---------------------------+
| January                   |
+---------------------------+
```

```sql
SELECT MONTHNAME(DATE('701301')) FROM DUAL;
```
**Output:**
```
+---------------------------+
| monthname(date('701301')) |
+---------------------------+
| NULL                      |
+---------------------------+
1 row in set, 1 warning (0.00 sec)
```

---

### Date Formatting

#### DATE_FORMAT(date, format) - Formats date according to pattern
```sql
SELECT DATE_FORMAT('20251207', '%m-%d-%y') FROM DUAL;
```
**Output:**
```
+------------------------------------+
| date_format('20251207','%m-%d-%y') |
+------------------------------------+
| 12-07-25                           |
+------------------------------------+
```

```sql
SELECT DATE_FORMAT('20251207', '%d-%m-%y') FROM DUAL;
```
**Output:**
```
+------------------------------------+
| date_format('20251207','%d-%m-%y') |
+------------------------------------+
| 07-12-25                           |
+------------------------------------+
```

```sql
SELECT DATE_FORMAT('20251207', '%dd-%mm-%yyyy') FROM DUAL;
```
**Output:**
```
+-----------------------------------------+
| date_format('20251207','%dd-%mm-%yyyy') |
+-----------------------------------------+
| 07d-12m-25yyy                           |
+-----------------------------------------+
```

**Common Format Specifiers:**
- `%d` - Day of month (01-31)
- `%m` - Month (01-12)
- `%Y` - Year (4 digits)
- `%y` - Year (2 digits)

---

### Date Calculations

#### DATEDIFF(date1, date2) - Returns difference in days
```sql
SELECT DATEDIFF(NOW(), dob) FROM student WHERE prn = 250840320001;
```
**Output:**
```
+---------------------+
| datediff(now(),dob) |
+---------------------+
|                8488 |
+---------------------+
```

#### LAST_DAY(date) - Returns last day of the month
```sql
SELECT LAST_DAY(NOW()) FROM DUAL;
```
**Output:**
```
+-----------------+
| last_day(now()) |
+-----------------+
| 2025-10-31      |
+-----------------+
```

#### DATE(expression) - Extracts date part
```sql
SELECT DATE("000101") FROM DUAL;
```
**Output:**
```
+----------------+
| date("000101") |
+----------------+
| 2000-01-01     |
+----------------+
```

```sql
SELECT DATE("700101") FROM DUAL;
```
**Output:**
```
+----------------+
| date("700101") |
+----------------+
| 1970-01-01     |
+----------------+
```

---

### Date Filtering Examples

#### Using BETWEEN for date ranges
```sql
SELECT prn, full_name FROM student 
WHERE dob BETWEEN '2001-01-01' AND '2003-01-01' 
LIMIT 10;
```
**Output:**
```
+--------------+-----------------------------+
| prn          | full_name                   |
+--------------+-----------------------------+
| 250840320001 | ) LT ( edarahK kohsA hsakaA |
| 250840320005 | Abhishek Narayan Jagtap     |
| 250840320009 | Aditya Abhijeet Adhikari    |
| 250840320010 | Aditya Sachin Korde         |
| 250840320011 | Afsha Zarreen Sayeed Khan   |
| 250840320016 | Akash Raghunath Bhadane     |
| 250840320017 | Akshay Keshav Balte         |
| 250840320018 | Amar Balasaheb Toge         |
| 250840320020 | Amey Arun Parab ( TL )      |
| 250840320022 | Aniket Bhagwat Sherkar      |
+--------------+-----------------------------+
```

#### Filtering by date comparison
```sql
SELECT prn, full_name FROM student WHERE dob < "20020101" LIMIT 10;
```
**Output:**
```
+--------------+-----------------------------+
| prn          | full_name                   |
+--------------+-----------------------------+
| 250840320003 | Abhishek Anilkumar Borse    |
| 250840320004 | Abhishek Dasondhi ( AL )    |
| 250840320005 | Abhishek Narayan Jagtap     |
| 250840320007 | Adarsh Kumar Chandel ( CL ) |
| 250840320011 | Afsha Zarreen Sayeed Khan   |
| 250840320012 | Ajay Raysing Patil          |
| 250840320013 | Akanksha Jeevan Puri ( CL ) |
| 250840320018 | Amar Balasaheb Toge         |
| 250840320020 | Amey Arun Parab ( TL )      |
| 250840320021 | Amey Shekhar Raut           |
+--------------+-----------------------------+
```

---

## 🔢 Aggregate Functions

Aggregate functions operate on sets of rows and return a single result.

### COUNT() - Counts rows or non-NULL values

```sql
SELECT COUNT(*) FROM student;
```
**Output:**
```
+----------+
| count(*) |
+----------+
|      238 |
+----------+
```

```sql
SELECT COUNT(*) AS total_number_of_students FROM student;
```
**Output:**
```
+--------------------------+
| total_number_of_students |
+--------------------------+
|                      238 |
+--------------------------+
```

```sql
SELECT COUNT(team_id) FROM student;
```
**Output:**
```
+----------------+
| count(team_id) |
+----------------+
|             24 |
+----------------+
```

```sql
SELECT COUNT(faculty_id) FROM student WHERE faculty_id = 1;
```
**Output:**
```
+-------------------+
| count(faculty_id) |
+-------------------+
|                 4 |
+-------------------+
```

---

### SUM() - Returns sum of numeric column

```sql
SELECT SUM(grade) FROM student;
```
**Output:**
```
+------------+
| sum(grade) |
+------------+
|    1245.65 |
+------------+
```

```sql
SELECT SUM(prn), SUM(grade) AS grade FROM student;
```
**Output:**
```
+----------------+---------+
| sum(prn)       | grade   |
+----------------+---------+
| 59699996188441 | 1245.65 |
+----------------+---------+
```

```sql
SELECT SUM(prn), SUM(grade), COUNT(prn) AS grade FROM student;
```
**Output:**
```
+----------------+------------+-------+
| sum(prn)       | sum(grade) | grade |
+----------------+------------+-------+
| 59699996188441 |    1245.65 |   238 |
+----------------+------------+-------+
```

---

### AVG() - Returns average of numeric column

```sql
SELECT AVG(grade) FROM student;
```
**Output:**
```
+------------+
| avg(grade) |
+------------+
|   5.233824 |
+------------+
```

```sql
SELECT AVG(dob) FROM student;
```
**Output:**
```
+---------------+
| avg(dob)      |
+---------------+
| 20015717.0294 |
+---------------+
```

---

### MIN() - Returns smallest value

```sql
SELECT MIN(grade) FROM student;
```
**Output:**
```
+------------+
| min(grade) |
+------------+
|       0.08 |
+------------+
```

```sql
SELECT MIN(dob) FROM student;
```
**Output:**
```
+------------+
| min(dob)   |
+------------+
| 2000-01-08 |
+------------+
```

---

### MAX() - Returns largest value

```sql
SELECT MAX(grade) FROM student;
```
**Output:**
```
+------------+
| max(grade) |
+------------+
|       9.96 |
+------------+
```

```sql
SELECT MAX(dob) FROM student;
```
**Output:**
```
+------------+
| max(dob)   |
+------------+
| 2003-12-27 |
+------------+
```

---

### Multiple Aggregates Together

```sql
SELECT MAX(grade), MIN(grade) FROM student;
```
**Output:**
```
+------------+------------+
| max(grade) | min(grade) |
+------------+------------+
|       9.96 |       0.08 |
+------------+------------+
```

---

### DISTINCT - Returns unique values

```sql
SELECT DISTINCT faculty_id FROM student;
```
**Output:**
```
+------------+
| faculty_id |
+------------+
|       NULL |
|          1 |
|          2 |
|          3 |
|          4 |
|          5 |
|          6 |
+------------+
7 rows in set (0.00 sec)
```

```sql
SELECT DISTINCT grade FROM student;
```
**Output:** 209 unique grade values (truncated for brevity)

---

## 🔗 SQL Joins

Joins combine rows from two or more tables based on related columns.

### 1. INNER JOIN
Returns only rows where the join condition is matched in both tables.

```sql
SELECT s.full_name, t.team_name 
FROM student s 
INNER JOIN team t ON s.team_id = t.team_id;
```
**Output:**
```
+---------------------------------+-------------------+
| full_name                       | team_name         |
+---------------------------------+-------------------+
| Purva Pradeep Thavai            | Access Modifiers  |
| Vaibhav Sanjay Sonawane ( AL )  | Algo-Warriors     |
| Krishna Aditya Chikkala         | AlgoVeer          |
| Amey Shekhar Raut               | AlphaCoder        |
| Snehal Shivaji Shinde           | Binary 2 Infinity |
| Suyog Avinash Joshi             | Binary Bandits    |
| Deepa Sushil Jadhav ( AL )      | Bug Brahmas       |
| Vedant Suryakant Mali ( AL )    | Code-Mafia        |
| Rohit Bhalse                    | CodeBlooded       |
| Pranjali Pramod Rane            | CodeRaptors       |
| Gunjan Pravin Chaudhari         | CodeStructs       |
| Ishan Raizada                   | Cyber_Shakti      |
| Apurva Nandkishor Dhonde        | Dacengers         |
| Afsha Zarreen Sayeed Khan       | DEVdas            |
| Dipti Sampat Akhade             | Ekalavya          |
| ) LT ( edarahK kohsA hsakaA     | Logic Legends     |
| Sanghapal Ishwar Gavhane        | Phoenix           |
| Shruti Dhanalal Wadile ( AL )   | Runtime Rebels    |
| Sachin Sanjay Waghchaure ( AL ) | sa_Algoholics     |
| Mohini Nikhil Kasar             | StackNova         |
| Parikshit Mahendra Urkande      | StackOfTen        |
| Nidhi Kumari                    | Super 10          |
| Bhagyesh Tushar Wani            | TechSpirits       |
| Saurabh Raju Vaidya             | TechUdaan         |
+---------------------------------+-------------------+
24 rows in set (0.00 sec)
```

```sql
SELECT s.full_name, f.full_name 
FROM student s 
INNER JOIN faculty f ON s.faculty_id = f.faculty_id;
```
**Output:**
```
+---------------------------------+-----------------------+
| full_name                       | full_name             |
+---------------------------------+-----------------------+
| ) LT ( edarahK kohsA hsakaA     | Aditya Kansana        |
| Afsha Zarreen Sayeed Khan       | Aditya Kansana        |
| Parikshit Mahendra Urkande      | Aditya Kansana        |
| Pranjali Pramod Rane            | Aditya Kansana        |
| Amey Shekhar Raut               | Aniket Takmare        |
| Apurva Nandkishor Dhonde        | Aniket Takmare        |
| Purva Pradeep Thavai            | Aniket Takmare        |
| Rohit Bhalse                    | Aniket Takmare        |
| Bhagyesh Tushar Wani            | Manoj More            |
| Deepa Sushil Jadhav ( AL )      | Manoj More            |
| Sachin Sanjay Waghchaure ( AL ) | Manoj More            |
| Sanghapal Ishwar Gavhane        | Manoj More            |
| Dipti Sampat Akhade             | Shilbhushan Khanderao |
| Gunjan Pravin Chaudhari         | Shilbhushan Khanderao |
| Saurabh Raju Vaidya             | Shilbhushan Khanderao |
| Shruti Dhanalal Wadile ( AL )   | Shilbhushan Khanderao |
| Ishan Raizada                   | Shweta Bhere          |
| Krishna Aditya Chikkala         | Shweta Bhere          |
| Snehal Shivaji Shinde           | Shweta Bhere          |
| Suyog Avinash Joshi             | Shweta Bhere          |
| Mohini Nikhil Kasar             | Vipul Tembulwar       |
| Nidhi Kumari                    | Vipul Tembulwar       |
| Vaibhav Sanjay Sonawane ( AL )  | Vipul Tembulwar       |
| Vedant Suryakant Mali ( AL )    | Vipul Tembulwar       |
+---------------------------------+-----------------------+
24 rows in set (0.00 sec)
```

---

### 2. LEFT JOIN (LEFT OUTER JOIN)
Returns all rows from the left table and matched rows from the right table.

```sql
SELECT s.full_name, f.full_name 
FROM student s 
LEFT JOIN faculty f ON s.faculty_id = f.faculty_id
LIMIT 20;
```
**Output:**
```
+------------------------------------------+-----------------------+
| full_name                                | full_name             |
+------------------------------------------+-----------------------+
| ) LT ( edarahK kohsA hsakaA              | Aditya Kansana        |
| Aasif Jamal                              | NULL                  |
| Abhishek Anilkumar Borse                 | NULL                  |
| Abhishek Dasondhi ( AL )                 | NULL                  |
| Abhishek Narayan Jagtap                  | NULL                  |
| Abhishek Vilas Gaikwad                   | NULL                  |
| Adarsh Kumar Chandel ( CL )              | NULL                  |
| Adarsh Kushwah                           | NULL                  |
| Aditya Abhijeet Adhikari                 | NULL                  |
| Aditya Sachin Korde                      | NULL                  |
| Afsha Zarreen Sayeed Khan                | Aditya Kansana        |
| Ajay Raysing Patil                       | NULL                  |
| Akanksha Jeevan Puri ( CL )              | NULL                  |
| Akanksha Somnath Dhanawade ( AL )        | NULL                  |
| Akash Pandurang Kokulwar                 | NULL                  |
| Akash Raghunath Bhadane                  | NULL                  |
| Akshay Keshav Balte                      | NULL                  |
| Amar Balasaheb Toge                      | NULL                  |
| Amarnath Ambadas Malpuri                 | NULL                  |
| Amey Arun Parab ( TL )                   | NULL                  |
+------------------------------------------+-----------------------+
238 total rows (showing first 20)
```

---

### 3. RIGHT JOIN (RIGHT OUTER JOIN)
Returns all rows from the right table and matched rows from the left table.

```sql
SELECT s.full_name, f.full_name 
FROM student s 
RIGHT JOIN faculty f ON s.faculty_id = f.faculty_id;
```
**Output:**
```
+---------------------------------+-----------------------+
| full_name                       | full_name             |
+---------------------------------+-----------------------+
| ) LT ( edarahK kohsA hsakaA     | Aditya Kansana        |
| Afsha Zarreen Sayeed Khan       | Aditya Kansana        |
| Parikshit Mahendra Urkande      | Aditya Kansana        |
| Pranjali Pramod Rane            | Aditya Kansana        |
| Amey Shekhar Raut               | Aniket Takmare        |
| Apurva Nandkishor Dhonde        | Aniket Takmare        |
| Purva Pradeep Thavai            | Aniket Takmare        |
| Rohit Bhalse                    | Aniket Takmare        |
| Bhagyesh Tushar Wani            | Manoj More            |
| Deepa Sushil Jadhav ( AL )      | Manoj More            |
| Sachin Sanjay Waghchaure ( AL ) | Manoj More            |
| Sanghapal Ishwar Gavhane        | Manoj More            |
| Dipti Sampat Akhade             | Shilbhushan Khanderao |
| Gunjan Pravin Chaudhari         | Shilbhushan Khanderao |
| Saurabh Raju Vaidya             | Shilbhushan Khanderao |
| Shruti Dhanalal Wadile ( AL )   | Shilbhushan Khanderao |
| Ishan Raizada                   | Shweta Bhere          |
| Krishna Aditya Chikkala         | Shweta Bhere          |
| Snehal Shivaji Shinde           | Shweta Bhere          |
| Suyog Avinash Joshi             | Shweta Bhere          |
| Mohini Nikhil Kasar             | Vipul Tembulwar       |
| Nidhi Kumari