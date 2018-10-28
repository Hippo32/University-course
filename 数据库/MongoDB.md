# MongoDB #
MongoDB是一个对象数据库，它没有表、行等概念，也没有固定的模式和结构，所有的数据以文档的形式存储。所谓文档就是一个关联数组式的对象，它的内部由属性组成，一个属性对应的值可能是一个数、字符串、日期、数组，甚至是一个嵌套的文档。

开源，高性能的NoSQL数据库；支持索引、集群、复制和故障转移、各种语言的驱动程序；高伸缩性。

MongoDB的默认端口是27017

## MongoDB特点 ##
- 使用BSOn存储数据
- 支持相对丰富的查询操作（相对其他nosql数据库）
- 支持索引
- 副本集（支持多个实例/多个服务器运行同个数据库）
- 分片（数据库水平扩展）
- 无模式（同个数据文档中的数据可以不一样）
- 部署简单方便（默认无密码，也带来安全问题）

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

## MongoDB概念解析 ##
|SQL术语/概念|MongoDB术语/概念|解释/说明|
|-|-|-|
|database|database|数据库|
|table|collection|数据库表/集合|
|row|document|数据记录行/集合|
|column|field|数据字段/域|
|index|index|索引|
|table joins| |表连接，MongoDB不支持|
|primary key|primary key|主键，MongoDB自动将_id字段设置为主键|

RDBMS与MongoDB对应的术语

|RDBMS|MongoDB|
|-|-|
|数据库|数据库|
|表格|集合|
|行|文档|
|列|字段|
|表联合|嵌入文档|
|主键|主键（MongoDB提供key为_id）|

## 文档 ##
文档是一组键值（key-value）对（即BSON）。

需要注意的是：

1. 文档中的键/值对是有序的。
2. 文档中的值不仅可以是在双引号里面的字符串，还可以是其他几种数据类型（甚至可以是整个嵌入的文档）。
3. MongoDB区分类型和大小写。
4. MongoDB的文档不能有重复的键。
5. 文档的键是字符串。除了少数例外情况，键可以使用UTF-8字符。

文档键命名规范：

- 键不能含有\0（空字符）。这个字符用来表示键的结尾。
- `.`和`$`有特别的意义，只有在特定环境下才能使用。
- 以下划线`_`开头的键是保留的（不是严格要求的）。

## 集合 ##
集合就是MongoDB文档组，类似于RDBMS（关系数据库管理系统：Relational Database Management System）中的表格。

集合存在于数据库中，集合没有固定的结构，这意味着你对集合可以插入不同格式和类型的数据。

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

## Schema ##
在Mongoose中，所有数据都由一个Schema开始创建。每一个schema都映射到一个Mongodb的集合（collection）（table），并定义了该集合（collection）中的文档（document）的形式。

### 定义一个Schema ###
	const mongoose = require('mongoose');
	const Schema = mongoose.Schema;
	const UserSchema = new Schema({
		name: { type: String, required: true },
		createTime: { type: Date, default: Date.now },
		favoriteIds: [String],
		sex: String,
		avatar: String,
		vip: Boolean,
	})
new Schema({})中的name称之为Schema的键

Schema中的每一个键都定义了一个文档（document）的一个属性。

这里我们定义：

- 用户名name，它将映射为String的Schema类，设置required: true规定创建文档（document）时name必须设置。
- 注册事件createTime会被映射为Date的Schema类型，如没设置默认为Date.now的值。
- 会员vip会被映射为Boolean的Schema类型。
- 收藏的id列表被映射为字符串Array的Schema类型，['1','2','3']。
- 允许的Schema类型有：
	- String
	- Number
	- Date
	- Buffer
	- Boolean
	- Mixed
	- ObjectId
	- Array

## Model ##
模型Model是根据Schema编译出的构造器，或者称为类，通过Model可以实例化出文档对象document。

文档document的创建和检索都需要通过模型Model来处理

Model使nodejs对象和MongoDB中的文档（行）相对应。

	mongoose.model()
使用model()方法，将Schema编译为Model。model()方法的第一个参数是模型名称。

例子：
	
	// models/user.js
	var mongoose = require('mongoose');
	var UserSchema = new mongoose.Schema({
		uid: Number,
		username: String,
		password: String
	});
	mongoose.model('User', UserSchema);

	// routes/reg.js
	var mongoose = require('mongoose');
	var User = mongoose.model('User');


参考文章：

- [Mongo基础使用，以及在Express项目中使用Mongoose](https://www.cnblogs.com/woodk/p/6155955.html)

# adminMongo #
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
