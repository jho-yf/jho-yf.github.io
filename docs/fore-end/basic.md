# 认识前端



## ES6

### 简介

ECMAScript6.0（简称ES6，ECMAScript是一种由Ecma国际通过ECMA-262标准化的脚本程序设计语言）是JavaScript语言的下一代标准。它的目标是使得JavaScript语言可以用来编写复杂的大型应用程序，成为企业级开发语言。

ECMAScript是浏览器脚本语言的规范，而JavaScript则是规范的具体实现。

### ES6新特性

#### let声明变量

`var`声明的变量往往会越域，`let`声明的变量由严格局部作用域

```javascript
{
    var a = 1;
    let b = 2;
}
console.log(a);		// 1
console.log(b);		// Uncaught ReferenceError: b is not defined
```

`var`可以声明多次，`let`只能声明一次

```javascript
var m = 1;
var m = 2;

let n = 3;
let n = 4;
console.log(m);		// 2
console.log(n);		// Uncaught SyntaxError: Identifier 'n' has already been declared
```

`var`会变量提升，`let`不存在变量提升

```javascript
console.log(x);		// undefined
var x = 1;
console.log(y);		// Uncaught ReferenceError: y is not defined
let y = 1;
```



#### const声明常量（只读变量）

`const`声明之后不允许改变

```javascript
const a = 1;
a = 3;				// Uncaught TypeError: Assignment to constant variable.
```

一旦声明必须初始化，否则会报错

```javascript
let a;				// undefined
const b;			// Uncaught SyntaxError: Missing initializer in const declaration
```



#### 结构表达式

##### 数组解构

```javascript
let arr = [1, 2, 3];
// x,y,z将于arr中的每个位置对于来取值
const [x, y, z] = arr;		
console.log(x, y, z);
```

##### 对象解构

```javascript
const person = {
    name: "jack",
    age: 21,
    language: ["java", "js", "css"]
}
const { name:alias, age, language} = person;
console.log(alias, age, language);
```



#### 字符串扩展

##### 扩展API

ES6为字符串扩展了几个新的API：

- `includes()`：返回布尔值，表示是否找到了参数字符串

- `startsWith()`：返回布尔值，表示参数字符串是否在源字符串头部

- `endsWith()`：返回布尔值，表示参数字符串是否在源字符串尾部

```javascript
let str = "hello.es6";
console.log(str.startsWith("hello"));
console.log(str.endsWith(".es6"));
console.log(str.includes("ll"));
```

##### 字符串模板

模板字符串用反引号`，除了作为普通字符串，还可以用来定义多行字符串，还可以在字符串中加入变量和表达式。

**多行字符串**

```javascript
let str = `
	<div>
		<span>hello world</span>
	</div>
`;
console.log(str);
```

**字符串插入变量和表达式**

变量名写在`${}`中，`${}`中可以放入JavaScript表达式

```javascript
function fun() {
    return "hello world";
}

let name = "jho";
let age = 18;
let info = `名字：${name},年龄：${age + 10},${fun()}`;
console.log(info);			// 名字：jho,年龄：28,hello world
```



#### 函数优化

##### 参数默认值

在ES6以前，无法给一个函数参数设置默认值，只能手动判断

```javascript
function add(a, b) {
    // 判断b是否为空，为空就给默认值1
    b = b || 1;
    return a + b;
}
// 调用的时候就可以只传一个参数
console.log(add(10));
```

ES6之后，可以手动设置参数默认值

```javascript
function add(a, b = 1) {
    return a + b;
}
console.log(add(10));
```

##### 不定参数

不定参数用来表示不确定参数个数的参数列表

```javascript
function fun(str, ...values) {
    console.log(str)
    console.log(values.length)
}
fun("hello", 1, 2)
fun("hello", 1, 2, 3, 4)
```

##### 箭头函数

ES6中定义**函数的简写方式**

```javascript
// 普通函数
var print = function(obj) {
    console.log(obj);
}

// 箭头函数(单参数)
var print = obj => console.log(obj);
print("hello")

// 箭头函数(多参数)
var sum = (a, b) => a + b;
console.log(sum(1, 2));

// 箭头函数(多行语句)
var sum = (a, b) => {
    c = a + b;
    return a + c;
}
console.log(sum(1, 2));

// 箭头函数 + 解构表达式
const person = {
    name: "jho",
    age: 21
}
var hello = ({name}) => console.log("hello," + name);
hello(person);			// hello,jho
```



#### 对象优化

##### 新增API

ES6给`Object`扩展了许多方法，如：

- `keys(obj)`：获取对象的所有key形成的数组

- `values(obj)`：获取对象的所有value形成的数组

- `entries(obj)`：获取对象的所有key和value形成的二维数组。格式：`[[k1, v1], [k2, v2]]`

- `assign(dest, ...src)`：将多个src对象的值拷贝到dest中。（第一层为深拷贝，第二层为浅拷贝）

```javascript
const person = {
    name: "jack",
    age: 21,
    language: ["java", "js", "css"]
}
console.log(Object.keys(person));			// ['name', 'age', 'language']
console.log(Object.values(person));			// ['jack', 21, Array(3)]
console.log(Object.entries(person));		// [Array(2), Array(2), Array(2)]

const target = {a: 1};
const source1 = {b: 2};
const source2 = {c: 3};
Object.assign(target, source1, source2);
console.log(target);						// {a: 1, b: 2, c: 3}
```

##### 声明对象的简写方式

ES6属性名和属性值变量名一样，可以忽略

```javascript
const age = 23
const name = "jho"

// 传统写法
const person1 = {
    age: age,
    name: name
}
console.log(person1)

// ES6
const person2 = { age, name }
console.log(person2)
```

##### 对象的函数属性简写

```javascript
let person = {
    name: "jho",
    // 传统
    eat1: function(food) {
        console.log(this.name + " eat " + food);
    },
    // 箭头函数：获取不到this
    eat2: food => console.log(person.name + " eat " + food),
    // 简写版
    eat3(food) {
        console.log(this.name + " eat " + food);
    }
}
person.eat1("apple");
person.eat2("cake");
person.eat3("banana");
```

##### 对象的扩展运算符

扩展运算符（`...`）用于取出参数对象所有可遍历属性然后拷贝到当前对象

```javascript
// 拷贝对象（深拷贝）
let person = {
    name: "jho",
    age: 15
}
let someone = { ...person }
console.log(someone);			// {name: 'jho', age: 15}

// 合并对象
let age = {age: 15}
let name = {name: "jho", age:21}
// 如果两个对象的字段名重复，后面对象字段会覆盖前面对象的字段值
let p = { ...age, ...name }
console.log(p);			// {age: 21, name: 'jho'}
```

#### 数组优化

##### map

`map()`：接收一个函数，将原数组中的所有元素用这个函数处理后放入新数组返回

##### reduce

`reduce()`：为数组中的每一个元素一次执行回调函数，不包括数组中被删除或从未被赋值的元素

## Vue

更新中...
