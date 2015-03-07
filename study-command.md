#important path
	~/.zshrc
	~/.bashrc
	~/.bash_profile
-----

#brew
	brew list
	brew upgrade php55
	brew upgrade git
	tree
	brew install mysql
	brew uninstall node
	brew update
	brew doctor	
	brew prune
	brew install node
-----

#terminal
	say hey
	open -a Finder .
	mkdir <path>
	mv oldname newname
	mv oldpath newpath
	cp thisfile path/thisfile
	cd <path>
	cd ..
	whoami
	pwd
	history
	exit [^ + d]
	clear [^ + l]
	kill [^ + c]
	which git
	tail test-ln.txt
	cat test-ln.txt
	< test-ln.txt
	vi link.txt
	echo abc > test-ln.txt
	ls -a | > test-ln.txt
	history | > history.txt
	rm -rf test
	rm -rfv test
	ls -lahS
	ln -s test-ln.txt link.txt

### du(directory usage stat)

* -s: ดูแบบ summary
* -h: ดูขาดไฟล์แบบมีหน่วย
* -d 1: ดู directory 1 ชั้น

ดู pwd นี้ว่า ขนาดรวมเท่าไร่ `du -sh`

ดู pwd นี้ว่า ขนาดแต่ละfolderเท่าไร่ `du -d 1 -h`

###permission
* read = 4, write = 2, execute = 1
* u = current user, g = group, o = other
* d = directory, - = file, l = link

```
chmod 777 <file -rwxrwxrwx>
chmod [u|g|o|a] [+|-|=] [r|w|x] <file rwxrwxrwx>
```
###watch process running
	top
	ps
	ps -A

###source(execute file) & $PATH(which cmd)
	source ~/.bash_profile
	source ~/.bashrc
	source ~/.zshrc
	echo $PATH
	export PATH="~/.composer/vendor/bin:$PATH"
	export PATH="~/.composer/vendor/bin/"

###curl(open data from url)
	curl -sS https://getcomposer.org/installer | php
-----

#iterm
	md <path>
	take <path>
	trash test.txt
	nvmstart
	rvmstart
	phpstart
	server
	st gulpfile.js
	stt
	l
	zshconfig
	goto-study
	google yn 568 ex ii
	youtube yn 568 ex ii
	imdb movie
	size-all
	size-this
-----

#oh-my-zsh
	git clone https://github.com/zenorocha/dracula-theme.git
	ln -s /Users/wanderer/Documents/iterm/dracula-theme/zsh/dracula.zsh-theme $OH_MY_ZSH/themes/dracula.zsh-theme
	open .oh-my-zsh
	cd themes
	vi dracula.zsh-theme
-----

##git

###git config
--global = current user
* ดูที่เคยตั้งค่าทั้งหมด `git config --list`
* ตั้งชื่อ `git config [--system | --global] user.name=wandererbear`
* ตั้ง email `git config [--system | --global] user.email=jaturong.poon@gmail.com`
* ไม่สนใจการเปลี่ยนแปลง whitespace `git config [--system | --global] apply.whitespace=no`
* จำไม่ได้ `git config [--system | --global] push.default=simple`
* มีสีสันได้แยกออกใน log `git config [--system | --global] color.ui=true`
* กำหนด default editor`git config [--system | --global] core.editor=vim`
* การสร้าง alias `git config [--system | --global] alias.lol=log --graph --decorate --pretty=oneline --abbrev-commit --all`
* `git config [--system | --global] alias.ilog=log --pretty=format:'%h %s [by %an] <%ad>' --abbrev-commit --graph`

###git basic
	git init
	git add .
	git add --all
	git status
	git commit -m 'Initialize stavle gulp'
	git commit --amend ['new message if want']
	sudo git commit -m "[php] Update hello.php"
	git checkout master
	git checkout -b <newbranch>
	git branch -D <newbranch>
	git branch
	git merge <newbranch>
	git log
	git mv <old> <new>
	git rm --cached <dir>

