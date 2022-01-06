# Java面向对象



## 类和对象的初始化

**父类**

```java
class Father {

    private int i = method();

    private static int j = staticMethod();

    static {
        System.out.println("(1)父类初始化静态代码块");
    }

    public Father() {
        System.out.println("(2)父类初始化构造函数");
    }

    {
        System.out.println("(3)父类初始化普通代码块");
    }

    public int method() {
        System.out.println("(4)父类初始化类变量");
        return 0;
    }

    public static int staticMethod() {
        System.out.println("(5)父类初始化类静态变量");
        return 0;
    }

}
```

**子类**

```java
class Son extends Father {

    private int i = method();

    private static int j = staticMethod();

    static {
        System.out.println("(6)子类初始化静态代码块");
    }

    public Son() {
        System.out.println("(7)子类初始化构造函数");
    }

    {
        System.out.println("(8)子类初始化普通代码块");
    }

    @Override
    public int method() {
        System.out.println("(9)子类初始化类变量");
        return 0;
    }

    public static int staticMethod() {
        System.out.println("(10)子类初始化类静态变量");
        return 0;
    }

}
```

**Main**

```java
public class ClassInitDemo {

    public static void main(String[] args) {
        Son son1 = new Son();
        System.out.println();
        Son son2 = new Son();
    }

}

// (5)父类初始化类静态变量
// (1)父类初始化静态代码块
// (10)子类初始化类静态变量
// (6)子类初始化静态代码块
// (9)子类初始化类变量
// (3)父类初始化普通代码块
// (2)父类初始化构造函数
// (9)子类初始化类变量
// (8)子类初始化普通代码块
// (7)子类初始化构造函数

// (9)子类初始化类变量
// (3)父类初始化普通代码块
// (2)父类初始化构造函数
// (9)子类初始化类变量
// (8)子类初始化普通代码块
// (7)子类初始化构造函数
```

**类初始化过程：**

- 一个类要创建实例需要先加载并初始化该类（main方法所在的类需要先加载和初始化）

- 一个子类初始化需要先初始化父类

- 一个类初始化就是执行`<clinit>()`方法

  - `<clinit>()`方法由**静态类变量显式赋值代码**和**静态代码块组成**
  - **类变量显式复制代码**和**静态代码块**从上到下顺序执行
  - `<clinit>()`方法只执行一次

  

**实例初始化过程：**

- 一个子类初始化构造方法的时候，默认会先调用`super()`来初始化父类构造方法
- 实例初始化就是执行`<init>()`方法
  - `<init>()`方法可能重载有多个，有个构造方法就有几个`<init>()`方法
  - `<init>()`方法由**非静态实例变量显式赋值代码**、**非静态代码块**和**构造器代码**组成
  - **非静态实例变量显式赋值代码**和**非静态代码块**从上到下执行，**构造器代码**最后执行

**方法的重写`Override`**

- 哪些方法不可以被重写
  - `final`方法
  - 静态方法
  - `private`等子类中不可见的方法
- 对象的多态性
  - 子类如果重写了父类的方法，通过子类对象调用的一定是子类重写过的代码
  - 非静态方法默认的调用对象是`this`
  - `this`对象在构造器或者`init()`方法中就是正在创建的对象
