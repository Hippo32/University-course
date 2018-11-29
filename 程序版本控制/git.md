# Git #
## 创建分支后的一系列操作 ##
	git branch huangry // 创建自己的分支
	git checkout huangry // 跳转到自己的分支
	git add . 
	git commit -m "xxx"
	git pull origin master
	git push -u origin huangry
	git checkout master
	git merge huangry // 合并分支
	git pull origin master
	git push -u origin master 
## 修改后再提交 ##

	git add ./
	git commit -m "xxx"
	git push -u origin master

参考链接：[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)