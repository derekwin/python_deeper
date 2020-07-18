### 六，迭代器和生成器

> 可迭代对象，迭代器和生成器
>
> 可迭代对象：内部实现有一个```__iter__()```方法，使得他可以迭代，用for循环
>
> 迭代器：可迭代对象基础上增加一个```__next__()```方法，这样就可以不使用for，用 next() 来实现获取下个值
>
> 生成器：在迭代器的基础上增加yield的概念，实现了在计算下一个值的时候不需要浪费空间的结构，异步编=程鼻祖，事件型编程的根基。

迭代对象和迭代器的值都在内存中，生成器是一点一点分配内存，节省空间



#### 迭代器

将迭代操作代理到容器内部的对象上

> Python的迭代器需要```__iter__()```返回一个实现了```__next__()```方法的迭代器对象

```python
# 伪代码 test是一个可迭代对象

a=iter(test)			# 通过iter()将可迭代对象转化成迭代器
next(a)					# 即可实现next
```



具体的容器实现示例见[python编程时光的系列文章第七节](https://www.cnblogs.com/wongbingming/p/9060989.html)



##### zip()迭代器

zip( list1,list2 ) 本质迭代器，用以将两个序列的元素依次迭代生成元组序列

1. 一种用法，用以字典键对序列反转。[click to 字典补充](#字典补充)

2. 同时遍历两个列表

    ```python
    a=[1,2,3]
    b=['a','b','c','d']
    for i in zip(a,b):
        pass
    # 此处适用短板效应
    ```

> 补充：解除短板限制
>
> itertools.zip_longest()，没有的项置None
>
> ```python
> from itertools import zip_longest
> ```

3. 转成字典和list

    ```python
    s=dict(zip(a,b))
    # {1: 'a', 2: 'b', 3: 'c'}
    l=list(zip(a,b))
    # [(1, 'a'), (2, 'b'), (3, 'c')]
    ```







#### 生成器

> 生成器的激活方式：
>
> 1. next( )									# 协程中要注意 预激			
>
> 2. generator.send ( None )                                  # 引申出协程概念 [协程](#协程)



##### 列表生成器

列表生成器自带异常抛出

```python
[x for x in range(3,7) if x > 0]
a = (x for x in range(3,7))
```



##### yield

yield实现的生成器，没有异常？需要自己抛出异常,,,,,(此处存在疑惑，我的环境py3.8是可以抛出异常的)

- yield:简单可以理解成不打断函数的return，返回值之后阻塞

    ```python
    def foo():
    	while True:
            res=yield 4
            print(res)
    
    run=foo()
    print(next(run))
    print(next(run))
    
    # 结果
    >>> print(next(run))
    4
    >>> print(next(run))
    None
    4
    ```

    解析：

    函数中使用yield返回结果之后，函数相当于生成器，本质是对象。生成器有一个next函数。~此处函数严格讲是生成器函数~

    run=foo()之后，next触发函数执行，执行到res=yield 4这句时候，yield 4，返回4。函数（生成器）停止

    下次调用next，从上次停止的地方开始执行。执行赋值操作，右侧上次执行已经return了，故此处给左侧的赋值是None，故print结果是None

- 一个关于生成器很好的解耦例子

    ```python
    from collections import deque
    
    def search(line,pattern,history=5):
        previous_lines=deque(maxlen=history)
        for line in lines:
            if pattern in line:
                yield line,previous_line
            previous_line.append(line)
    
           
    if __name__ == "__main__":
        with open(r'path/to/file.txt') as f:
            for line,prevlines in search(f,python,5):
                for pline in prevlines:
                    print(pline,end="")
    # 查找关键字，并保留最近5项的历史记录
    
    ```



##### yield from

> yield from 后面需要加的是可迭代对象，可以是普通的可迭代对象，也可以是迭代器，==生成器==。

> yield from + 迭代器  可以实现将迭代对象里面的元素yield出来
>
> yield from + 生成器  实现生成器的嵌套

1. 嵌套展开

    ```python
    A='absd'
    B=[1,2,3]
    C={'key1':23,'key2':24}
    D=(i for i in range(2,6))
    
    def gen(*args,**kwargs):
        for item in args:
            for i in item:
                yield i
                    
    def gen(*args,**kwargs):
    	for item in args:
            yield from item
            
    testlist=gen(A,B,C,D)
    print(list(testlist))
    
    #结果一样
    ['a','b','s','d',1,2,3,'key1','key2',2,3,4,5]
    ```

​    

2. 生成器的嵌套

    > - 调用方：调用委派生成器的代码
    >
    > - 委托生成器：包含yield from的生成器函数
    >     - 在调用方和子生成器之间建立==双向通道==
    >     - 调用方可以通过```send()```方法向子生成器发送消息，子生成器yield可以直接返回给调用方
    >     - 委托生成器只起桥梁作用，不会拦截中间传递的数据
    >
    > - 子生成器：yield from后面的生成器函数

    ```python
    # 实时均值计算
    # 子生成器
    def average_gen():
        total=0
        count=0
        average=0
        while True:
            new_num = yield average
            if new_num is None:
                break
            count += 1
            total += new_num
            average = total/count
            
        return total,count,average
    	# return生效的时候，代表着这次协程结束
    
    # 委托生成器
    def proxy_gen():
        while True:
            # 子生成器结束即return之后，yield才会释放，并进行左侧的赋值
            total,count,average=yield from average_gen()
            print("{},{},{}".format(total,count,average))
    
    # 调用方
    def main()
    	caculate_average=proxy_gen()
        next(caculate_average)				# 预激协程
        print(caculate_average.send(10))
        print(caculate_average.send(20))
        caculate_average.send(None)			# 协程结束
        
        # 如果此处再调用caculate_average.send(10)，由于上一协程已经结束，将重开一协程
        
    if __name__ == "__main__":
        main()
    ```

    > 预激协程的意义 在于 生成器起初的时候并不在yield这个地方，预激之后便停留在阻塞点

    > **yield from**可以协助进行异常的捕获和处理

---危

> 1. 迭代器（即可指子生成器）产生的值直接返还给调用者
> 2. 任何使用send()方法发给委派生产器（即外部生产器）的值被直接传递给迭代器。如果send值是None，则调用迭代器next()方法；如果不为None，则调用迭代器的send()方法。如果对迭代器的调用产生StopIteration异常，委派生产器恢复继续执行yield from后面的语句；若迭代器产生其他任何异常，则都传递给委派生产器。
> 3. 子生成器可能只是一个迭代器，并不是一个作为协程的生成器，所以它不支持.throw()和.close()方法,即可能会产生AttributeError 异常。
> 4. 除了GeneratorExit 异常外的其他抛给委派生产器的异常，将会被传递到迭代器的throw()方法。如果迭代器throw()调用产生了StopIteration异常，委派生产器恢复并继续执行，其他异常则传递给委派生产器。
> 5. 如果GeneratorExit异常被抛给委派生产器，或者委派生产器的close()方法被调用，如果迭代器有close()的话也将被调用。如果close()调用产生异常，异常将传递给委派生产器。否则，委派生产器将抛出GeneratorExit 异常。
> 6. 当迭代器结束并抛出异常时，yield from表达式的值是其StopIteration 异常中的第一个参数。
> 7. 一个生成器中的return expr语句将会从生成器退出并抛出 StopIteration(expr)异常。







##### 数据处理管道

> 引言：SMTP邮件过滤系统中，使用了文件作为缓冲区处理数据，改为数据管道或者传输队列。一种更好的解决方法是创建数据处理管道流，定义一套处理管道函数，实现实时流式处理。本质也是传统多线程或多进程带来的性能上的局限性，数据处理管道也引申出 基于事件驱动的编程模式

实现思路：

​		通过定义一套层层嵌套的处理函数，返回值的时候使用yield，可以实现模块化处理函数。 yield 语句作为数据的生产者而 for 循环语句作为数据的消费者

[dabeaz生成器详解](http://www.dabeaz.com/generators/)





#### 一些迭代函数

1. re模块的[re.finditer](#正则匹配)函数
2. 内置的enumerate()函数生成序列的索引，本质是迭代



参考 ： 

1. [MING - Python编程时光---py3并发编程系列](