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
mysql -uroot
mysqladmin -uroot password
```

## Basic Command

### Databases
### Tables
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