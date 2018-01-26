# java8
@(技能学习)[java8, 函数]

## 函数式接口


**静态方法**
* Java 8以前的规范中接口中不允许定义静态方法。 静态方法只能在类中定义。 Java 8中可以定义静态方法。

**默认方法**
* Java 8中允许接口实现方法， 而不是简单的声明， 这些方法叫做默认方法，使用特殊的关键字default。

* 因为默认方法不是抽象方法，所以不影响我们判断一个接口是否是函数式接口。


## Lambda 表达式


**Lambda 表达式由三个部分组成：**
* 第一部分为一个括号内用逗号分隔的形式参数，参数是函数式接口里面方法的参数；

* 第二部分为一个箭头符号：->；

* 第三部分为方法体，可以是表达式和代码块。

语法如下：

1. 方法体为表达式，该表达式的值作为返回值返回。
```
(parameters) -> expression
```
2. 方法体为代码块，必须用 {} 来包裹起来，且需要一个 return 返回值，但若函数式接口里面方法返回值是 void，则无需返回值。
```
(parameters) -> { statements; }
```
**Lambda 常用函数式接口：**

要使用 Lambda 表达式，需要定义一个函数式接口，这样往往会让程序充斥着过量的仅为 Lambda 表达式服务的函数式接口。
为了减少这样过量的函数式接口，Java 8 在 java.util.function 中增加了不少新的函数式通用接口。例如：
* Function<T, R>：
> 将 T 作为输入，返回 R 作为输出，他还包含了和其他函数组合的默认方法。
* Predicate<T> ：
> 将 T 作为输入，返回一个布尔值作为输出，该接口包含多种默认方法来将 Predicate 组合成其他复杂的逻辑（与、或、非）。
* Consumer<T> ：
> 将 T 作为输入，不返回任何内容，表示在单个参数上的操作。


## 集合之流式操作

Java 8 引入了流式操作（Stream），

* 通过该操作可以实现对集合（Collection）的并行处理和函数式操作。

* 流式操作实现了集合的过滤、排序、映射等功能 

  Stream 和 Collection 集合的区别：Collection 是一种静态的内存数据结构，而 Stream 是有关计算的。
  
  前者是主要面向内存，存储在内存中，后者主要是面向 CPU，通过 CPU 实现计算。
* 根据操作返回的结果不同，流式操作分为**中间操作**和**最终操作**两种。

  最终操作返回一特定类型的结果，而中间操作返回流本身，这样就可以将多个操作依次串联起来。
  
* 根据流的并发性，流又可以分为**串行** 和 **并行** 两种。  

### 1.串行和并行的流

* 流有串行和并行两种，串行流上的操作是在一个线程中依次完成，而并行流则是在多个线程上同时执行。
* 并行与串行的流可以相互切换：通过 stream.sequential() 返回串行的流，通过 stream.parallel() 返回并行的流。
* 相比较串行的流，并行的流可以很大程度上提高程序的执行效率。
  该操作会保持 stream 处于中间状态，允许做进一步的操作。它返回的还是的 Stream，允许更多的链式操作。

### 2.中间操作和最终操作

**2.1常见的中间操作有：**

* filter()：对元素进行过滤；

* sorted()：对元素排序；

* map()：元素的映射；

* distinct()：去除重复元素；

* subStream()：获取子 Stream 等。

例如，下面是对一个字符串集合进行过滤，返回以“s”开头的字符串集合，并将该集合依次打印出来：
```
list.stream()
.filter((s) -> s.startsWith("s"))
.forEach(System.out::println);
```
这里的 filter(...) 就是一个中间操作，该中间操作可以链式地应用其他 Stream 操作。

**2.2 终止操作**

> 该操作必须是流的最后一个操作，一旦被调用，Stream 就到了一个终止状态，而且不能再使用了。

常见的终止操作有：

* forEach()：对每个元素做处理；

* toArray()：把元素导出到数组；

* findFirst()：返回第一个匹配的元素；

* anyMatch()：是否有匹配的元素等。

