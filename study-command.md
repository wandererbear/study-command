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