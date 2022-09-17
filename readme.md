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
- Select Data
```SQL
SELECT * FROM member;
SELECT * FROM buy;
```
- Designating Database
```SQL
USE market_db;
```
