#  如何给列表降维？sum()函数的妙用

转载自：[如何给列表降维？sum()函数的妙用](https://segmentfault.com/a/1190000018903731)

上个月，学习群里的 S 同学问了个题目，大意可理解为`列表降维` ，例子如下：

```
oldlist = [[1, 2, 3], [4, 5]]

# 想得到结果：
newlist = [1, 2, 3, 4, 5]
```

原始数据是一个二维列表，目的是获取该列表中所有元素的具体值。从抽象一点的角度来理解，也可看作是列表解压或者列表降维。

这个问题并不难，但是，怎么写才比较优雅呢？

```
# 方法一，粗暴拼接法：
newlist = oldlist[0] + oldlist[1]
```

这种方法简单粗暴，需要拼接什么内容，就取出来直接拼接。然而，如果原列表有很多子列表，则这个方法就会变得繁琐了。

我们把原问题升级一下：**一个二维列表包含 n 个一维列表元素，如何优雅地把这些子列表拼成一个新的一维列表？**

方法一的做法需要写 n 个对象，以及 n - 1 次拼接操作。当然不可行。下面看看方法二：

```
# 方法二，列表推导式：
newlist = [i for j in range(len(oldlist)) for i in oldlist[j]]
```

这个表达式中出现了两个 for 语句，在第一个 for 语句中，我们先取出原列表的长度，然后构造 range 对象，此时 j 的取值范围是 [0, n-1] 的闭区间。

在第二个 for 语句中，oldlist[j] 指的正是原列表的第 j 个子列表，`for i in oldlist[j]` 则会遍历取出 j 子列表的元素，由于 j 取值的区间正对应于原列表的全部索引值，所以，最终达到解题目的。

这种方法足够优雅了，而且理解也并不难。

然而，我们是否就能满足于此了呢？有没有其它奇技淫巧，哦不，是其它高级方法呢？F 同学贡献了一个思路：

```
# 方法三，巧用sum：
newlist = sum(oldlist,[])
```

说实话，这个方法令我大感意外！sum() 函数不是用于求和的么？怎么竟然有此用法？

这个写法利用了什么原理呢？由于我开始时不知道 sum() 函数可以接收两个参数，不清楚它们是怎么用于计算的，所以一度很困惑。但是，当我知道 sum() 的完整用法时，我恍然大悟。

接下来也不卖关子了，直接揭晓吧。

语法： `sum(iterable[, start])` ，sum() 函数的第一个参数是可迭代对象，如列表、元组或集合等，第二个参数是起始值，默认为 0 。其用途是以 start 值为基础，再与可迭代对象的所有元素相“加”。

在上例中，执行效果是 oldlist 中的子列表逐一与第二个参数相加，而列表的加法相当于 extend 操作，所以最终结果是由 [] 扩充成的列表。

这里有两个关键点：**sum() 函数允许带两个参数，且第二个参数才是起点。** 可能 sum() 函数用于数值求和比较多，然而用于作列表的求和，就有奇效。它比列表推导式更加优雅简洁！

至此，前面的升级版问题就得到了很好的回答。简单回顾一下，s 同学最初的问题可以用三种方法实现，第一种方法中规中矩，第二种方法正道进阶，而第三种方法旁门左道（没有贬义，只是说它出人意料，却效果奇佳）。

这道并不算难的问题，在众人的讨论与分享后，竟还引出了很有价值的学习内容。前不久，同样是群内的一个问题，也产生了同样的学习效果，详见《[Python进阶：如何将字符串常量转为变量？](https://mp.weixin.qq.com/s/4eWQmJ15QZabNViePCDmNw)》。

我从中得到了一个启示：**应该多角度地思考问题，设法寻求更优解，同时，基础知识应掌握牢固，并灵活贯通起来。**

学无止境，这里我还想再开拓一下思路，看看能发现些什么。

1、如果原列表的元素除了列表，还有其它类型的元素，怎么把同类的元素归并在一起呢？

2、如果是一个三维或更高维的列表，怎么更好地把它们压缩成一维列表呢？

3、sum() 函数还有什么知识要点呢？

前两个问题增加了复杂度，解决起来似乎没有“灵丹妙药”了，只能用笨方法分别拆解，逐一解压。

第三个思考题是关于 sum() 函数本身的用法，我们看看官方文档是怎么说的：

> The
>
>  
>
> iterable
>
> ’s items are normally numbers, and the start value is not allowed to be a string.
>
> For some use cases, there are good alternatives to [`sum()`](https://docs.python.org/3/library/functions.html#sum). The preferred, fast way to concatenate a sequence of strings is by calling `''.join(sequence)`. To add floating point values with extended precision, see [`math.fsum()`](https://docs.python.org/3/library/math.html#math.fsum). To concatenate a series of iterables, consider using [`itertools.chain()`](https://docs.python.org/3/library/itertools.html#itertools.chain).

sum() 的第二个参数不允许是字符串。如果用了，会报错：

> TypeError: sum() can't sum strings [use ''.join(seq) instead]

为什么不建议使用 sum() 来拼接字符串呢？哈哈，文档中建议使用 join() 方法，因为它更快。为了不给我们使用慢的方法，它竟特别限定不允许 sum() 的第二个参数是字符串。

文档还建议，在某些使用场景时，不要用 sum() ，例如当以扩展精度对浮点数求和时，推荐使用 `math.fsum()` ；当要拼接一系列的可迭代对象时，应考虑使用 `itertools.chain()` 。

浮点数的计算是个难题，我曾转载过一篇《[如何在 Python 里面精确四舍五入？](https://mp.weixin.qq.com/s/4Se4j-_N0cXiWvoQSRHp1w)》，对此有精彩分析。而`itertools.chain()` 可以将不同类型的可迭代对象串联成一个更大的迭代器，这在旧文《[Python进阶：设计模式之迭代器模式](https://mp.weixin.qq.com/s/7MbRCn37fIIN42rLm6ho3g)》中也有论及。

不经意间，sum() 函数的注意事项，竟把 Python 其它的进阶内容都联系起来了。小小的函数，竟成为学习之路上的一个枢纽。

前段时间，我还写过 range() 、locals() 和 eval() 等内置函数，也是通过一个问题点，而关联出多个知识点， 获益良多。这些内置函数/类的魔力可真不小啊。

本文到此结束，希望对你有所帮助。

## ---END---

