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

#### note
* เนื่องจากทำแบบนี้อาจยากหลาย box เลยเตรียมทุกอย่างให้แล้วและ อาจใช้ puppet ทำให้เราแทนมันเหมือน bower คือกำหนดให้ว่า เริ่ม vagrant แล้วให้ลงอะไร dependence กับอะไรบ้างได้


## Scotch box (web tutorial ดีๆสร้าง box ให้ใช้)
ในกล่องจะลงให้หมดนี้ซึ่งลงง่ายมาก

* Apache
* Vim
* MySQL
* PHP 5.5
* Git
* Screen
* Composer
* cURL
* GD and Imagick
* Mcrypt
* Memcache and Memcached
* NPM
* Grunt
* Bower
* Yeoman
* Gulp

1. ไปที่ root ของ project ที่เราจะสร้าง และ clone ไฟล์ที่จะใช้มา

```
git clone https://github.com/scotch-io/scotch-box.git ≤my-project≥
```

จะได้ไฟล์พวกนี้มาแล้วงานจริงๆให้เราใส่ใน public ทั้งหมด
	* .
	* ..
	* .DS_Store
	* .git
	* .gitignore
	* .vagrant
	* README.md
	* Vagrantfile
	* public

2. เข้าไปที่ my-project แล้ว `vagrant up` จะ install ทุกอย่างให้เอง

3. ต้องการ update 
	* `vagrant box outdate` เพื่อดูว่า boxt ไหนเก่าแล้ว
	* `vagrant box updated` เพื่อ update มัน

4. การเข้า mysql ใช้ 

	Key	| Value 
	---| ---
	 Database Name | scotchbox
	 Database User | root
	 Database Password |  root
	 Database Host | localhost

	อยากง่ายใช้app: Sequl Pro ไป tab SSH เพื่อตั้งค่า
		* Name: scotchbox
		* MySQL Host: 127.0.0.1
		* Username: root
		* Password: root
		* SSH Host: 192.168.33.10
		* SSH User: vagrant
		* SSH Password: vagrant
	
5. การเข้าไปจัดการใน `vagrant ssh`

	สังเกตว่า มี parent root พวก jquery ให้ในกรณีไม่ได้ต่อเนท

	* commonly-used-assets 
			* bootstrap.css
			* bootstrap.min.js
			* jquery.min.js?ver=1.9.1
			* jquery-2.1.1.min.js
			* bootstrap.min.css
	* node_modules
	* postinstall.sh

6. การตั้งชื่อ url เพื่อเข้าไปดูแทนพิมพ์ ip

	`sudo vi /etc/hosts`

แล้วเพิ่มบรรทัดนี้ลงไป ต่อไปนี้พิมพ์ study-php.app จบ

```
	# vagrant-scotch-box
	192.168.33.10   study-php.app
```


## Homestead (Box สำหรับ Laravel)

โปรแกรมที่จะติดตั้งมาให้

* Ubuntu 14.04
* PHP 5.6
* HHVM
* Nginx
* MySQL
* Postgres
* Node (With Bower, Grunt, and Gulp)
* Redis
* Memcached
* Beanstalkd
* Laravel Envoy
* Fabric + HipChat Extension
* Blackfire Profiler

-------------------------------------------
1. Install

	ไปที่ root
	`vagrant box add laravel/homestead`

	clone ~/Homestead มาใช้สำหรับทำ vagrant up
	`git clone https://github.com/laravel/homestead.git Homestead`

	ที่ root(~) ให้สร้าง Homestead.yaml เพื่อปรับแต่ง
	`bash init.sh`

	โดยเมื่อสำเร็จ ไฟล์จะอยู่ที่ `~/.homestead/Homestead.yaml`

	ในกรณีที่ไม่เคยเชื่อม git กับ github มาก่อนต้องสร้าง SSH Key
	`ssh-keygen -t rsa -C "you@homestead"`

2. ปรับแต่ง `~/.homestead/Homestead.yaml`

	```
		folders:
		    - map: /Applications/XAMPP/xamppfiles/htdocs/sandbox/study/study-laravel/
		      to: /home/vagrant/Code

		sites:
		    - map: homestead.app
		      to: /home/vagrant/Code/my-first-app/public
		      hhvm: true
		    - map: test.app
		      to: /home/vagrant/Code/your-project-name/public

		databases:
		    - homestead
	```
	ทุกครั้งที่ add site เพิ่มไปต้อง  `vagrant provision` ด้วย

	ไปเพิ่มชื่อใน `sudo vi /etc/hosts`

	```
		# vagrant-homestead-box
		192.168.10.10   homestead.app
	```

3. การเข้าไปใน ssh ให้สร้าง alias ไว้ใช้เลยใน ~/zsh.rc

	`alias vm="ssh vagrant@127.0.0.1 -p 2222"`

	เสร็จแล้วอย่าลืม `source ~/zsh.rc`

4. ใน Sequel Pro ให้ ไป tab: Standard
	* Name: homestead
	* Host: 127.0.0.1
	* Username: homestead
	* Password: secret
	* Port: 33060

เพิ่มเติมไปที่ [Homestead](https://scotch.io/tutorials/getting-started-with-laravel-homestead)


## Puppet
ตัวจัดการระบบเวลาเริ่มต้น vagrant ให้ install อะไรบ้าง