###git commit

#### การส้ราง commit message ที่ดี
* eng only in present tense
* บอกกลุ่มไฟล์ที่แก้ไขได้ search ได้ `git log --grep "html"`
* ชนิดของ commit และ หัวข้อ
* อธิบายแยกเป็นบรรทัดใช้ 'ในนี้ enter ได้'
* ใช้ bullet ได้

```
[html/, html, file.html] Fix bug: ปัญหาไม่แสดง..

ได้มีการเปลี่ยนแปลงบางอย่างแทนของเดิมเพื่อที่จะ
ทำให้การแสดงผลกลับมาเป็นปกติแต่ในนี้ไม่รองรับภาษาไทย

สิ่งที่ได้แก้ไข
* อย่างแรกคือ
* อย่างที่สอง
* อย่างที่สาม
```

###git reset
#### before commit after staged
* staged > unstaged > modified `git reset HEAD`
* staged > modified > last commit `git reset --`

#### after commit จะ staged ใหม่หรือยังก้อได้
* unstage โดย keep modified `git reset --soft HEAD~`
* unstage โดย delete modified `git reset --hard HEAD~`

###git merge
* branch ใหม่จะรวม commit msg เข้ามา

### git branch
* branch ใหม่ไฟล์ที่ยังไม่ add แต่ modified แล้ว เวลาย้าย branch จะไม่ได้ revert ไฟล์นั้น มา add ไฟล์ที่ modified นั้นใน brach ที่เพิ่งย้ายมาได้

###git log
* ดูlogแค่ 5 อันล่าสุด `git log -n 5`
* ดูlogเมื่อ 1ชม ที่แล้ว `git log --since 1.hour.ago`
* ดูlogแบบสวยๆ`git log --oneline --graph`
* ดูlogแบบสวยๆสร้างมาเอง`git lol`
* ดูlogแบบสวยๆสร้างมาเอง`git ilog`

###git pro
	git stash
	git stash --apply
	git diff [<file>]
	git diff --staged [<file>]
	git diff HEAD~~ [<file>]
	git diff 432435 [<file>]
	git diff --since 2015-06-12 [<file>]
	git diff --since 1.[day|month|week|minute|hour].ago [<file>]
	git diff --since 2.[days|months|weeks|minutes|hours].ago [<file>]

###git remote
	git remote add origin git@github.com:wandererbear/study-gulp-basic.git
	git remote rm origin branch
	git remote -v
	git remote show origin
	git remote prune origin
	git push -u origin master
	git push --force origin master
	git checkout -b gh-pages master
	git push -u origin gh-pages
	pit pull

	echo somefile.txt > .gitignore
	git clone <http://> [dirname]

###git-heroku
	heroku login
	heroku create
	heroku create laravel-5
	git push heroku master
	heroku ps:scale web=1
	heroku open
	heroku help
	heroku logs
-----

#jade
	npm install jade --global
	jade sourceDir --pretty --watch --output destDir
	jade -Pwo sourceDir .
-----

#compass
	gem install compass
	compass -v
	compass install compass
	compass init
	compass watch
-----

#coffeescript
	npm install -g coffee-script
	coffee -c --bare gulpfile.coffee
-----

#laravel
	phpstart
	curl -sS https://getcomposer.org/installer | php
	sudo composer update
	composer global require "laravel/installer=~1.1"
	composer require bower
	composer update
	laravel new <project name>
	php artisan tinker
	php artisan make:controller SongsControllerDB --plain
	php artisan make:migration create_songs_table --create="songs"
	php artisan migrate
	php artisan migrate:rollback
	php artisan serve
-----

#mysql
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

การใช้งานพิมพ์นี่หมายถึง log in ด้วย root'@'localhost แล้วใส่ pw เช่น 1234
	mysql -uroot -p

