### 函数



#### 函数带默认参数

```python
def test(a,b=10):
	print('b默认执行的时候参数值是10')
```



#### 动态参数:*与**

> 在形参前面加上*和**，成为动态参数

加*，函数可以接收任意多个参数，全部放进一个元组中

```python
def fun(*args):
	print(args)

F(123,"542")
# (123,'542')
```

加**，函数接收参数，传入的格式是字典

```python
def fun(**kwargs):
	print(kwargs)

F(k1=123,k2="543")
# {'k1':123,'k2':'543'}
```

再者，对于实参是列表和字典的情况

```python
def F(*args,**kwargs):
	print(args,kwargs)
li=[2,3,1,4]
di={'k1':1,'k2':2}
F(li,k=di)
# ([2,3,1,4]) , {'k':{'k1':1,'k2':2}}
F(*li,**di)
# (2,3,1,4) , {'k1':1,'k2':2}
# ps 输进来的**kwargs
# 如果直接print(**kwargs)		# TypeError: 'key1' is an invalid keyword argument for print()
# 如果print(*kwargs)			# 返回的是 key1 key2  键

# 所以本质上来说 *解包 一维数据结构	**解包 二维数据结构
# 对字典进行解压
# *dict			-> 对应的键
# **dict		-> 对应的键值对	（不能直接使用，可以在函数中参数传入使用）
```



#### eval()    

