#mysql

## Basic Install

```
brew install mysql
gem install mysql2
view Gemfile
bundle install
mysql --version
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
cal MySQL server through socket '/tmp/mysql.sock' (2)
```


### Login
1. ไม่ต้องใช้ password `mysql -u≤root≥`
2. การตั้ง password `mysqladmin -u≤root≥ password`
3. ใส่ password ให้เห็นเลย `mysql -u≤root≥ -p≤root≥`
4. ใส่ password แบบซ่อน `mysql -u≤root≥ -p` แล้วจะมีให้กรอก password
5. login ผ่าน xamp `/Applications/xampp/xamppfiles/bin/mysql -u≤root≥ -p`

## Basic Command

### Databases
ตรวจสอบว่ามี databases ชื่ออะไรบ้าง เหมือน ชื่อ folder เก็บงาน
```
SHOW DATABASES
```

จะเจออะไรประมาณนี้
```
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| mysql              |
	| performance_schema |
	| test               |
	+--------------------+
	5 rows in set (0.20 sec)
```

ถ้าเราไม่อยากแจ้งเตื่อน error ใส่ IF NOT EXISTS ไปด้วย
```
CREATE DATABASE «IF NOT EXISTS» ≤ชื่อที่อยากตั้งเช่น simplify_mysql≥
```

ระบุชื่อ databases ที่อยากใช้
```
USE ≤simplify_mysql≥
```

ลบ databases
```
DROP DATABASE ≤ชื่อฐานข้อมูลเช่นsimplify_mysql≥
```

### Tables
ตรวจสอบว่ามี tables ชื่ออะไรบ้าง ใน db นั้นๆ
```
SHOW TABLES
```

ดูรายละเอียด field ใน table 
```
DESCRIBE ≤ชื่อตาราง≥
```

#### ทำความเข้าใจพื้นฐาน
##### ชนิดของข้อมูล
1. Number
	* TINYINT(127)
	* SMALLINT(32,767)
	* MEDIUMINT(8,388,607)
	* INT(2,147,483,647)
	* BIGINT(9,223,372,036,854,775,807)
	* FLOAT(7,4): 123.4567 7คือเลขทั้งหมดไม่รวม -/. 4คือ ทศนิยม
	* DOUBLE(7,4): 123.4567 7คือเลขทั้งหมดไม่รวม -/. 4คือ ทศนิยม

2. String
	* CHAR(255): ถ้าใช้ไม่ครบมันจะเติมช่องว่างจนเต็มเลยไม่นิยม
	* VARCHAR(255)
	* TINYTEXT(255)
	* TEXT(65,535)
	* MEDIUMTEXT(16,777,215)
	* LONGTEXT(4,294,967,295)

3. ARRAY
	* ENUM: แนวradio เลือกได้1จากทั้งหมด เริ่มจาก 1
		
		การสร้าง

		```
		CREATE TEMPORARY TABLE test 
		(
		id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, 
		data ENUM('ant','book','cat')
		);
		```
		
		การเพิ่ม

		```
		INSERT INTO ≤ชื่อตาราง≥ SET ≤ชื่อฟิลด์≥ = 1
		INSERT INTO ≤ชื่อตาราง≥ SET ≤ชื่อฟิลด์≥ = 2
		INSERT INTO ≤ชื่อตาราง≥ SET ≤ชื่อฟิลด์≥ = 3
		```

		การเปิดดู

		```
		SELECT ≤*≥ FROM ≤ชื่อตาราง≥

		+----+------+
		| id | data |
		+----+------+
		|  1 | ant  |
		|  2 | book |
		|  3 | cat  |
		+----+------+
		```

	* SET: แนวcheckbox เลือกได้หลายตัวเลือกจากทั้งหมด

	การสร้าง

	```
	CREATE TEMPORARY TABLE test2 
	(
	id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, 
	data SET('ant','book','cat')
	);
	```
	
	การเพิ่ม
	* อยากได้ตัวแรก 2 ^ 0
	* อยากได้ตัวสอง 2 ^ 1
	* แบบนี้ไปเรื่อยๆ แล้วบวกเลขได้ผลรวมไหนก็ได้ set นั้น

	```
	INSERT INTO test2 SET data = 1;
	INSERT INTO test2 SET data = 3;
	INSERT INTO test2 SET data = 7;
	```

	การเปิดดู

	```
	SELECT ≤*≥ FROM ≤ชื่อตาราง≥

	+----+--------------+
	| id | data         |
	+----+--------------+
	|  1 | ant          |
	|  2 | ant,book     |
	|  3 | ant,book,cat |
	+----+--------------+
	```

