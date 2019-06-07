# JavaAPI——java.lang

# Error）

### AbstractMethodError

> Thrown when an application tries to call an abstract method. Normally, this error is caught by the compiler; this error can only occur at run time if the definition of some class has incompatibly changed since the currently executing method was last compiled.
>
> - Since:
>
>      JDK1.0
>
> - Author:
>
>     unascribed

当应用程序调用抽象方法时抛出。一般是在编译时发现。如果某些编译好的类在执行方法的时候不兼容，则此错误将在运行时发生。

总结：

```java
public interface ClassFather{
    public void method(Integer num);
}

public class ClassSon implements ClassFather{
    
    @override
    public void method(String str){
        
    }
}
//当调用ClassFather的method方法时，由于参数类型不匹配，就会抛出AbstractMethodError错误。我们在编码时，编译器就会发现此种错误，所以此种错误只会发生在编译好的jar包上，也就是引用的jar包不兼容时才会出现。

```

### IncompatibleClassChangeError

> Thrown when an incompatible class change has occurred to some class definition. The definition of some class, on which the currently executing method depends, has since changed.
>
> - Since:
>
>      JDK1.0
>
> - Author:
>
>     unascribed

# Interface和Class

### Appendable

> An object to which `char` sequences and values can be appended.  The  `Appendable` interface must be implemented by any class whose instances are intended to receive formatted output from a `java.util.Formatter`.  
>
>  The characters to be appended should be valid Unicode characters as described in [Unicode Character Representation](https://docs.oracle.com/javase/8/docs/api/java/lang/Character.html#unicode).  Note that supplementary characters may be composed of multiple 16-bit `char` values.  
>
>  Appendables are not necessarily safe for multithreaded access.  Thread safety is the responsibility of classes that extend and implement this interface.  
>
>  Since this interface may be implemented by existing classes with different styles of error handling there is no guarantee that errors will be propagated to the invoker.
>
> - Since:
>
>    1.5

1. 可追加的字符序列或结果对象。通过formatter实例输出的任何对象必须实现appendable接口。

2. 追加的字符必须满足unicode规范。注意，补充字符可以由多个16bit值组成。

3. appendable并不保证线程安全，这需要实现它的类自己实现。

4. 由于该接口可能有抛出各种不同样式的错误的实现类，因此，在此不提供统一的异常。

   ```java
   
   ```



### AutoCloseable

> An object that may hold resources (such as file or socket handles) until it is closed. The `close()` method of an `AutoCloseable` object is called automatically when exiting a `try`-with-resources block for which the object has been declared in the resource specification header. This construction ensures prompt release, avoiding resource exhaustion exceptions and errors that may otherwise occur.
>
> - Since:
>
>    1.7
>
> - Author:
>
>    Josh Bloch
>
> - @apiNote
>
>   It is possible, and in fact common, for a base class to implement AutoCloseable even though not all of its subclasses or instances will hold releasable resources.  For code that must operate in complete generality, or when it is known that the `AutoCloseable` instance requires resource release, it is recommended to use `try`-with-resources constructions. However, when using facilities such as  `java.util.stream.Stream` that support both I/O-based and non-I/O-based forms, `try`-with-resources blocks are in general unnecessary when using non-I/O-based forms.

一个对象（比如文件或者套接字句柄）在被关闭前可能会占用系统资源。如果AutoCloseable对象在try-catch-resources代码块中申明，那么close()方法将会自动调用。这种结构确保迅速释放，避免资源耗尽异常和可能发生的错误。

注意：

实际上，对于所有实现AutoCloseable的基础类，尽管并不是所有的子类或者实例都拥有可以释放的资源。对于必须完成通用操作的代码，或者知道Autocloseable实例需要释放资源，推荐使用try-with-resources结构。然而，对于一些工具类，比如java.util.stream包中的Stream同时支持I/O和N-I/O模式，N-I/O模式通常不必使用try-with-resources结构。



**1.在1.7之前，我们通过try{} finally{} 在finally中释放资源。**



在finally中关闭资源存在以下问题：

1、自己要手动写代码做关闭的逻辑；

2、有时候还会忘记关闭一些资源；

3、关闭代码的逻辑比较冗长，不应该是正常的业务逻辑需要关注的；

 

**2.对于实现AutoCloseable接口的类的实例，将其放到try后面（我们称之为：带资源的try语句），在try结束的时候，会自动将这些资源关闭（调用close方法）。**

 

带资源的try语句的3个关键点：

1、由带资源的try语句管理的资源必须是实现了AutoCloseable接口的类的对象。

2、在try代码中声明的资源被隐式声明为fianl。

3、通过使用分号分隔每个声明可以管理多个资源。

### 