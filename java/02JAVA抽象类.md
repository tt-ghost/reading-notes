# 02JAVA抽象类

## 抽象类

> 用 abstract class 定义抽象类，抽象类不能实例化，只能被继承

```java
public abstract class Employee {
	public Employee(String name, String address, init number) {
		this.name = name;
		this.address = address;
		this.number = number;
	}
}
```

## 抽象方法

> 希望类的一些方法由子类实现；声明时只有方法名，没有方法体；抽象方法必须在抽象类中；构造方法、类方法（用static修饰的方法）不能声明为抽象方法；抽象类的子类必须给出抽象类中抽象方法的具体实现，除非子类也是抽象类

```java
public abstract class Employee {
	public Employee(String name, String address, init number) {
		this.name = name;
		this.address = address;
		this.number = number;
	}
	public abstract double computePay();
}
```