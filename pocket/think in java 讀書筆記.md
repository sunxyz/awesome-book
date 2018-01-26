### think in java 讀書筆記



在static方法的内部不能调用非静态方法，反过来倒是可以


>对于静态变量、静态初始化块、变量、初始化块、构造器，它们的初始化顺序依次是（静态变量、静态初始化块）>（变量、初始化块、构造器。）

继承与初始化分为三步 （p273）：
>
> 1 找到所有的基类从根基类开始加载 

> 2.根基类的static初始化然后是下一个导出类 以此类推 

> 3.所有的类都已经加载完 对象可以被创建了，首先对象中所有基本类型都会被设为默认值，对象引用都被设为null，赋初值，构造器从根往下执行赋值 


**main方法可以用来传参**

**用 import static ...导入静态工具库**

---
**修饰符:**

每个编译单元（文件）都只能有一个public类

**类访问权限：**

每个编译单元（文件）都只能包括一个public修饰的类

---
**组合和继承的选择:**

 is-a（是一个）用继承 has-is（有一个）用组合 （慎用继承-一个较好的判断方式：是否需要从新类向基类进行向上转型）

---
**final 关键字：**

1数据 ：
常量-必须是基本数据类型以final表示 ，对象 final使引用恒定不变 对象自身可以被修改 （数组也是如此） 
空白final(未赋值的数据) 必须在域的定义处或每个构造器中用表达式对final进行赋值

2方法
1）.在继承中防止方法被子类覆盖（类中所有的private 方法都隐式的制定为是final的）
2）.在早期版本中可以提高效率

3类
不能被继承，不需要做任何变动，或出于安全考虑，不希望它有子类

----
**多态**

1.继承转型的指向问题 域是单独存在的 方法才能被覆盖

```
	class Super{
		int field  = 0;
		public int getField(){
			return field;
		}
	}

	class Sub extends Super{
		int field = 1;
		public int getField(){
			return field;
		}
		public int getSuperField(){
			return super.field;
		}
	}
	
	@Test
	public void test1() {
		Super sup = new Sub();
		println("sup.field = " + sup.field + ", sup.getField() = " + sup.getField());
		Sub sub = new Sub();
		print("sub.field = " + sub.field + ", sub.getField() = " + sub.getField() + ", sub.getSuperField() = "+ sub.getSuperField());
	}
```
控制台打印：
sup.field = 0, sup.getField() = 1
sub.field = 1, sub.getField() = 1, sub.getSuperField() = 0

2.在继承中静态方法是与类，而非与单个对象相关联

---
**内部类：**
外部类.this 获取外包类对象  外部类对象.new 内部类（）创建内部类

**在方法和作用域内的内部类**
**匿名内部类**
没有名字的内部类
使用外部定义的参数时需要将参数引用改为final

**内部类的好处： 可以解决“多重继承” 的问题**

**嵌套类：**
静态的内部类 
1不需要外围对象 2  不能访问非静态的外围类对象

---

泛型的作用
在编译期防止将错误的类型放置到容器中

----
**异常的处理：**

 **Throwable：** 

 有两个重要的子类：**Exception（异常）和 Error（错误）**，二者都是 Java 异常处理的重要子类，各自都包含大量子类。
注意：异常和错误的区别：异常能被程序本身可以处理，错误是无法处理。

**Error:**  

是程序无法处理的错误，表示运行应用程序中较严重问题,用来表示编译时或系统错误

**Exception:**  

是程序本身可以处理的异常。
非运行时异常 （编译异常）：是RuntimeException以外的异常，类型上都属于Exception类及其子类。从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过
 
**RuntimeException:** 

会自动被java虚拟机抛出
运行时异常：都是RuntimeException类及其子类异常
运行时异常的特点是Java编译器不会检查它，也就是说，当程序中可能出现这类异常，即使没有用try-catch语句捕获它，也没有用throws子句声明抛出它，也会编译通过。