หลังจากนี้จะเป็น mysql prompt > ถ้า -> หมายถึงต่อบรรทัดเวลาพิมพ์ต้อง ; เพื่อจบประโยค

* ดู DB ทั้งหมด `SHOW DATABASES`
* เลือกใช้ นั้นๆให้ `USE ชื่อDB`
* ดู table ทั้งหมดใน DB นั้นๆ `SHOW TABLES`
* ทดสอบการสร้างตาราง -> หมายถึงคนละบรรทัดใช้ enter เว้นได้

```
	>  CREATE TABLE pet (name VARCHAR(20), 
	-> owner VARCHAR(20),
  -> species VARCHAR(20),
  -> sex CHAR(1),
  -> birth DATE,
  -> death DATE);
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
	> INSERT INTO pet
  -> VALUES ('Puffball','Diane','hamster','f','1999-03-30',NULL);
```

* ดูข้อมูลในตาราง `SELECT * FROM pet`
* ออกจาก mysql `EXIT`



-----

#gem
	gem update sass compass
-----

#rvm
	\curl -sSL https://get.rvm.io | bash -s stable --rails --ruby
	source /Users/wanderer/.rvm/scripts/rvm
	rvm reload
	rvm use ruby-2.2.0
	rvm get stable --auto-dotfiles
	rvmstart
	rvm list
	rvm install sass
	rvm install compass
-----

#ruby on rails
	irb
	brew install rail
	ruby introduction.rb
	rails new simple_cms
	rails new simple_cms -d mysql
	rails generate controller
	rails server
-----

#nvm
	curl https://raw.githubusercontent.com/creationix/nvm/v0.23.3/install.sh | bash
	source ~/.nvm/nvm.sh
	nvm install stable
	nvm use stable
	nvm use 0.10
	vi .nvmrc
-----

#node & npm
	nvm use stable
	npm init
	npm list
	npm search yeoman-generator
	npm install
	npm install [--save-dev] <module name>
	npm install [--save | -S] <module name>
	npm install --save-dev gulp-livereload
	npm install jsdoc@"<=3.3.0"
	npm install jsdoc@"^3.3.0"
	npm install jsdoc@"~3.3.0"
	npm uninstall -g npm
	npm list -g --depth=0
-----

###uninstall
	npm cache clean
	sudo npm cache clean -f
	sudo rm /usr/local/bin/npm
	sudo rm /usr/local/share/man/man1/node.1
	sudo rm /usr/local/lib/dtrace/node.d
	sudo rm -rf ~/.npm
	sudo rm -rf ~/.node-gyp
	sudo rm /opt/local/bin/node
	sudo rm /opt/local/include/node
	sudo rm -rf /opt/local/lib/node_modules
-----
	
#yeoman
	npm install -g yo bower grunt-cli gulp
	npm install -g generator-webapp
	npm search yeoman-generator
	npm install -g generator-generator
	yo
	yo webapp
	yo generator
	yo wanderer
	yo angular
	npm install & bower install
	bower search angular-ui-sortable
	bower install --save angular-ui-sortable
	bower install --save jquery-ui
	bower install --save angular-local-storage
-----

#bower
	touch .bowerrc
	vi .bowerrc
	bower init
-----

#grunt
	npm init
	npm install grunt
	sudo npm install -g grunt@lastest
	grunt init
	touch GruntFile.coffee
	npm install grunt-contrib-uglify --save-dev
	npm install
	grunt serve:dist
	grunt serve
	grunt build
-----

#gulp
	npm install --save-dev gulp-ruby-sass gulp-for-compass gulp-autoprefixer gulp-cssmin gulp-jade gulp-minify-html gulp-svgmin gulp-coffee
	npm install  --save-dev
-----

#mac apache
	sudo apachectl start
	sudo apachectl stop
-----

#simpleHTTPServer
	cd study-SimpleHTTPServer
	python -m SimpleHTTPServer 8000
	l
	open http://localhost:8000 && python -m SimpleHTTPServer 8000