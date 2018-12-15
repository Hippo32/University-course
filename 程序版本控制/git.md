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

## git commit message ##
- init：项目初始化（用于项目初始化或其他某种行为的开始描述，不影响代码）
- feat：新功能（feature）
- fix：修改bug
- docs：文档（documentation）
- opt：优化和改善，比如弹窗进行确认提示等相关的，不会改动逻辑和具体功能等）
- style：格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- save：单纯地保存记录
- other：用于难以分类的类别（不建议使用，但一些如删除不必要的文件，更新.ignore之类的可以使用）

参考链接：[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)