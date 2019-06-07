## Lambda

java中引入的Lambda表达式是为了让java能够编写函数式风格的代码。





## 为何需要Lambda表达式

- 在Java中，我们无法将函数作为参数传递给一个方法，也无法声明返回一个函数的方法。
- 在JS却很常见。

## Lambda表达式作用

- Lambda表达式为Java添加了缺失的函数式编程特性，使我们能将函数当做一等公民看待。
- 在将函数作为一等公民的语言中，Lambda表达式的类型是函数。但在Java中，Lambda表达式是对象，它必须依附于一类特别的对象类型——函数式接口（functional interface）。

## 函数式接口

> An informative annotation type used to indicate that an interface type declaration is intended to be a functional interface as defined by the Java Language Specification. Conceptually, a functional interface has exactly one abstract method. Since default methods have an implementation, they are not abstract. If an interface declares an abstract method overriding one of the public methods of java.lang.Object, that also does not count toward the interface's abstract method count since any implementation of the interface will have an implementation from java.lang.Object or elsewhere.
> Note that instances of functional interfaces can be created with lambda expressions, method references, or constructor references.
>
>
>
> If a type is annotated with this annotation type, compilers are required to generate an error message unless:
> The type is an interface type and not an annotation type, enum, or class.
> The annotated type satisfies the requirements of a functional interface.
>
>
>
> However, the compiler will treat any interface meeting the definition of a functional interface as a functional interface regardless of whether or not a FunctionalInterface annotation is present on the interface declaration

- 如果一个接口只有一个抽象方法，那么这个接口就是一个函数式接口。
- 如果我们在某个接口上声明`@FunctionalInterface`注解，那么编译器就会按照函数式接口来要求该接口。
- 接口中只有一个让子类实现的抽象方法。



方法引用

构造函数引用



expression:表达式

statement：语句