【Built-In】和repr()函数对应，没什么用目前看   跳转[repr](#默认父类object)

eval() 函数用来执行一个字符串表达式，并返回表达式的值。

```
>>> eval( '3 * x' )
21
```

> repr() 用来给外围增加 “ ”
>
> eval() 用来去掉外围“ ”





#### 匿名函数

```python
y=lambda a,b:a+b
```

使用场景

```python
sorted(list_of_names,key=lambda name: name.split()[-1].lower())

min(dict1 ,key=lambda k: dict1[k])	
```

匿名函数的变量捕获问题

```python
a=10
y=lambda b:a+b
a=20
k=lambda b:a+b
y(10)
k(10)
# 最终 y k均为30，因为函数执行时候,a是一个动态获取的变量，运行的时候 才会绑定值
```

```python
# 解决方法
y= lambda b,a=a : a+b
# 思路是在定义的时候 就把值写入成函数的局部变量

# 案例
funcs = [lambda x: x+n for n in range(5)]
funcs = [lambda x,n=n: x+n for n in range(5)]
```



#### 闭包

>  ✔ python cookbook3这块用 "将单方法的类转换成函数" 这种思想引出了闭包的概念
>
>  很nice

```python
class test:
	def __init__(self,outside_value):
        self.value=outside_value
    
    def run(self,inter_value):
        return outside_value+inter_value
    
>>> a=test(20)
>>> a.run(10)
```

转成闭包

```python
def test(outside_value):
    def run(inter_value):
		return outside_value+inter_value
    return run										# 注意这块返回的是函数  run()则是执行,变成内部定义，内部嵌套了
>>> a=test(1)
>>> a(2)
3
```

> 闭包实质是将一个内部函数返回给外层函数的调用者，内部函数可以记忆当前定义环境时外部函数给他提供的变量等外部环境，也是一个局部环境。
>
> ==闭包就是引用了自有变量的函数，这个函数保存了执行的上下文，可以脱离原本的作用域独立存在。==
>
> 其实闭包不好理解的时候，将他返回到类的角度，就解决了

【ps 获取闭包内部的变量】

```python
def test():
	value=10
	def run(n):
        return value+n
    def get_value():
    	return value
    def set_value(x):
        nonlocal value					# nonlocal使得内部变量value可以被修改
        value=x
    run.get_value=get_value
    run.set_value=set_value
    return run

>>> a=test()
>>> a(1)
11
>>> a.set_value(20)
>>> a(1)
21
>>> a.get_value()
20
```



#### 装饰器

【decorator】

1. 简单的装饰器 (不带参数)

    ```python
    def decorator(fun):
        def foo(*args,**kwargs):
            print(kwargs):
            fun(*args)
        return foo
    
    def function(test):
        print(test)
    function=decorator(function)			# 原理上的掉用方法
    >>> function('args',key=1)
    {'key':1}
    'args'
    
    # 使用 @
    @decorator
    def function(test):
        print(test)
    >>> function('args',key=1)
    {'key':1}
    'args'
    
    # 起到包装函数的作用
    ```

2. 带参数的装饰器

    ```python
    def outsidefun(value):
        num=value
        def decorator(fun):
            def insidefun(*args):
                fun(num)
                fun(*args)
            return insidefun
        return decorator
    @outsidefun(1)
    def test(n):
        print(n)
    
    >>> test(2)
    1
    2
    # 本质就是装饰器加装了一个闭包
    ```

3. 装饰器的一个问题

    简单包装后，函数的元信息变成了insidefun的信息，出问题了

    【解决方法 functools.warps】

    作用，将原函数的元信息拷贝到新的装饰器函数

    ```python
    from functools import wraps
    
    def outsidefun(value):
        num=value
        def decorator(fun):
            @wraps(fun)
            def insidefun(*args):
                fun(num)
                fun(*args)
            return insidefun
        return decorator
    @outsidefun(1)
    def test(n):
        '''test.doc'''
        print(n)
        
    >>> print(test.__name__,test.__doc__)
    test test.doc
    ```

4. 类装饰器 

    [跳转](#默认父类object)

    ```python
    # __call__() 方法
    # 使得类变成一个可调用对象，即
    class Foo:
        def __init__(self,function):
            self.func=function				# 类可以存储数据
        
        def __call__(self):					# __call__()内可以对类的属性进行修改
    		# 代码块
            self.func()
            # 代码块
    @Foo
    def test():
        print('pass')
    
    test()
    
    # 这样的话，test其实变成实例化的类了
    ```

    

5. 内置装饰器

    ```python
    @staticmethod
    @classmethod
    @staticmethod
    ```

     To [@staticmethod 和 @classmethod](#@staticmethod 和 @classmethod)





> 1). jianshu](https://www.jianshu.com/p/ee82b941772a)
>
> 2). [from zhihu](https://www.zhihu.com/question/26930016)





#### 回调函数

【应用场景，函数库，框架】，事件处理器

> 常见的系统都会开发出很多库，库里面有很多函数。而有些函数，需要调用者根据自己的需求来写入要调用的函数。因为这个在编写库的时候没法预测，只能由调用者输入，所以就需要回调机制。

**回调函数就是一个通过函数指针调用的函数。如果你把函数的指针（地址）作为参数传递给另一个函数，当这个指针被用来调用其所指向的函数时，我们就说这是回调函数。回调函数不是由该函数的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应。**

**把一个函数作为参数传给另一个函数，第一个函数称为回调函数。**

```python
def f1(a,b):
    return a+b
def f2(a,b):
    return a-b

def F(a,b,fun):
    return fun(a,b)

if __name__ == "__main__":
    F(2,3,f1)
# 把函数当作参数调用，就是回调，被调者叫做回调函数
```

> 回调本质是将回调函数的指针传到调用者
>
> 在python中，则用对象引用来理解

```c++
#include<iostream>
using namespace std;
float sum(float a,float b){
    return a+b;
}
float sub(float a,float b){
    return a-b;
}
float computer(float a,float b,float (*fun)(float,float)){
    return fun(a,b);
}

int main(){
    float a;
    float b;
    cin >> a;
    cout << "--" << endl;
    cin >> b;
    printf("sum is %f,sub is %f",computer(a,b,&sum),computer(a,b,&sub)); 
}
```



###### 基础回调案例

```python
# 一个标准的基础回调函数
def apply_async(func,*args,callback):
	result=func(*args)
	callback(result)

# 测试
def add(a,b):
    return a+b
def print_result(result):
    print(result)
    
>>> apply_async(add,5,6,callback=print_restult)
11
```

缺点：只有一个参数可以传到回调函数,不能访问一些环境



###### 优化

【让回调函数访问其他变量或者特定环境的变量值】

基本思想：利用类或者闭包来保存变量

**基于类的优化**

```python
class result_handle(object):
	def __init__(self,value):
    	self.value=value
        self.code=1
    def handle(self,result):
        print('{},{},{}'.format(self.value,self.code,result))
        
>>> r=result_handle("value")
>>> apply_async(add,2,4,callback=r.handle)
value16
```

**基于闭包的优化**

```python
def result_handle():
	value=1
	def handle(result):
		print("handle {},value {}".format(result,value))
    def set_value(val):
        nonlocal value
        value=val
    return handle
>>> r=result_handle()
>>> apply_async(add,3,4,callback=r)
```

```python
# 完整例子
def add(*args):
    sum=0
    for i in args:
        sum += i
    return sum

def apply_async(func,*args,callback=None):
    result=func(*args)
    callback(result)


def result_handle():
    value=1
    def handle(result):
        print("handle {},value {}".format(result,value))
    def set_value(val):
        nonlocal value
        value=val
    handle.set_value=set_value
    return handle

# 或者
def result_handle2():
    value=1
    def handle(result)
    	nonlocal value
        print('value:{},result:{}'.format(value,result))
        value+=1
    return handle

if __name__=="__main__":
    r=result_handle()
    r.set_value(5)
    apply_async(add,3,4,6,callback=r)
    
# 或者
	handle=result_handle2()
    apply_async(add,'hello','world',callback=handle)

```

**基于协程的优化**

思路是将回调函数写成一个生成器函数，每次调用回调函数的时候，利用send()将值送到生成器里面，执行过程

```python
def handle():
	value=1
    while True:
        result=yield
        print(value)
        value+=1
        
>>> a=handle()
>>> next(a)
>>> apply_async(add,3,5,callback=a.send)
1 --- 8
>>> apply_async(add,3,3,callback=a.send)
2 --- 6
```



###### 内联回调

【学习配合 Twisted框架的学习】

【内联函数：inline function】

前面的缺点是，定义了很多小函数，随着代码量的增加，程序调用的控制流会很乱，所以使用内联函数，将控制流内联到一个函数，实现流程的序列化

```python
def apply_async(func,args,callback):
    print(*args)
    result=func(*args)
    callback(result)

def add(*args):
    if isinstance(*args[:1],str):
        sum=''
    elif isinstance(*args[:1],int):
        sum=0
    else:
        raise ValueError('not int or str')
    for i in args:
        sum += i
    return sum  

def sub(*args):
    if not isinstance(*args[:1],int):
        raise ValueError('not int')
    else:
        r=args[0]
        for i in args[1:]:
            r -= i
        return r

from functools import wraps
from queue import Queue

class Async:
    def __init__(self,func,*args):
        self.func=func
        self.args=args

def inline_function(fun):
    @wraps(fun)
    def wrapper(*args):					
        f=fun(*args)					# 初始化生成器
        result_queue=Queue()
        result_queue.put(None)
        while True:
            result=result_queue.get()
            try:
                r=f.send(result)		# generator.send()
                apply_async(r.func,r.args,callback=result_queue.put)
            except StopIteration as e:
                break
    return wrapper

@inline_function
def run():
    r=yield Async(add,3,4)
    print(r)
    r=yield Async(sub,3,2)
    print(r)

if __name__=="__main__":
    run()
```

> 队列，生成器，装饰器





#### 常用内建函数

- map() , reduce() 
    map( fn , [1,2,3,4] ) 将列表每个元素用fn处理，结果返回一个列表

    reduce( fx ,[1,2,3,4] ) 迭代累计叠加

- filter()，[过滤元素](#过滤元素)

- sorted()

    sorted( list[] , key=)   `key`函数来实现自定义的排序,这个函数作用于每个元素

- enmurator()、

    将一个可遍历对象组合成一个索引序列，同时列出数据和数据下标

- getattr(object , name , default])

    用于返回对象的属性值

    不设置default，则如果对象o的属性name不存在，返回一个AttributeError

    设置default,则返回default。

    ```python
    a=A()
    >>> getattr(a,'bar',3)
    3
    ```

    

#### 现代化的python

##### 类型提示

就像javascript发展成typescript一样，类型提示的缺失使得语言本身被迫产生了变种，我不是很了解最新的js的语言规范的发展，但是从对于原始的js学习以及es6的捎带了解之中，我发现js是真的不完美。（对比python）

对比js，python的完美程度要高的多得多。（当然这里默认不考虑py2，就完美程度来说，现在的js有点像py2，个人理解）。我做大胆预测，过几年，python底层完全把GIL锁这个东西完美过度之后，python将变成动态语言之王。（不接受反驳[狗头]）

---

python的【类型提示】是3.6+加入的

```python
def func_name(name: str,age: int = None) -> str :
	#
    return f'{name} {age}'
```

普通类型

- int
- float
- bool
- bytes

嵌套类型

- dict

    ```python
    from typing import Dict
    def test(dicnamr: Dict[str,float]):
    	# Dict[键类型，值类型]
    ```

- list

    ```python
    from typing import List
    def test(items: List[str]):
       #
    ```

- set

- tuple

类

- ```python
    class Person:
    	def __init__(self,name: str):
    		self.name = name
    		
    def test(one_person: Person):
    	return one_person
    ```

> python的 类型提示 只是提示，由于py的动态性，python程序执行过程中，类型不符合并不影响程序执行



##### Pydantic:真正的类型校验

> 执行数据校验的库