**异常处理的建议：**
迅速的定位错误发生的地点和原因，改正错误，恢复代码 合理巧妙的使用异常处理
[异常讲解](http://blog.csdn.net/hguisu/article/details/6155636) 

----
类型信息：
获取类型信息的两种方式： RTTI 、反射
RTTI在运行时识别一个对象的类型
所有的类都是在第一次使用时，动态加载到JVM中的
类型判断：
instanceof与isInstance() 同为判断该对象是不是这个类及子类的实例对象
= = 和equals() 同为判断该对象是不是是该类的实例对象



泛型化方法 将泛型参数列表置于返回类型之前即可
```
public static <K,V> Map<K,V> map(){
    return  new HashMap<K,V>();
}
```
泛型数组创建

```
1.使用ArrarList<T>创建
2.T[] array = (T[])new Object[size];
3.Object[] array =  new Object[size];
使用时(T[])array
4.最好的方式
T[] array = (T[])Array.newInstance(Class_type,size);
```

java泛型擦除的补偿：需要显式地传递类型Class对象

**泛型的使用：**

? 通配符类型

> <? extends T> 表示类型的上界 ，表示 参数化类型可能是T 或是T的子类
> <? supper T>表示类型下界（java core 中叫超类型限定） 表示参数化类型是此类型的超类型（父类型），直至Object

自限定类型
就是要求在继承关系中像下面这样使用这个类

```
class SelfBounded<T extends SelfBounded<T>>
class A extends  SelfBounded<A>
```
----

**数组：**

**数组与泛型的使用：**
通常情况下 数组与泛型不能很好的结合 你不能实例化具有参数化的数组 如：

```
Pell<Banana>[] pells = new Pell<Banana>[10];
```
擦错会移除参数类型信息，而数组必须知道它们所持有的确切类型，以强制保证类型安全。 但是可以参数化数组本身的类型：

```
public <T> T[] f(T[] arg){return arg}
```

诚然，编译器确实不让你实例化泛型数组，但是，它允许你创建对这种数组的引用：

```
List<String>[] ls ;
```

尽管你不能创建实际的持有泛型的数组对象，但是你可以创建非泛型的数组，然后将其转型

```
List<String>[] ls;
List[] la = new List[10];
ls = (List<String>)la;
ls[0] = new ArrayList<String>();
```

关于数组与包装类的选择：优选容器而不是数组

----

**枚举：**
除了不能继承自一个enum之外，我们基本上可以将enum看做一个常规的类


**枚举+siwtch:**
如果在case语句中调用了return，那么编译器就会缺少default语句了

**枚举一些常用方法：**
ordinal()： 返回一个int值 和 enum实例的声明次序相关 从0开始
name()： 返回enum实例声明时的名字
values()： 返回enum所有实例 （有编译器掺入到enum定义中的static方法 向上转型为Enum时，可以使用 class.getEnumConstants（）方法来遍历）

所有的enum都继承自 java.lang.Enum类

enum一个有趣的特性 ，运行程序员为enum实例编写方法，从而为每个enum实例赋予不同的行为：

```
enum Test{
	CLASSPATH{
		String getInfo(){
			return System.getenv("CLASSPATH");
		}
	}
	abstract String getInfo();
}
```

----

**注解：**

三种标准注解：

```
@Override 表示当前方法定义将覆盖超类中的方法
@Deprecated 被注解的元素以过时 不建议使用
@SupressWarnings 关闭不当编译器警告信息
```

五种元注解：

```
@Traget 表示该注解可以用于什么地方
@Retention 表示需要在什么级别保存改注解的信息
@Documented 将此注解包含在javadoc中
@Inherited 允许子类继承父类中的注解
@Repeatable 重复注解 
```

----

**并发：**

1.实现Runable接口

```
Thread t = new Thread(new RunableImpl());
t.start();
```
2.Executor

```
ExecutorService exec = new Executors.newCachedThreadPool();
exec.execute(new RunableImpl());
exec.shutdown();
```
3.几种线程池：

```
newFixedThreadPool(int nThreads)创建固定数目线程的线程池
newCachedThreadPool()创建一个可缓存的线程池
newSingleThreadExecutor()创建一个单线程化的Executor。
newScheduledThreadPool(int corePoolSize)创建一个支持定时及周期性的任务执行的线程池
```

4.带有返回值得任务 实现Callable< T >接口：

```
import java.util.ArrayList;   
import java.util.List;   
import java.util.concurrent.*;   

public class CallableDemo{   
    public static void main(String[] args){   
        ExecutorService executorService = Executors.newCachedThreadPool();   
        List<Future<String>> resultList = new ArrayList<Future<String>>();   

        //创建10个任务并执行   
        for (int i = 0; i < 10; i++){   
            //使用ExecutorService执行Callable类型的任务，并将结果保存在future变量中   
            Future<String> future = executorService.submit(new TaskWithResult(i));   
            //将任务执行结果存储到List中   
            resultList.add(future);   
        }   

        //遍历任务的结果   
        for (Future<String> fs : resultList){   
                try{   
                    while(!fs.isDone);//Future返回如果没有完成，则一直循环等待，直到Future返回完成  
                    System.out.println(fs.get());     //打印各个线程（任务）执行的结果   
                }catch(InterruptedException e){   
                    e.printStackTrace();   
                }catch(ExecutionException e){   
                    e.printStackTrace();   
                }finally{   
                    //启动一次顺序关闭，执行以前提交的任务，但不接受新任务  
                    executorService.shutdown();   
                }   
        }   
    }   
}   

class TaskWithResult implements Callable<String>{   
    private int id;   

    public TaskWithResult(int id){   
        this.id = id;   
    }   

    /**  
     * 任务的具体过程，一旦任务传给ExecutorService的submit方法， 
     * 则该方法自动在一个线程上执行 
     */   
    public String call() throws Exception {  
        System.out.println("call()方法被自动调用！！！    " + Thread.currentThread().getName());   
        //该返回结果将被Future的get方法得到  
        return "call()方法被自动调用，任务返回的结果是：" + id + "    " + Thread.currentThread().getName();   
    }   
}  
```

方法：

```
Thread.currentThread（）来获得对驱动任务的Thread对象的引用
Thread.currentThread（）.setPiority()设置当前线程的优先级
```

扩展：

```
Volatile 修饰的成员变量在每次被线程访问时，都强迫从共享内存中重读该成员变量的值。而且，当成员变量发生变化时，强迫线程将变化值回写到共享内存。这样在任何时刻，两个不同的线程总是看到某个成员变量的同一个值。

volatile 是一种稍弱的同步机制，在访问 volatile 变量时不会执行加锁操作，也就不会执行线程阻塞，因此 volatilei 变量是一种比 synchronized 关键字更轻量级的同步机制。
```

----
**扩展：**
接口的方法默认是public abstract
接口的属性默认是public static final 常量，且必须赋初值

----
**建议：**
参数是为方法提供参数的而不是想让方法改变自己的




