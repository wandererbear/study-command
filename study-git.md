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

### git check out
* ถ้าไม่ต้องการย้ายไปอดีตถาวรเพียงไปเอาไฟล์บางอย่างให้ `git checkout <รหัส HEAD หรือจุดที่จะไป>` เส็ดแล้วก้อสามารถ checkout กลับมาได้

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

###การสร้าง ssh key ก่อน remote

คำสั่งสร้าง ssh key
```
ssh-keygen -t rsa -C "≤email registered for github≥"
```

ใส่รหัสผ่านที่เราคิดว่าจำได้
```
	Enter passphrase (empty for no passphrase): [Type a passphrase]
	Enter same passphrase again: [Type passphrase again]
```

ดูว่าทำงานแล้ว
```
	eval "$(ssh-agent -s)"
	# Agent pid 59566
```

แสดงข้อมูลใน id_rsa ว่าได้ generate แล้ว
```
	ssh-add ~/.ssh/id_rsa
```

copy ข้อมูลไป paste
```
	pbcopy < ~/.ssh/id_rsa.pub
```

ไปที่ [github setting](https://github.com/settings/ssh) แล้ว Add SSH Key
โดย paste ที่ copy ไว้ลงไป

```
	ssh-rsa AAAAB3NzaC1dssssssss ≤pass phase≥
```

ทดสอบเชื่อมต่อระบบ ไม่ต้อง แก้อะไรพิมพ์แบบนี้เลย
```
	ssh -T git@github.com
```

ถ้าสำเร็จแล้วจะขึ้นแบบนี้และใน https://github.com/settings/ssh จะขึ้นไฟเขียว
```
	Hi ≤username≥! You've successfully authenticated, but GitHub does not
  provide shell access.
```

เพิ่มเติมไปที่ [Generate SSH Key](https://help.github.com/articles/generating-ssh-keys/#step-4-add-your-ssh-key-to-your-account)


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