例如，下面是对一个字符串集合进行过滤，返回以“s”开头的字符串集合，并将该集合依次打印出来：
```
list.stream() //获取列表的 stream 操作对象
.filter((s) -> s.startsWith("s"))//对这个流做过滤操作
.forEach(System.out::println);
```
这里的 forEach(...) 就是一个终止操作，该操作之后不能再链式的添加其他操作了。

## 注解的更新
对于注解，Java 8 主要有两点改进：类型注解和重复注解。

### 类型注解

新增的两个注释的程序元素类型 ElementType.TYPE_USE 和 ElementType.TYPE_PARAMETER 用来描述注解的新场合。

ElementType.TYPE_PARAMETER 表示该注解能写在类型变量的声明语句中。

而 ElementType.TYPE_USE 表示该注解能写在使用类型的任何语句中（例如声明语句、泛型和强制转换语句中的类型）。

### 重复注解
另外，在该版本之前使用注解的一个限制是相同的注解在同一位置只能声明一次，不能声明多次。

Java 8 引入了重复注解机制，这样相同的注解可以在同一地方声明多次。重复注解机制本身必须用 @Repeatable 注解。

例如，下面就是用 @Repeatable 重复注解的例子：

```
@Retention(RetentionPolicy.RUNTIME) \\该注解存在于类文件中并在运行时可以通过反射获取
@interface Annots {
  Annot[] value();
} 
@Retention(RetentionPolicy.RUNTIME) \\该注解存在于类文件中并在运行时可以通过反射获取
@Repeatable(Annots.class)
@interface Annot {
  String value();
}
@Annot("a1")@Annot("a2")
public class Test {
  public static void main(String[] args) {
    Annots annots1 = Test.class.getAnnotation(Annots.class);
    System.out.println(annots1.value()[0]+","+annots1.value()[1]); 
    // 输出: @Annot(value=a1),@Annot(value=a2)
    Annot[] annots2 = Test.class.getAnnotationsByType(Annot.class);
    System.out.println(annots2[0]+","+annots2[1]); 
    // 输出: @Annot(value=a1),@Annot(value=a2)
  }
}
```
注释 Annot 被 @Repeatable( Annots.class ) 注解。

Annots 只是一个容器，它包含 Annot 数组, 编译器尽力向程序员隐藏它的存在。

通过这样的方式，Test 类可以被 Annot 注解两次。重复注释的类型可以通过 getAnnotationsByType() 方法来返回。

## 扩展

**这里有一些lambdas的例子：**

 一个函数式接口非常有价值的属性就是他们能够用lambdas来实例化。

```
左边是指定类型的逗号分割的输入列表，右边是带有return的代码块：
(int x, int y) -> { return x + y; }

左边是推导类型的逗号分割的输入列表，右边是返回值：
(x, y) -> x + y

左边是推导类型的单一参数，右边是一个返回值：
x -> x * x

左边没有输入 (官方名称: "burger arrow")，在右边返回一个值：
() -> x

左边是推导类型的单一参数，右边是没返回值的代码块（返回void）：
x -> { System.out.println(x); }

静态方法引用：
String::valueOf

非静态方法引用：
Object::toString

继承的函数引用：
x::toString

构造函数引用：
ArrayList::new
```


## 参考文档

**java8简介及使用**

[Java 8 新特性概述](https://www.ibm.com/developerworks/cn/java/j-lo-jdk8newfeature)

[Java 8 的新特性和改进总览](http://www.oschina.net/translate/everything-about-java-8)

[Java8 lambda表达式10个示例](http://www.importnew.com/16436.html)

[Java 8十个lambda表达式案例](http://www.jdon.com/idea/java/10-example-of-lambda-expressions-in-java8.html)

[《JAVA8开发指南》使用流式操作](http://ifeve.com/java8-adopting-streams/)

[Java8初体验（一）lambda表达式语法](http://ifeve.com/lambda/)

[Java8初体验（二）Stream语法详解](http://ifeve.com/stream/)  good

[Java8简明指南](http://ifeve.com/java8/) 常用功能说明

**注解新特性以及使用**

[Java 注解指导手册 – 终极向导](http://ifeve.com/java-annotations-tutorial/)

**扩展**

[Java 8学习资料汇总](http://ifeve.com/java8-learning-resources/)
