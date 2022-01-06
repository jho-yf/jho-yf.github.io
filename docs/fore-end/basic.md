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

`includes()`：返回布尔值，表示是否找到了参数字符串

`startsWith()`：返回布尔值，表示参数字符串是否在源字符串头部

`endsWith()`：返回布尔值，表示参数字符串是否在源字符串尾部

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

