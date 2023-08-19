---
  title: JavaScript
---
## JavaScript
`补充下之前学习JavaScript的知识,以下笔记出自珠峰or王红元老师,这里非常感谢两位老师！`

### Symbol类型

Symbol 创建一个唯一值

- 给对象设置”唯一值”的属性名
    - 字符串
    - Symbol类型
    - Map新的数据结构(可以允许属性名是对象)
- Symbol.asyncIterator/iterator/hasInstance/toPrimitive/toStringTag…(某些js知识底层实现机制)
- 在派发行为标识统一进行管理的时候,可以基于Symbol实现

### BigInt类型

服务器端存储长整型数据,JS无法存储大数字

Number.MAX_INTEGER JS中的最大安全数

Number.MIN_INTEGER JS中的最小安全数(超过安全数后,进行运算或访问时,结果不正确)

解决方案:

- 服务器返回给客户端的大数,按照字符串格式返回
- 客户端把其变为BigInt,然后按照BigInt进行计算
- 最后把运算后的BigInt转换为字符串,在传递给服务器即可

### 数据类型检测

- typeof
    - typeof检测数据类型
        - 所有的数据类型值,在计算机底层都是按照”32/64位“的二进制值进行存储的
        - typeof是按照二进制值进行检测类型的
        - 检测未被声明的变量,值为”undefined“

| Value | Result |
| --- | --- |
| undefined | “undefined” |
| null | “object” |
| boolean | “boolean” |
| number | “number” |
| NaN | “number” |
| string | “string” |
| symbol | “symbol” |
| object | “object” |
| bigint | “bigint” |
| function | “function” |
| regexp | “object” |
- instanceof
    - typeof操作符适合用来判断一个变量是都为原始类型,判断一个变量是否为字段串、数值、布尔值、undefined，如果变量为对象或者null,那么typeof返回”object“;JS提供了instanceof操作符
    - **`instanceof`运算符**用于检测构造函数的 `prototype`属性是否出现在某个实例对象的原型链上。
    - 如果变量是给定的引用类型(由其原型链决定)的实例,则instanceof操作符返回true
    
    <aside>
    💡 result = variable instanceof constructor “variable”—实例对象 ”constructor“—构造函数
    
    </aside>
    
- constructor
    - constructor是Object的原型属性,能够返回当前对象的构造器(类型函数)
    - undefined和null没有constructor属性,不能直接读取,否则抛出异常

```jsx
const num = 1 
consloe.log(num.constructor)  //ƒ Number() { [native code] }
```

- Object.prototypr.toString.call()
    - 对于Object.prototypr.toString()方法,会返回一个”[object …]“的字符串
    - 如果toStirng()方法未被重写,就会返回上面形式的字符串
- Array.isArray()
    - Array.isArray()用于确定传递的值是否是一个Array;如果值数组返回true,否则为false
- isNaN
    - 用来确定一个值是否为NaN;如果值为NaN则返回true,否则为false

### 数据类型转换

1. 其他类型转换为Number
    1. 字符串:空字符串变为0,如果出现任何非有效数字字符,结果为NaN
    2. 布尔值:true→1 false→0
    3. null→0 undefined→NaN
    4. Symbol无法转换为数字
    5. BigInt去除‘n’(超出安全数会按照科学计数法处理)
    6. 对象:
        1. 先调用对象的Symbol.toPrimitive(),如果不存在
        2. 再调用对象的valueOf获取原始值,如果获取的不是原始值
        3. 再调用对象的toString把其变为字符串
        4. 最后再把字符串基于Number方法转换为数字
        
        ```jsx
        const obj = new Date()
        console.log(obj[Symbol.toPrimitive]('number'));
        console.log(Number(obj));
        ```
        
    7. parseInt([val],[radix])
        1. [val]必须是字符串,如果不是,要先隐式转换为字符串Stirng([val])
        2. [radix]进制:不写默认是10,如果字符串是以0x开始的默认是16(有效范围2~36),不在这个区间,结果为NaN
        3. 从[val]字符串左侧第一个字符开始查找,查找出符合[radix]进制的值(遇到不符合的则结束查找,不论后面是否还有符合的),把找到的内容,按照[radix]进制,转换为十进制
    
    ```jsx
    let arr = [27.2, 0, '0013', '14px', 123]
    arr = arr.map(parseInt)
    console.log(arr) // [ 27, NaN, 1, 1, 27 ]  #每行value对应index parseInt(value,index)
    ```
    
