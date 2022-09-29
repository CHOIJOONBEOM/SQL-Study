# SQL-Study
## Chapter 1 - Database and SQL
- Database: Collection of data  
- DBMS(Database Management System): SW that manages database(users can share and access at the same time)  
- SQL: Language used to manage data in DMBS  
  
- Type of DBMS
  - Hierarchical - Network - Relational - Object Oriented - Object Relational

## Chapter 2 - SQL Basics
### 2-1
- Database modeling: modeling before making database
- Project: Entire process of making software (real life case -> computer system)
- Waterfall model: Representative sw developing process
  1. Project Planning &nbsp; 2. Project Analysis &nbsp; 3. System Design &nbsp;  4. Coding &nbsp;  5. Testing &nbsp;  6. Deployment(유지보수)  
  
- How to make Database  
  - Schemas - Create Schema  
- How to make Table
  - Select Database - Right click - Create Table  
- How to Insert Data
  - Select Table - Right click - Select Rows Limits 1000  
- Selecting Data
   ```SQL
   SELECT * FROM member;
   SELECT * FROM member WHERE member_name = '아이유';
   ```
     
  - Format: SELECT "data" FROM "table" WHERE "condition"
  - '*' selects all data from table
  
### 2-3 Database Object
#### Index
  ```SQL
  CREATE INDEX idx_member_name ON member(member_name);
  ```
#### View
  ```SQL
  CREATE VIEW member_view AS SELECT * FROM member;
  SELECT * FROM member_view;
  ```
#### Stored Procedure
  ```SQL
  DELIMITTER //
  CREATE PROCEDURE myProc()
  BEGIN
        SELECT * FROM member WHERE member_name = '나훈아';
        SELECT * FROM product WHERE product_name = '삼각김밥';
  END //
  DELIMITTER;
  CALL myProc();
  ```
  
## Chapter 3 - SQL Basic Grammer
### 3-1 SELECT ~ FROM ~ WHERE
- Creating and Dropping Database
```SQL
DROP DATABASE IF EXISTS market_db;
CREATE DATABASE market_db;
```
- Creating 'member' table
USE market_db;
```SQL
CREATE TABLE member -- 회원 테이블
( mem_id  		CHAR(8) NOT NULL PRIMARY KEY, -- 사용자 아이디(PK)
  mem_name    	VARCHAR(10) NOT NULL, -- 이름
  mem_number    INT NOT NULL,  -- 인원수
  addr	  		CHAR(2) NOT NULL, -- 지역(경기,서울,경남 식으로 2글자만입력)
  phone1		CHAR(3), -- 연락처의 국번(02, 031, 055 등)
  phone2		CHAR(8), -- 연락처의 나머지 전화번호(하이픈제외)
  height    	SMALLINT,  -- 평균 키
  debut_date	DATE  -- 데뷔 일자
);
```
- Creating 'buy' table
```SQL
CREATE TABLE buy -- 구매 테이블
(  num 		INT AUTO_INCREMENT NOT NULL PRIMARY KEY, -- 순번(PK)
   mem_id  	CHAR(8) NOT NULL, -- 아이디(FK)
   prod_name 	CHAR(6) NOT NULL, --  제품이름
   group_name 	CHAR(4)  , -- 분류
   price     	INT  NOT NULL, -- 가격
   amount    	SMALLINT  NOT NULL, -- 수량
   FOREIGN KEY (mem_id) REFERENCES member(mem_id)
);
```
- Insert Data
```SQL
INSERT INTO member VALUES('TWC', '트와이스', 9, '서울', '02', '11111111', 167, '2015.10.19');
INSERT INTO member VALUES('BLK', '블랙핑크', 4, '경남', '055', '22222222', 163, '2016.08.08');
INSERT INTO member VALUES('WMN', '여자친구', 6, '경기', '031', '33333333', 166, '2015.01.15');
...
```
- Designating Database
  - You should designate database using 'database_name.table_name'.
  - But if you use 'USE' you can just type 'table_name'.
