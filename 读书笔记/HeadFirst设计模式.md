# Effective Java

## *第二章* 创建和销毁对象

### 1、用静态工厂方法代替构造器

#### 为什么/优点？
 
1. 静态工厂方法的名称比构造函数更容易表示其意义。
     > eg. 当一个类需要多个带有相同签名的构造函数时，就用静态工厂方法代替构造器，并仔细的选择名称一边突出静态工厂方法之间的区别。

2. 不用每次在调用的时候都创建一个新的对象。静态工厂可以为重复的调用返回相同的对象。

3. 静态工厂方法可以返回原类型的子类的对象。

4. 返回的对象的类可以随着静态工厂方法的参数值不同而发生变化。

5. 静态工厂方法在编写时可以返回不存在的类。

#### 缺点

1.  类如果包含公有的或者受保护的构造器，就不能被子类化。
2. 开发者难以发现。在API文档中没有明确的标识出来。


### 遇到多个构造器参数时要考虑使用构建器

#### 什么是构建器？
在一个含有多个参数的类中创建一个内部类，内部类中有着和这个外部类一样的参数列表，客户端通过创建内部类来初始化要创建对象的成员变量，然后调用类接口获取外部类的实例对象。

```java

public class People {

    private String name;
    private String code;

// 构建器
    public static class Builder {
        private String name;
        private String code;

        public Builder(String name, String code) {
            this.name = name;
            this.code = code;
        }

        public People build() {
            return new People(this);
        }
    }

// 私有的构造函数
    private People(Builder builder) {
        this.name = builder.name;
        this.code = builder.code;
    }
}

```

#### 为什么需要用构造器来代替多个参数的构造器

* 如果使用重叠构造器，当用许多的参数的时候，客户端代码会很难编写，且难以阅读。

* 如果使用JavaBeans模式，在构造的过程中JavaBeans可能处于一个不一致的状态。

#### 为什么JavaBeans 容易处于一个不一致的状态？

* JavaBeans 提供Setter方法，通过构造器创建出来的实例可以随时的更改实例中的参数值。