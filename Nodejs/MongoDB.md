# MongoDB #
MongoDB是一个对象数据库，它没有表、行等概念，也没有固定的模式和结构，所有的数据以文档的形式存储。所谓文档就是一个关联数组式的对象，它的内部由属性组成，一个属性对应的值可能是一个数、字符串、日期、数组，甚至是一个嵌套的文档。

开源，高性能的NoSQL数据库；支持索引、集群、复制和故障转移、各种语言的驱动程序；高伸缩性。

MongoDB的默认端口是27017

默认存在“admin“和“local”两个数据库；admin数据库是存放管理员信息的数据库，认证会用到；local是存放replication相关的数据。

	show dbs // 查看数据库
	use 数据库名 // 用指定数据库
	db.createCollection('要新建的表名') // 新建表
	show collections // 查看当前数据库下表
	db.表名.drop() // 删除当前数据库指定表
	db.dropDatabase() // 删除当前数据库

	db.表名.insert(数据) // 插入
	db.表名.save(数据) // 可达到跟insert一样的插入效果

	// 查询
	db.表名.find()
	db.表名.find(条件)
	db.表名.findOne(条件) // 查询第一条
	db.表名.find().limit(数量） // 限制数量
	db.表名.find().skip(数量) // 跳过指定数量
	db.表名.find().count() // 查询数量
	db.表名.find().sort({"字段名": 1}) // 排序1表示升序，-1表示降序
	db.表名.find({}, {"字段名":0}) // 指定字段返回，1：返回，0：不返回

	// 修改
	db.表名.update({"条件字段名":"字段值"},{$set:{"要修改的字段名":"修改后的字段值"}})

	// 删除
	db.表名.remove(条件)
## 名词解释 ##
- Schema：一种以文件形式存储的数据库模型骨架，不具备数据库的操作能力
- Model：由Schema发布生成的模型，具有抽象属性和行为的数据库操作对。

## mongodb启动 ##
以管理员模式启动CMD，切换到MongoDB\bin的安装目录，并执行命令：

	mongod --dbpath=\data\db

关于命令中的参数说明：

- `--bind_ip`：绑定服务IP，若绑定127.0.0.1，则只能本机访问，不指定默认本地所有IP
- `--lopath`：指定MongoDB日志文件，注意是指定文件不是目录
- `--logappend`：指定追加的方式写日志
- `--dbpath`：指定数据库路径
- `--port`：指定服务端口号，默认端口27017
- `--serviceName`：指定服务名称
- `--serviceDisplayName`：指定服务名称，有多个mongodb时执行
- `--install`：指定作为一个Windows服务安装

接着在命令行输入

	net start MongoDB

## 停止服务 ##
	net stop mongodb

# Mongoose #
Mongoose是NodeJS的驱动，不能作为其他语言的驱动。Mongoose有两个特点：

1. 通过关系型数据库的思想来设计非关系型数据库
2. 基于mongodb驱动，简化操作

### 安装 ###
	npm install mongoose

### 使用 ###
	require('mongoose')
### 连接MongoDB数据库 ###
	// mongoose.connect(url);
	mongoose.connect('mongodb://localhost/db1');
	// 如果还需要传递用户名、密码、则可以使用如下方式
	mongoose.connect('mongodb://username:password@host:post/database?options...');

connect()方法还接受一个选项对象options，该对象将传递给底层驱动程序。这里所包含的所有选项优先于连接

## adminMongo ##
一款mongoDB的可视化工具

### 安装方法 ###
1. 把git仓库克隆到本地

		git clone https://github.com/mrvautin/adminMongo
2. 进入仓库

		cd adminMongo
3. 安装

		npm install
4. 启动

		npm start
5. 访问地址`http://127.0.0.1:1234`