```SQL
SELECT * FROM market_db.member;
```
```SQL
USE market_db;
SELECT * FROM member;
```
- Where Conditions
  - Relational, Logical Operators
    - you can use relational operators(<=) or logical operators(AND, OR)
  - Between ~ AND: used to get values between two numbers
    ```SQL
    SELECT mem_name, height
      FROM member
      WHERE height BETWEEN 163 AND 165;
    ```
  - IN: use instead of OR
    ```SQL
    SELECT mem_name, addr
      FROM member
      WHERE addr IN('경기', '전남', '경남');
    ```
  - LIKE: search text that contains specific data
      - '% includes every text that contains specific data'
    ```SQL
    SELECT * FROM member WHERE mem_name LIKE '우%';
    ```
       - 'searches text with exact number of _. In this case searches text that has two words infront of pink'
    ```SQL
    SELECT * FROM member WHERE mem_name LIKE '__pink';
    ```
   
### 3-2
- ORDER BY
```SQL
SELECT mem_id, mem_name, debut_date
  FROM member
  ORDER BY debut_date;
```

## SQL Grammer - Advanced
### 4-1 Data Type of MySQL
- Integer
  - TINYINT:  1 byte, range -128 ~ 127
  - SMALLINT: 2 byte, -32,768 ~ 32,767
  - INT:      4 byte, -21 trillion ~ 21 trillion
  - BIGINT:   8 byte, -900경 ~ 900경
  * UNSIGNED: Range of the Integer starts from 0
  e.g.) if use TINYINT UNSIGNED, data range is from 0 to 255
  * Error Message: Out of Range


```SQL
USE market_db;
CREATE TABLE hongong4(
  tinyint_col TINYINT,
  smallint_col SMALLINT,
  int_col INT,
  bigint_col BIGINT
);
```

- Text
  - CHAR: 1~255 byte
  - VARCHAR: 1~16383 byte

  - CHAR has fixed length of character while VARCHAR has variable length of character.
  - Using VARCHAR is efficient but slower than using CHAR in process.
    - LONGTEXT: 1 ~ 42 trillioni
    - BLOB(Binary Long Object)/LONGBLOB: Image, Video data

- Actual Number
  - FLOAT: 4 byte, 7 decimal point
  - DOUBLE: 8 byte, 15 decimal point

- Date and Time
  - DATE: 3 byte, saves only date, YYYY-MM-DD
  - TIME: 3 byte, saves only time, HH:MM:SS
  - DATETIME: 8 byte, date and time, YYYY-MM-DD HH:MM:SS

- Variable
  - SQL can set variable and use it like any other programming languages.
  - Variables cannot be used when using LIMIT
  - PREPARE and EXECUTE are used instead of LIMIT
  ```SQL
  SET @count = 3:
  PREPARE mySQL FROM 'SELECT mem_name, height FROM member ORDER BY height LIMIT ?';
  EXECUTE mySQL USING @count;
  ```

#### Data Type Conversion
  - Type Conversion: Str to Int, Int to Str
    - Explicit Conversion: use function for conversion
      - CAST(), CONVERT()
      ```SQL
       SELECT CAST(AVG(price) AS SIGNED) '평균 가격' FROM buy;
      -- 또는
      SELECT CONVERT(AVG(price) , SIGNED) '평균 가격' FROM buy;
      ```
    - Implicit Conversion: happens without command
    ```SQL
    SELECT 100 + '200';
    ```
      - The result shows 300 although 100 was integer and 200 was string.  
        String was implicitly converted into integer.


