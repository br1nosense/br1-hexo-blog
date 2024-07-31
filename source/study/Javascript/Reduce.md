title: 杂记
author: 白无意
tags:
  - Javascript
categories:
  - Javascript
date: 2022-8-05

# Object
## hasOwnProperty方法
### 作用：
    Object的hasOwnProperty()方法返回一个布尔值，判断对象是否包含特定的自身（非继承）属性。
### 运用    
    //1.判断自身属性是否存在
    var o = new Object();
    o.prop = 'exists';

    function changeO() {
    o.newprop = o.prop;
    delete o.prop;
    }

    o.hasOwnProperty('prop');  // true
    changeO();
    o.hasOwnProperty('prop');  // false

#### 2. 判断自身属性与继承属性
    //自身属性为真，继承属性为假
    function foo() {
    this.name = 'foo'
    this.sayHi = function () {
        console.log('Say Hi')
    }
    }

    foo.prototype.sayGoodBy = function () {
    console.log('Say Good By')
    }

    let myPro = new foo()

    console.log(myPro.name) // foo
    console.log(myPro.hasOwnProperty('name')) // true
    console.log(myPro.hasOwnProperty('toString')) // false
    console.log(myPro.hasOwnProperty('hasOwnProperty')) // fasle
    console.log(myPro.hasOwnProperty('sayHi')) // true
    console.log(myPro.hasOwnProperty('sayGoodBy')) // false
    console.log('sayGoodBy' in myPro) // true

## Array.reduce方法
### 作用：
    计算数组元素相加后的总和
    常用于数组去重、构建对象、数组扁平化、求和、统计等

### 运用
    Array.reduce((did,cur,index,arr) => {} , initVal)
    did：累计器（操作后的结果）
    cur：当前值
    index：当前索引（如果有 initVal 索引从0开始，否侧从1开始）
    arr：数组本身

### 实例
#### 1.求和
    let arr = [1,2,3,4]
    let sum = arr.reduce((did,cur)=>{
        did += cur;
        return did;
    })
#### 2.次数统计
    let arr = ['a','a','a','b']
    let sum = arr.reduce((did,cur)=>{
      if(!did.hasOwnProperty(cur)){
            did[cur] =1
      }else{
        did[cur] ++ 
      }
      return did;
    })
#### 3.分组累加
    const arr = [
    {count: 10, weight: 1, height: 2},
    {count: 20, weight: 2, height: 4},
    {count: 30, weight: 3, height: 6}];
    let sum =arr.reduce((did,cur)=>{
        for(let key in cur){
            if(!did.hasOwnProperty(key)){
                did[key] = 0
            }else{
                did[key]+=cur[key]
            }
        }
    })
#### 4.二位数组转化为一维数组
    let arr = [1,2,[3,4,5],6,[7,8,9]];
    let sum =arr.reduce((did,cur)=>{
        return did.concat(cur)
    },[])
#### 5.数组去重
    let arr = [1,2,2,3,3,3]
    let sum = arr.reduce((did,cur)=>{
        if(!did.includes(cur)){
            did.push(cur)
        }
        return did 
    },[])