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


## edu mysql

#### ต้องการหาลูกค้าไม่ซ้ำกันโดยโชว์แค่ปี 2014

```
select * from work
group by c_phone1 
having date_format(work_time, '%y') = '14'
```

### ต้องการหาลูกค้าไม่ซ้ำกันโดยโชว์แค่ปี 2014/02 

โดยไม่เอาพวกสถาบันโรงเรียนและเอาแค่บางวิชา

```
select * from work
where work_id like '%EN%' 
or work_id like '%JP%' 
and `name_company` not like '%สถาบัน%'
and `name_contact` not like '%สถาบัน%'
group by c_phone1 
having date_format(work_time, '%m') = '02' 
and date_format(work_time, '%y') = '14'
```


### ค้นหาเบอร์โทรที่ไม่ซ้ำและไม่เป็นสถาบันเรียงจากงานล่าสุด

สังเกตุว่าถ้าเรากำหนดเงื่อนไข having ไว้จะทำให้
เวลา select ต้องมี field นั้นอยู่ด้วย ไม่งั้น error

```
select name_contact, c_phone1, place from work group by c_phone1 having place not like '%สถาบัน%' order by work_time DESC
```

### เลือกเวลาสุดท้ายที่ลงงาน

```
select work_time from work order by work_time desc limit 1
```

การใช้งานพิมพ์นี่หมายถึง log in ด้วย `root'@'localhost` แล้วใส่ pw เช่น 1234
	mysql -uroot -p

หลังจากนี้จะเป็น mysql prompt > ถ้า -> หมายถึงต่อบรรทัดเวลาพิมพ์ต้อง ; เพื่อจบประโยค

* ดู DB ทั้งหมด `SHOW DATABASES`
* เลือกใช้ นั้นๆให้ `USE ชื่อDB`
* ดู table ทั้งหมดใน DB นั้นๆ `SHOW TABLES`
* ทดสอบการสร้างตาราง -> หมายถึงคนละบรรทัดใช้ enter เว้นได้

```
CREATE TABLE pet (name VARCHAR(20), 
owner VARCHAR(20),
species VARCHAR(20),
sex CHAR(1),
birth DATE,
death DATE);
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
INSERT INTO pet
VALUES ('Puffball','Diane','hamster','f','1999-03-30',NULL);
```

* ดูข้อมูลในตาราง `SELECT * FROM pet`
* ออกจาก mysql `EXIT`
-----