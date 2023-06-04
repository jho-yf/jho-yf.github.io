# 设计模式

**创建型模式**：单例模式、工厂模式、抽象工厂模式、建造者模式、原型模式

**结构型模式**：适配器模式、桥接模式、装饰模式、组合模式、外观模式、享元模式、代理模式

**行为型模式**：模板方法模式、命令模式、迭代器模式、观察者模式、中介者模式、备忘录模式、解释器模式、状态模式、策略模式、责任链模式、访问者模式



## 面向对象七大原则

- 单一职责原则

- 接口隔离原则
- 依赖倒置原则
- 里氏替换原则

- 开闭原则

- 迪米特法则
- 合成复用原则

### 单一职责原则

即一个类应该只负责一项职责。控制类的粒度大小，将对象解耦、提高其内聚性

反例

```java
public class SingleResponsibility {

    public static void main(String[] args) {
        Vehicle vehicle = new Vehicle();
        vehicle.run("货车");
        vehicle.run("轮船");
        vehicle.run("飞机");
    }

    /**
     * 交通工具类
     * <p>
     * 违法单一职责原则：大量的if-else
     */
    private static class Vehicle {

        public void run(String vehicle) {
            if ("货车".equals(vehicle)) {
                System.out.println(vehicle + " 在路上运行...");
            } else if ("轮船".equals(vehicle)) {
                System.out.println(vehicle + " 在水中运行...");
            } else {
                System.out.println(vehicle + " 在天空运行...");
            }
        }

    }

}
```

正例

```java
interface Vehicle {
	void run(String vehicle);
}

class RoadVehicle implements Vehicle {

    @Override
    public void run(String vehicle) {
        System.out.println(vehicle + " 在路上运行...");
    }

}

static class WaterVehicle implements Vehicle {

    @Override
    public void run(String vehicle) {
        System.out.println(vehicle + " 在水中运行...");
    }
}

static class AirVehicle implements Vehicle {

    @Override
    public void run(String vehicle) {
        System.out.println(vehicle + " 在天空运行...");
    }
}

class SingleResponsibility {

    public static void main(String[] args) {
        Vehicle roadVehicle = new RoadVehicle();
        Vehicle waterVehicle = new WaterVehicle();
        Vehicle airVehicle = new AirVehicle();
        roadVehicle.run("小车");
        waterVehicle.run("轮船");
        airVehicle.run("飞机");
    }

}
```

### 接口隔离原则

即一个类对另外一个类的依赖应该建立在最小的接口上。要为各个类建立他们需要的专用接口

反例：违法接口隔离原则，每个接口方法都需要实现一遍

```java
interface AnimalAction {

    void run();

    void swim();

    void fly();

}

class Dog implements AnimalAction {

    @Override
    public void run() {
        System.out.println("dog run...");
    }

    @Override
    public void swim() {
        System.out.println("dog run...");
    }

    @Override
    public void fly() {
        throw new IllegalStateException("dog cannot fly");
    }

}

class Fish implements AnimalAction {

    @Override
    public void run() {
        throw new IllegalStateException("fish cannot run");
    }

    @Override
    public void swim() {
        System.out.println("fish swim...");
    }

    @Override
    public void fly() {
        throw new IllegalStateException("fish cannot fly");
    }
}
```

正例：声明最小的接口，遵循接口隔离原则

```java
interface AnimalAction {

}

interface FlyAction extends AnimalAction {

    void fly();

}

interface SwimAction extends AnimalAction {

    void swim();

}

interface runAction extends AnimalAction {

    void run();

}

class Dog implements SwimAction, runAction {

    @Override
    public void run() {
        System.out.println("dog run...");
    }

    @Override
    public void swim() {
        System.out.println("dog run...");
    }

}

class Fish implements SwimAction {

    @Override
    public void swim() {
        System.out.println("fish swim...");
    }

}
```



### 依赖倒置原则

- 高层模块不应该依赖于底层模块，二者都应该依赖于抽象
- 要面向接口编程，不要面向实现（细节）编程
- 使用接口或者抽象的目的是制定好**规范**，而不设计任何具体的操作，把展现细节的任务交给他们的实现类去完成



### 里氏替换原则

- 子类必须完全实现父类的抽象方法，但不能覆盖父类的非抽象方法；
- 子类可以实现自己特有的方法；
- 当子类的方法实现父类的抽象方法时，方法的后置条件（即方法的返回值）要比父类更严格；
- 子类的实例可以替换任何父类的实例，但反之不成立；
- 重写父类方法常常会使整个继承体系的复用性变得比较差，特别是运行多态比较频繁的时候

- 继承会使两个类耦合性增强，在适当的情况下，可以让原有的父类和子类都继承一个跟抽象的基类，去掉原有的继承关系，通过**聚合**、**组合**、**依赖**来解决问题



### 开闭原则

对扩展开发，对修改关闭



### 迪米特法则

只与你的直接朋友交谈，不跟“陌生人”说话



### 合成复用原则

尽量先使用组合或者聚合等关联关系来实现，其次才考虑使用继承关系来实现



## 工厂模式

- 作用：实现了创建者和调用者的分离
- 详细分类：
  - 简单工厂模式
  - 工厂方法模式
  - 抽象工厂模式
- 遵循原则：开闭原则、依赖倒转原则、迪米特法则

### 核心本质

- 实例化对象不使用new，用工厂方法代替
- 将选择实现类，创建对象统一管理和控制。 