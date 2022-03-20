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

```javascript
let arr = ['1', '20', '-5'];
arr = arr.map((item)=>{
   return item * 2; 
});
// arr = arr.amp(item=>item * 2);
console.log(arr);			// [2, 40, -10]
```

##### reduce

`reduce()`：为数组中的每一个元素一次执行回调函数，不包括数组中被删除或从未被赋值的元素。接收四个参数：初始值（或者上一次回调函数的返回值）、当前元素值、当前索引、调用`reduce`的数组。

**语法：**

```javascript
arr.reduce(callback, [initialValue])
```

`callback`（执行数组中每个值的函数，包含四个参数）：

- `previousValue`：上次调用回调返回的值，或者是提供初始值（`initialValue`）
- `currentValue`：数组中当前被处理的元素
- `index`：当前元素在数组中的索引
- `array`：调用reduce的数组

`initialValue`：作为第一次调用callback的第一个参数

```javascript
let arr = [1, 20, -5];
let result = arr.reduce((a, b)=> {
    console.log("上次处理后：" + a);
    console.log("当前正处理：" + b);
    return a + b;
}, 100);
console.log(result);			// 116
```



#### Promise异步编排

以下笔记来自：[理解JavaScript Promise](https://zhuanlan.zhihu.com/p/26523836)

`Promise`说得通俗一点就是一种写代码的方式，并且是用来写JavaScript编程中的异步代码的，封装异步操作。

##### 基本用法

```javascript
let p = new Promise((resolve, reject) => {
  // 做一些事情
  // 然后在某些条件下resolve，或者reject
  if (/* 条件随便写^_^ */) {
    resolve()
  } else {
    reject()
  }
})

p.then(() => {
    // 如果p的状态被resolve了，就进入这里
}, () => {
    // 如果p的状态被reject
}).then(...).then(...)
```

第一段调用了`Promise`构造函数创建实例，第二段调用了`Promise`示例的`then()`方法。



#### 模块化

##### 什么是模块化

**模块化**就是把代码进行拆分，方便重复利用。类似于Java中的导包，而在JS中没有包的概念，而是**模块**。

模块功能主要由`export`和`import`两个命令构成：

- `export`：用于规定模块的外对接口
- `import`：用于导入其他模块提供的功能

##### export

`export`可以导出基本类型变量、函数、数组、对象。

```javascript
// common.js
const util = {
    sum(a, b) {
        return a + b;
    }
}

function add(a, b) {
    return a + b;
}

var name = "jho";
var age = 21;

export {util, add, name, age}
```

默认`export`

```javascript
// test.js
export default {
    sum(a, b) {
        return a + b;
    }
}
```

##### import

```javascript
import util from "./common.js"
import {name,age} from "./common.js"
import test from "./test.js"			// 这里的test可以任意命名

util.sum(1, 2);
console.log(name);
test.sum(1, 2);
```



## Vue

### MVVM思想

![edd0080fb145315fbc96164c219fee7e_hd](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202201102248019.png)

在`MVVM`之前，前端人员从后端获取需要的数据模型，然后要通过`DOM`操作`Model`渲染到`View`中。而后当用户操作视图，还需通过`DOM`获取`View`中的数据，然后同步到`Model`中。`MVVM`中的**VM**做的事情就是把`DOM`操作完全封装起来，开发人员不用再去更新`Model`和`View`之间是如何相互影响的。

**M：**Model，模型，包括数据和一些基本操作

**V：**View，视图，页面渲染结果

**VM：**View-Model，模型与视图间的双向操作（无需开发人员操作）

- 只要Model发生改变，View上自然就会表现出来。

- 当用户修改了View，Model中的数据也会跟着改变



### Vue简介

`Vue`是一套用于构建用户界面的**渐进式框架**。与其他大型框架不同的是，Vue被设计为可以自底向上逐层应用。Vue的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue也完全能够为复杂的单页应用提供驱动。

> 官网：https://cn.vuejs.org/
>
> 教学文档：[2.x](https://cn.vuejs.org/v2/guide/)、[3.x](https://v3.cn.vuejs.org/guide/introduction.html)
>
> Github：https://github.com/vuejs/vue



### HelloWorld

使用`npm`安装vue

```bash
# 最新稳定版
npm install vue
```

编写HelloWorld

```html
<body>

    <div id="app">
        <h1>hello,{{name}}</h1>
        <input type="text" v-model="str">
        <p>{{str}}</p>
        <button v-on:click="num++">点赞{{num}}</button>
        <button v-on:click="cancel">取消</button>
    </div>

    <script src="./node_modules/vue/dist/vue.js"></script>

    <script>
        // vue声明式渲染
        // 双向绑定：模型变化，视图变化。反之亦然。
        let vm = new Vue({
            el: "#app",                 // el 绑定元素
            data: {                     // data 封装数据
                name: "JHO",
                str: "输入什么就显示什么",
                num: 0
            },
            methods: {                  // methods 封装方法
                cancel() {
                    if (this.num > 0) {
                        this.num--;
                    }
                }
            }
        });

    </script>
</body>
```



### 指令（v-xx）

#### 插值表达式

##### 花括号

格式：`{{表达式}}`

说明：

- 该表达式支持JS语法，可以调用JS内置函数（必须有返回值）
- 表达式必须有返回结果
- 可以直接获取Vue示例中定义的数据或函数

##### 插值闪烁

使用`{{xxx}}`方式在网速较慢时会出现问题。在数据未加载完成时，页面会显示出原始的`{{xxx}}`，加载完毕才会显式正确数据。

#### v-text、v-html

- `v-text`：给标签体绑定文字
- `v-html`：给标签体内嵌html
- `v-text`、`v-html`属于**单向绑定**

**代码**

```html
<body>
    <div id="app">
        {{msg}} {{1+1}} {{myMethod()}}
        <br/>
        <span v-html="msg"></span>
        <br/>
        <span v-text="msg"></span>
    </br>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        new Vue({
            el:"#app",
            data: {
                msg: "<h1>Hello</h1>"
            },
            methods: {
                myMethod() {
                    return "myMethod...return"
                }
            }
        })
    </script>
</body>
```

**输出**

![image-20220304072055614](C:/Users/JHO/AppData/Roaming/Typora/typora-user-images/image-20220304072055614.png)



#### v-bind

给html标签的属性绑定内容，`v-bind`属于**单向绑定**

**代码**

```html
<body>
    <div id="app">
        <a v-bind:href="blogLink">乙方小弟的个人笔记</a>
        <!-- class,style增强 
                v-bind:class="{class名:变量}"
                v-bind:style="{style名:变量}"
            -->
        <span 
            v-bind:class="{active:isActive,'text-danger':hasError}" 
            v-bind:style="{color: colorVar,fontSize:size}">Hello~</span>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        let vm = new Vue({
            el:"#app",
            data: {
                blogLink: "https://jho-yf.github.io",
                isActive: true,
                hasError: false,
                colorVar: 'blue',
                size: '36px'
            }
        })
    </script>
</body>
```

**输出**

![image-20220304073335842](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202203040733996.png)

#### v-model

对表单项，自定义组件的双向绑定

**代码**

```html
<body>
    <!-- 表单项目，自定义组件 -->
    <div id="app">
        精通的语言：
        <input type="checkbox" v-model="language" value="Java"> Java
        <br/>
        <input type="checkbox" v-model="language" value="PHP"> PHP
        <br/>
        <input type="checkbox" v-model="language" value="Python"> Python
        <br/>
        选中了 {{language.join(",")}}
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        let vm = new Vue({
            el: "#app",
            data: {
                language: []
            }
        });
    </script>
</body>
```

**输出**

![image-20220304075306676](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202203040753848.png)



#### v-on

##### 基本用法

```html
<div id="app">
    <!-- 事件中直接写入js片段 -->
    <button v-on:click="num++">点赞</button>

     <!-- 事件指定一个回调函数，必须是Vue实例中定义的函数 -->
    <button v-on:click="cancel">取消</button>

    <!-- @click是v-on:click的简写 -->
    <button @click="cancel">取消</button>
</div>
    
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            num: 0,
        },
        methods: {
            cancel() {
                if (this.num > 0) {
                    this.num--;
                }
            }
        }
    });
</script>
```

##### 事件修饰符

`Vue.js`为`v-on`提供了**事件修饰符**，修饰符是由点开头的指令后缀来表示的。

- `.stop`：阻止事件冒泡到父元素
- `.prevent`：阻止默认事件发生
- `.capture`：使用事件捕获模式
- `.self`：只有元素自身触发事件才执行，冒泡或捕获都不会执行
- `.once`：只执行一次

##### 按键修饰符

在监听键盘事件时，经常需要检查常用的键值。

```html
<!-- 只有在keyCode是13时调用submit -->
<input v-on:keyup.13="submit">

<!-- 常用keyCode有提供别名 -->
<input v-on:keyup.enter="submit">
<input @keyup.enter="submit">
```

常用按键别名：

- `.enter`
- `.tab`
- `.delete`：捕获“删除”和“退格”键
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`



#### v-for

[代码示例](https://github.com/jho-yf/yf-vue-quick-start/blob/main/vue2/02%E6%8C%87%E4%BB%A4/05_v-for.html)

#### v-if和v-show

[代码示例](https://github.com/jho-yf/yf-vue-quick-start/blob/main/vue2/02%E6%8C%87%E4%BB%A4/06_v-if%E5%92%8Cv-show.html)

#### v-else和v-else-if

[代码示例](https://github.com/jho-yf/yf-vue-quick-start/blob/main/vue2/02%E6%8C%87%E4%BB%A4/07_v-else%E5%92%8Cv-else-if.html)



### 计算属性和侦听器

[计算属性和侦听器代码示例](https://github.com/jho-yf/yf-vue-quick-start/blob/main/vue2/03%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7%E5%92%8C%E4%BE%A6%E5%90%AC%E5%99%A8/01_%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7%E5%92%8C%E4%BE%A6%E5%90%AC%E5%99%A8.html)

[过滤器代码示例](https://github.com/jho-yf/yf-vue-quick-start/blob/main/vue2/03%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7%E5%92%8C%E4%BE%A6%E5%90%AC%E5%99%A8/02_%E8%BF%87%E6%BB%A4%E5%99%A8.html)



### 组件化

在大型应用开发的时候，页面可以划分成很多部分。往往不同的页面，也会有相同的部分。但是如果每个页面都是独立开发，这无疑增加了我们的开发成本。所以我们会把页面的不同部分拆分成独立的组件，然后在不同页面就可以共享这些组件，避免重复开发。

在vue中，所有的vue实例都是组件。

![Component Tree](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202203191518996.png)

- 组件其实是个Vue实例，因此它在定义时也会接收：`data`、`methods`、生命周期函数等
- 组件是不与页面元素绑定的，否则无法复用，因此没有el属性，组件的渲染需要html模板，所以添加了`template`属性，值就是HTML模板
- 全局组件定义完毕，任何Vue实例都可以直接在HTML中通过组件名称来使用组件了
- `data`必须是一个函数，不再是一个对象

#### 代码

```html
<div id="app">
    <button v-on:click="count++">我被点击了{{count}}次</button>
    <counter></counter>
    <counter></counter>
    <counter></counter>
    <counter></counter>
    <br>
    <button-counter></button-counter>
    <button-counter></button-counter>
    <button-counter></button-counter>
</div>
<script src="../node_modules/vue/dist/vue.js"></script>
<script>
    // 1、全局声明注册一个组件
    Vue.component("counter", {
        template: `<button v-on:click="count++">全局组件，我被点击了{{count}}次</button>`,
        data() {
            return {
                count: 0
            }
        }
    })

    // 2、局部声明一个组件
    const buttonCounter = {
        template: `<button v-on:click="count++">局部组件，我被点击了{{count}}次</button>`,
        data() {
            return {
                count: 0
            }
        }
    };

    new Vue({
        el: "#app",
        data: {
            count: 0
        },
        // 注册组件
        components: {
            'button-counter': buttonCounter
        }        
    })
</script>
```



### 生命周期和钩子函数

#### 生命周期

每个Vue实例在被创建时都要经过一系列的初始化过程：创建实例、装载模板、渲染模板等等。

![Vue 实例生命周期](https://gitee.com/jho-yf/yf-pic-repo/raw/master/202203191549739.png)

#### 钩子函数

Vue为生命周期中的每一个状态都设置了钩子函数（监听函数）。每当Vue实例处于不同的生命周期时，对应的函数就会被触发调用。



### vue模块化开发

#### 全局安装`webpack`

```shell
npm install webpack -g
```

#### 全局安装vue脚手架

```shell
npm install -g @vue/cli-init
```

#### 初始化vue项目

```shell
# vue脚手架使用webpack模块初始化一个appname项目
vue init webpack appname
```

#### 启动vue项目

项目中的`package.json`中`scripts`，代表我们能运行的命令

```shell
# 启动项目方式一
npm run dev
# 启动项目方式二
npm start

# 打包项目
npm run 
```

