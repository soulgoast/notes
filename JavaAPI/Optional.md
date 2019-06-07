# Optional

> A container object which may or may not contain a non-null value. If a value is present, isPresent() will return true and get() will return the value.
>
> `optional`是一个可能包含`null`值的对象容器。如果值存在，`isPresent()`方法返回`true`,使用`get()`方法获取容器中的值。	
>
> Additional methods that depend on the presence or absence of a contained value are provided, such as orElse() (return a default value if value not present) and ifPresent() (execute a block of code if the value is present).
>
> 提供的额外方法也是基于容器中的值是否存在，比如`orElse()`(如果值不存在返回默认的值)和`ifPresent()`(如果值存在将会执行一段代码)。
>
> This is a value-based class; use of identity-sensitive operations (including reference equality (==), identity hash code, or synchronization) on instances of Optional may have unpredictable results and should be avoided.
>
> 这是一个基于值的类；`Optional`的实例在进行一些敏感的一致性操作（包括引用相等（==）、哈希码一致性或者时同步）时，可能会产生不可预测的结果，应该避免使用。

## API

`Optional`将构造方法私有化了，对外提供了可以获取`Optional`实例的静态方法。

``` java
//返回一个空的实例
public static<T> Optional<T> empty() {
    @SuppressWarnings("unchecked")
    Optional<T> t = (Optional<T>) EMPTY;
    return t;
}

//返回一个值存在的实例
public static <T> Optional<T> of(T value) {
    return new Optional<>(value);
}

//返回一个值不一定存在的实例
public static <T> Optional<T> ofNullable(T value) {
    return value == null ? empty() : of(value);
}

```



## Optional的应用场景

1. 获取Optional中的值
2. 方法返回集合类的注意事项
3. 使用Optional避免集合对象的多重判断

## 基于值的类（Value-based Classes）



1. 

------

注意事项

Optional不能作为类的属性或者方法的参数，只能作为方法的返回值。



<https://docs.oracle.com/javase/8/docs/api/java/lang/doc-files/ValueBased.html>



Ctrl + P:查看方法参数

Ctrl + Q:查看类、方法、属性注释





