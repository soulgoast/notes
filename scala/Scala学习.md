# Scala学习

## Scala介绍

------

### Scala与Java的关系

Scala基于Java虚拟机，是一门依赖JVM的编程语言。所有Scala的代码，都需要经过编译为字节码，然后交由Java虚拟机来运行。

``` scala
object HelloWorld {
    /* 这是一个Scala程序
     * 这是一行注释
     * 这里演示了多行注释
     */
    def main(args: Array[String]){
        //这是一个单行注释
        //输出Hello World
        println("Hello World!")
    }
}
```



## 基础语法

### 关键字

1. 修饰符：abstract、final

2. 流程控制：case、do、else、for、if、return、while、

3. 异常控制：catch、throw、try、finally

4. 元素定义：class、def、object、package、trait、val、var

5. 继承：extends、

6. 值：false、null、super、this、true

7. 不知道的：forSome、implicit、lazy、match、sealed、type、with、yield

8. 导入包：import、

9. 创建对象：new

10. 覆盖：override

11. 作用于：private、protected、public

12. 符号：

    ``` scala
    "-"、":"、"="、"=>"、"<-"、"<:"、"<%"、">:"、"#"、"@"
    ```


> abstract

``` scala
abstract


case
```

> case

``` sca

```



> val vs var



> import

``` scala
import java.awt.Color 	//引入Color
import java.awt._		//引入包内所有成员
```

> private vs portected vs public

```scala

```

# Scala解析器的使用

- REPL：Read(取值)——>Evaluation(求值)——>Print（打印）——>Loop(循环)。scala解释器也被称为REPL，会快速编译scala代码为字节码，然后交给JVM来执行。
- 

```scala
//字符串常见方法

```

# 函数

```scala
//函数的定义与调用
def sayHello(name:String, age:Int) = {
    if(age > 18){
        printf("Hi,%s,you are a big boy!!")
        age
    }else{
        printf("Hi,%s,you are a little boy!!")
        age
    }
}

sayHello("soulgoast", 18)
```

```scala
//for循环与累加函数
def sum(num:Int) = {
    var result = 0
    for(i <- 1 to num){
        result += i
    }
    result
}
```

```scala
//递归函数
def fab(n:Int):Int = {
    if(n <= 1) 1
    else fab(n -1) + fab(n - 2)
}

fab(10)
```































































### 变量声明方式

### 函数

### 类

表达式

### 控制语句

### 常见数据类型