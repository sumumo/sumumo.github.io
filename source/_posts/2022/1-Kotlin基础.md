---
title: Kotlin基础
date: 2022-05-25 18:01:30
tags: Kotlin
pid: 1
---
# 变量和函数

## 变量

val ( value 的简写 ) 用来声明一个不可变的变量

var ( variable 的简写 ) 用来声明一个可变的变量

显式的声明变量类型：

```kotlin
val a: Int 10
```

<!-- more -->

## 函数

语法规则：

```kotlin
fun methodName(param1: Int, param2: Int): Int {
    return 0
}
```

一个函数中只有一行代码时可以将代码写在函数定义的尾部，中间用等号连接：

```kotlin
fun largerNumber(num1: Int, num2: Int): Int {
    return max(num1, num2)
}
```

可以简化成如下形式：

```kotlin
fun largerNumber(num1: Int, num2: Int) = max(num1, num2)
```

# 逻辑控制

## if 条件语句

与 Java 不同的是，Kotlin 中的 if 语句是可以使用每个条件的最后一行代码作为返回值的

```kotlin
fun largerNumber(num1: Int, num2: Int): Int {
    var value = 0
    if (num1 > num2) {
        value = num1
    } else {
        value = num2
    }
    return value
}
```

以上代码可以精简为：

```kotlin
fun largerNumber(num1: Int, num2: Int) = if (num1 > num2) num1 else num2
```

## when 条件语句

when 语句同样是可以有返回值的：

```kotlin
fun getScore1(name: String) = when (name) {
    "Tom" -> 86
    "Jim" -> 77
    "Jack" -> 95
    "Lily" -> 100
    else -> 0
}
```

when 语句还允许类型匹配：

```kotlin
fun checkNum(num: Number) {
    when (num) {
        is Int -> println("number is Int")
        is Double -> println("number is Double")
        else -> println("number not support")
    }
}
```

不带参数的用法：

```kotlin
fun getScore(name: String) = when {
    name.startsWith("Tom") -> 86
    name == "Jim" -> 77
    name == "Jack" -> 95
    name == "Lily" -> 100
    else -> 0
}
```

## 循环语句

### while 循环

Kotlin 中的 while 循环与 Java 中的没有任何区别

### for 循环

`..` 用于创建两端闭区间，`0..10` 表示创建了一个0到10的两端闭区间，包含0和10，用数学表达式表示就是 [0,10]

`until` 创建一个左闭右开的升序区间，`0 until 10` 的数学表达方式是 [0,10) ，可以使用 `step` 关键字跳过元素

```kotlin
for (i in 0 until 10 step 2) {
    println(i)
}
```

执行结果为

```
0
2
4
6
8
```

`downTo` 创建一个降序两端闭区间，`10 downTo 0` 的数学表达方式是 [10,0] ， 同样可以使用 `step` 关键字跳过元素

# 面向对象编程

## 类与对象

创建一个类：

```kotlin
class Person {
}
```

对类进行实例化：

```kotlin
val p = Person()
```

## 继承与构造函数

### 继承

Kotlin默认所有非抽象类不可以被继承，使用`open`关键字让一个类可以被继承：

```kotlin
open class Person {
  ...
}
```

使用冒号去继承一个类：

```kotlin
class Student : Person() {
  ...
}
```

### 构造函数

Kotlin中的构造函数分为两种：主构造函数和次构造函数

#### 主构造函数

每个类默认都有一个不带参数的主构造函数，可以显式的指明参数。 主构造函数没有函数体，直接定义再类名后面，使用`init`结构体编写主构造函数中的逻辑：

```kotlin
class Student(val sno: String, val grade: Int) : Person() {
  init {
    println("sno is " + sno)
    println("grade is " + grade)
  }
}
```

在主构造函数中声明成`val`或`var`的参数将自动成为该类的字段，不加则作用域仅限定在主构造函数中

```kotlin
open class Person(val name:String,val age:Int) {
  ...
}

class Student(val sno: String, val grade: String, name: String, age: Int) : Person(name, age) {
  ...
}
```

#### 次构造函数

任何一个类都只能有一个主构造函数，但是可以有多个次构造函数，次构造函数也可以用于实例化一个类。

Kotlin规定，当一个类既有主构造函数又有次构造函数时，所有的次构造函数都必须调用主构造函数（包括间接调用）。

```kotlin
class Student(val sno: String, val grade: Int, name: String, age: Int) : Person(name, age) {
    constructor(name: String, age: Int) : this("", 0, name, age){
    }

    constructor() : this("", 0){
    }
}
```

次构造函数通过`constructor`关键字定义，上面的代码定义了两个次构造函数：第一个次构造函数接收name和age两个参数，又通过`this`关键字调用了主构造函数，并将sno和grade两个参数赋值成初始值；第二个次构造函数不接受任何参数，通过`this`关键字调用第一个次构造函数，并将name和age也赋值成初始值，这样就是间接调用了主构造函数。

现在可以通过不带参数的构造函数、带两个参数的构造函数和带4个参数的构造函数3种方式对Student类进行实例化：

```kotlin
val student1 = Student()
val student2 = Student("Jack", 19)
val student3 = Student("a123", 5, "Jack", 19)
```

Kotlin允许类中只有次构造函数没有主构造函数。当一个类中没有显式地定义主构造函数且定义了次构造函数时，他就是没有主构造函数的。

```kotlin
class Student : Person {
    constructor(name: String, age: Int) : super(name, age) {
    }
}
```

既然没有主构造函数，Student类继承Person类的时候也就不需要加上括号了。由于没有主构造函数，次构造函数只能通过`super`关键字直接调用父类的构造函数。


# 参考

- [第一行代码 第3版](https://union-click.jd.com/jdc?e=618%7Cpc%7C&p=JF8BANIJK1olXDYCV1dfC0sVAl9MRANLAjZbERscSkAJHTdNTwcKBlMdBgABFksUCm0LG1kUQl9HCANtFj8WAy1zXCB3KWRxIB4FDSIUUCR3e1cZbQcyV19dCEseC2kOGmslXQEyAjBdCUoWAm4NH1wSbQcyVFlZCk8SC2YAH10QXjYFVFdtUx55Bm1cH18RCAVWXA1bC3snM2w4HFscSQBwFQxJDjknM284GGtXMwIEUlkID0pFUDxfTF5GWQUKAVYIW0ITB2YIS1oVCFEEZFxcCU8eM18)
