---
layout: post
title: Scala
category: Language
comments: true
---

# Scala

------

 - 表达能力：函数是一等公民、闭包
 - 简洁：类型推断、函数创建的文法支持
 - Java互操作性：可重用Java库、可重用Java工具、没有性能惩罚

#### 启动解释器

```
sbt console
```

## Basics

#### 表达式、值、函数（匿名函数）

```
scala> 1 + 1
res0: Int = 2

scala> val two = 1 + 1
two: Int = 2

scala> var name = "steve"
name: java.lang.String = steve

scala> name = "marius"
name: java.lang.String = marius

scala> def addOne(m: Int): Int = m + 1
addOne: (m: Int)Int

scala> val three = addOne(2)
three: Int = 3

scala> def three() = 1 + 2
three: ()Int

scala> three()
res2: Int = 3

scala> three
res3: Int = 3

scala> (x: Int) => x + 1
res2: (Int) => Int = <\function1>

scala> res2(1)
res3: Int = 2

scala> val addOne = (x: Int) => x + 1
addOne: (Int) => Int = <\function1>

scala> addOne(1)
res4: Int = 2

def timesTwo(i: Int): Int = {
  println("hello world")
  i * 2
}

scala> { i: Int =>
  println("hello world")
  i * 2
}
res0: (Int) => Int = <\function1>
```

#### 部分应用（Partial application）

```
scala> def adder(m: Int, n: Int) = m + n
adder: (m: Int,n: Int)Int

scala> val add2 = adder(2, _:Int)
add2: (Int) => Int = <\function1>

scala> add2(3)
res50: Int = 5
```

#### 柯里化函数

```
scala> def multiply(m: Int)(n: Int): Int = m * n
multiply: (m: Int)(n: Int)Int

scala> multiply(2)(3)
res0: Int = 6

scala> val timesTwo = multiply(2) _
timesTwo: (Int) => Int = <\function1>

scala> timesTwo(3)
res1: Int = 6

scala> (adder _).curried
res1: (Int) => (Int) => Int = <\function1>
```

#### 可变长度参数

```
def capitalizeAll(args: String*) = {
  args.map { arg =>
    arg.capitalize
  }
}
```

#### 类

```
scala> class Calculator {
     |   val brand: String = "HP"
     |   def add(m: Int, n: Int): Int = m + n
     | }
defined class Calculator

scala> val calc = new Calculator
calc: Calculator = Calculator@e75a11

scala> calc.add(1, 2)
res1: Int = 3

scala> calc.brand
res2: String = "HP"
```

#### 构造函数

```
class Calculator(brand: String) {
  /**
   * A constructor.
   */
  val color: String = if (brand == "TI") {
    "blue"
  } else if (brand == "HP") {
    "black"
  } else {
    "white"
  }

  // An instance method.
  def add(m: Int, n: Int): Int = m + n
}

scala> val calc = new Calculator("HP")
calc: Calculator = Calculator@1e64cc4d

scala> calc.color
res0: String = black
```

#### 函数 vs 方法

```
scala> class C {
     |   var acc = 0
     |   def minc = { acc += 1 }
     |   val finc = { () => acc += 1 }
     | }
defined class C

scala> val c = new C
c: C = C@1af1bd6

scala> c.minc // calls c.minc()

scala> c.finc // returns the function as a value:
res2: () => Unit = <\function0>
```

<http://stackoverflow.com/questions/2529184/difference-between-method-and-function-in-scala>

#### 继承

```
class ScientificCalculator(brand: String) extends Calculator(brand) {
  def log(m: Double, base: Double) = math.log(m) / math.log(base)
}

class EvenMoreScientificCalculator(brand: String) extends ScientificCalculator(brand) {
  def log(m: Int): Double = log(m, math.exp(1))
}

scala> abstract class Shape {
     |   def getArea():Int    // subclass should define this
     | }
defined class Shape

scala> class Circle(r: Int) extends Shape {
     |   def getArea():Int = { r * r * 3 }
     | }
defined class Circle

scala> val s = new Shape
<\console>:8: error: class Shape is abstract; cannot be instantiated
       val s = new Shape
               ^

scala> val c = new Circle(2)
c: Circle = Circle@65c0035b
```

#### 特质（Traits）

```
trait Car {
  val brand: String
}

trait Shiny {
  val shineRefraction: Int
}

class BMW extends Car {
  val brand = "BMW"
}

class BMW extends Car with Shiny {
  val brand = "BMW"
  val shineRefraction = 12
}
```

 - 优先使用特质。一个类扩展多个特质是很方便的，但却只能扩展一个抽象类。
 - 如果你需要构造函数参数，使用抽象类。因为抽象类可以定义带参数的构造函数，而特质不行。例如，你不能说trait t(i: Int) {}，参数i是非法的。

<http://stackoverflow.com/questions/1991042/what-is-the-advantage-of-using-abstract-classes-instead-of-traits>

#### 类型

```
trait Cache[K, V] {
  def get(key: K): V
  def put(key: K, value: V)
  def delete(key: K)
}

def remove[K](key: K)
```

## Basics continued

***相关连接***

 - https://twitter.github.io/scala_school/zh_cn/basics.html

