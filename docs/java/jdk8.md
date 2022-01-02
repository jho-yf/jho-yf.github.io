# JDK8新特性

- 速度更快：优化底层数据结构
- 代码更少：**`Lamdda`表达式**
- **`Stream API`**
- 便于并行
- 最大化减少空指针异常：`Optional`

## Lambda表达式

### 基础语法

Java8引入一个新的操作符号`->`，称为`箭头操作`或`Lambda操作符`

- `->`左侧：`Lambda`表达式的参数列表
- `->`右侧：表达式中所需执行的功能，即`Lambda`体

### 语法格式

- 无参数，无返回值

```java
() -> System.out.println("Hello Lambda")
```

- 有一个参数，无返回值

```java
(x) -> System.out.println(x)
```

- 若只有一个参数，小括号可以省略不写

```java
x -> System.out.println(x)
```

- 多个参数，有返回值，`Lambda`体中有多条语句

```java
Comparator<Integer> compare = (x, y) -> {
    System.out.println(x);
    return Integer  .compare(x, y);
}
```

- 若`Lambda`体中只有一条语句，`return`和大括号可以不写

```java
Comparator<Integer> compare = (x, y) -> Integer.compare(x, y);
```

- `Lambda`表达式的参数列表的数据类型可以省略不写，因为JVM编译器可以通过上下文进行类型推断

```java
// Comparator<Integer> com = (Integer x, Integer y) -> Integer.compare(x, y);
Comparator<Integer> com = (x, y) -> Integer.compare(x, y);
```





## 函数式接口

> `Lambda`表达式需要**函数式接口**的支持

- 接口中，只有一个抽象方法，称为**函数式接口**
- 可以使用注解`@FunctionalInterface`修饰，检查是否是函数式接口

```java
@FunctionalInterface
public interface MyPredicate<T> {
    boolean test(T t);
}
```



## Java内置函数式接口

### 四大核心接口

| 类型       | 函数式接口      | 参数类型 | 返回值类型 | 用途                                                         |
| ---------- | --------------- | -------- | ---------- | ------------------------------------------------------------ |
| 消费型接口 | `Consumer<T>`   | `T`      | `void`     | 对类型为`T`的对象应用操作，包含方法：`void accept(T t)`      |
| 供给型接口 | `Supplier<T>`   | 无       | `T`        | 返回类型为`T`的对象，包含方法：`T get()`                     |
| 函数型接口 | `Funtion<T, R>` | `T`      | `R`        | 对类型为`T`的对象应用操作，并返回结果为`R`类型的对象，包含方法：`R apply(T t)` |
| 断言型接口 | `Predicate<T>`  | `T`      | `boolean`  | 确定类型为`T`的对象是否满足约束，并返回`boolean`值，包含方法：`boolean test(T t)` |

### 其他接口

| 函数式接口                                                   | 参数类型                  | 返回值类型                | 用途                                                         |
| ------------------------------------------------------------ | ------------------------- | ------------------------- | ------------------------------------------------------------ |
| `BiFunction<T, U, R>`                                        | `T,U`                     | `R`                       | 对类型为`T`、`U`的对象应用操作,返回类型为`R`的对象，包含方法：`R apply(T t, U u)` |
| `UnaryOperator<T>`                                           | `T`                       | `T`                       | 对类型为`T`的对象应用操作，返回类型为`T`的对象，包含方法：`T apply(T t)` |
| `BinaryOperator<T>`<br />`BiFunction<T, U, R>`的子接口       | `T,T`                     | `T`                       | 对类型为`T`的对象进行二元运算，并返回类型为`T`的对象，包含方法：`T apply(T t1, T t2) ` |
| BiConsumer<T, U>                                             | `T,U`                     | void                      | 对类型为`T`、`U`的对象应用操作，包含方法：`void accept(T t, U u)` |
| ToIntFunction<T><br />ToLongFunction<T><br />ToDoubleFunction<T> | `T`                       | int<br />long<br />double | 分别计算`int`、`long`、`double`值的函数                      |
| IntFunction<R><br />LongFunction<R><br />DoubleFunction<R>   | int<br />long<br />double | `R`                       | 参数分别为`int`、`long`、`double`类型的函数                  |



## 方法引用和构造器引用

### 方法引用

- Lambda表达式的另外一种表现形式

- 若`Lambda`体中的内容有方法已经实现，我们可以使用方法引用


#### 语法格式

- `Lambda`体中调用方法的参数列表与返回值类型，需要与函数式接口中抽象方法参数列表和返回值类型一致

```java
// 对象::实例方法名;
Consumer<String> consumer = System.out::println;

// 类::静态方法名;
Comparator<Integer> comparator = Integer::compare;

// 类::实例方法名;
// Lambda表达式的第一个参数是实例方法的调用者，第二个参数是实例方法的参数，可以使用
BiPredicate<String, String> predicate = String::equals;
```



### 构造器引用

- 需要调用的构造器参数列表需要与函数式接口中抽象方法的参数列表保持一致

```java
// ClassName::new
Supplier<Employee> sup = Employee::new;
Function<Integer, Employee> func = Employee::new;
```



### 数组引用

```java
// xType::new
```



## Stream

