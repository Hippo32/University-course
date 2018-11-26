# JavaScript原型和原型链 #
## prototype ##
每个函数都有一个prototype属性，prototype是函数才会有的属性。

函数的prototype属性指向一个对象，这个对象是调用该构造函数而创建的实例的原型。

什么是原型？每一个JavaScript对象（null除外）在创建的时候就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型“继承”属性。

## __proto__ ##
这是每一个JavaScript对象（除了null）都具有的一个属性，叫`__proto__`，这个属性会指向该对象的原型。

![](https://i.imgur.com/G9IFkSu.png)

## constructor ##
每个原型都有一个constructor属性指向关联的构造函数。