2. 其他类型转换为String
    1. 用’’或“”包起来
    2. 特殊值:Object.prototype.toString()
    3. String([val])或[val].toString()
    
    ```jsx
    console.log(10 + '10') // '1010'
    
    console.log(10 + new Date(10)) // 10Thu Jan 01 1970 08:00:00 GMT+0800 (中国标准时间)
    // new Date()[Symbol.toPrimitive]('default') -> Thu Jan 01 1970 08:00:00 GMT+0800 (中国标准时间)
    
    console.log(10 + new Number(10)) // 20
    // new Number(10)[Symbol.toPrimitive] -> undefined
    // new Number(10).valueOf() -> 10
    
    console.log(10 + [10]) // '1010'
    // [10].toString() -> '10'
    ```
    
3. 其他数据类型转换为Boolean
    1. 除了0/NaN/false/undefined/空字符串五个值是false,其余都是true
4. ‘==’与‘===’比较
    1. 双等,两边数据类型不同,需要先转为相同类型,然后再进行比较
    2. 对象 == 字符串 对象转字符串Symbol.toPrimitive → valueOf → toString
    3. null == undefined → true null/undefiend和其他任何值都不相等 null === undefined → false
    4. 对象 == 对象 比较的是堆内存地址,地址相同则相同
    5. NaN ！== NaN
    6. 除了以上情况,只要两边类型不一致,剩下的都是转换为数字,然后再进行比较; ‘===’全等,如果两边类型不同,则直接是false

---

> 0.1 + 0.2 = 0.3?
> 

```jsx
0.1 + 0.2 = 0.3? // JS中利用二进制进行存储,对浮点数转换成二进制数,可能会存在无限循环
```

解决方法:

1. toFixed()保留小数点后面n位
2. 扩大系数
3. 三方库Math.js,big.js

> let a = ? 满足a==1&&a==2&&a==3
> 

```jsx
let a = {
  i: 0,
  [Symbol.toPrimitive]() {
    return ++this.i
  }
}
if (a == 1 && a == 2 && a == 3) {
  console.log('ok');
}
/*
	let a = [1,2,3]
	a.toString = a.shift
*/
/*
	var i = 0
	Object.defineProperty(window,'a'){
			get(){
				return ++i
			}
	} 
*/
```

### JS堆栈内存

浏览器在打开一个页面时,首先会从计算机的虚拟内存中分配两块内存出来

- 栈内存Stack
    - 供代码执行
    - 存储声明的变量和原始值类型的值
- 堆内存Heap
    - 存储引用数据类型的值
    - 默认在堆内存中,开辟一个空间‘16进制地址’GO(global object)全局对象,存储了浏览器为js提供的内置API

在栈内存中创建一个全局的执行上下文EC(G)

- 供全局代码执行的环境
- 进栈执行

代码执行过程中可能会声明变量,所以需要一个存放变量的地方→变量对象VO/AO

创建值:

- 原始值类型:直接存储在栈内存中
- 引用数据类型:
    - 在堆内存中开辟一个新的空间(16进制地址),把键值对存储到堆内存空间中,
    - 声明变量(declare)将声明的变量放到栈内存中,
    - 让创建的内存地址指向声明的变量

### 变量提升

在‘当前上下文’中,代码执行之前,浏览器首先会把所有带var/function关键字的进行提前声明或定义:带var的只是提前声明&function的,此阶段声明+定义[赋值]都完成了

> 其实最开始浏览器从服务端获取的JS都是文本(字符串).只不过声明了其格式是(Contnet-Type:application/javascript),浏览器首先按照这个格式去解析代码→‘词法解析’阶段(目标是生成‘AST词法解析树’);基于let/const等声明的变量:在词法解析阶段,其实就已经明确了,未来在此上下文中,必然会存在这些变量;代码执行中,如果出现在具体声明的代码之前使用这些变量,浏览器会抛出错误
> 

