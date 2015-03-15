# Vagrant

ใช้สำหรับส้ราง environment เฉพาะ project นั้นๆให้แยกออกจากตัวอื่นๆแล้วจาก OS หลักทำให้ไม่มีปัญหาในการ install แล้วชนกับของ OS หลัก

## Basic Install

1. Install Virtual Box
	* https://www.virtualbox.org/wiki/Downloads
	* เหมือน vm fusion
	* สำหรับ สร้างเครื่องจำลองในเครื่องหลัก

2. Install Vagrant
	* https://www.vagrantup.com/downloads.html
	* เป็นตัวสร้าง environment ให้กับ Virtual box

3. Install Vagrant Box
	* http://www.vagrantbox.es/
	* เป็นเหมือน template เริ่มต้นไม่ต้อง load เองทุกอย่าง

## Basic Command

1. `vagrant --version`
	* ตรวจสอบว่าเราลงเสร็จแล้ว

2. `vagrant box list`
	* ดูว่าในระบบเรามี box ไหนบ้างที่โหลดมาใช้

```
laravel/homestead (virtualbox, 0.2.5)
precise32         (virtualbox, 0)
```

3. `vagrant box add precise32 http://files.vagrantup.com/precise32.box`
	* ติดตั้ง precise32 โดยจะลง ubuntu ให้เราเป็นตัวจัดการ server
	* ดูเพิ่มเติมได้ที่ `http://www.vagrantbox.es/`

4. `vagrant box remove precise32 'virtualbox'`
	* เวลาลบให้บอกได้ว่าลบตั้วไหน 'ในวงเล็บ'

5. ไปยัง directory ที่จะทำการสร้างเครื่องจำลอง แล้ว init เริ่มต้นระบบสร้าง Vagrantfile
	* `take firstVM`
	* `vagrant init precise32` ต้องระบุ box ด้วย
	* `ls` ทดสอบดูว่ามี Vagrantfile แล้ว

6. `vagrant up` เริ่มต้นระบบที่ directory นั้นๆ
	* ลองเปิด app: VirtulBox ดูได้ว่า start แล้ว

7. `vagrant destroy` ปิดระบบ directory นั้นๆ
	* ลองเปิด app: VirtulBox ดูได้ว่า stop แล้ว
	* ไม่ได้ลบอะไรในไฟล์เลยเพียงแค่ปิดการใช้ทรัพยากรเครื่อง
	* แต่การใช้คำสั่งนี้เวลา up ใหม่จะกินเวลานิดนึง

8. คำสั่งเพิ่มเติมในกรณีที่ไม่อยากปิดระบบนานๆ
	* `vagrant suspend` เป็นการ sleep ระบบชั่วคราว
	* `vagrant resume` เป็นการ wake ระบบจาก suspend
	* `vagrant halt` เป็นการ พักระบบ เหมือน logout
	* `vagrant up` เป็นการ เข้าระบบจาก halt ได้
	* `vagrant reload` เป็นการ restart

9. `vagrant ssh` สำหรับเข้าไปดู directory จากเครื่องจำลอง
	* ถ้าเข้าไปแล้วจะขึ้น prompt อีกแบบ `vagrant@homestead:~$`
	* โดยแต่ละ box จะมีกำหนดไม่เหมือนกัน ถ้า precise32 ต้อง `cd /vagrant` อีกชั้นเพื่อเข้าไปดู folder ที่ sync กับด้านนอกอยู่
	* `exit` ถ้าตองการออกจาก ssh ของ vm

## Vagrantfile (เป็นภาษา ruby)
```
Vagrant.configure(2) do |config|
	# กำหนด box
	config.vm.box = "precise32"
	
	# กำหนดมห้เวลาเปิด server ให้ localhost:9000 เข้าดู port 90 ของ vm ได้จะได้ไม่ซ้ำอันอื่นๆ
	config.vm.network "forwarded_port", guest: 90, host: 9090
end
```

## Basic Server
1. `vagrant ssh` สำหรับเข้าไปดู directory
2. `sudo python -m SimpleHTTPServer 90` เปิด server
3. เข้า chrome พิมพ์ `localhost:9000` เพิ่อเปิดดู
4. กรณี static ip คอมไว้สามารถเข้าผ่านเครือข่ายได้เช่น `192.168.1.250:90` หรือถ้าเปิด port นี้ใน router สามารถเข้าจากนอกบ้านได้โดย `xxxxx.dyn.dns.xx:9000`
5. ต้องการเปลี่ยนตำแหน่ง port แก้ที่ 
	* `config.vm.network "forwarded_port", guest: 90, host: 9090`
	* ใน Vagrantfile

## Vagrant Provision(จัดเตรียมระบบ)

ไปยัง Vagrantfile

```
Vagrant.configure(2) do |config|
	# กำหนดให้เวลาใช้คำสั่ง vagrant provision จะ run คำสั่ง shell แบบ inline
	config.vm.provision :shell, inline: "echo 'Hello World!'"
end
```

ถ้าต้องการให้ run จากไฟล์

```
Vagrant.configure(2) do |config|
	config.vm.provision :shell, path: './provision.sh'
end
```

สร้างไฟล์ ./provision.sh แล้วพิมพ์คำสั่งที่ต้องการเช่น install vim ให้หน่อย

```
echo "This is our provissioning script"
apt-get install vim -y
```

เมื่อ run `vagrant provision` จะทำงานตามไฟล์ใน path

* แต่ตอนนี้ vim มีปัญหา ในไฟล์อาจตั้ง run

```
sudo apt-get remove vim-common 
sudo apt-get clean && sudo apt-get purge 
sudo apt-get update && sudo apt-get install vim
```
* เนื่องจากทำแบบนี้อาจยากหลาย box เลยเตรียมทุกอย่างให้แล้วและ อาจใช้ puppet ทำให้เราแทนมันเหมือน bower คือกำหนดให้ว่า เริ่ม vagrant แล้วให้ลงอะไร dependence กับอะไรบ้างได้


## Homestead (Box สำหรับ Laravel)

## Puppet
ตัวจัดการระบบเวลาเริ่มต้น vagrant ให้ install อะไรบ้าง