4. DATE
	* DATE
	* TIME
	* DATETIME
	* YEAR
	* TIMESTAMP: YYYMMDDHHMMSS
		* TIMESTAMP(14): YYYYMMDDHHMMSS
		* TIMESTAMP(12): YYMMDDHHMMSS
		* TIMESTAMP(10): YYMMDDHHMM
		* TIMESTAMP(8): YYYYMMDD
		* TIMESTAMP(6): YYMMDD
		* TIMESTAMP(4): YYMM
		* TIMESTAMP(2): YY

5. Binary Large OBject
	* BLOB(64KB)
	* TINYBLOB(1b)
	* MEDIUMBLOB(16MB)
	* LONGBLOB(4GB)

##### Attribute ของ Field
	* NOT NULL: ห้ามว่าง ปกติว่างได้
	* UNSIGNED: ไม่เอาเลขติดลบทำให้เพิ่มชุดข้อมูลได้มากขึ้น
	* BINARY: เป็น case sinsitive
	* AUTO_INCREMENT: เพิ่มเลขให้เองกำหนดได้ว่าจะให้เริ่มจาเลขไหนได้ด้วย
	* DEFAULT: กำหนดค่าเริ่มต้นให้ field นั้นๆ
	* INDEX: ไปจัดทำ A-Z ไว้อีกไฟล์ได้หาเร็วขึ้น
	* UNIQUE: ใน column นั้นๆห้ามมีชื่อซ้ำกัน
	* PRIMARY: KEY ใน column นั้นๆห้าม ซ้ำใช้ร่วม cloumn ได้โดยรวมกันต้องไม่ซ้ำ

#### คำสั่งที่เกี่ยวข้องกับ Table

##### สร้างตาราง

แบบรวม attribute ไว้หลังแต่ละ field

```
	CREATE «IF NOT EXISTS» «TEMPORARY» TABLE ≤ชื่อตาราง≥
	(
	≤id≥ «INT» «NOT NULL» «AUTU_INCREMENT» «PRIMARY KEY»,
	≤name≥ «VARCHAR(10)» DEFAULT 'abc',
	≤surname≥ «VARCHAR(10)» BINARY UNIQUE,
	≤nickname≥ «VARCHAR(10)» BINARY,
	≤phone≥ «INT(10)» UNSIGNED,
	) «ENGINE = «MYISAM || INNODB»»
```

แบบรวม attribute ไว้ตอนจบสามารถรวมcolumn เพื่อกำหนดสิทธิได้

```
	CREATE «IF NOT EXISTS» «TEMPORARY» TABLE ≤ชื่อตาราง≥
	(
	≤id≥ «INT» «NOT NULL» «AUTU_INCREMENT»,
	≤name≥ «VARCHAR(10)» DEFAULT 'abc',
	≤surname≥ «VARCHAR(10)» BINARY UNIQUE,
	≤nickname≥ «VARCHAR(10)» BINARY,
	≤phone≥ «INT(10)» UNSIGNED,

	PRIMARY KEY(≤name≥, ≤surname≥),
	INDEX(≤name≥),
	UNIQUE(≤surname≥, ≤phone≥)
	) «ENGINE = «MYISAM || INNODB»»
```

แบบสร้างมาจากบางส่วยของตารางอื่น

