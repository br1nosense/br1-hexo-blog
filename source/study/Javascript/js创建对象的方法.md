title: 杂记
author: 白无意
tags:
  - Javascript
categories:
  - Javascript
date: 2022-8-05

## js创建对象的方法

我们一般使用字面量的形式直接创建对象，但是这种创建方式对于创建大量相似对象的时候，会产生大量的重复代码。但 js
和一般的面向对象的语言不同，在 ES6 之前它没有类的概念。但是我们可以使用函数来进行模拟，从而产生出可复用的对象
创建方式，我了解到的方式有这么几种：

### 1.工厂模式
用函数来封装对象的细节

优点：解决创建多个相似对象代码复用的问题

缺点：没有解决对象识别问题（不知道这个对象是谁

    function createPerson(name, age, job){
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function(){
            alert(this.name);
        };
        return o;
    }

    var person1 = createPerson("james"，9，"student");

    var person2 = createPerson("kobe"，9，"student");

### 2.构造函数模式
当我们使用构造函数实例化一个对象的时候，对象中会包含一个 __proto__ 属性指向构造函数原型对象，而原型对象中则包含一个 constructor 属性指向构造函数。因此在实例对象中我们可以通过原型链来访问到 constructor 属性，从而判断对象的类型。

优点：解决了对象类型无法识别的问题，
缺点：在使用构造函数创建对象时，每个方法都会在实例对象中重新创建一遍。拿上面的例子举例，这意味着每创建一个对象，我们就会创建一个 sayName 函数的实例，但它们其实做的都是同样的工作，因此这样便会造成内存的浪费。

    function createPerson(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        alert(this.name);
    };
    return o;
}

    var person1 = new createPerson("james"，9，"student");

    var person2 = new createPerson("kobe"，9，"student");

### 3.原型模式

每一个函数都有一个 prototype 属性，这个属性指向函数的原型对象，这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。我们通过使用原型对象可以让所有的对象实例共享它所包含的属性和方法，因此这样也解决了代码的复用问题。

优点：解决了构造函数模式中多次创建相同函数对象的问题，所有的实例可以共享同一组属性和函数。

缺点：

首先第一个问题是原型模式省略了构造函数模式传递初始化参数的过程，所有的实例在默认情况下都会取得默认的属性值，会在一定程度上造成不方便。

因为所有的实例都是共享一组属性，对于包含基本值的属性来说没有问题，但是对于包含引用类型的值来说（例如数组对象），所有的实例都是对同一个引用类型进行操作，那么属性的操作就不是独立的，最后导致读写的混乱。我们创建的实例一般都是要有属于自己的全部属性的，因此单独使用原型模式的情况是很少存在的。

    function Person(){

    }

    Person.prototype.name = "james";
    Person.prototype.age = 9;
    Person.prototype.job = "student";
    Person.prototype.sayName = function(){
        alert(this.name);
    }

    var person1 = new Person();
    person1.sayName(); // "james"

    var person2 = new Person();
    person2.sayName(); // "james"


    console.log(person1.sayName === person2.sayName) // true

### 4.组合使用构造函数模式和原型模式
用构造函数船舰对象，原型模式创建方法

优点：采用了构造函数模式和原型模式的优点，这种混成模式是目前使用最广泛，认同度最高的一种创建自定类型的方法。

缺点：由于使用了两种模式，因此对于代码的封装性来说不是很好。

    function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;
    }

    Person.prototype = {
        constructor: Person,
        sayName: function(){
            alert(this.name);
        }
    }

    var person1 = new createPerson("james"，9，"student");

    var person2 = new createPerson("kobe"，9，"student");

    console.log(person1.name); // "james"
    console.log(person2.name); // "kobe"
    console.log(person1.sayName === person2.sayName); // true


### 5.动态原型模式
把所有信息都封装在构造函数里，通过判断只初始化一次原型

优点：解决了混合模式中封装性的问题

    function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;

        if(typeof this.sayName !== "function" ){

            Person.prototype.sayName: function(){
                alert(this.name);
            } 
        } 
    }

    var person1 = new createPerson("james"，9，"student");

    person1.sayName(); // "james"

### 6.稳妥构造函数模式
和工厂模式一样的创建，但是采用new操作符最后来创建对象

优点：我对这个模式的理解是，它主要是基于一个已有的类型，在实例化时对实例化的对象进行扩展。这样既不用修改原来的构造函数，也达到了扩展对象的目的。

缺点：和工厂模式一样的问题，不能依赖 instanceof 操作符来确定对象的类型。

    function Person(name, age, job){
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function(){
            alert(this.name);
        };
        return o;
    }

    var person1 = new Person("james"，9，"student");

### 7.稳妥构造函数模式
稳妥对象，既没有公共属性，而且其方法也不使用this的对象，比较封闭，防止数据修改

优点：以上面为例，除了 sayName 方法外，没有别的方法可以访问数据成员，这就是稳妥构造函数提供的安全性。

缺点：和寄生构造函数一样，没有办法使用 instanceof 操作符来判断对象的类型

    function Person(name, age, job){

        //创建要返回的对象
        var o = new Object();

        //可以在这里定义私有变量和函数

        //添加方法
        o.sayName = function(){
            console.log(this.name);
        }

        //返回对象
        return o;
    } 

    var person1 =  Person("james"，9，"student");

    person1.sayName(); // "james"