### 4-2 Join
- Join: combining tables into one to extract data
  - Inner Join: In general, Join refers to an Inner Join and it is most frequently used
      - One to Many Relationship
        - Tables of database have several relationships, and One to Many Relationship is necessary for join
        - One table can have only one value(Primary Key), while the other table can have multiple values(Foreign Key)   

      - Example of joining 'buy' table and 'member' table to get members' address, contact and deliver the goods.
      ```SQL
      USE market_db;
      SELECT *
        FROM buy
        INNER JOIN member
        ON buy.mem_id = member.mem_id
      WHERE buy.mem_id = 'GRL';
      ```
        * If there are same column names for the joining tables, they should be typed as "Table_Name.Column_Name"
        * If WHERE code is omitted, every row of buy table will joing will member table
        * Only data from the tables used for join can be printed

      - Simple Expression of Inner Join
        - To show from which table the column came from, users should write table name infront of column name
        - Table name can be simplified using alias(별칭)
        ```SQL
        SELECT buy.mem_id, member.mem_name, buy.prod_name, member.addr, CONCAT(member.phone1, member.phone2) '연락처'
          FROM buy
            INNER JOIN member
            ON buy.mem_id = member.mem_id;
        ```
        ```SQL
        SELECT B.mem_id, M.mem_name, B.prod_name, M.addr, CONCAT(M.phone1, M.phone2) '연락처'
          FROM buy B
            INNER JOIN member M
            ON B.mem_id = M.mem_id;
        ```

      - Selecting one Distinct Result
        ```SQL
        SELECT DISTINCT M.mem_id, M.mem_name, M.addr
          FROM buy B
            INNER JOIN member M
            ON B.mem_id = M.mem_id
          ORDER BY M.mem_id;
        ```
  - Outer Join: Even if there is data in only one table, the results can be printed
      - Format
        - Printing purchase record of every member(includes member who didn't buy anything)
      ```SQL
      SELECT M.mem_id, M.mem_name, B.prod_name, M.addr
        FROM member M
          LEFT OUTER JOIN buy B
          ON M.mem_id = B.mem_id
        ORDER BY M.mem_id;
      ```
        * LEFT OUTER JOIN: All contents of the left table(member) should be printed out. Can be written as 'LEFT JOIN'
        * RIGHT OUTER JOIN: All contents of the right table(buy) should be printed out. Can be written as 'RIGHT JOIN'
        * FULL OUTER JOIN: Any contents of the left and right table(member, buy)

      - Printing data that is null
        - Use WHERE 'Column_Name' IS NULL
        - Printing member who has never bought items
      ```SQL
        SELECT DISTINCT M.mem_id, B.prod_name, M.mem_name, M.addr
          FROM member M
            LEFT OUTER JOIN buy B
            ON M.mem_id = B.mem_id
          WHERE B.prod_name IS NULL
          ORDER BY M.mem_id;
      ```      
  - Other Join
      - Cross Join
        - Join all rows in one table and all rows in the other table
        - The number of result is multiplication of number or rows from two tables
        - Cannot use ON
        - Since it is randomly joined, the result is meaningless
        - Purpose of cross join is to generate large amounts of data
        ```SQL
        SELECT *
          FROM buy
            CROSS JOIN member;
        ```
        - CREATE TABLE ~ SELECT can be used with CROSS JOIN to make large amounts of data
        ```SQL
        CREATE TABLE cross_table
          SELECT *
            FROM sakila.actor
              CROSS JOIN world.country;
        SELECT * FROM cross_table LIMIT 5;
        ``` 
      
      - Self Join
        - Join data in the table itself. So, self join uses one table.
        - A Representative example is the organization chart of a company
          - ![alt text](https://github.com/CHOIJOONBEOM/SQL-Study/blob/main/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-09-22%20012338.png)
          - Director is a manger of senior manager while he is also an employee. To know Director's contact, user should self join EMP and MANAGER column
          ```SQL
          SELECT A.emp "EMP", B.emp "MANAGER", B.phone "CONATCT"
            FROM emp_table A
              INNER JOIN emp_table B
              ON A.manager = B.emp
            WHERE A.emp = 'Senior Manager';
          ```
          * Used alias of emp_table with emp_table A, emp_table B to use them as seperate tables
<br><br>
  ### 4-3 SQL Programming
    - SQL Programming should be done inside 'Stored Procedure'
    - Stored Procedure Format
      ```SQL
      DELIMITER $$
      CREATE PROCEDURE 'Stored_Procedure_Name'
      BEGIN 'Code'
      END $$
      DELIMITER;
      CALL 'Stored_Procedure_Name'
      ```

    ### IF STATEMENT
    - Format
      ```SQL
      IF 'Condition' THEN 'output'
      END IF;
      ```

    - If there are more than two outputs, use 'BEGIN~END'
      ```SQL
      DROP PROCEDURE IF EXISTS ifProc1;
      DELIMITER $$
      CREATE PROCEDURE ifProc1()
      BEGIN
        IF 100 = 100 THEN
          SELECT '100 is same as 100';
        END IF;
      DELIMITER;
      CALL ifProc1();
      ```

    ### IF ~ ELSE STATEMENT
    -
      ```SQL
      DROP PROCEDURE IF EXISTS ifProc2;
      DELIMITER $$
      CREATE PROCEDURE ifProc2()
      BEGIN
        DECLARE myNum INT;
        SET myNum = 200;
        IF myNum = 100 THEN
          SELECT 'The number is 100';
        ELSE
          SELECT 'The number is not 100';
        END IF;
      END $$
      DELIMITER;
      CALL ifProc2();
      ```

    ### CASE STATEMENT
    - Format
      ```SQL
      CASE
        WHEN 'Condition1' THEN
          'Output1'
        WHEN 'Condition2' THEN
          'Output2'
        WHEN 'Condition3' THEN
          'Output3'
        ELSE
          'Output4'
      END CASE;
      ```
    - Exam score and grade example
      ```SQL
      DROP PROCEDURE IF EXISTS caseProc;
      DELIMITER $$
      CREATE PROCEDURE caseProc()
      BEGIN
        DECLARE point INT;
        DECLARE credit CHAR(1);
        SET point = 88;

        CASE
          WHEN point >=90 THEN
            SET credit = 'A';
          WHEN point >=80 THEN
            SET credit = 'B';
          WHEN point >=70 THEN
            SET credit = 'C';
          WHEN point >=60 THEN
            SET credit = 'D';
          ELSE
            SET credit = 'F';
        END CASE
        SELECT CONCAT('취득점수==>', point), CONCAT('학점==>', credit);
      END $$
      DELIMITER;
      CALL caseProc();
      ```
    ### WHILE LOOP
    - Format
      ```SQL
      WHILE 'Condition' DO 'SQL sentences'
      END WHILE;
      ```

    - Adding every number from 1 to 100 using WHILE
      ```SQL
      DROP PROCEDURE IF EXISTS whileProc;
      DELIMITER $$
      CREATE PROCEDURE whileProc()
      BEGIN
        DECLARE i INT;
        DECLARE hap INT;
        SET i = 1;
        SET hap = 0;

        WHILE (i <= 100) DO
          SET hap = hap + i;
          SET i = i + 1;
        END WHILE;

        SELECT 'Sum from 1 to 100:', hap;
      END $$
      DELIMITER;
      CALL whileProc();
      ```
    - ITERATE [label]: starts coding at the designated label
    - LEAVE [label]: ends While loop
    <br><br>
    ### DYNAMIC SQL
    - PREPARE: doesn't execute but prepare SQL sentence
    - EXECUTE: executes prepared SQL sentence
    - DEALLOCATE PREFARE: deallocate prepare after execution
      ```SQL
      use market_db:
      PREPARE myQuery FROM 'SELECT * FROM member WHERE mem_id = "BLK"';
      EXECUTE myQuery;
      DEALLOCAE PREPARE myQuery;
      ```