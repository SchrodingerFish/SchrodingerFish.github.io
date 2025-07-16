---
title: Java中常见的设计模式
date: 2025-07-16 10:55:11
categories:
    - Java
tags:
    - Java
    - 设计模式
---
Java中常见的设计模式可以分为三大类：

## 🏗️ 创建型模式（Creational Patterns）

### 1. 单例模式（Singleton）
**作用**：确保一个类只有一个实例
**例子**：公司的CEO只有一个人
```java
public class CEO {
    private static CEO instance;
    private CEO() {} // 私有构造函数
    
    public static CEO getInstance() {
        if (instance == null) {
            instance = new CEO();
        }
        return instance;
    }
}
```

### 2. 工厂模式（Factory）
**作用**：创建对象时不暴露创建逻辑
**例子**：汽车工厂根据订单生产不同品牌的车
```java
public class CarFactory {
    public static Car createCar(String type) {
        switch (type) {
            case "BMW": return new BMW();
            case "Toyota": return new Toyota();
            default: return null;
        }
    }
}
```

### 3. 建造者模式（Builder）
**作用**：分步骤创建复杂对象
**例子**：建造房子需要一步步完成
```java
public class House {
    private String foundation;
    private String walls;
    private String roof;
    
    public static class Builder {
        private House house = new House();
        
        public Builder buildFoundation() {
            house.foundation = "混凝土地基";
            return this;
        }
        
        public Builder buildWalls() {
            house.walls = "砖墙";
            return this;
        }
        
        public Builder buildRoof() {
            house.roof = "瓦片屋顶";
            return this;
        }
        
        public House build() {
            return house;
        }
    }
}
```

## 🔧 结构型模式（Structural Patterns）

### 4. 适配器模式（Adapter）
**作用**：让不兼容的接口能够协同工作
**例子**：电源适配器让不同规格的插头都能用
```java
// 旧接口
interface OldPrinter {
    void oldPrint();
}

// 新接口
interface NewPrinter {
    void newPrint();
}

// 适配器
class PrinterAdapter implements NewPrinter {
    private OldPrinter oldPrinter;
    
    public PrinterAdapter(OldPrinter oldPrinter) {
        this.oldPrinter = oldPrinter;
    }
    
    @Override
    public void newPrint() {
        oldPrinter.oldPrint(); // 调用旧方法
    }
}
```

### 5. 装饰器模式（Decorator）
**作用**：动态地给对象添加功能
**例子**：给咖啡加糖、加奶
```java
interface Coffee {
    String getDescription();
    double getCost();
}

class SimpleCoffee implements Coffee {
    public String getDescription() { return "简单咖啡"; }
    public double getCost() { return 5.0; }
}

class MilkDecorator implements Coffee {
    private Coffee coffee;
    
    public MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
    
    public String getDescription() {
        return coffee.getDescription() + " + 牛奶";
    }
    
    public double getCost() {
        return coffee.getCost() + 2.0;
    }
}
```

### 6. 代理模式（Proxy）
**作用**：为其他对象提供一种代理以控制对这个对象的访问
**例子**：房产中介代理房东卖房
```java
interface House {
    void sell();
}

class RealHouse implements House {
    public void sell() {
        System.out.println("房子已出售");
    }
}

class HouseProxy implements House {
    private RealHouse realHouse;
    
    public void sell() {
        if (realHouse == null) {
            realHouse = new RealHouse();
        }
        System.out.println("中介收取服务费");
        realHouse.sell();
    }
}
```

## 🎭 行为型模式（Behavioral Patterns）

### 7. 观察者模式（Observer）
**作用**：当对象状态改变时，自动通知所有依赖者
**例子**：微信公众号发文章，所有订阅者都会收到通知
```java
interface Observer {
    void update(String message);
}

class WeChat {
    private List<Observer> followers = new ArrayList<>();
    
    public void addFollower(Observer observer) {
        followers.add(observer);
    }
    
    public void publishArticle(String article) {
        for (Observer follower : followers) {
            follower.update(article);
        }
    }
}

class User implements Observer {
    private String name;
    
    public User(String name) { this.name = name; }
    
    public void update(String message) {
        System.out.println(name + "收到消息: " + message);
    }
}
```

### 8. 策略模式（Strategy）
**作用**：定义一系列算法，让它们可以互相替换
**例子**：出行方式有多种选择（开车、坐地铁、骑车）
```java
interface TravelStrategy {
    void travel();
}

class CarStrategy implements TravelStrategy {
    public void travel() { System.out.println("开车出行"); }
}

class SubwayStrategy implements TravelStrategy {
    public void travel() { System.out.println("坐地铁出行"); }
}

class TravelContext {
    private TravelStrategy strategy;
    
    public void setStrategy(TravelStrategy strategy) {
        this.strategy = strategy;
    }
    
    public void executeTravel() {
        strategy.travel();
    }
}
```

### 9. 命令模式（Command）
**作用**：将请求封装成对象，可以排队、记录、撤销
**例子**：遥控器控制电视
```java
interface Command {
    void execute();
}

class TV {
    public void turnOn() { System.out.println("电视开机"); }
    public void turnOff() { System.out.println("电视关机"); }
}

class TurnOnCommand implements Command {
    private TV tv;
    
    public TurnOnCommand(TV tv) { this.tv = tv; }
    
    public void execute() { tv.turnOn(); }
}

class RemoteControl {
    private Command command;
    
    public void setCommand(Command command) {
        this.command = command;
    }
    
    public void pressButton() {
        command.execute();
    }
}
```

### 10. 状态模式（State）
**作用**：让对象在内部状态改变时改变其行为
**例子**：电梯在不同楼层有不同的行为
```java
interface ElevatorState {
    void open();
    void close();
}

class GroundFloor implements ElevatorState {
    public void open() { System.out.println("一楼开门"); }
    public void close() { System.out.println("一楼关门"); }
}

class SecondFloor implements ElevatorState {
    public void open() { System.out.println("二楼开门"); }
    public void close() { System.out.println("二楼关门"); }
}

class Elevator {
    private ElevatorState currentState;
    
    public void setState(ElevatorState state) {
        this.currentState = state;
    }
    
    public void open() { currentState.open(); }
    public void close() { currentState.close(); }
}
```

## 📝 总结

这些设计模式的核心思想都是**面向对象的基本原则**：
- **单一职责**：每个类只负责一件事
- **开闭原则**：对扩展开放，对修改关闭
- **里氏替换**：子类可以替换父类
- **依赖倒置**：依赖抽象而不是具体实现

在实际开发中，不需要强行使用设计模式，而是在遇到相应问题时自然地选择合适的模式来解决。