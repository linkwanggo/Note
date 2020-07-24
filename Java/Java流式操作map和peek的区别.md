# Java流式操作map和peek的区别

首先看定义

```java
Stream<T> peek(Consumer<? super T> action);
```

peek方法接收一个Consumer的入参。了解λ表达式的应该明白 Consumer的实现类 应该只有一个方法，该方法返回类型为void。

```java
Consumer<Integer> c =  i -> System.out.println("hello" + i);
```

而map方法的入参为 Function。

```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
```

Function 的 λ表达式 可以这样写

```java
Function<Integer,String> f = x -> {return  "hello" + i;};
```

我们发现Function 比 Consumer 多了一个 return。 
这也就是peek 与 map的区别了。

总结：peek接收一个没有返回值的λ表达式，可以做一些输出，外部处理等。map接收一个有返回值的λ表达式，之后Stream的泛型类型将转换为map参数λ表达式返回的类型

# ---END---

