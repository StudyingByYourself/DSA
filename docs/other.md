# java相关

## 拆箱与装箱

**装箱**：把一个基本数据类型值,转换为对应包装类的对象.

**拆箱**：把一个包装类对象,转换为对应的基本类型的变量(Java5开始,提供了自动装拆箱).

## 打印数组

一维数组：Arrays.toString(arr)

二维数组：Arrays.deepToString(arr)

## 设计模式(可维护、可扩展、可复用、灵活性好)

### 1.设计模式六大原则

1. 单一职责
2. 依赖倒转
3. 开放封闭：对扩展开放，对修改关闭
4. 里氏代换：可以用基类的地方，也可以用其子类代替
5. 接口隔离
6. 迪米特：两个类之间不直接联系，通过第三方联系

### 2.常见设计模式

1. 简单工厂模式(Simple Factory Pattern)

2. 策略模式(Strategy Pattern)

3. 装饰模式(Decorator Pattern)

4. 工厂方法(Factory Method Pattern)

5. 代理模式(Proxy Pattern)

6. 单例设计模式(Singleton Pattern)

!!! note "饿汉式"
	私有化构造器;私有化类变量;提供向外暴露该类变量的静态方法
	```java
	public class Hero{
		private Hero(){};
		private static Hero instance = new Hero();
		public static Hero getInstance(){
			return instance;
		}
	}
	```






