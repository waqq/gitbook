### call、apply

##### 一、call、apply 相同点

call和apply是为了改变函数体内的this指向

```javascript
function fruits(){};
fruits.prototype = {
  color: 'red',
  sayColor: function(){
    return this.color;
  }
}
let apple = new fruits();
apple.sayColor();//'red'

let bannar = {
  color: 'yellow'
}
apple.sayColor.call(bannar);//'yellow'
apple.sayColor.apply(bannar);//'yellow'

```

当对象A中没有某个方法，但对象B有，可以借用B的方法通过call、apply来执行

##### 二、call、apply 区别

传参不同

```javascript
var fn = function(){}
fn.call(this, arg1, arg2);
fn.apply(this, [arg1, arg2]);
```

call需要一个一个传、apply通过数组传

##### 三、call、apply实例

1. 数组追加

   ```javascript
   var arr1 = [1, 2, 3, 4],
   		arr2 = ['a', 'b'];
   Array.prototype.push.apply(arr1, arr2);
   console.log(arr1);//[1, 2, 3, 4, "a", "b"]
   ```

2. 数组中的最大最小值

   ```javascript
   var numbers = [1, 0, 3, 2];
   var max = Math.max.apply(Math, numbers);
   console.log(max);//3
   ```

3. 伪数组使用数组的方法

   ```javascript
   var docNodes = Array.prototype.slice.call(document.getElementsByTagName('p'));
   ```

   `document.getElementsByTagName('p')`返回的数组属于伪数组，没有push、pop等方法，使用数组的slice方法可以将伪数组转换为 真正数组所有带有属性的 数组对象，类似的还有arguments

4. 验证是否为数组

   ```javascript
   let isArray = obj => Object.prototype.toString.call(obj) === "[object Array]";
   ```

5. 综合用例 使用log代替console.log 并且在输出前添加特殊字符串 `'(app)'`

   ```javascript
   let log = (...arguments) => {
     var args = Array.prototype.slice.call(arguments);
     args.unshift('(app)');
     console.log.apply(console, args);
   }
   log('2', '3');//(app) 2, 3
   ```

### bind

##### bind的用法

```javascript
var alWrite = document.write;
alWrite();//Uncaught TypeError: Illegal invocation
alWrite.bind(docuemnt)('hello');//hello
```

直接调用_write 会报出 非法调用的 错误 
在执行alWrite时，alWrite指向的是global（window），加上bind后，alWrite的this指向document

bind的用法：使得一个函数不论在什么情况下调用，其this都指向同一个对象

实例1：

```javascript
this.num = 1;
let moudel = {
  num: 2,
  getNum: function(){
    console.log(this.num)
  }
}
moudel.getNum();//2
let newGetNum = moudel.getNum;
newGetNum();//1
newGetNum.bind(moudel)();//2
```

实例2：

```javascript
let fnc = function{
	console.log(this.name)
}
let obj1 = {
  name: 'objName'
}
fnc();//undefined
let fncBind = fnc.bind(obj1);
fncBind();//'objName'
```

实例3：

```javascript
let obj = {
  name: 'name',
  domBind: function(){
    $('#id').click(funciton(){
    	consle.log(this.name)               
    }.bind(this))
  }
}
```

### call、apply、bind区别

```javascript
var obj = {
    x: 81,
};
 
var foo = {
    getX: function() {
        return this.x;
    }
}
 
console.log(foo.getX.bind(obj)());  //81
console.log(foo.getX.call(obj));    //81
console.log(foo.getX.apply(obj));   //81
```

bind相比与call、apply多了一个括号 也就是说 在某些情况下只想改变函数的this，但不需要立即执行 这时可以用bind

------

`apply` 、 `call` 、`bind` 三者都是用来改变函数的this对象的指向的；
`apply` 、 `call` 、`bind` 三者第一个参数都是this要指向的对象，也就是想指定的上下文；
`apply` 、 `call` 、`bind` 三者都可以利用后续参数传参；
`bind` 是返回对应函数，便于稍后调用；`apply` 、`call` 则是立即调用 。

