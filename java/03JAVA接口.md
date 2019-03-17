# 03JAVA接口

- 接口不是类
- 接口包含类要实现的方法
- 除非实现接口的类是抽象类，否则必须实现接口中的所有方法
- 接口无法被实例化
- 接口没有构造方法
- 接口中所有方法必须为抽象方法
- 接口不能包含成员变量，除 `static 和final` 变量
- 接口支持多继承

## 特性

- 接口中每个方法是隐式抽象的，只能是 `public abstract`
- 接口中可以包含变量，隐式 `public static final`
- 一个类只能继承一个抽象类，而一个类可以实现多个接口
- 接口中不能包含静态代码块以及静态方法（用static修饰的方法），抽象类可以

## 声明

```	java
public interface NameOfInterface{
	public void eat();
}
```

- 接口是隐式的，声明时可以不用加`abstract`，方法也不用

```java
interface Animal {
	public void eat();
	public void travel();
}
```

## 实现

```
...implements 接口名称[, 接口名称, ...] ...
```

## 接口继承

接口继承使用`extends`关键字

```java
// 文件名: Sports.java
public interface Sports
{
   public void setHomeTeam(String name);
   public void setVisitingTeam(String name);
}
 
// 文件名: Football.java
public interface Football extends Sports
{
   public void homeTeamScored(int points);
   public void visitingTeamScored(int points);
   public void endOfQuarter(int quarter);
}
 
// 文件名: Hockey.java
public interface Hockey extends Sports
{
   public void homeGoalScored();
   public void visitingGoalScored();
   public void endOfPeriod(int period);
   public void overtimePeriod(int ot);
}
```

- 接口可以多继承，类不可以

```java
public interface Hockey extends Sports, Event
```

## 标记接口

没有任何方法和属性的接口成为标记接口，目的是：简历一个公共的父接口；向一个类添加数据类型；

```java
package java.util;
public interface EventListener {}
```
