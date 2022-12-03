# SQL-Study
## Chapter 1 - Database and SQL
- Database: Collection of data  
- DBMS(Database Management System): SW that manages database(users can share and access at the same time)  
- SQL: Language used to manage data in DMBS  
  
- Type of DBMS
  - Hierarchical - Network - Relational - Object Oriented - Object Relational

## Chapter 2 - SQL Basics
### 2-1 Database Modeling
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
  (  num 		INT AUTO_INCREMENT NOT NULL PRIMARY KEY, -- 순번(PK) AUTO_INCREMENT는 PRIMARY KEY로 지정해야 함
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
   
### 3-2 Select Statement
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
      - Format (Printing purchase record of every member - includes member who didn't buy anything)
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
        ```SQL
        SELECT *
          FROM buy
            CROSS JOIN member;
        ```
        - Join all rows in one table and all rows in the other table
        - The number of result is multiplication of number or rows from two tables
        - Cannot use ON
        - Since it is randomly joined, the result is meaningless
        - Purpose of cross join is to generate large amounts of data
        
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
## Chapter 5 - Table and View
  ### 5-1 Creating Table
  - Table consists of row(record), column(field)
    #### Create table in GUI
    - Refer Chapter 3-1

  ### 5-2  Table Constraint
  - Constraint: Applying a number of rules to prevent inappropriate data from entering the table and ensure "data integrity"
    - e.g.) Primary Key, Foreign Key, Unique, Check(prevent entering out-of-range data), Default, NOT NULLL

  - Data Integrity: Data is flawless and remains accurate, consistent, and valid

  #### Primary Key Constraint
  - Primary Key: key in a relational database that is unique for each record(Unique identifier).
  - Cannot enter duplicate or NULL value
  - Only 1 PK in each table
  - How to assign PK
    ```SQL
    USE naver_db;
    DROP TABLE IF EXISTS buy, member;
    CREATE TABLE member
    (mem_id CHAR(8) NOT NULL PRIMARY KEY,
    mem_name VARCHAR(10) NOT NULL,
    height TINYINT UNSINGED NULL
    );
    ```
    OR
    ```SQL
    DROP TABLE IF EXISTS member;
    CREATE TABLE member
    (mem_id CHAR(8) NOT NULL,
    mem_name VARCHAR(10) NOT NULL,
    height TINYINT UNSINGED NULL
    PRIMARY KEY(mem_id)
    );
    ```
    OR use "ALTER TABLE"
    ```SQL
    DROP TABLE IF EXISTS member;
    CREATE TABLE member
    (mem_id CHAR(8) NOT NULL,
    mem_name VARCHAR(10) NOT NULL,
    height TINYINT UNSINGED NULL
    );
    ALTER TABLE member
      ADD CONSTRAINT
      PRIMARY KEY(mem_id);
    ```

  - You can add PK name by entering its name after PRIMARY KEY code
    - e.g.) CONSTRAINT PRIMARY KEY "PK_member_mem_id" (mem_id)
  - When deleting table: Drop FK table first. If you drop PK table first, you won't get any data from FK table.

  #### Foreign Key Constraint
  - Foreign Key: Connects two table and ensure data integrity
  - Is conneccted with PK of the other table
  - 기준 테이블: table that has PK / 참조 테이블: table that has FK
  - How to assign FK
    ```SQL
      USE naver_db;
    DROP TABLE IF EXISTS buy, member;
    CREATE TABLE member
    (mem_id CHAR(8) NOT NULL PRIMARY KEY,
    mem_name VARCHAR(10) NOT NULL,
    height TINYINT UNSINGED NULL
    );
    CREATE TABLE buy
    (num INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
    mem_id CHAR(8) NOT NULL,
    prod_name CHAR(6) NOT NULL,
    FOREIGN KEY(mem_id) REFERENCES member(mem_id)
    );
    ```
    OR use ALTER TABLE
    ```SQL
    DROP TABLE IF EXISTS buy;
    CREATE TABLE buy
    (num INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
    mem_id CHAR(8) NOT NULL,
    prod_name CHAR(6) NOT NULL
    );
    ALTER TABLE buy
      ADD CONSTRAINT
      FOREIGN KEY(mem_id)
      REFERENCES member(mem_id);
    ```
    - mem_id of 기준 테이블(member) is PK
    - If 기준 테이블 isn't PK or Unique, FK relationship is unsettled

    #### When 기준 테이블 column changes
    - Error occurs -> if column changes, data doesn't match
    - Deleting column also shows error
    - ON UPDATE CASCADE: Updates both 기준 테이블 and 참조 테이블
    - ON DELETE CASCADE: Deletes both 기준 테이블 and 참조 테이블
    ```SQL
    DROP TABLE IF EXISTS buy;
    CREATE TABLE buy
    (num INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
    mem_id CHAR(8) NOT NULL,
    prod_name CHAR(6) NOT NULL
    );
    ALTER TABLE buy
      ADD CONSTRAINT
      FOREIGN KEY(mem_id) REFERENCES member(mem_id)
      ON UPDATE CASCADE
      ON DELETE CASCADE;
    ```
    - Enter data into buy table
      ```SQL
      INSERT INTO buy VALUES(NULL, 'BLK', 'Wallet');
      INSERT INTO buy VALUES(NULL, 'BLK', 'Macbook');
      ```
    - Change member table BLK into PINK -> No error
      ```SQL
      UPDATE member SET mem_id = 'PINK' WHERE mem_id='BLK';
      ```
    - Delete member table BLK into PINK -> No error
      ```SQL
      DELETE member SET mem_id = 'PINK' WHERE mem_id='BLK';
      ```
    
  ### Other Constraints
  - Unique Constrant: ensures that all values in a column are different
    - Similar with PK constraint but allow NULL
    - Multiple NULL values allowed
    - Multile Unique Keys allowed
    ```SQL
    DROP TABLE IF EXISTS member;
    CREATE TABLE member
    (mem_id CHAR(8) NOT NULL,
    mem_name VARCHAR(10) NOT NULL,
    height TINYINT UNSINGED NULL
    email CHAR(30) NULL UNIQUE
    ); 
    ```
    - Insert data -> Error: email of 3rd row duplicates
    ```SQL
    INSERT INTO member VALUES('BLK', '블랙핑크', '163', 'pink@gamil.com');
    INSERT INTO member VALUES('TWC', '트와이스', '167', NULL);
    INSERT INTO member VALUES('APN', '에이핑크', '164', 'pink@gamil.com');
    ```
  
  - Check Constraint: check input data about the constraint
    - Check if the height is greater than or equal to 100
      ```SQL
      DROP TABLE IF EXISTS member;
      CREATE TABLE member
      (mem_id CHAR(8) NOT NULL,
      mem_name VARCHAR(10) NOT NULL,
      height TINYINT UNSINGED NULL CHECK (height >= 100),
      phone1 CHAR(3) NULL
      );
      ```
    - Error: height of second row violates check constraint
      ```SQL
      INSERT INTO member VALUES('BLK', '블랙핑크', '163', NULL);
      INSERT INTO member VALUES('TWC', '트와이스', '99', NULL);
      ```
    - USE ALTER TABLE for Check Constraint(After CREATE TABLE)
      ```SQL
      ALTER TABLE member
        ADD CONSTRAINT
        CHECK (phone1 IN('02', '031', '032', '054', '055', '061 ));
      ```
    - Error -> Second row phone1 violates check constraint
      ```SQL
      INSERT INTO member VALUES('BLK', '블랙핑크', '163', '02';
      INSERT INTO member VALUES('TWC', '트와이스', '99', '010');
      ```
  - Default Value: Set default value if there's no input
    ```SQL
    DROP TABLE IF EXISTS member;
    CREATE TABLE member
    (mem_id CHAR(8) NOT NULL PRIMARY KEY,
    mem_name VARCHAR(10) NOT NULL,
    height TINYINT UNSINGED NULL DEFAULT 160,
    phone1 CHAR(3) NULL
    );
    ```
    - USE ALTER TABLE
    ```SQL
    ALTER TABLE member
      ALTER COLUMN phone1 SET DEFAULT '02';
    ```
    - To manually enter default value in column
    ```SQL
    INSERT INTO member VALUES('RED', '레드벨벳', '161', '054');
    INSERT INTO member VALUES('SPC', '우주소녀', default, default);
    SELECT * FROM member;
    ```
    - Then go to the column and enter default value
  
  - NULL Value
    - To allow Null value, omit or use NULL
    - Not to allow, use NOT NULL
    - PRIMARY KEY column is automatically recognized as NOT NULL

  ### 5-3 View
  - View: bring a table through select statement
    - Format
      ```SQL
      CREATE VIEW 'View_Name'
      AS
        SELECT ~;
      ```
    - To bring View
      ```SQL
      SELECT col_name FROM view_name
        WHERE ~;
      ```
    - Features of View
      - Can change table data using view (with some conditions)
      - Supoorts security: create view without including essential information, unauthorized users can only approach view
      - Simplify SQL: don't need to repeat complicated queries
        ```SQL
        SELECT B.mem_id, M.mem_name, B.prod_name, M.addr, CONCAT(M.phone1, M.phone2) '연락처'
        FROM buy B
          INNER JOIN member M
          ON B.mem_id = M.mem_id;
        ```
        TO
        ```SQL
        CREATE VIEW v_memberbuy
        AS
          SELECT B.mem_id, M.mem_name, B.prod_name, M.addr, CONCAT(M.phone1, M.phone2) '연락처'
          FROM buy B
            INNER JOIN member M
            ON B.mem_id = M.mem_id;
        ```

    #### Operating View
    - Create View
      - Alias: New name for view column, space allowed, write AS to clarify code, when selecting view use `
        ```SQL
        USE market_db;
        CREATE VIEW v_viewtest1
        AS
          SELECT B.mem_id 'Member ID', M.mem_name AS 'Member Name', B.prod_name "Product Name", CONCAT(M.phone1, M.phone2) AS "Office Phone"
          FROM buy B
            INNER JOIN member M
            ON B.mem_id = M.mem_id;

        SELECT DISTINCT `Member ID`, `Member Name` FROM v_viewtest1;
        ```
    - Modify View
      - 'ALTER VIEW' statement
        ```SQL
        ALTER VIEW v_viewtest1
        AS
          SELECT B.mem_id 'Member ID', M.mem_name AS 'Member Name', B.prod_name "Product Name", CONCAT(M.phone1, M.phone2) AS "Office Phone"
            FROM buy B
              INNER JOIN member M
              ON B.mem_id = M.mem_id;

        SELECT DISTINCT `Member ID`, `Member Name` FROM v_viewtest1;
        ```
    - Delete View
      - DROP VIEW
        ```SQL
        DROP VIEW v_viewtest1;
        ```
    - Check View Info
      - DESCRIBE or DESC, PK is not shown in View
        ```SQL
        DESCRIBE v_viewtest2;
        ```
      - SHOW CREATE VIEW
        ```SQL
        SHOW CREATE VIEW v_viewtest2;
        ```

    - View Data Update / Delete
      - UPDATE
        ```SQL
        UPDATE v_member SET addr = 'Busan' WHERE mem_id='BLK';
        ```

      - INSERT INTO (Table column with NOT NULL must be included in View)
        ```SQL
        INSERT INTO v_member(mem_id, mem_name, addr) VALUES('BTS', '방탄소년단', '경기');
        ```
        - Create View with Condition
          ```SQL
          CREATE VIEW v_height167
          AS
            SELECT * FROM member WHERE heigth >= 167;

          SELECT * FROM v_height167;
          ```
        - WITH CHECK OPTION: doesn't allow value out of range
          - Error: only allows value equal or greater than 167
            ```SQL
            ALTER VIEW v_height167
            AS
                SELECT * FROM member WHERE heigth >= 167
                  WITH CHECK OPTION;

            INSERT INTO v_height167 VALUES('TOB', '텔레토비', 4, '영국', NULL, NULL, 140, '1995-01-01');
            ```

      - DELETE data from View
        ```SQL
        DELETE FROM v_height167 WHERE height < 167;
        ```

    - Complex View
      - View made up of more than two tables, used when creating View with joined table
      - View made with one table is called Simple View
        ```SQL
        CREATE VIEW v_complex
        AS
          SELECT B.mem_id, M.mem_name, B.prod_name, M.adder
            FROM buy B
              INNER JOIN member M
              ON B.mem_id = M.mem_id;
        ```

    - Delete Reference Table
      - Table can be deleted even though View exists
      - If select View after deleting Table, error occurs
        ```SQL
        DROP TABLE IF EXISTS buy, member;
        ```
      
## CH6 - Index
### 6-1 Index Concept
- Index: tool to find data quickly
  - Clustered Index: When assigned Primary Key, automatically created and aligned based on PK column. Only one for each table.
  - Secondary Index: When assigned Unique Key, can make many secondary index but not automatically aligned
  -  Index Pros and Cons
      - Pros
        - Search speed improves
        - Increased computer performance with less burden
      - Cons
        - Additional space needed for index(10% of table size)
        - Takes time to make initial index
        - Frequent changes in data(INSERT, UPDATE, DELETE) would worsen computer performance

  #### Index Types
   - Clustered Index: Automatically create Clustered Index on 'mem_id' column
      ```SQL
      CREATE TABLE member
      (mem_id CHAR(8) NOT NULL PRIMARY KEY,
      mem_name VARCHAR(10) NOT NULL,
      mem_number INT NOT NULL,
      ...)
      ```
    - To check
      - Key_name will show 'PRIMARY' which means it's automatically created index
      - Column_name 'col1' means there's index on col1
      - Non_Unique: 0 means FALSE(Unique), 1 means TRUE(Non_Unique)
        ```SQL
        SHOW INDEX FROM table1;
        ```
    - Change PK and new Clustered Index
      - It can take much time to change PK and it shouldn't be duplicated
        ```SQL
        ALTER TABLE member DROP PRIMARY KEY;
        ALTER TABLE member
          ADD CONSTRAINT
          PRIMARY KEY(mem_name);
        SELECT * FROM member;
        ```
  
  - Secondary Index
    - Unique key also automatically creates secondary index
    - column name is shown on Key_name('col2','col3')
    - Can make many secondary index
      ```SQL
      CREATE TABLE table2
      (col1 INT PRIMARY KEY,
      col2 INT UNIQUE,
      col3 INT UNIQUE
      );
      SHOW INDEX FROM table2;
      ```

    - Adding Secondary Index
      ```SQL
      ALTER TABLE member
        ADD CONSTRAINT
        UNIQUE(mem_id);
      SELECT * FROM member;
      ```

    - Add new data
      - Added on the very last of the table
        ```SQL
        INSERT INTO member VALUES('GRL', '소녀시대', 8, '서울');
        SELECT * FROM member;
        ```

### 6-2 Index Inner Process
- Index is composed of 'Balanced Tree(B-tree)'

#### Balanced Tree
- Balanced Tree: data structure used with Clustered and Nonclustered indexes to make data retrieval faster and easier
  - 3 Nodes(Space where data is stored): Root, Internal, Leaf
  - In MySQL, node is called 'page'
  - Page: the smallest store unit, 16Kbyte
    - Even if the user enter 1 data, 1 page is needed
  - Every node starts from root node and ends at leaf node
  
  #### B-tree page division
  - Page division: dividing data with new page
    - slow down the speed and performance of SQL
    - When inserting new data and if there's not enough space in page, page division will occur which will make extra pages which takes time

#### Index Structure
#### Clustered Index
```SQL
USE market_db;
CREATE TABLE cluster
(mem_id CHAR(8), mem_name VARCHAR(10));
INSERT INTO cluster VALUES('TWC', '트와이스');
INSERT INTO cluster VALUES('BLK', '블랙핑크');
INSERT INTO cluster VALUES('WMN', '여자친구');
INSERT INTO cluster VALUES('OMY', '오마이걸');
INSERT INTO cluster VALUES('GRL', '소녀시대');
INSERT INTO cluster VALUES('ITZ', '잇지');
INSERT INTO cluster VALUES('RED', '레드벨벳');
INSERT INTO cluster VALUES('APN', '에이핑크');
INSERT INTO cluster VALUES('SPC', '우주소녀');
INSERT INTO cluster VALUES('MMU', '마마무');
```
```SQL
ALTER TABLE cluster
  ADD CONSTRAINT
  PRIMARY KEY (mem_id);
