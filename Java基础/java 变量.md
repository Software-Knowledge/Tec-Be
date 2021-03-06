Java 变量
---
<!-- TOC -->

- [1. 变量的概念](#1-变量的概念)
- [2. 变量的属性](#2-变量的属性)
  - [2.1. 变量的标识符](#21-变量的标识符)
  - [2.2. 常量](#22-常量)
  - [2.3. 地址](#23-地址)
    - [2.3.1. 很特殊的字符串](#231-很特殊的字符串)
  - [2.4. 生存期](#24-生存期)
    - [2.4.1. 栈](#241-栈)
    - [2.4.2. 堆](#242-堆)
  - [2.5. 作用域](#25-作用域)
- [3. 变量的行为](#3-变量的行为)
  - [3.1. 构成声明语句](#31-构成声明语句)
  - [3.2. 和操作符结合成为表达式](#32-和操作符结合成为表达式)
  - [3.3. 初始化(对象)](#33-初始化对象)
- [4. 常用变量的用法](#4-常用变量的用法)
  - [4.1. 变量类型](#41-变量类型)
  - [4.2. 访问](#42-访问)

<!-- /TOC -->

# 1. 变量的概念
1. 不同角度看变量：
    1. 物理角度：计算机的数据存储单元
    2. 逻辑角度：计算机模型中的抽象数据单位
    3. 语义角度：类的属性、方法的状态
2. 赋值：改变现实对象的状态
    1. 通过赋值来改变模拟变量状态的**程序设计语言常规的符号名字**
    2. 赋值打破了引用透明性
    3. 与所有状态必须显示地操作的传递额外的参数的方式相比，通过引进赋值和将状态隐藏在局部变量的技术。我们能以一种更模块化的方式构造系统

# 2. 变量的属性
1. types:强类型语言
    1. 基础数据类型
        1. 物理视角
        2. 使用预定义好的、基础的数据类型
        3. 其中主类型和别的类型是不同的，比如int&&Integer,double&&Double
    2. 引用类型
        1. 来源与语义视角
        2. 对象的遥控器
2. values：
    1. 值
        1. 基础数值类型：浮点数类型有精度损失
3. variables

## 2.1. 变量的标识符
1. 必须由字母、下划线、或者美元符开头
2. 区分大小写
3. 不允许包含有空格和制表符
4. 符合命名规则的好处：
    1. 有助于在项目间传递知识
    2. 在新的项目中更加快速的学习代码
    3. 减少名字的增生
    4. 弥补编程语言的不足
5. java命名规则：
    1. 除了变量名以外，类、类常量，均使用大小写混合的方法
    2. 第一个单词的首字母小写，其后单词的首字母大写
    3. 变量名不应该以下划线或美元符号开头
    
## 2.2. 常量
命名：应该全部大写，单词间用下划线分隔

## 2.3. 地址
1. 变量的地址是与这个变量相关联的地址
2. 多个变量可以具有同一地址当用多个变量名来访问单个存储地址时，这些变量名就成为了别名

### 2.3.1. 很特殊的字符串
1. ==比较的是地址，`.equals()`比较的是内部的值
2. new String(str):可以强制分配对象
3. 相同字符串一般情况下只保存一个
```java
String a = new String();
a = "abc";
String b = new String();
b = "abc";
System.out.print(a==b);//true,分配两个对象，指向一个对象
String a = "abc";
String b = "abc";
System.out.print(a==b);//true，直接指向同一个
String a = new String("abc");
String b = new String("abc");
System.out.print(a==b);//false，强制分配两个
```

## 2.4. 生存期
1. 生存期是该变量绑定于某一特定存储地址的时间
2. 变量的生存期：
    1. 静态变量
    2. 栈动态变量
    3. 显性堆动态变量
    4. 隐性堆动态变量

### 2.4.1. 栈
1. 速度较快
2. 要求在编译的时候知道变量占用内存空间的大小和时间——局部变量

### 2.4.2. 堆
1. 效率较低
2. 不需要让编译器知道所占有内存空间的大小和时间
3. 在离开其作用域后虽然无法访问，但是依旧占据着内存空间

## 2.5. 作用域
1. 语句的一个范围，在这个范围之内，变量为可见的。如果一个变量在一条语句中可以被引用，这个变量即在这条语句中为可见的
2. 一般和代码块相关
3. 成员变量在类中声明，整个类中可见，涉及到访问权限的问题
4. 如果是使用成员变量，使用this关键字

# 3. 变量的行为

## 3.1. 构成声明语句
## 3.2. 和操作符结合成为表达式
## 3.3. 初始化(对象)
1. 对象的初识化的顺序：
    1. 静态成员变量
    2. 静态初始化块
    3. 非静态成员变量
    4. 构造器中初始化
    5. 所有的成员变量都会在构造器执行之前被初始化为指定的值或默认值，在构造器中如果有对其赋值的语句将会再次对其进行初始化


# 4. 常用变量的用法
## 4.1. 变量类型
1. 局部变量(在方法内部)
    + 用于在方法内部临时存放数据
    + 不管是数据类型还是引用类型，如果在没有显示初始化的情况下使用，都会出现错误
2. 属性字段(在对象内部)
    + 用于存放表示类的属性的值
3. 静态变量(全局)
## 4.2. 访问
1. 直接访问(内部使用者)
2. 间接访问(外部使用者)