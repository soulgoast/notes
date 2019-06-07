# Java内存区域与OOM

​	我们把Java虚拟机在执行Java程序的过程中所管理的内存称之为**运行时数据区域**。就像宅男的腹肌，看起来是一块，实际上是由隐藏的八块合并而成，**运行时数据区域**也是由不同的数据区域组合而成。

![img](https://ask.qcloudimg.com/http-save/yehe-3733067/czo4bh3y1t.png?imageView2/2/w/1620)

​								**运行时数据区（Running Data Area）**

如图所示，方法区和堆随着虚拟机进程的启动而存在，它们是所有线程共享的数据区。虚拟机栈、本地方法区与程序计数器依赖用户线程的启动和结束而建立和销毁，它们是线程隔离的数据区。

## 程序计数器

​	它的定义是这样的：程序计数器是一个记录着当前线程所执行的字节码的行号指示器。也就是说程序执行哪条指令是由程序计数器控制的。

​	我们知道程序计数器只是一个存储单元，假如说程序执行哪条指令是由程序计数器控制的，那么程序计数器是怎么知道该执行哪一条指令呢？流程是这样的，执行引擎在执行一条指令之前，先去程序计数器中获取它该执行哪条指令，然后执行，执行完成后获取下一条指令的地址，保存到程序计数器中。

​	你可能会说既然一条指令执行完成后，指令本身就会告诉执行引擎下一条执行的指令，那程序计数器的功能是不是多余的呢？答案肯定是“非也，非也”，由于Java虚拟机的多线程是通过线程轮流切换并分配处理器执行时间的方式来实现的，在任何一个确定的时刻，一个处理器（对于多核处理器来说是一个内核）都只会执行一条线程中的指令。因此，为了线程切换后能恢复到正确的执行位置，每条线程都需要有一个独立的程序计数器，各个线程之间计数器互不影响，独立存储，我们称这类内存区域为“线程私有”的内存。

​	上面讲了，程序计数器用于控制程序的执行。那么下面我们来探讨一下，程序到底是如何执行的。

- 无分支方法

``` java
public static int add(int a, int b) {
    int c = 0;
    c = a + b;
    return c;
}
```

**查看字节码的命令：javap -verbose ByteCode.class**

``` java
public static int add(int, int);
  descriptor: (II)I					// 描述方法参数为两个int类型的变量和方法的返回类型是int的
  flags: ACC_PUBLIC, ACC_STATIC		// 修饰方法public和static
  Code:
    stack=2, locals=3, args_size=2	// 操作数栈深度为2，本地变量表容量为3，参数个数为2
       0: iconst_0					// 将int值0压栈
       1: istore_2					// 将int值0出栈，存储到第三个局部变量（slot）中
       2: iload_0					// 将局部变量表中第一个变量压栈
       3: iload_1					// 将局部变量表中第二个变量压栈
       4: iadd						// 将操作数栈顶两个int数弹出，相加后再压入栈中
       5: istore_2					// 将栈顶的int数弹出，存储到第三个局部变量（slot）中
       6: iload_2					// 将局部变量表中第三个变量压栈
       7: ireturn					// 返回栈中数字
    LineNumberTable:
      line 6: 0						// 代码第6行对应字节码第0行
      line 7: 2						// 代码第7行对应字节码第2行
      line 8: 6						// 代码第8行对应字节码第6行	
    LocalVariableTable:
      Start  Length  Slot  Name   Signature
          0       8     0     a   I	// a占用第1个solt
          0       8     1     b   I	// b占用第2个solt
          2       6     2     c   I // c占用第3个solt

```

根据上面字节码画出下面局部变量表和操作数栈之间的操作关系。

![局部变量表和操作数栈关系](http://upload-images.jianshu.io/upload_images/2843224-116b81e2c4cd9e6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. 指令0执行后：局部变量表中有两个数字10、和20，操作数栈一个值0，程序计数器指向第0行字节码指令

   0: iconst_0 //将int值0压栈

2. 指令1执行后：局部变量表中有三个数字10、20和0，操作数栈没有值，程序计数器指向第1行字节码指令

   1: istore_2 //将int值0出栈，存储到第三个局部变量（slot）中

3. 指令2执行后：局部变量表中有三个数字10、20和0，操作数栈一个值10，程序计数器指向第2行字节码指令

   2: iload_0 //将局部变量表中第一个变量10压栈

4. 指令3执行后：局部变量表中有三个数字10、20和0，操作数栈两个值10和20，程序计数器指向第3行字节码指令

   3: iload_1 //将局部变量表中第一个变量20压栈

5. 指令4执行后：局部变量表中有三个数字10、20和0，操作数栈一个值30，程序计数器指向第4行字节码指令

   4: iadd //将操作数栈顶两个int数弹出10和20，相加后再压入栈中

6. 指令5执行后：局部变量表中有三个数字10、20和30，操作数栈没有值，程序计数器指向第5行字节码指令

   5: istore_2 //将栈顶的int数（30）弹出，存储到第三个局部变量（slot）中

7. 指令6执行后：局部变量表中有三个数字10、20和30，操作数栈一个值30，程序计数器指向第6行字节码指令

   6: iload_2 //将局部变量表中第三个变量压栈

8. 指令7执行后:将栈中的数字返回给调用方法，并销毁此栈帧

   7: ireturn //返回栈中数字30

- if分支方法

``` java
public static int app(int a) {
    if(a > 10) {
        return a;
    }
    return -a;
}

  public static int app(int);
    descriptor: (I)I
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=1, args_size=1
         0: iload_0
         1: bipush        10
         3: if_icmple     8
         6: iload_0
         7: ireturn
         8: iload_0
         9: ineg
        10: ireturn
      LineNumberTable:
        line 6: 0
        line 7: 6
        line 9: 8
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      11     0     a   I
      StackMapTable: number_of_entries = 1
        frame_type = 8 /* same */
```



- if-else分支方法

``` java
public static int app(int a) {
    if(a > 10) {
        return a;
    } else if (a > 5) {
        return a + 5;
    } else {
        return -a;
    }
}

  public static int app(int);
    descriptor: (I)I
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=1, args_size=1
         0: iload_0
         1: bipush        10
         3: if_icmple     8
         6: iload_0
         7: ireturn
         8: iload_0
         9: iconst_5
        10: if_icmple     17
        13: iload_0
        14: iconst_5
        15: iadd
        16: ireturn
        17: iload_0
        18: ineg
        19: ireturn
      LineNumberTable:
        line 6: 0
        line 7: 6
        line 8: 8
        line 9: 13
        line 11: 17
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      20     0     a   I
      StackMapTable: number_of_entries = 2
        frame_type = 8 /* same */
        frame_type = 8 /* same */
```



- while分支方法

``` java
public static int app(int a) {
    while (a < 10) {
        a ++;
    }
    return a;
}

  public static int app(int);
    descriptor: (I)I
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=1, args_size=1
         0: iload_0
         1: bipush        10
         3: if_icmpge     12
         6: iinc          0, 1
         9: goto          0
        12: iload_0
        13: ireturn
      LineNumberTable:
        line 6: 0
        line 7: 6
        line 9: 12
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      14     0     a   I
      StackMapTable: number_of_entries = 2
        frame_type = 0 /* same */
        frame_type = 11 /* same */
```



- for分支方法

``` java
public static int app(int[] a) {
    int c = 0;
    for (int b : a){
        c += b;
    }
    return c;
}

  public static int app(int[]);
    descriptor: ([I)I
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=6, args_size=1
         0: iconst_0
         1: istore_1
         2: aload_0
         3: astore_2
         4: aload_2
         5: arraylength
         6: istore_3
         7: iconst_0
         8: istore        4
        10: iload         4
        12: iload_3
        13: if_icmpge     33
        16: aload_2
        17: iload         4
        19: iaload
        20: istore        5
        22: iload_1
        23: iload         5
        25: iadd
        26: istore_1
        27: iinc          4, 1
        30: goto          10
        33: iload_1
        34: ireturn
      LineNumberTable:
        line 6: 0
        line 7: 2
        line 8: 22
        line 7: 27
        line 10: 33
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
           22       5     5     b   I
            0      35     0     a   [I
            2      33     1     c   I
      StackMapTable: number_of_entries = 2
        frame_type = 255 /* full_frame */
          offset_delta = 10
          locals = [ class "[I", int, class "[I", int, int ]
          stack = []
        frame_type = 248 /* chop */
          offset_delta = 22
```



- swith分支方法

``` java
public static int app(int a) {
    int b;
    switch (a){
        case 1:
            b = 1;
            break;
        case 2:
            b = 2;
            break;
        case 3:
            b = 3;
            break;
        default:
            b = 10;
    }
    return b;
}

  public static int app(int);
    descriptor: (I)I
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=1, locals=2, args_size=1
         0: iload_0
         1: tableswitch   { // 1 to 3
                       1: 28
                       2: 33
                       3: 38
                 default: 43
            }
        28: iconst_1
        29: istore_1
        30: goto          46
        33: iconst_2
        34: istore_1
        35: goto          46
        38: iconst_3
        39: istore_1
        40: goto          46
        43: bipush        10
        45: istore_1
        46: iload_1
        47: ireturn
      LineNumberTable:
        line 7: 0
        line 9: 28
        line 10: 30
        line 12: 33
        line 13: 35
        line 15: 38
        line 16: 40
        line 18: 43
        line 20: 46
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
           30       3     1     b   I
           35       3     1     b   I
           40       3     1     b   I
            0      48     0     a   I
           46       2     1     b   I
      StackMapTable: number_of_entries = 5
        frame_type = 28 /* same */
        frame_type = 4 /* same */
        frame_type = 4 /* same */
        frame_type = 4 /* same */
        frame_type = 252 /* append */
          offset_delta = 2
          locals = [ int ]
```







``` java
public static void main(String[] args) {
    sum(Integer.MAX_VALUE);
}

public static int sum(int i){
    int b = 0 ;
    b += sum(i);
    return b;
}

Exception in thread "main" java.lang.StackOverflowError
	at com.qunce.jvm.memory.StackOverFlowDemo.sum(StackOverFlowDemo.java:11)
	at com.qunce.jvm.memory.StackOverFlowDemo.sum(StackOverFlowDemo.java:11)
```



``` java
    public static void main(String[] args) {
        List<String> list = new LinkedList<>();
        for (int i = 0;i < Integer.MAX_VALUE; i++){
            list.add(new String("你可能会说既然一条指令执行完成后，指令本身就会告诉执行引擎下一条执行的指令，那程序计数器的功能是不是多余的呢？答案肯定是“非也，非也”，由于Java虚拟机的多线程是通过线程轮流切换并分配处理器执行时间的方式来实现的，在任何一个确定的时刻，一个处理器（对于多核处理器来说是一个内核）都只会执行一条线程中的指令。因此，为了线程切换后能恢复到正确的执行位置，每条线程都需要有一个独立的程序计数器，各个线程之间计数器互不影响，独立存储，我们称这类内存区域为“线程私有”的内存。你可能会说既然一条指令执行完成后，指令本身就会告诉执行引擎下一条执行的指令，那程序计数器的功能是不是多余的呢？答案肯定是“非也，非也”，由于Java虚拟机的多线程是通过线程轮流切换并分配处理器执行时间的方式来实现的，在任何一个确定的时刻，一个处理器（对于多核处理器来说是一个内核）都只会执行一条线程中的指令。因此，为了线程切换后能恢复到正确的执行位置，每条线程都需要有一个独立的程序计数器，各个线程之间计数器互不影响，独立存储，我们称这类内存区域为“线程私有”的内存。"));
        }
    }
-Xms1m -Xmx1m -XX:MaxMetaspaceSize=50M -XX:+HeapDumpOnOutOfMemoryError  

Exception in thread "Monitor Ctrl-Break" Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
	at com.qunce.jvm.memory.OutOfMemoryDemo.main(OutOfMemoryDemo.java:11)
    
-Xms1m -Xmx1m -XX:MaxMetaspaceSize=50M -XX:+HeapDumpOnOutOfMemoryError
```