### 块级私有上下文

除函数和对象的‘{}’外,例如判断体/循环体/代码块,如果{}中出现了let/const/function/class等关键字声明,则当前大括号会产生一个块级私有上下文,它的上级上下文是所处的环境;var不产生,也不受块级上下文的影响

var、let、const三者区别

- 变量提升
    - var声明的变量存在变量提升,即变量可以在声明之前调用,值为undefined;let/const不存在变量提升,即先声明后使用,否则报错
- 暂时性死区
    - var不存在暂时性死区;let/const存在暂时性死区,只有等到声明变量的那一行代码出现,才可以获取和使用该变量
- 块级作用域
    - var不存在块级作用域;let/const存在块级作用域
- 重复声明
    - var允许重复声明变量;let/const在同一作用域不允许重复声明变量
- 修改声明的变量
    - var/let可以修改已声明的变量,const声明的是常量(不准确),它声明的函数变量,基于cont声明的变量,首先必须赋值初始值,而且一旦与某个值关联,后期不允许更改其指针指向(不能重新赋值),对于对象而言,const声明的对象不更改地址指向是可以修改对象的value

### 匿名函数

- 匿名函数具名化:更规范、可以让原本的匿名函数实现递归操作
    - 自执行函数
    - 函数表达式 const fn = function fn(){} fn()
1. 不会再所处上下文(宿主环境)中进行声明:设置的名字在外面用不了
2. 在自己执行产生的上下文中会被声明赋值,赋值为当前函数本身—(function fn() { fn = 100; console.log(fn)})() //当前函数
3. 而且赋的值默认是不能被修改的;但是如果此名字被其他方式声明了(let/const/var),则以其他声明方式为主

### 浏览器的垃圾回收机制(内存释放)

堆内存:

- 看当前堆内存空间的地址,是否被占用;如果没有占用:浏览器会在空闲的时候释放这个堆内存;如果被占用:则不能被释放;将所引用的赋值为null,可以手动释放无用的内存

栈内存:

- EC(G)全局上下文,打开页面的时候形成,只有页面关闭才会释放
    - 正常境况下,代码执行完,产生的私有上下文会被释放
    - 特殊情况,如果上下文中创建的某个函数,被上下文意外的事物占用,则不仅这个东西不能被释放,而且当前这个私有上下文也不能被释放

### 闭包

函数执行产生的私有上下文,这里的私有变量被’保护’起来,不受外界干扰,而且如果上下文不被释放,则里面的私有变量值也会被’保护’下来,这样其下级上下文就可以操作(访问/修改)这些值,这种‘保护’+‘保存’的机制,称为闭包

柯里化函数(curring)
一种思想,一种预先存储的思想,也是闭包的进阶应用(保存);我们让函数执行产生闭包,把一些后续要用到的值,存储到闭包的某个私有变量中,那么其下级上下文想用的时候直接拿来用即可;所谓"柯里化"，就是把一个多参数的函数，转化为单参数函数
```js
const curring = function curring() {
	// EC(CURRING)会产生闭包
	let params = []
	const add = (...args) => {
		// 把每次执行add函数传递的实参值,都存储到闭包的params
		params = params.concat(args)
		return add
	}
	// 在把对象转换为数字的时候,我们对params进行求和
	add[Symbol.toPrimitive] = () => {
		return params.reduce((prev,curr) => prev + curr)
	}
	return add
}
```
​
compose函数(函数合成)
在函数式编程当中有一个很重要的概念就是函数组合,实际上就是把处理数据的函数像管道一样连接起来,然后让数据穿过管道得到最终的结果
```js
const add = x=>x+1
const mul = x=>x*3
const div = x=>x/2

const compose =  function compose(...funcs) {
	// EC(COMPOSE) 闭包
	if(func.length===0) return x=>x
	return function operate(x) {
		return funcs.reduceRight((x,func) =>{
			return func(x)
		},x)
	}
}
```
​