```
- Suppose 1 page can contain 4 data
- B-tree of the index
  - ![Clustered B-tree](https://github.com/CHOIJOONBEOM/SQL-Study/blob/main/%ED%99%94%EB%A9%B4%20%EC%BA%A1%EC%B2%98%202022-10-16%20010017.png)

#### Secondary Index
```SQL
USE market_db;
CREATE TABLE second
(mem_id CHAR(8), mem_name VARCHAR(10));
INSERT INTO cluster VALUES('TWC', '트와이스');
INSERT INTO cluster VALUES('BLK', '블랙핑크');
INSERT INTO cluster VALUES('WMN', '여자친구');
INSERT INTO cluster VALUES('OMY', '오마이걸');
INSERT INTO cluster VALUES('GRL', '소녀시대');
INSERT INTO cluster VALUES('ITZ', '잇지');
INSERT INTO cluster VALUES('RED', '레드벨벳');
INSERT INTO cluster VALUES('APN', '에이핑크');
INSERT INTO cluster VALUES('SPC', '우주소녀');
INSERT INTO cluster VALUES('MMU', '마마무');
```
```SQL
ALTER TABLE second
  ADD CONSTRAINT
  UNIQUE (mem_id);
SELECT * FROM second;
```
- Suppose 1 page can contain 4 data
- B-tree of the index
  - ![Secondary B-tree](https://github.com/CHOIJOONBEOM/SQL-Study/blob/main/Secondary%20Index.png)
  
#### Select data from Index
1. Read Root page and check which leaf page to find
2. Read leaf page
3. Find data from leaf page
- Clustered index is faster than Secondary Index in selecting data

### 6-3 Real Use of Index
- Create Index(Secondary Index)
  ```SQL
  CREATE [UNIQUE] INDEX index_name
    ON table_name (column_name) [ASC | DESC]
  ```

- Drop Index
  ```SQL
  DROP INDEX index_Name ON table_name
  ```
  - Cannot drop PK or Unique key Index
  - Should use ALTER TABLE statement to delete PK or Unique key, then drop Index
  - Better to delete Secondary Index first: if delete clustered index first, it will reorganize data
  - Should drop index that is not used

- Create Index Example
    ```SQL
    USE market_db;
    SELECT * FROM member;
    ```
  - Check which index is in member table (clustered index in mem_id)
    ```SQL
    SHOW INDEX FROM member;
    ```
  - Check Index size
    - Data_length: size of clustered index
    - Index_length: size of secondary index
    ```SQL
    SHOW TABLE STATUS LIKE 'member';
    ```

  - Create Secondary Index
    ```SQL
    CREATE INDEX idx_member_Addr
      ON member(addr);
    ```
  - Apply Created Index
    ```SQL
    ANALYZE TABLE member;
    SHOW TABLE STATUS LIKE 'member';
    ```

- Using Index
  - Check Index
    ```SQL
    ANALYZE TABLE member;
    SHOW INDEX FROM member;
    ```
  - Select Indexed column and Check Execution Plan
    ```SQL
    SELECT mem_id, mem_name, addr
      FROM member
      WHERE mem_name = '에이핑크';
    ```
    - Execution Plan will show 'Single Row(constant)' -> Index is used

  - Selecting data using number
    ```SQL
    SELECT mem_name, mem_number
      FROM member
      WHERE mem_number >= 7;
    ```

  - Cases not using Index
    - SQL will determine whether to use index based on the workload (mem_number>=1 should bring most of the columns)
      ```SQL
      SELECT mem_name, mem_number
        FROM member
        WHERE mem_number >= 1;
      ```
    - When there's calculation in WHERE
      ```SQL
      SELECT mem_name, mem_number
        FROM member
        WHERE mem_number*2 >= 14;
      ```
      - This code will use index since operator not used in WHERE
      ```SQL
      SELECT mem_name, mem_number
        FROM member
        WHERE mem_number >= 14/2;
        ```

  - Drop Index
    - Check Index name
      ```SQL
      SHOW INDEX FROM member;
      ```

    - Drop Secondary Index
      ```SQL
      DROP INDEX idx_member_mem_name ON member;
      DROP INDEX idx_member_addr ON member;
      DROP INDEX idx_member_mem_number ON member;
      ```

    - Drop Clustered Index
      ```SQL
      ALTER TABLE member
        DROP PRIMARY KEY;
      ```

    - If error occurs, that's because of PK-FK relationship
    - Find FK name
      ```SQL
      SELECT table_name, constraint_name
        FROM information_schema.referential_constraints
        WHERE constraint_schema = 'market_db';
      ```

    - Delete FK and PK
      ```SQL
      ALTER TABLE buy
        DROP FOREIGN KEY buy_ibfk_1;
      ALTER TABLE member
        DROP PRIMARY KEY;
      ```

## CH7 - Stored Procedure
### 7-1 How to Use Stored Procedure
- Stored Procedure: Programming function provided by MySQL
  - Combination of Queries
  - Batch processing of actions

- Create Stored Procedure
  - Instead of $$, can use $, ##, %%, &&, // etc.
  ```SQL
  DELIMITER $$
  CREATE PROCEDURE stored_procedure_name(IN or OUT parameter)
  BEGIN

  WRITE CODE HERE

  END $$
  DELIMITER;
  ```

- Call Stored Procedure
  ```SQL
  CALL stored_procedure_name();
  ```

- Delete Stored Procedure
  ```SQL
  DROP PROCEDURE user_proc;
  ```

- IN or OUT parameter
  - IN parameter
    ```SQL
    CREATE PROCEDURE procedure_name(IN in_parameter_name Data_Type);
    CALL procedure_name(value);
    ```
    - Example: send '에이핑크' to userName parameter and selects data whose userName('에이핑크') is mem_name
    ```SQL
    DELIMITER $$
    CREATE PROCEDURE user_proc1(IN userName VARCHAR(10))
    BEGIN
      SELECT * FROM member WHERE mem_name = userName;
    END $$
    DELIMITER;

    CALL user_proc1('에이핑크');
    ```

  - OUT parameter
    ```SQL
    CREATE PROCEDURE procedure_name(OUT out_parameter_name Data_Type);
    CALL procedure_name(@variable_name);
    SELECT @variable_name;
    ```
    - Example
     ```SQL
    DELIMITER $$
    CREATE PROCEDURE user_proc3(IN txtValue CHAR(10), OUT outValue INT)
    BEGIN
      INSERT INTO noTable VALUES(NULL,txtValue);
      SELECT MAX(id) INTO outValue FROM noTable;
    END $$
    DELIMITER;
    ```
    - There's no table called noTable, so create one
      - Although table doesn't exist, it is able to create stored procedure but impossible to call
    ```SQL
    CREATE TABLE IF NOT EXISTS noTable(
      id INT AUTO_INCREMETN PRIMARY KEY,
      txt CHAR(10)
    );
    ```
    - Call Stored Procedure
    ```SQL
    CALL user_proc3('테스트1', @myValue);
    SELECT CONCAT('입력된 ID 값 ==>', @myValue);
    ```


  - Dynamic SQL using Procedure
  ```SQL
  DROP PROCEDURE IF EXISTS dynamic_proc;
  DELIMITER $$
    CREATE PROCEDURE dynamic_proc(IN tableName VARCHAR(20))
    BEGIN
      SET @sqlQuery = CONCAT('SELECT * FROM ', tableName);
      PREPARE myQuery FROM @sqlQuery;
      EXECUTE myQuery;
      DEALLOCATE PREPARE myQuery;
    END $$
    DELIMITER;

    CALL dynamic_proc('member');
  ```

### 7-2 Stored Function and Cursor
### Stored Function
  - Definition: Defined(Created) function that returns a single value
  - 'RETURNS' statement assigns data type of returning value
  - All parameters in Stored Function are Input parameters (No IN)
  - Use Select to bring function (Stored Procedure: CALL)
  - Cannot use SELECT inside Stored Function Code (Stored Procedure: can use SELECT)
  ```SQL
  DELIMITER $$
  CREATE FUNCTION stored_function_name(parameter)
    RETURNS reteive format
  BEGIN
    coding
    RETURN retreive value;
  END $$
  DELIMITER;
  SELECT stored_function_name();
  ```

  #### Using Stored Function
  - First, should quthorize stored function
    ```SQL
    SET GLOBAL log_bin_trust_function_creators = 1;
    ```
  - Stored function of adding to numbers
    ```SQL
    USE market_db;
    DROP FUNCTION IF EXISTS sumFunc;
    DELIMITER $$
    CREATE FUNCTION sumFunc(number1 INT, number2 INT)
      RETURNS INT
    BEGIN
      RETURN number1 + number2;
    END $$
    DELIMITER;

    SELECT sumFunc(100, 200) AS 'SUM'; --> 300
    ```

  - Stored function of calculating years since debut
    ```SQL
    DROP FUNCTION IF EXISTS calcYearFunc;
    DELIMITER $$
    CREATE FUNCTION calcYearFunc(dYear INT)
      RETURNS INT
    BEGIN
      DECLARE runYear INT;
      SET runYear = YEAR(CURDATE()) - dYear;
      RETURN runYear;
    END $$
    DELIMITER;

    SELECT calcYearFunc(2010) AS 'Years of activity'; --> 12(current year 2022)
    ```
    - Store data using SELECT INTO to get result
      ```SQL
      SELECT calcYearFunc(2007) INTO @debut2007;
      SELECT calcYearFunc(2013) INTO @debut2013;
      SELECT @debut2007-@debut2013 AS 'Year between 2007 and 2013';
      ```

  - Deleting Function
    ```SQL
    DROP FUNCTION calcYearFunc;
    ```
  
  ### Cursor
  - Definition: A way to manage each row in table (first row to the end row)
  - Mostly used with Stored Procedure
  - Steps for using Cursor
    1. Declare Cursor
    2. Declare Repetitive Condition
    3. Open Cursor
    4. Bring data and process
    5. End Cursor
  
  #### Steps
1. Set variables
    ```SQL
    DECLARE memNumber INT;
    DECLARE cnt INT DEFAULT 0; -- counted row num
    DECLARE totNumber INT DEFAULT 0; -- number of total member
    DECLARE endOfRow BOOLEAN DEFAULT FALSE; -- variable to know the end of row
    ```

2. Declare Cursor
    ```SQL
    DECLARE memberCursor CURSOR FOR
      SELECT mem_number FROM member;
    ```

3. Declare Repetitive Condition
    ```SQL
    DECLARE CONTINUE HANDLER
      FOR NOT FOUND SET endOfRow = TRUE;
    ```
    - FOR NOT FOUND sets 'endOfRow = TRUE' if there's no row to implement
  
4. Open Cursor
    ```SQL
    OPEN memberCursor;
    ```

5. Bring data and process(Loop rows)
    ```SQL
    cursor_loop: LOOP
      FETCH memberCursor INTO memNumber;
 
      IF endOfRow THEN
        LEAVE cursor_loop;
      END IF;

      SET cnt = cnt + 1;
      SET totNumber = totNumber + memNumber;
    END LOOP cursor_loop;
    ```

6. Close Cursor
    ```SQL
    CLOSE memberCursor;
    ```

  #### Code with Cursor
  ```SQL
  USE market_db;
  DROP PROCEDURE IF EXISTS cursor_proc;
  DELIMITER $$
  CREATE PROCEDURE cursor_proc()
  BEGIN
    DECLARE memNumber INT;
    DECLARE cnt INT DEFAULT 0;
    DECLARE totNumber INT DEFAULT 0;
    DECLARE endOfRow BOOLEAN DEFAULT FALSE;
    
    DECLARE memberCursor CURSOR FOR
      SELECT mem_number FROM member;
    
    DECLARE CONTINUE HANDLER
      FOR NOT FOUND SET endOfRow = TRUE;

    OPEN memberCursor;

    cursor_loop: LOOP
      FETCH memberCursor INTO memNumber;
 
      IF endOfRow THEN
        LEAVE cursor_loop;
      END IF;

      SET cnt = cnt + 1;
      SET totNumber = totNumber + memNumber;
    END LOOP cursor_loop;

    SELECT (totNumber/cnt) AS 'Average number of members';

    CLOSE member Cursor;
  END $$
  DELIMITER;
  ```

  ```SQL
  CALL cursor_proc(); -- result: 6.6
  ```

### 7-3 Trigger
- Trigger: Code that automatically runs when there's DML statement(INSERT, UPDATE, DELETE)
  - DML(Data Manipulation Language)
- Prevent users from making mistakes (prevent error in data = Maintain data integrity)
  - e.g.) If a member leaves group, we can use trigger to auto save data to look up later
- Mostly AFTER trigger is used (BEFORE trigger is less used)
- Can use CALL statement, IN/OUT parameter to use Trigger
- Can attach numerous triggers on one table

### Example 1
- Create Table
  ```SQL
  USE market_db;
  CREATE TABLE IF NOT EXISTS trigger_table (id INT, txt VARCHAR(10));
  INSERT INTO trigger_table VALUES(1, 'Red Velvet');
  INSERT INTO trigger_table VALUES(2, 'ITZY');
  INSERT INTO trigger_table VALUES(1, 'Black Pink');
  ```
- Attach Trigger to table
  ```SQL
  DROP TRIGGER IF EXISTS myTrigger;
  DELIMITER $$
  CREATE TRIGGER myTrigger
    AFTER DELETE
    ON trigger_table
    FOR EACH ROW
  BEGIN
    SET @msg = 'Singer Group is deleted'; -- Code when trigger runs
  END $$
  DELIMITER;
  ```
- Using INSERT or UPDATE -> Trigger doesn't run(@msg returns nothing)
  ```SQL
  set @msg = '';
  INSERT INTO trigger_table VALUES(4, 'MAMAMU');
  SELECT @msg;
  UPDATE trigger_table SET txt = '블핑' WHERE id = 3;
  SELECT @msg;
  ```

- Using DELETE -> Trigger runs(@msg returns 'Singer Group is deleted')
  ```SQL
  DELETE FROM trigger_table WHERE id=4;
  SELECT @msg;
  ```

### Example 2
- Trigger that records data when table(member) information is changed
- Create table
  ```SQL
  USE market_db;
  CREATE TABLE singer(SELECT mem_id, mem_name, mem_number, addr FROM member);
  ```
- Create backup table to save data
  ```SQL
  CREATE TABLE backup_singer
  (mem_id CHAR (8) NOT NULL,
   mem_name VARCHAR(10) NOT NULL,
   mem_number INT NOT NULL,
   addr CHAR(@) NOT NULL,
   modType CHAR(@), -- Type of change: 'Insert' or 'Delete'
   modDate DATE, -- Changed Date
   modUser VARCHAR(30) -- User who changed the information
  );
  ```

- Attach Trigger to singer table - UPDATE
  ```SQL
  DROP TRIGGER IF EXISTS singer_updateTrg;
  DELIMITER $$
  CREATE TRIGGER singer_updateTrg
    AFTER UPDATE
    ON singer
    FOR EACH ROW
  BEGIN
    INSERT INTO backup_singer VALUES(OLD.mem_id, OLD.mem_name, OLD.mem_number, OLD.addr, 'Update', CURDATE(), CURRENT_USER());
  END $$
  DELIMITER;
  ```
  - OLD table is a temporary table supported by MySQL where data is stored before UPDATE or DELETE
  - When UPDATE runs at OLD table, pre-updated data is inserted into backup table(backup_singer)

- Attach Trigger to singer table - DELETE
  ```SQL
  DROP TRIGGER IF EXISTS singer_deleteTrg;
  DELIMITER $$
  CREATE TRIGGER singer_deleteTrg
    AFTER DELETE
    ON singer
    FOR EACH ROW
  BEGIN
    INSERT INTO backup_singer VALUES(OLD.mem_id, OLD.mem_name, OLD.mem_number, OLD.addr, 'Delete', CURDATE(), CURRENT_USER());
  END $$
  DELIMITER;
  ```

- Change data
  ```SQL
  UPDATE singer SET addr = 'England' WHERE mem_id = 'BLK';
  DELETE FROM singer WHERE mem_number >= 7;
  ```

- Check the result
  ```SQL
  SELECT * FROM backup_singer;
  ```
  - If we use 'TRUNCATE TABLE table_name' to delete, Trigger doesn't run since it's not a DELETE statement

### Temporary table run by MySQL
- NEW / OLD table: created when using INSERT, UPDATE, DELETE
- INSERT(NEW): before new value is inserted into the table, it is temporarily stored in NEW table
- DELETE(OLD): value that is going to be deleted is temporarily stored in OLD table
- UPDATE(NEW, OLD): new value is temporarily stored in NEW table and old value is temporarily stored in OLD table

## Chapter 8 - Connecting SQL and Python
### 8-1 Preparing Python Environment
- PyMySQL: External library that is needed to connect MySQL and Python
- Benefits of using Python
  - Strong functions for free
  - Easy to install and build environment
  - Various and strong External Libraries(function supplied by the external developers)
- Modes of Python
  - Interpreter Mode: Read source code one by one and execute right away
  - Script Mode: Write several rows of code and execute

### 8-2 Linking Python and MySQL
- After installing PyMySQL, user can create 'Database Link Program'
- By using program, users can use database with simple code without learning SQL

#### Practice
#### Create DB
```SQL
DROP DATABASE IF EXISTS soloDB;
CREATE DATABASE soloDB;
```
#### Insert data using Python
- Steps
  1. Connect MySQL
  2. Create Cursor
  3. Create table
  4. Insert data
  5. Store inserted data
  6. Close MySQL

- Exercise
1. Connect MySQL
    ```SQL
    import pymysql
    conn = pymysql.connect(host='127.0.0.1', user='root', password='0000', db='soloDB', charset='utf8') --utf8 is charset for Korean words
    ```

2. Create Cursor
    ```SQL
    cur = conn.cursor()
    ```

3. Create table
    ```SQL
    cur.execute("CREATE TABLE userTable(id char(4), userName char(15), email char(20), birthYear int)") -- cur.execute() is executed on the database connected to SQL statement
    ```

4. Insert data
    ```SQL
    cur.execute("INSERT INTO userTable VALUES('hong', '홍지윤', 'hong@naver.com', 1996)")
    cur.execute("INSERT INTO userTable VALUES('kim', '김태연', 'kim@daum.net', 2011)")
    cur.execute("INSERT INTO userTable VALUES('star', '별사랑', 'star@paran.com', 1990)")
    cur.execute("INSERT INTO userTable VALUES('yang', '양지은', 'yang@gmail.com', 1993)")
    ```

5. Store inserted data
    ```SQL
    conn.commit() -- conn.commit() stores inserted data
    ```

6. Close MySQL
    ```SQL
    conn.close()
    ```

#### Using Link Programming
- Creating loop code for user to enter data
  ```SQL
  import pymysql

  -- Declare variables
  conn, cur = None, None
  data1, data2, data3, data4 = "", "", "", ""
  sql=""

  -- Main Code
  conn = pymysql.connect(host='127.0.0.1', user='root', password='0000', db='soloDB', charset='utf8')
  cur = conn.cursor()

  while(True):
    data1 = input("USER ID ==> ")
    if data1 == "": -- if press enter the loop ends
      break;
    data2 = input("USER NAME ==> ")
    data3 = input("USER EMAIL ==> ")
    data4 = input("USER BIRTH YEAR ==> ")
    sql = "INSERT INTO userTable VALUES('" + data1 + "', '" + data2 + "', '" + data3 + "', " + data4 + ")" -- data4 is integer so don't bind with ''
    cur.execute(sql)

  onn.commit()
  conn.close()
  ```

- Result
  ```SQL
  USER ID ==> su
  USER NAME ==> 수지
  USER EMAIL ==> suji@hanbit.com
  USER BIRTH YEAR ==> 1994
  ...Repeat
  USER ID ==> -- if press enter, loop ends
  ```

#### Selecting MySQL data using Python
1. Connect MySQL
2. Create Cursor
3. Select data
4. Print data
5. Clost MySQL

  ```SQL
  import pymysql

  -- Declare variables
  conn, cur = None, None
  data1, data2, data3, data4 = "", "", "", ""
  row=None

  -- Main Code
  conn = pymysql.connect(host='127.0.0.1', user='root', password='0000', db='soloDB', charset='utf8')
  cur = conn.cursor()

  cur.execute("SELECT * FROM userTable") -- selected data is stored at cur variable

  print("USER ID   USER NAME    EMAIL   BIRTH YEAR")
  print("-----------------------------------------")

  while(True):
    row = cur.fetchone() -- fetchone() function prints each row of selected data
    if row== None:
      break
    data1 = row[0]
    data2 = row[1]
    data3 = row[2]
    data3 = row[3]
    print("%5s    %15s    %20s    %d" % (data1, data2, data3, data4)) -- data stored with tuple format

  conn.close()
  ```

### 8-3 GUI applictaion program
- GUI(Graphical User Interface): Inteface used in Window graphic environment
  - using python, we can use GUI application program
  - tkinter: Window library used that supports GUI module

- GUI composition
  ```SQL
  from tkinter import *

  root = Tk() -- Tk() retrieves base Window
  # code written here
  root.title("GUI PRACTIVE") -- Window title
  root.gemetry("400x200") -- Window size

  root.mainloop() -- functions used for an event
  ```

  - Label: widget for text
    - widget: word that represents button, text, radio button, image, etc.
    - All widgets should use pack()
    ```SQL
    from tkinter import *
    root = Tk()
    root.geometry("300x100")

    label1 = Label(root, text="혼공 SQL은")
    label2 = Label(root, text="쉽습니다.", font = ("궁서체", 30), bg="blue", fg="yellow") -- fg = foreground, bg = background

    label1.pack()
    label2.pack()

    root.mainloop()
    ```

  - Button: event starts when button is clicked
    - command option: user has command on button
    ```SQL
    from tkinter import *
    from tkinter import messagebox

    def clickButton() :
      messagebox.showinfo('버튼 클릭', '버튼을 눌렀습니다..')

    root = Tk()
    root.geometry("200x200")

    button1 = Button(root, text="여기를 클릭하세요", fg="red", bg="yellow", command=clickButton) -- msgbox shows when button is clicked
    button1.pack(expand = 1) -- shows button in the middle of the screen

    root.mainloop()
    ```

  - Widget Arrangement
    - pack(side=LEFT): from the left
    - you can put RIGHT, TOP, BOTTOM instead of LEFT
    ```SQL
    from tkinter import *

    root = Tk()

    button1 = Button(root, text="혼공1")

    button1.pack(side=LEFT)

    root.mainloop()
    ```

    - Space between widget: "padx=pixel value" or "pady=pixel value"
      ```SQL
      from tkinter import *

      root = Tk()

      button1 = Button(root, text="혼공1")

      button1.pack(side=TOP, fill =X, padx=10, pady=10)

      root.mainloop()
      ```

  - Frame, Entry, List box
    - Frame: diviide screen into a number of sections
    - Entry: shows input box
    - Listbox: shows list
    ```SQL
    from tkinter import *
    root = Tk()
    root.geometry("200x250")

    upFrame = Frame(root)
    upFrame.pack()
    downFrame = Frame(root)
    downFrame.pack()
    -- 2 frame(upFrame, downFrame) added. Not shown on the screen

    editBox = Entry(upFrame, width = 10) -- entry appear on upFrame
    editBox.pack(padx = 20, pady = 20)

    listbox = Listbox(downFrame, bg = 'yellow') -- listbox appear on downFrame
    listbox.pack()

    listbox.insert(END, "하나") -- END means insert data in the end
    listbox.insert(END, "둘")
    listbox.insert(END, "셋")

    root.mainloop()
    ```



