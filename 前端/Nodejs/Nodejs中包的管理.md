# Node.js中包的管理 #
如果一个包是某个工程依赖，那么我们需要在工程的目录下使用本地模式安装这个包，如果要通过命令喊调用这个包中的命令，则需要用全局模式安装。

## exports和module.exports的区别 ##
1. module.exports初始值为一个空对象{}
2. exports是指向的module.exports的引用
3. require()返回的是module.exports而不是exports