> `Stream`是Java8中处理**集合**的关键概念，可以对集合进行指定操作，可以执行非常复杂的查找、过滤和映射数据等操作。



### Stream的概念

Stream是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。

> “集合讲的是数据，流讲的是计算”

- `Stream`不会存储元素
- `Stream`不会改变源对象。但是会返回一个持有结果的新`Stream`
- `Stream`操作是延迟执行。意味着他们会等到需要结果的时候才执行。



### Stream操作的三个步骤

!["Stream操作步骤"](https://gitee.com/jho-yf/yf-pic-repo/raw/master/Stream%E6%93%8D%E4%BD%9C%E6%AD%A5%E9%AA%A4.jpg)

- 创建Stream

  > 一个数据源（如：集合、数组），获取一个流

- 中间操作

  > 一个中间操作链，对数据源的数据进行处理

- 终止操作（终端操作）

  > 一个终止操作，执行中间链，并产生结果



### Stream的中间操作

多个**中间操作**可以连接起来形成一个**流水线**，除非流水线上触发终止操作，否则**中间操作不会执行任何的处理**，而在**终止操作时一次性全部处理，称为“惰性求值”。**



#### 筛选与切片

| 方法                        | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| filter(Predicate predicate) | 接收Lambda，从流中排除某些元素                               |
| distinct()                  | 筛选，通过流所生成元素的hashCode()和equals()去除重复元素     |
| limit(n)                    | 截断流，使其元素不超过给定数量                               |
| ship(n)                     | 跳过元素，返回一个舍弃前n个元素的流，若元素个数不足n个，则返回一个空流。与limit(n)互补 |



#### 映射

- 接收`Lambda`，将元素转换成其他形式获取信息。

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| map(Function<? super T, ? extends R> mapper)                 | 接受一个函数作为参数，该函数会被应用到每个元素中，并将其映射成一个新的元素。 |
| flatMap(Function<? super T, ? extends Stream<? extends R>> mapper) | 接收一个函数作为参数，将流中的每个值都换成了一个流，然后把所有流连接成一个流 |



#### 排序

- sorted：自然排序（Comparable）
- sorted(Comparable)：定制排序（Comparator）



### 终止操作

#### 匹配

- `allMatch`：检查是否匹配所有元素
- `anyMatch`：检查是否至少匹配一个元素
- `noneMatch`：检查是否没有匹配所有元素

#### 查找

- `findFirst`：返回第一个元素
- ` findAny`：返回当前流中的任意元素
- `count`：返回流中元素的总个数
- `max`：返回流中最大值
- `min`：返回流中最小值

#### 归约
> 将流中元素反复结合起来操作得到一个值
- `reduce(T identity, BinaryOperator<T> accumulator);`
- `reduce(BinaryOperator<T> accumulator);`

#### 收集
> 将流转换成为另一种形式。接收一个Collector接口的实现，用于给Stream中元素做汇总的方法
>
> `collect(Collector<? super T, A, R> collector)`：Collector接口的实现决定了如何对流执行收集操作（如：List、Set、Map）。Collector接口的实现类提供了很多静态方法，可以方便的创建常见的收集器实例 



## 并行流和顺序流

### 并行流

- 并行流就是把一个内容分成多个数据块，并用不同的线程分别处理每个数据块的流。



## Optional类

`Optional<T>`类是个容器类，代表一个值存在或者不存在，可以避免空指针异常

### 常用方法

```java
Optional.of(T t);			// 创建一个Optional实例
Optional.empty();			// 创建一个空的Optional实例
Optional.ofNullable(T t);	// 若t不为null，创建Optional实例，否则创建空实例
isPresent();				// 判断是否包含值
orElse(T t);				// 如果调用对象包含值，返回该值。否则返回t
orElseGet(Supplier s);		// 如果调用对象包含值，返回该值。否则返回s获取的值
map(Function f);			// 如果有值对其处理，并返回处理后的Optional，否则返回Optional.empty()
flatMap(Funtion mapper);	// 与map类似，要求返回值必须是Optional
```



## 接口的默认方法

```java
public interface MyFun {

    // 默认方法
    default String getName() {
        return "MyFun";
    } 

}
```

> **接口默认方法的“类优先”原则**
>
> 若一个接口中定义了一个默认方法，而另一个父类或接口中又定义了一个同名的方法时：
>
> - 选择父类中的方法。如果父类提供了具体的实现，那么接口中具有相同名称和参数的默认方法就会被忽略
> - 接口冲突。如果一个接口提供一个默认方法，而另一个接口也提供了一个具有相同名称和参数列表的方法（不管方法是否是默认方法），那么必须覆盖该方法来解决冲突。



## 新日期和时间API



## 重复注解和类型注解

- 可重复注解
- 用于类型的注解

```java
@Repeatable(MyAnnotations.class)        // 指定容器类
// TYPE_PARAMETER 泛型注解
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, TYPE_PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {

    String value() default "myAnnotation";

}

@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotations {

    MyAnnotation[] value();

}

@MyAnnotation("Hello")
@MyAnnotation("World")
@MyAnnotation("d")
@MyAnnotation("d")
public void show(@MyAnnotation("jho") String str) {

}
```