```
	CREATE «IF NOT EXISTS» «TEMPORARY» TABLE ≤ชื่อตาราง≥
		SELECT ≤เอามาบางcolumn≥, ≤เอามาบางcolumn≥ 
		FROM «ชื่อตาราง»
		«WHERE»	
		«GROUP BY»	
		«HABING»	
		«ORDER BY»	
		«LIMIT»	
```


##### เปลี่ยนแปลงตาราง

##### ล้างเฉพาะข้อมูลทั้งหมดใน table

```
TRUNCATE TABLE ≤ชื่อตาราง≥
```

##### ลบ table

```
DROP TABLE ≤ชื่อตาราง≥
```


### CRUD
### SELECT
### SELECT Advance 
### SQL Function 
### View 
### Transaction 
### Stored Procedures & Trigger 
### GRANT & REVOKE
### BACK UP/ RESTORE/ CHECK/ REPAIR

## PHP with MYSQL Basic
## PHP with MYSQL PDO
## Laravel with MYSQL
## jQuery AJAX with MYSQL
## Angular JS with MYSQL
## ROR with MYSQL

## edu mysql

#### ต้องการหาลูกค้าไม่ซ้ำกันโดยโชว์แค่ปี 2014

```
SELECT * FROM work
GROUP BY c_phone1 
HAVING date_format(work_time, '%y') = '14'
```

### ต้องการหาลูกค้าไม่ซ้ำกันโดยโชว์แค่ปี 2014/02 

โดยไม่เอาพวกสถาบันโรงเรียนและเอาแค่บางวิชา

```
SELECT * FROM work
WHERE work_id like '%EN%' 
OR work_id like '%JP%' 
AND `name_company` not like '%สถาบัน%'
AND `name_contact` not like '%สถาบัน%'
GROUP BY c_phone1 
HAVING date_format(work_time, '%m') = '02' 
AND date_format(work_time, '%y') = '14'
```


### ค้นหาเบอร์โทรที่ไม่ซ้ำและไม่เป็นสถาบันเรียงจากงานล่าสุด

สังเกตุว่าถ้าเรากำหนดเงื่อนไข HAVING ไว้จะทำให้
เวลา SELECT ต้องมี field นั้นอยู่ด้วย ไม่งั้น error

```
SELECT name_contact, c_phone1, place 
FROM work GROUP BY c_phone1 
HAVING place not like '%สถาบัน%' 
ORDER BY work_time DESC
```

### เลือกเวลาสุดท้ายที่ลงงาน

```
SELECT work_time 
FROM work 
ORDER BY work_time DESC LIMIT 1
```

การใช้งานพิมพ์นี่หมายถึง log in ด้วย `root'@'localhost` แล้วใส่ pw เช่น 1234
	mysql -uroot -p

หลังจากนี้จะเป็น mysql prompt > ถ้า -> หมายถึงต่อบรรทัดเวลาพิมพ์ต้อง ; เพื่อจบประโยค

* ดู DB ทั้งหมด `SHOW DATABASES`
* เลือกใช้ นั้นๆให้ `USE ชื่อDB`
* ดู table ทั้งหมดใน DB นั้นๆ `SHOW TABLES`
* ทดสอบการสร้างตาราง -> หมายถึงคนละบรรทัดใช้ enter เว้นได้

```
CREATE TABLE pet 
(
name VARCHAR(20), 
owner VARCHAR(20),
species VARCHAR(20),
sex CHAR(1),
birth DATE,
death DATE
);
```

* ดูรายละเอียด(ไม่ใช่ data ข้างใน) ตารางโดย `DESCRIBE pet`

```
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| name    | varchar(20) | YES  |     | NULL    |       |
| owner   | varchar(20) | YES  |     | NULL    |       |
| species | varchar(20) | YES  |     | NULL    |       |
| sex     | char(1)     | YES  |     | NULL    |       |
| birth   | date        | YES  |     | NULL    |       |
| death   | date        | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
```

* ลงข้อมูล

```
INSERT INTO pet VALUES 
('Puffball','Diane','hamster','f','1999-03-30',NULL);
```

* ดูข้อมูลในตาราง `SELECT * FROM pet`
* ออกจาก mysql `EXIT`
-----



