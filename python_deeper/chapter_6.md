

> python cookbook部分内容老旧，很多用法在现代版本的python已经不再适用，and有些问题也被后续优化解决。

### 一，数据结构

#### 内建数据结构：

```
基本数据结构list[1,2,3]dict{num1:1,num2:2}tuple(1,2,3)set无序不重复元素集set(),frozenset()
```

> ###### 操作方法

```
# list:                 Sequence Types          可变类型
list1=["test",2,4]              # 初始化
list1[0]                        # 索引
slice1=list1[1:]                # 切片
slice2=list1[1:5:2]             # 从1到5按步数2摘出
list1.index('2')                # 查询
list1[1]=5                      # 修改
list1.append('new')             # 追加元素
list1.insert(1,'insert1')       # 插入元素
del list1[1]
list1.remove['test']
list1.pop()                     # 删除元素
list1.extend(['add1','add2'])   # 拓展列表
list1.count('test')             # 统计
list1.sort()                    # 排序  
list1.reverse()                 # .sort(reverse=True)
list2=list1.copy()              # copy
                                # 深拷贝，浅拷贝的区别---import copy
list1=list(set(list1))          # 用set()实现 去重
list2=[]
[list2.append(i) for i in list1 if i not in list2]  # 去重
if 'a' in list:
if 'a' not in list:

# dict:                                     可变类型
dict1={'a':1,'b':2,'c':3}       # 初始化
dict1.keys()            >>> dict_keys(['a', 'b', 'c'])
dict1.values()          >>> dict_values([1, 2, 3])
dict1.items()           >>> dict_items([('a', 1), ('b', 2), ('c', 3)])
list(dict1.keys())      >>> ['a', 'b', 'c']
dict2=dict1.copy()              # copy 此处既有深拷贝也有浅拷贝
dict1.update({'b':4,'d':5})     # b的键值更新，新加d
dict1.pop('c')                  # 删除指定键值对
dict1.popitem()                 # ”随机“删除键值对,本质删除顺序存储最后一键
dict1.setdefault('new1',8)      # new1不存在则，加入new1并给值8
dict1.setdefault('new2')        # 默认是None，加入new2赋None
dict1.setdefault('a',10)        # a存在，则返回a原来的值，不会覆盖指定的值

# tuple:                Sequence Types      不可变类型
tuple1=tuple1+tuple2
# 元组中的元素不可以更改，即不能通过tuple[2]=a,来修改元素，但是元组内可以放任意数据结构，即如果元素是列表，那列表内的元素是可以改变的。
del tuple1
len(tuple1)
# 其他用法，同sequence types通用用法
# 比较两个元组的元素，可以导入operator模块

# set:
# 创建方法
set1=set()                              # 可以用来创建空集合
set2={'a','b'}                          # 不能用来创建空集合
in / not in
a,b=set('slice'),set('test')
c=a-b                                   # a含b不含
c=a|b                                   # a含或b含
c=a&b                                   # a且b
c=a^b                                   # 不同时含于ab的集合
c={x for x in a if x not in b}
a.add()                                 # 添加元素
a.update()                              # 添加元素，其他数据结构
a.remove(x)
a.discard(x)                            # 删除元素
a.pop()                                 # “随机”删除，存储顺序出栈
len(a)
a.clear()                               # 清空集合

```

###### Sequence Types 通用操作

> - in / not in
> - 连接 +
> - 复制 *
> - 下标取值s[i]
> - 切片
> - 长度检测 len(s)
> - 最大最小值，max(),min()
> - 索引取值 .index(i)
> - 字符串统计  .count(i)

###### 深 / 浅拷贝

> - 深拷贝：拷贝所有层的结构
> - 浅拷贝：只拷贝最外层一层结构，里面的结构与原结构共享，故浅，改变原结构，会影响copy生成的结构

###### 哈希-字典的优化

> - 字典的实现依靠哈希表，所以字典这种数据结构会比较占用存储空间，但是访问速度更好
>
> - python3.6之前的字典实现中，会对key进行一次非传统意义的hash计算，hash值随内容和时间变化，即” Python自带的这个 hash 函数计算出来的值，只能保证在每一个运行时的时候不变 “
>
>     ~python3.6之前的字典，本质是初始化一个二维哈希表，对应长度是容纳数据需要的行数(有自动扩容机制，每次扩容，重新构建字典)，和，存储哈希值，指向key的指针，指向value的指针，这3列~
>
>     ~上面的实现方法，占用空间较大，哈希表散列性质~
>
>     python3.6~的优化，改用一个一维散列表和一个二维数组来实现，计算hash值，对哈希表目数（此处是一位散列表的长度）求余，得到在表二中保存的位置下标，将[hash值,指向key的指针,指向value的指针]保存到二维数组中，并在一维表中对应下标位置，存入此时对应的元素序号~
>
>     ~由于存入二维数组中时，一维列表保存了存入的先后顺序（序号），所以此时字典是有序的，并且，这种实现方法下，整体上更省内存，再者，遍历字典时候，直接遍历表二就好，加上表二较短，整体提高了效率~

###### 堆排序 补充

> - 模块： heapq
>     - heapq.heapify(list)
>     - heapq.heappop(list)        #出堆
>     - heapq.heappush(list)      #入堆

###### 字典补充

> 直接对字典进行数学运算，只会作用于键
>
> - 一种常用操作，找字典中值的最值的键对
>
>     ```
>     # zip() 本质迭代器，只生效一次
>     ```
>
>     ```
>     p=min(zip(price.values(),price.keys()))     #利用zip()反转key                                               和value
>     ```
>
>     ```
>     # 上面方法，在值相同时，键的比较结果决定返回值
>     ```
>
>     ```
>     
>     ```
>
>     ```
>     # 利用min(),max()中提供的key函数
>     ```
>
>     ```
>     min(dict1 ,key=lambda k: dict1[k])      # key通过lambda遍历了所有的键,迭代器
>     ```
>
>     [click to zip()迭代器](#zip()迭代器)
>
> - 删除指定键构造新字典
>
>     ```
>     c={key:dict1[key] for key in dict1.keys() - {'z','w'}}
>     ```
>
>     ```
>     # 利用生成器，按字典的key进行差预算
>     ```
>
>     字典的键也是可以进行并差交运算的



#### collections拓展模块

- defaultdict			--> 多值字典

    ```
    from collections import defaultdict
    ```

    ```
                                # 构造一对多字典
    ```

    ```
    d=defaultdict(list)         # 亦可以是元组
    ```

    ```
    d['a'].append(1)
    ```

    ```
    d['b'].append(2)
    ```

    ```
    
    ```

    ```
    # 此处对标一对一标准字典中.setdefault('key',value)方法，添加键对时，无则创建赋值
    ```

    [应用](#根据字段将记录分组)

- ~~OrdereDict~~ 

    ```
    # 控制字典中元素的顺序
    ```

    ```
    from collections import OrderedDict
    ```

    ```
    
    ```

    ```
    d=OrderedDict()
    ```

    ```
    d['a']=1
    ```

    ```
    d['b']=2
    ```

    ```
    
    ```

    ```
    # 这样的字典会保持插入时的顺序        本质双向链表
    ```

    ps，python3.6之后，由于字典的底层结构的更改，自动生成的字典就是有序的

    再者，OrdereDict这个方法，由于维护双向链表，所以大小是正常字典的二倍，加之3.6的优化，这种方法已经被淘汰了

- deque

    ```
    # 队列
    ```

    ```
    from collections import deque
    ```

    ```
    q = deque(maxlen=5)                 # 不指定，默认无长度限制
    ```

    ```
    q.append(2)                         
    ```

    ```
    q.appendleft(4)                     >>> deque([4,2],maxlen=5)
    ```

    ```
    q.pop()                             >>> deque([4],maxlen=5)
    ```

- ChainMap

- Counter

    ```
    # 找出序列中出现次数最多的元素
    ```

    ```
    from collections import Counter
    ```

    ```
    words=['a','b','c']
    ```

    ```
    word_counts=Counter(words)
    ```

    ```
    top_three=word_counts.most_common(3)
    ```

    ```
    # top_three=[('a',1),('b',1),('c',1)]
    ```

    

- namedtuple                   --> 命名元组

    使用方法

    ```
    from collections import namedtuple
    ```

    ```
    Stock=namedtuple('Stock',['name','shares','price'])
    ```

    ```
    s=Stock('ACME',100,123.23)
    ```

    ```
    # 元组的值无法修改，所以得用重写一个元组
    ```

    ```
    s= s._replace(shares=75)
    ```

    通用思路

    ```
    # 构筑命名元组原型，通过_replace()初始化
    ```

    ```
    Stock=namedtuple('Stock',['name','shares','price','date'])
    ```

    ```
    stock_list=Stock('',0,0.0,None) #实例原型
    ```

    ```
    def dict_to_stock(s):
    ```

    ```
        return stock_list._replace(**s)     #将字典导入
    ```

    [*&** 用法](#动态参数:*与**)

    以往的优点是作为字典的替代品，使用命名元组会占用更少的空间，助于摆脱下标解码，提升可维护，



#### queue 

【多线程编程，任务队列，[内联回调](#内联回调)】

提供两种队列对象，FIFO > Queue , LIFO > LifoQueue , PriorityQueue(优先级队列) ，deque(双向队列)

```
# Queue
from queue import Queue
test=Queue(maxsize=10)
# test=LifoQueue(maxsize=5)
# test=PriorityQueue(maxsize=5)
exp=deque

#对于前三种
test.put(item)
test.get()      # remove and return one item
test.empty()    # empty return True,else False
test.full()     # if full return True,else Flase
test.qsize()    # return size of test
# 
test.get_nowait()   # 等价于 test.get(Flase) 非阻塞方法
test.task_done()    # 完成一项工作后，给已经完成的队列发送一个信号
                    # test.get()调用得到任务
                    # test.task_done()告诉队列已经处理完
test.join()         # 等到队列为空，再执行别的操作  -> 对比思考一下 线程/进程池中的join() ->本质阻塞进程

#对于双边队列deque
exp.append(item)
exp.appendleft(item)
exp.pop()
exp.popleft()

```





#### 变量操作

##### 解压序列

> 解压序列不仅可以用在列表和元组，亦可以用在字符串，文件对象，迭代器和生成器

- 解压部分变量时，可以使用占位变量名

    ```
    >>> data=[1,2,3,4]
    ```

    ```
    >>> _,a,_,b=data
    ```

    ```
    >>> _
    ```

    ```
    3
    ```

    ```
    >>> b
    ```

    ```
    4
    ```

- 星号-匹配元素

    ```
    >>> first,*middle,last=data
    ```

    ```
    >>> middle
    ```

    ```
    [2,3]
    ```

    ```
    # 分割字符的一种新方法，简单的场景可以取代正则
    ```

    ```
    >>> line="··· name :test: where : dir :/var/test"
    ```

    ```
    >>> _,name,*_,dir=line.split(":")
    ```

    ```
    # 元组结合的案例
    ```

    ```
    >>> record=[2,3,3,(4,5,6)]
    ```

    ```
    >>> *_,num1,(*_,num2)=record
    ```

    

##### 索引切片

> 个人用索引切片会更好

```
>>> data=[2,3,4,5,6,7]
>>> data[2:]
[4,5,6,7]
>>> data[-2:]
[6,7]
>>> data[::2]           # 步进器
[2,4,6]
>>> data[::-1]
[7,6,5,4,3,2]
>>> data[::-3]
[7,4]
```

##### 补充：切片对象

```
a=slice(2,10,2)
a.start
a.stop
a.step
# 消除硬编码，创建切片对象，使得代码可维护性提升
record="......sss.sss."
get=record[a]
```





#### 过滤元素

##### 删除序列相同元素

1. 不需要保持顺序（不稳定方法）

    ```
    a=[1,6,3,4,1]
    ```

    ```
    a1=[{'a':1,'b':2},{'a':1,'b':3}]
    ```

    ```
    b=list(set(a))                          # 集合
    ```

2. 保持顺序（稳定）

    ```
    # 构造生成器
    ```

    ```
    
    ```

    ```
    # 一般序列
    ```

    ```
    def dedupe(items):
    ```

    ```
        seen=set()
    ```

    ```
        for item in items:
    ```

    ```
            if item not in seen:
    ```

    ```
                yield item
    ```

    ```
                seen.add(item)
    ```

    ```
    # 含有字典等hash的序列结构
    ```

    ```
    def dedupe2(items,key=None):
    ```

    ```
        seen=set()
    ```

    ```
        for item in items:
    ```

    ```
            var=item if key is not None key(item)   # key需要传入一个函数
    ```

    ```
            if var not in seen:
    ```

    ```
                yield item
    ```

    ```
                seen.add(var)
    ```

    ```
    
    ```

    ```
    b=list(dedupe(a))
    ```

    ```
    b1=list(dedupe(a1,key=lambda d:(d['a'],d['b'])))
    ```

##### 过滤元素

- 列表推导

    [ f(x) for x in list if x is not None ]

- 生成器表达式，迭代产生

    pos=( n for n in list if n > 0 )

    此时的pos是一个生成器

    ```
    >>> for x in pos:
    ```

    ```
    ···     print(x)
    ```

    ```
    ···
    ```

    ```
    1
    ```

    ```
    4
    ```

    ```
    10
    ```

    ```
    >>> 
    ```

- filter( ) 过滤

    ```
    #filter()本质创建一个迭代器
    ```

    ```
    values=['1','2','no','5','a']
    ```

    ```
    def is_int(var):
    ```

    ```
        try: 
    ```

    ```
            x=int(var)
    ```

    ```
            return True
    ```

    ```
        except ValueError:
    ```

    ```
            return False
    ```

    ```
    
    ```

    ```
    ivals=list[filter(is_int,values)]
    ```

    ```
    # 因为是迭代器，所以这块用list接收值
    ```





#### 实用字典序列操作

##### 通过某关键字排序字典列表

operator 模块 itemgetter( )			更快的方法

```
rows=[
    {'fname':'test1','lname':'test2','uid':1},
    {'fname':'test11','lname':'test22','uid':2},    
    {'fname':'test1','lname':'test22','uid':3},
]
from operator import itemgetter
rows_by_fname=sorted(rows,key=itemgetter('fname'))
rows_by_uid=sorted(rows,key=itemgetter('uid'))
```



传统方法,lambda自定函数

```
row_by_fname=sorted(rows,key=lambda x: x['fname'])
```

> 补充：对对象排序  [引申概念点]()
>
> ```
> class User:
>     def __init__(self,user_id):
>         self.user_id=user_id
>     
>     def __repr__(self):
>         return 'User({})'.format(self.user_id)
> def sort_class():
>     users=[User(23),User(2),User(4)]
>     sorted_by_user_id=sorted(users,key= lambda k: k.user_id)
> 
>  # or 采用attrgetter()模块
>     from operator import attrgetter
>  sorted_by_user_id_2=sorted(users,key=attrgetter('user_id','first_name'))
>  # attrgetter支持多参数排序
> ```

##### 根据字段将记录分组

itertools.groupby( ) 模块

```
# extern rows[]
from operator import itemgetter
from itertools import groupby
rows.sort(key=itemgetter('fname'))
for fname,items in groupby(rows,key=itemgetter('fname')):
    print fname
    for i in items:
        print(' ',i)
# groupby扫描序列，查找 连续 相同值  ，返回 一个值 和 一个迭代器对象
```

利用[多值字典](#collections拓展模块)实现

```
from collections import defaultdict
rows_by_fname=defaultdict(list)
for row in rows:
    rows_by_fname[row['fname']].append(row)
    
# 返回的数据结构
>>> print(rows_by_fname)
>>> defaultdict(<class 'list'>, {'test1': [{'fname': 'test1', 'lname': 'test2', 'uid': 1}, {'fname': 'test1', 'lname': 'test
22', 'uid': 3}], 'test11': [{'fname': 'test11', 'lname': 'test22', 'uid': 2}]})
                                                                     
for i in rows_by_fname['test1']:
    print(i)
```

















### 二，字符串文本操作

#### 编码问题（py3）

**本质上来说，编码和解码就是Python中str和bytes这两种字符串类型之间的互相转换**

python3默认字符是Unicode

str	： 包含Unicode字符，有一个方法是 encode()，转化成bytes类型

bytes   ：8位值，方法是 decode() ，转化成str类型

```
s='测试'
b=s.encode('utf-8')
>>> b
b'\xe6\xb5\x8b\xe8\xaf\x95'

# 本质是按utf-8

s= b.decode(encoding='utf-8')
>>> s
'测试'
```



参考    [知乎 张雨萌的回答](https://www.zhihu.com/question/26921730)

​			[unicode相关](https://blog.csdn.net/weixin_42232219/article/details/89369259)



#### 字符串拆分拼接

##### 拆分

- 基础方法

    ```
    'a,b,c,d'.split(',')
    ```

    ```
    >>> ['a','b','c','d']
    ```

- 进阶  re.splite()模块

    ```
    import re
    ```

    ```
    line='test,est;ddd,eee fff'
    ```

    ```
    
    ```

    ```
    >>> re.split(r'[;,\s]',line)                    # 按;, \s(空格) 分割
    ```

    ```
    >>> re.split(r'([;])',line)                     # 把;提取出来， '前面部分',';','后面部分'
    ```

    ```
    >>> re.split(r'(?:[;])',line)                   # 不保留';', '前面部分','后面部分'
    ```

##### 拼接

- ```
    ''.join(list)
    ```

- ```
    string1 + ' ' + string2
    ```

> 使用加号 (+) 操作符去连接大量的字符串的时候是非常低效率的，因为加号连接会引起内存复制以及垃圾回收操作。
>
> ```
> # 错误示范
> s = ''
> for p in parts:
> s += p
> ```

> ```
> # 比较好的写法
> data=['test',26,'test2',65]
> ','.join(str(x) for x in data)
> ```

> IO操作和字符串拼接时的性能考量
>
> ```
> f.write(string1+string2)
> 
> f.write(string1)
> f.write(string3)
> ```
>
> 性能考量:  当s1，s2较短时，+引起的临时空间较小，合并时间开销也小，此时相较于io性能考虑，第一钟比较好。反正s1，s2较大时，后者更好。



#### 字符串匹配查找与替换

##### 简单匹配

- 切片匹配，缺点是硬编码

    > 一种解决方案，采用切片对象，消除硬编码 ，[跳转](#补充：切片对象)

- 一些基础函数

    ```
    # 开头结尾匹配
    ```

    ```
    strname.startswith('match.what')
    ```

    ```
    strname.endswith('match.what')
    ```

    ```
    strname.startswith(('http:','https:','ftp:'))       # 元组
    ```

    ```
    
    ```

    ```
    # shell通配符
    ```

    ```
    from fnmatch import fnmatch,fnmatchcase
    ```

    ```
    >>> fnmatch('a.txt','*.txt')                # 大小写敏感取决于系统底层
    ```

    ```
    True
    ```

    ```
    >>> names = ['Dat1.csv', 'Dat2.csv', 'config.ini', 'foo.py']
    ```

    ```
    >>> [for name in names if fnmatchcase(name,'Dat')]      # 严格区分大小写
    ```

    ```
    ['Dat1.csv', 'Dat2.csv']
    ```

##### 正则匹配

```
import re
re.compile:编译正则表达式字符串
re.match:总是从字符串开始去匹配
re.findall:一次匹配所有,返回列表
re.finditer:迭代方式返回
```

- 将 模式字符串 预编译成 模式对象    [匹配过程重复使用时]

    ```
    datepat=re.compile(r'(\d+)/(\d+)/(\d+)$')
    ```

    > r' ' 用法：匹配字段前加r之后，原字符串不需要解析\，，没r需要 \ \ 双反斜杠

    > 正则：
    >
    > ​		+：匹配1次或者多次
    >
    > ​		*：匹配0次或者多次
    >
    > ​		?：非贪心，只匹配一次 或者0次
    >
    > ```
    > (.*) 贪婪匹配最长             (.*?) 非贪婪 最短匹配
    > ```
    >
    > ```
    > # 实现换行匹配
    > ```
    >
    > ```
    > comment=re.compile(r'/\*(.*?)\*/') # 失败例子
    > ```
    >
    > ```
    > comment=re.compile(r'/\*((?:.|\n)*?)\*/') # (?: 点或者\n) 因为.不能匹配换行 
    > ```

- 匹配

    ```
    m=datepat.match('string')
    ```

    ```
    m=datepat.findall('string')
    ```

    ```
    m=datepat.finditer('string')
    ```

    ```
    # 直接使用的情况
    ```

    ```
    m=re.match(r'(\d+)/(\d+)/(\d+)$',text)
    ```

##### 替换和删除

###### 替换

```
# 简单替换
text='string test'
text.replace('test','succeed')
# re中的sub()
text = 'Today is 11/27/2012. PyCon starts 3/13/2013.'
>>> re.sub(r'(\d+)/(\d+)/(\d+)', r'\3-\1-\2', text)
'Today is 2012-11-27. PyCon starts 2013-3-13.'
# 回调函数
具体遇到再说，目测短期不会碰到这种需求
```

###### 删除

```
' string '.strip()
' string '.lstrip()
' string '.rstrip()
'---string===='.strip('-')
```

###### 字符串审查清理

```
# 基础操作
str.upper()
str.lower()
str.replace('A','') or str.sub()

# 正式用法
str.translate(remap)
# 创建remap字典
remap={                                 # 手动
    ord('\t'):' ',
    ord('\r'):None,
    ord('A'):'B'
}

import unicodedata,sys
remap=dict.fromkeys(c for c in range(sys.maxunicode) if unicodedata.combining(chr(c)))
```



#### 格式化输出

```
'{name} got {m} messages'.format(name='jack',m=10)
```

> 拓展   .format_map() 用于搭配 var( ) 接收变量域

3.6之后推荐以下

```
varname = 'test'
f'this is the string {varname}'
```





#### html & Xml 处理

相关库补充



#### 令牌流解析

```
正则表达式定义所有令牌
scanner方法
打包到生成器函数
```

#### 递归下降解析器 补充 编译原理相关-解析器编译器原理实现

```
获得所有的语法规则
将其转换为一个函数或者方法
```

推荐解析工具 PyParsing 或者 PLY









### 三，数字处理

#### 进制转换

```
>>> bin(x)
>>> oct(x)
>>> hex(x)
```

#### Numpy



#### random

```
import random
random.choice([1,2,3,4])                    # 随机单取一值
random.sample([1,2,3,4],2)                  # 返回[2,3]
random.shuffle([1,2,3,4])                   # 打乱顺序
random.randint(0,20)                        # 0~20随机数
random.random()                             # 0~1随机浮点数
# random默认是确定性算法，可以通过 random.seed(1231) 修改初始化种子
```

















### 四，文件 与 IO

##### 文件读写

```
# 传统方式
f=open('path/to/file.txt','w')
data=f.read()
k=f.readline()
f.close()

# 比较好的写法
with open('path/to/file.txt','w') as f:
    pass
# 好处：使用结束自动关闭文件
```

> 补充，换行符在不同系统中是不同的
>
> 但是python中的换行符是统一的 `\n`
>
> linux -> `\n`   windows -> `\r\n`

- 文本文件      rt		wt		at
- 读写二进制文件  		rb		wb
- 禁止覆盖写       xt      xb

> 直接将字节写入文本文件
>
> ```
> import sys
> ✖ sys.stdout.write()
> ✔ sys.stdout.buffer.write(b'test') 
> # 直接写入buffer即可
> ```



##### 补充 ，with 上下文管理

当出现 with语句的时候，对象的 **enter**() 方法被触发，它返回的值 (如果有的话) 会被赋值给as 声明的变量。然后，with 语句块里面的代码开始执行。最后，**exit**() 方法被触发进行清理工作。

```
让对象支持上下文管理
class Test:
    def __init__(self):
        pass
    
    def __enter__(self):
        
        pass                        #
    
    def __exit__(self):
        what.close()
        pass                        #
```









##### 压缩文件处理

```
import gzip
import bz2
with gzip.open('file.gz','rt') as f:
    text=f.read()
    f.write(text)
with bz2.open('file.bz2','rt') as f:
    text=f.read()
    f.write(text)
```



##### 路径名

创建文件或者目录时候要注意权限问题

python3.4之前

```
import os
path='/path/test/to/file.csv'
os.path.basename(path)              # file.csv
os.path.dirname(path)               # /path/test/to
os.path.join('new','path','to',os.path.basename(path))
# '/new/path/to/file.csv'

# 路径存在与否
os.path.exists('/etc/test')
# 创建目录
os.mkdir('test')                # 生成一级目录
os.makedirs()                   # 多级目录

# 获取文件夹文件列表
os.listdir('/etc/test')
[name for name in os.listdir('/etc/test') if                   os.path.isfile(os.path.join('/etc/test',name))]
[name for name in os.listdir('/etc/test') if name.endswith('.py')]
# 使用os.listdir()获取目录下的信息
```

现在推荐的路径处理方案（3.6之后使用）

```
from pathlib import Path            # 面向对象式
p=Path('/path/test/to/file.csv')        
# PosixPath('/path/test/to/file.csv')   表明是一个PosixPath对象

# 文件名字
p.name
p.suffix                    # 后缀
p.suffixs                   # 后缀们
p.stem                      # 文件名字
# 获取父级目录属性
p.parents[0]                # /path/test/to
p.parents[2]                # /path
p.parent                    # /path/test/to
p.parent.parent             # /path/test
# 目录拼接
Path('/').joinpath('path','test/test')          # PosixPath('/path/test/test')
Path('/')/'home'/'test/test'
Path('home') / 'dongwm/code'                    # PosixPath('home/dongwm/code')
# 路径检测
p.is_file()
p.is_dir()
# 创建目录
Path('1/2/3').mkdir()       # 可以直接创建多级目录
# 获取目录列表/获取文件列表
[x for x in p.iterdir() if x.is_dir()]
[x for x in p.iterdir() if x.is_file()]
[x for x in p.iterdir() if x.endswith('.py')]
[x for x in p.glob('**/*.py')]
# 路径替换
p.with_name('replace.name')
p.with_suffix('.pyc')
p.with_stem('name')
# Path对象自带文件操作方法

```



参考 [python之美-你该用pathlib代替os.path](https://zhuanlan.zhihu.com/p/87940289)



##### StringIO和ByteIO

受限目前的接触面，等有了使用场景，再深入使用



##### 串行端口通信

```
import serial
```

> 具体有了应用场景再深入



##### 序列化对象

###### pickle

```
import pickle

data=python对象
f=open('file','wb')
pickle.dump(data,f)                 # dump进文件
f=open('file','rb')
data=pickle.load(f)                 # 从文件恢复对象

s=pickle.dumps(data)                # 对象转成字符串
data=pickle.loads(s)                # 字符串转成对象

```

> 千万不要对不信任的数据使用 pickle.load()。
>
> pickle 在加载时有一个副作用就是它会自动加载相应模块并构造实例对象。
>
> 但是某个坏人如果知道 pickle 的工作原理，
>
> 他就可以创建一个恶意的数据导致 Python 执行随意指定的系统命令。
>
> 因此，一定要保证 pickle 只在相互之间可以认证对方的解析器的内部使用。

```
# 依赖外部环境的一些比较特殊的对象，不能被序列化
# 如打开的文件，网络连接，线程，进程，栈帧等等
# 解决方法，提供__getstate__() 和 __setstate__() 方法
# dump调用__getstate__()获取序列化的对象，反序列化调用__setstate__()

# 例子 带有线程的类
import time
import threading
class Countdown:
    def __init__(self,n):
        self.n=n
        self.thr=threading.Thread(target=self.run)
        self.thr.daemon=True
        self.thr.start()
    def run(self):
        while self.n>0:
            print('T=',self.n)
            self.n-=1
            time.sleep(4)
    def __getstate__(self):
        return self.n
    
    def __setstate__(self,n):
        self.__init__(n)
```

> pikle的一种使用场景是python内部处理多进程的来进行密集型任务处理，celery使用了pickle，但是貌似现在也不推荐使用了，转成了json。安全性问题。
>
> 一种多进程方式是分成master进程和woker进程，master做调度，worker做计算。因为进程间是隔离的，所以使用pickle做 任务对象 的传递
>
> [from 知乎](https://www.zhihu.com/question/38355589)



> 在数据库和存档文件中存储数据时，你最好使用更加标准的数据编码格式如 XML，CSV 或 JSON
>
> 由于 pickle 是 Python 特有的并且附着在源码上，所有如果需要长期存储数据的时候不应该选用它。例如，如果源码变动了，你所有的存储数据可能会被破坏并且变得不可读取。

[此处跳转到django中序列化的模块深入解决]()

###### json

| json 类型  | python 类型  |
| ---------- | ------------ |
| {}         | dict         |
| []         | list         |
| "string"   | str          |
| 123.34     | int or float |
| true/false | True/False   |
| null       | None         |

常见数据类型的序列化和反序列化

```
import json

d = dict(name='Bob', age=20, score=88)
json_str=json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'

json.loads(json_str)
{'age': 20, 'score': 88, 'name': 'Bob'}
```

类的序列化/反序列化方案

```
# 序列化
import json
class test:
    def __init__(self,name,age):
        self.name=name
        self.age=age
m=test('jack',20)
json_string=json.dumps(m.__dict__)

# 反序列化
import json
class test:
    def __init__(self,name,age):
        self.name=name
        self.age=age
def get(string):
    return test(string['name'],string['age'])
n=json.loads(json_string,object_hook=get)           # 钩子函数
>>> n
<__main__.test object at 0x7f9ef3c74190>
>>> n.name
'jack'
>>> n.age
20
```









参考  [廖雪峰的python3教程](https://www.liaoxuefeng.com/wiki/1016959663602400/1017624706151424)



### 补充 ：is 和 ==

- **is** : 同一性算符，比较对象间的唯一身份标识
- **==** ：比较值是否相等

> Python中引用的思想：
>
> ​		python中万物皆对象，
>
> ​		变量是对内存中对应值这个对象的引用，当引用为0时，内存释放
>
> ```
> a=[1,2,3]
> b=[1,2,3]
> a==b
> True
> a is b
> False
> ```













### 五，函数



#### 函数带默认参数

```
def test(a,b=10):
    print('b默认执行的时候参数值是10')
```



#### 动态参数:*与**

> 在形参前面加上*和**，成为动态参数

加*，函数可以接收任意多个参数，全部放进一个元组中

```
def fun(*args):
    print(args)

F(123,"542")
# (123,'542')
```

加**，函数接收参数，传入的格式是字典

```
def fun(**kwargs):
    print(kwargs)

F(k1=123,k2="543")
# {'k1':123,'k2':'543'}
```

再者，对于实参是列表和字典的情况

```
def F(*args,**kwargs):
    print(args,kwargs)
li=[2,3,1,4]
di={'k1':1,'k2':2}
F(li,k=di)
# ([2,3,1,4]) , {'k':{'k1':1,'k2':2}}
F(*li,**di)
# (2,3,1,4) , {'k1':1,'k2':2}
# ps 输进来的**kwargs
# 如果直接print(**kwargs)       # TypeError: 'key1' is an invalid keyword argument for print()
# 如果print(*kwargs)          # 返回的是 key1 key2  键

# 所以本质上来说 *解包 一维数据结构    **解包 二维数据结构
# 对字典进行解压
# *dict         -> 对应的键
# **dict        -> 对应的键值对   （不能直接使用，可以在函数中参数传入使用）
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

```
y=lambda a,b:a+b
```

使用场景

```
sorted(list_of_names,key=lambda name: name.split()[-1].lower())

min(dict1 ,key=lambda k: dict1[k])  
```

匿名函数的变量捕获问题

```
a=10
y=lambda b:a+b
a=20
k=lambda b:a+b
y(10)
k(10)
# 最终 y k均为30，因为函数执行时候,a是一个动态获取的变量，运行的时候 才会绑定值
# 解决方法
y= lambda b,a=a : a+b
# 思路是在定义的时候 就把值写入成函数的局部变量

# 案例
funcs = [lambda x: x+n for n in range(5)]
funcs = [lambda x,n=n: x+n for n in range(5)]
```



#### 闭包

> ✔ python cookbook3这块用 "将单方法的类转换成函数" 这种思想引出了闭包的概念
>
> 很nice

```
class test:
    def __init__(self,outside_value):
        self.value=outside_value
    
    def run(self,inter_value):
        return outside_value+inter_value
    
>>> a=test(20)
>>> a.run(10)
```

转成闭包

```
def test(outside_value):
    def run(inter_value):
        return outside_value+inter_value
    return run                                      # 注意这块返回的是函数  run()则是执行,变成内部定义，内部嵌套了
>>> a=test(1)
>>> a(2)
3
```

> 闭包实质是将一个内部函数返回给外层函数的调用者，内部函数可以记忆当前定义环境时外部函数给他提供的变量等外部环境，也是一个局部环境。
>
> 闭包就是引用了自有变量的函数，这个函数保存了执行的上下文，可以脱离原本的作用域独立存在。
>
> 其实闭包不好理解的时候，将他返回到类的角度，就解决了

【ps 获取闭包内部的变量】

```
def test():
    value=10
    def run(n):
        return value+n
    def get_value():
        return value
    def set_value(x):
        nonlocal value                  # nonlocal使得内部变量value可以被修改
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

    ```
    def decorator(fun):
    ```

    ```
        def foo(*args,**kwargs):
    ```

    ```
            print(kwargs):
    ```

    ```
            fun(*args)
    ```

    ```
        return foo
    ```

    ```
    
    ```

    ```
    def function(test):
    ```

    ```
        print(test)
    ```

    ```
    function=decorator(function)            # 原理上的掉用方法
    ```

    ```
    >>> function('args',key=1)
    ```

    ```
    {'key':1}
    ```

    ```
    'args'
    ```

    ```
    
    ```

    ```
    # 使用 @
    ```

    ```
    @decorator
    ```

    ```
    def function(test):
    ```

    ```
        print(test)
    ```

    ```
    >>> function('args',key=1)
    ```

    ```
    {'key':1}
    ```

    ```
    'args'
    ```

    ```
    
    ```

    ```
    # 起到包装函数的作用
    ```

2. 带参数的装饰器

    ```
    def outsidefun(value):
    ```

    ```
        num=value
    ```

    ```
        def decorator(fun):
    ```

    ```
            def insidefun(*args):
    ```

    ```
                fun(num)
    ```

    ```
                fun(*args)
    ```

    ```
            return insidefun
    ```

    ```
        return decorator
    ```

    ```
    @outsidefun(1)
    ```

    ```
    def test(n):
    ```

    ```
        print(n)
    ```

    ```
    
    ```

    ```
    >>> test(2)
    ```

    ```
    1
    ```

    ```
    2
    ```

    ```
    # 本质就是装饰器加装了一个闭包
    ```

3. 装饰器的一个问题

    简单包装后，函数的元信息变成了insidefun的信息，出问题了

    【解决方法 functools.warps】

    作用，将原函数的元信息拷贝到新的装饰器函数

    ```
    from functools import wraps
    ```

    ```
    
    ```

    ```
    def outsidefun(value):
    ```

    ```
        num=value
    ```

    ```
        def decorator(fun):
    ```

    ```
            @wraps(fun)
    ```

    ```
            def insidefun(*args):
    ```

    ```
                fun(num)
    ```

    ```
                fun(*args)
    ```

    ```
            return insidefun
    ```

    ```
        return decorator
    ```

    ```
    @outsidefun(1)
    ```

    ```
    def test(n):
    ```

    ```
        '''test.doc'''
    ```

    ```
        print(n)
    ```

    ```
        
    ```

    ```
    >>> print(test.__name__,test.__doc__)
    ```

    ```
    test test.doc
    ```

4. 类装饰器 

    [跳转](#默认父类object)

    ```
    # __call__() 方法
    ```

    ```
    # 使得类变成一个可调用对象，即
    ```

    ```
    class Foo:
    ```

    ```
        def __init__(self,function):
    ```

    ```
            self.func=function              # 类可以存储数据
    ```

    ```
        
    ```

    ```
        def __call__(self):                 # __call__()内可以对类的属性进行修改
    ```

    ```
            # 代码块
    ```

    ```
            self.func()
    ```

    ```
            # 代码块
    ```

    ```
    @Foo
    ```

    ```
    def test():
    ```

    ```
        print('pass')
    ```

    ```
    
    ```

    ```
    test()
    ```

    ```
    
    ```

    ```
    # 这样的话，test其实变成实例化的类了
    ```

    

5. 内置装饰器

    ```
    @staticmethod
    ```

    ```
    @classmethod
    ```

    ```
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

```
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

```
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

```
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

```
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

```
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

```
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

```
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
        f=fun(*args)                    # 初始化生成器
        result_queue=Queue()
        result_queue.put(None)
        while True:
            result=result_queue.get()
            try:
                r=f.send(result)        # generator.send()
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

- map() , reduce()  map( fn , [1,2,3,4] ) 将列表每个元素用fn处理，结果返回一个列表

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

    ```
    a=A()
    ```

    ```
    >>> getattr(a,'bar',3)
    ```

    ```
    3
    ```

    

#### 现代化的python

##### 类型提示

就像javascript发展成typescript一样，类型提示的缺失使得语言本身被迫产生了变种，我不是很了解最新的js的语言规范的发展，但是从对于原始的js学习以及es6的捎带了解之中，我发现js是真的不完美。（对比python）

对比js，python的完美程度要高的多得多。（当然这里默认不考虑py2，就完美程度来说，现在的js有点像py2，个人理解）。我做大胆预测，过几年，python底层完全把GIL锁这个东西完美过度之后，python将变成动态语言之王。（不接受反驳[狗头]）

python的【类型提示】是3.6+加入的

```
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

    ```
    from typing import Dict
    ```

    ```
    def test(dicnamr: Dict[str,float]):
    ```

    ```
        # Dict[键类型，值类型]
    ```

- list

    ```
    from typing import List
    ```

    ```
    def test(items: List[str]):
    ```

    ```
       #
    ```

- set

- tuple

类

- ```
    class Person:
    ```

    ```
        def __init__(self,name: str):
    ```

    ```
            self.name = name
    ```

    ```
            
    ```

    ```
    def test(one_person: Person):
    ```

    ```
        return one_person
    ```

> python的 类型提示 只是提示，由于py的动态性，python程序执行过程中，类型不符合并不影响程序执行



##### Pydantic:真正的类型校验

> 执行数据校验的库







### 六，迭代器和生成器

> 可迭代对象，迭代器和生成器
>
> 可迭代对象：内部实现有一个`__iter__()`方法，使得他可以迭代，用for循环
>
> 迭代器：可迭代对象基础上增加一个`__next__()`方法，这样就可以不使用for，用 next() 来实现获取下个值
>
> 生成器：在迭代器的基础上增加yield的概念，实现了在计算下一个值的时候不需要浪费空间的结构，异步编=程鼻祖，事件型编程的根基。

迭代对象和迭代器的值都在内存中，生成器是一点一点分配内存，节省空间



#### 迭代器

将迭代操作代理到容器内部的对象上

> Python的迭代器需要`__iter__()`返回一个实现了`__next__()`方法的迭代器对象

```
# 伪代码 test是一个可迭代对象

a=iter(test)            # 通过iter()将可迭代对象转化成迭代器
next(a)                 # 即可实现next
```



具体的容器实现示例见[python编程时光的系列文章第七节](https://www.cnblogs.com/wongbingming/p/9060989.html)



##### zip()迭代器

zip( list1,list2 ) 本质迭代器，用以将两个序列的元素依次迭代生成元组序列

1. 一种用法，用以字典键对序列反转。[click to 字典补充](#字典补充)

2. 同时遍历两个列表

    ```
    a=[1,2,3]
    ```

    ```
    b=['a','b','c','d']
    ```

    ```
    for i in zip(a,b):
    ```

    ```
        pass
    ```

    ```
    # 此处适用短板效应
    ```

> 补充：解除短板限制
>
> itertools.zip_longest()，没有的项置None
>
> ```
> from itertools import zip_longest
> ```

1. 转成字典和list

    ```
    s=dict(zip(a,b))
    ```

    ```
    # {1: 'a', 2: 'b', 3: 'c'}
    ```

    ```
    l=list(zip(a,b))
    ```

    ```
    # [(1, 'a'), (2, 'b'), (3, 'c')]
    ```







#### 生成器

> 生成器的激活方式：
>
> 1. next( )									# 协程中要注意 预激			
> 2. generator.send ( None )                                  # 引申出协程概念 [协程](#协程)



##### 列表生成器

列表生成器自带异常抛出

```
[x for x in range(3,7) if x > 0]
a = (x for x in range(3,7))
```



##### yield

yield实现的生成器，没有异常？需要自己抛出异常,,,,,(此处存在疑惑，我的环境py3.8是可以抛出异常的)

- yield:简单可以理解成不打断函数的return，返回值之后阻塞

    ```
    def foo():
    ```

    ```
        while True:
    ```

    ```
            res=yield 4
    ```

    ```
            print(res)
    ```

    ```
    
    ```

    ```
    run=foo()
    ```

    ```
    print(next(run))
    ```

    ```
    print(next(run))
    ```

    ```
    
    ```

    ```
    # 结果
    ```

    ```
    >>> print(next(run))
    ```

    ```
    4
    ```

    ```
    >>> print(next(run))
    ```

    ```
    None
    ```

    ```
    4
    ```

    解析：

    函数中使用yield返回结果之后，函数相当于生成器，本质是对象。生成器有一个next函数。~此处函数严格讲是生成器函数~

    run=foo()之后，next触发函数执行，执行到res=yield 4这句时候，yield 4，返回4。函数（生成器）停止

    下次调用next，从上次停止的地方开始执行。执行赋值操作，右侧上次执行已经return了，故此处给左侧的赋值是None，故print结果是None

- 一个关于生成器很好的解耦例子

    ```
    from collections import deque
    ```

    ```
    
    ```

    ```
    def search(line,pattern,history=5):
    ```

    ```
        previous_lines=deque(maxlen=history)
    ```

    ```
        for line in lines:
    ```

    ```
            if pattern in line:
    ```

    ```
                yield line,previous_line
    ```

    ```
            previous_line.append(line)
    ```

    ```
    
    ```

    ```
           
    ```

    ```
    if __name__ == "__main__":
    ```

    ```
        with open(r'path/to/file.txt') as f:
    ```

    ```
            for line,prevlines in search(f,python,5):
    ```

    ```
                for pline in prevlines:
    ```

    ```
                    print(pline,end="")
    ```

    ```
    # 查找关键字，并保留最近5项的历史记录
    ```

    ```
    
    ```



##### yield from

> yield from 后面需要加的是可迭代对象，可以是普通的可迭代对象，也可以是迭代器，生成器。

> yield from + 迭代器  可以实现将迭代对象里面的元素yield出来
>
> yield from + 生成器  实现生成器的嵌套

1. 嵌套展开

    ```
    A='absd'
    ```

    ```
    B=[1,2,3]
    ```

    ```
    C={'key1':23,'key2':24}
    ```

    ```
    D=(i for i in range(2,6))
    ```

    ```
    
    ```

    ```
    def gen(*args,**kwargs):
    ```

    ```
        for item in args:
    ```

    ```
            for i in item:
    ```

    ```
                yield i
    ```

    ```
                    
    ```

    ```
    def gen(*args,**kwargs):
    ```

    ```
        for item in args:
    ```

    ```
            yield from item
    ```

    ```
            
    ```

    ```
    testlist=gen(A,B,C,D)
    ```

    ```
    print(list(testlist))
    ```

    ```
    
    ```

    ```
    #结果一样
    ```

    ```
    ['a','b','s','d',1,2,3,'key1','key2',2,3,4,5]
    ```

​    

1. 生成器的嵌套

    > - 调用方：调用委派生成器的代码
    > - 委托生成器：包含yield from的生成器函数
    >     - 在调用方和子生成器之间建立双向通道
    >     - 调用方可以通过`send()`方法向子生成器发送消息，子生成器yield可以直接返回给调用方
    >     - 委托生成器只起桥梁作用，不会拦截中间传递的数据
    > - 子生成器：yield from后面的生成器函数

    ```
    # 实时均值计算
    ```

    ```
    # 子生成器
    ```

    ```
    def average_gen():
    ```

    ```
        total=0
    ```

    ```
        count=0
    ```

    ```
        average=0
    ```

    ```
        while True:
    ```

    ```
            new_num = yield average
    ```

    ```
            if new_num is None:
    ```

    ```
                break
    ```

    ```
            count += 1
    ```

    ```
            total += new_num
    ```

    ```
            average = total/count
    ```

    ```
            
    ```

    ```
        return total,count,average
    ```

    ```
        # return生效的时候，代表着这次协程结束
    ```

    ```
    
    ```

    ```
    # 委托生成器
    ```

    ```
    def proxy_gen():
    ```

    ```
        while True:
    ```

    ```
            # 子生成器结束即return之后，yield才会释放，并进行左侧的赋值
    ```

    ```
            total,count,average=yield from average_gen()
    ```

    ```
            print("{},{},{}".format(total,count,average))
    ```

    ```
    
    ```

    ```
    # 调用方
    ```

    ```
    def main()
    ```

    ```
        caculate_average=proxy_gen()
    ```

    ```
        next(caculate_average)              # 预激协程
    ```

    ```
        print(caculate_average.send(10))
    ```

    ```
        print(caculate_average.send(20))
    ```

    ```
        caculate_average.send(None)         # 协程结束
    ```

    ```
        
    ```

    ```
        # 如果此处再调用caculate_average.send(10)，由于上一协程已经结束，将重开一协程
    ```

    ```
        
    ```

    ```
    if __name__ == "__main__":
    ```

    ```
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

1. [MING - Python编程时光---py3并发编程系列](https://www.cnblogs.com/wongbingming/p/9060989.html)



### 七，类

> 封装
>
> > 封装原则：隐藏对象的属性和实现细节，仅对外公开接口，并且控制访问级别在OOP（面向对象）编程中，用类来实现上面的要求。用类实现封装，用封装来实现高内聚，低耦合
> >
> > 高聚合是说一个对象的功能都在内部搞定，紧紧地结合在一起
> >
> > 低耦合是说对象之间的依赖要松散，不要牵一发动全身
> >
> > [property](#@property)
>
> 继承
>
> 多态
>
> > 同一方法对不同类型的对象会有相应的结果



> 补充 : 一个约定是任何以单下划线 _ 开头的名字都应该是内部实现
>
> ​		   `__`双下划线命名的属性，在继承的时候考虑，是无法通过继承覆盖的，`__var`会被重命名成`_classname__var`，方法`__functions()`被重命名成`_classname__functions()`



#### 封装

【添加单个属性】

##### @property

作用：把方法变成属性，使之可以被调用。一来隐藏原来类内部的一些属性，满足封装原则。二来，可以在公用接口处（.setter()）设置检查参数。

```
class test:
    def __init__(self，width):
        self._width=width
    # 重写父类初始化,此处本质上也解决了属性暴露的问题
    
    @property                               # 本质 是getter()方法
    def width(self):
        return self._width
    # Q2 ？ 属性名不能和方法名字一样

    @width.setter
    def width(self,value):
        if not isinstance(value,int):
            raise ValueError("value must be a intnumber")
        if value<0 or value>100:
            raise ValueError("value must >0 and <100")
        self._width=value
        
     @width.deleter
     def width(self):
            raise AttributeError("Can't delete attribute")
test=test()
test.width=10
# __init__初始化类，本质也解决了属性私有问题，但局限性是初始化类时候必须传入所需的参数。一些需要自定类属性的（数据库操作），需要先初始化类，再按需引入私有属性，此时使用@property装饰器完成方法的属性化包装。
```

> 所以在创建类的时候，可以使用`__init__()`来初始化一些默认属性，使用@property来构建一些可插入的属性



##### @property的子类继承

`@A.width.setter`装饰函数，重写功能，`super(B,B).width.__set__(self,width)`实现对父类的覆盖

```
class A:
    def __init__(self,width):
        self._width=width

    @property
    def width(self):
        return self._width

    @width.setter
    def width(self,width):
        if width<0 :
            raise ValueError("error of 0")
        self._width=width
    
    @width.deleter
    def width(self):
        raise AttributeError("can't be deleted")

class B(A):
    @A.width.setter
    def width(self,width):
        if width>100:
            raise ValueError("error of 100")
        super(B,B).width.__set__(self,width)
        # 注意 此处的super参数两个都必须是子类名
    
if __name__=="__main__":
    test=B(2)
    print(test.width)
    test.width=10
    print(test.width)
```



##### 描述器

python的描述器 https://docs.python.org/zh-cn/3/howto/descriptor.html

一般地，一个描述器是一个包含 “绑定行为” 的对象，对其属性的存取被描述器协议中定义的方法覆盖。这些方法有：[`__get__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__get__)，[`__set__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__set__) 和 [`__delete__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__delete__)。如果某个对象中定义了这些方法中的任意一个，那么这个对象就可以被称为一个描述器。

属性访问的默认行为是从一个对象的字典中获取、设置或删除属性。例如，`a.x` 的查找顺序会从 `a.__dict__['x']` 开始，然后是 `type(a).__dict__['x']`，接下来依次查找 `type(a)` 的基类，不包括元类。 如果找到的值是定义了某个描述器方法的对象，则 Python 可能会重载默认行为并转而发起调用描述器方法。这具体发生在优先级链的哪个环节则要根据所定义的描述器方法及其被调用的方式来决定。

 [描述器协议](https://docs.python.org/zh-cn/3/howto/descriptor.html#id5)

```
descr.__get__(self, obj, type=None) -> value
descr.__set__(self, obj, value) -> None
descr.__delete__(self, obj) -> None
```

以上就是全部。定义这些方法中的任何一个的对象被视为描述器，并在被作为属性时覆盖其默认行为。

如果一个对象定义了 [`__set__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__set__) 或 [`__delete__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__delete__)，则它会被视为数据描述器。 仅定义了 [`__get__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__get__) 的描述器称为非数据描述器（它们通常被用于方法，但也可以有其他用途）。

数据和非数据描述器的不同之处在于，如何计算实例字典中条目的替代值。如果实例的字典具有与数据描述器同名的条目，则数据描述器优先。如果实例的字典具有与非数据描述器同名的条目，则该字典条目优先。

为了使数据描述器成为只读的，应该同时定义 [`__get__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__get__) 和 [`__set__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__set__) ，并在 [`__set__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__set__) 中引发 [`AttributeError`](https://docs.python.org/zh-cn/3/library/exceptions.html#AttributeError) 。用引发异常的占位符定义 [`__set__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__set__) 方法使其成为数据描述器。

描述器是特征属性、方法静态方法、类方法和 [`super()`](https://docs.python.org/zh-cn/3/library/functions.html#super) 背后的实现机制





##### @staticmethod 和 @classmethod

@staticmethod 静态方法装饰器：作用是装饰后的函数是一个可以无须实例化类就可以调用的函数，本质上和单独的一个函数一样，只是放在了类里面。这么做是为了提升类的聚合性。

> 静态方法装饰下的函数，参数处无需传入self或者cls之类的东西，只是一个单纯的函数
>
> 适用于   不需要访问任何实例方法和属性，纯粹地通过传入参数并返回数据的功能性方法

@classmethod 类方法装饰器：作用是创建一个不需要实例化的函数，传入的第一个参数必须是cls，表示本身，返回的是函数的类方法,,即把 类 的方法直接按自己的名字实例化

```
class A(object):
    num=10
    def m1(self,n):             # 必须实例化 才能使用
        pass
    
    @staticmethod               # 等同于普通函数，可以随便用
    def m2(n):
        pass
        
    @classmethod                # 可以不经实例化就使用，这块的cls表示没实例化的时候的本身
    def m3(cls,n):              # 本质用构建了一个类的
        cls.m1(cls,n)
        # 或者 cls().m1(n)        # 推荐第一种表示方法，理解起来比较顺
        print(cls.num)
        pass
    
# 说明
# 对于类方法
# 就算是实例化了一个对象test，test.m3()还是会定向到A.m3()
# 即
# >>> print(A.m3)
# >>> print(test.m3)
# <bound method A.m3 of <class '__main__.A'>>
# <bound method A.m3 of <class '__main__.A'>>
```





#### 继承

##### 默认父类object

- ```
    __init__(self)
    ```

    ```
    # 初始化实例
    ```

- ```
    __new__(cls)与__call__(self)
    ```

    ```
    __new__()在实例创建前被调用，在__init__()之前,用于创建一个实例
    ```

    ```
    
    ```

    ```
    class A:
    ```

    ```
        def __init__(self)
    ```

    ```
        pass
    ```

    ```
    
    ```

    ```
    def __new__(cls):
    ```

    ```
        return object.__new__(cls)
    ```

    ```
    # 或者
    ```

    ```
    # return super().__new__(cls)
    ```

    ```
                
    ```

    一般开发用不到，`__new__`常作为构造函数创建对象，工厂函数，专用于生产实例对象。eg, 单例模式，开发框架		[跳转设计模式-单例](#单例)

```
class BaseController(object):
    _singleton = None
    def __new__(cls, *a, **k):
        if not cls._singleton:
            cls._singleton = object.__new__(cls, *a, **k)
            return cls._singleton
```

​		通过 `__new__` 方法是实现单例模式的的一种方式，如果实例对象存在了就直接返回该实例即可，如果还没		有，那么就先创建一个实例

```
__call__(self),使得对象是一个可调用对象。（callable）
```

> 可以用在类装饰器 [跳转](#装饰器)



- ```
    __repr__(self)与__str__(self)
    ```

    ```
    __str__()返回一个字符串，描述对象
    ```

    ```
    __repr__()是类信息，，repr()会在返回字符串外围增加“ ”，多用在eval(),通过求值运算，从新得到对象
    ```

    ```
    
    ```



##### super()

解决父类调用问题，父类多次调用只执行一次，优化执行逻辑

```
class A:
    def __init__(self):
        print("A")
class B(A):
    def __init__(self):
        super(B,self).__init__()
class C(A):
    def __init__(self):
        super(C,self).__init__()
class D(B,C):
    def __init__(self):
        super(D,self).__init__()

# 上述过程只调用了一次A
```

[[参考 super()\]](https://blog.csdn.net/liwei825755184/article/details/54425572?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1)

super( a , b ) 表示获取 b 的mro表中 a 的下一个类

a代表类，b 代表实例

c3线性化算法生成mro表，python3是新式类，广度优先算法



##### 多重继承

```
class  子类类名（父类1,父类2,…）:
    类体
```



**Mixin思想**

多重继承设计时，继承的应该是类，角色是 I am，Minxin的角色是能力，I can

一种规范: 继承用单一继承，之后的插入的性质用mixin类



#### 多态

同一方法对不同类型的对象会有相应的结果





#### 构建类的技巧

##### 定义抽象基类

【抽象基类，不能被实例化，只是提供接口扩展】

```
# abs模块
from abs import ABCMeta,abstramethod
class ISream(metaclass=ABCMeta):
    @abstramethod
    def read(self,maxbytes=-1):
        pass
    @abstramethod
    def write(self):
        pass
```



##### 利用描述器实现自定义类型

数据结构 or  容器



##### 多构造器

```
class A:
    def __init__(self):
        pass
        # 通过__init__构造
    @classmethod
    def test(cls):
        return cls()
    # 通过方法构造类
```



##### 访问者模式

不常使用的模式



##### 内存管理

1. `__slots__`节约类空间

2. 创建缓存实例

    ```
    class classname:
        def __init__(self,name):
            self.name=name
    import weakref
    _classname_cache=weakref.WeakValueDictionary()
    def get_classname(name):
        if name not in _classname_cache:
            s=classname(name)
            _classname_cache[name]=s
        else:
            s=_classname_cache[name]
        return s
    ```

3. 循环引用的数据结构

    产生原因，举例，在树中，双亲结点和孩子节点相互引用，导致双方的引用计数始终不能为0，无法触发垃圾回收机制

    ```
    # 解决方法 使用弱引用
    import weakref
    n_1=Node()
    n_1_ref=weakref(n_1)
    ```

    

    





### 元编程

[跳转->函数装饰器](#装饰器)

#### 类装饰器

@property 本质是一个类装饰器，

```python
class property:
	def deleter(self,fun):
        pass
    def setter(self,fun):
        pass
    def getter(self,fun):
        pass
```



#### 给类或静态方法提供装饰器

装饰器要在 @classmethod 或@staticmethod 之前，即最外层是@staticmethod和@classmethod

原因上面两个方法并不是装饰器，他们使用描述器实现的，返回的对象是一个描述器对象。

```python
from functools import wraps
def decorator(fun):
    @wraps(fun)
    def wrapper(*args):
        print("wraps")
        fun(*args)
    return wrapper

class test(object):
    
    @staticmethod
    @decorator
    def test1(*args):
        print(*args)

A=test()
A.test1('testest')
```



#### 类装饰器装饰类-代替继承

```python
def decorator(cls):
    old_method=cls.add
    def method(*args):
        print("new method {}".format(args))
        old_method(*args)
    cls.add=method
    return cls
    
@decorator
class A:
    def add(self,*args):
        print(*args)

test=A()
test.add(33)
```



#### 元类

元类实现单例

```python
class Singleton(type):
    def __init__(self,*args,**kwargs):
        self.__instance=None
        super().__init__(*args,**kwargs)

    def __call__(self,*args,**kwargs):
        if self.__instance is None:
            self.__instance=super().__call__(*args,**kwargs)
            return self.__instance
        else:
            return self.__instance

class test(metaclass=Singleton):
    def __init__(self):
        print("test singleton")
        
```

元类实现 缓存实例

```python
# 元类实现缓存实例
import weakref
class Cached(type):
    def __init__(self,*args,**kwargs):
        super().__init__(*args,**kwargs)
        self._instance_cache=weakref.WeakValueDictionary()

    def __call__(self,*args):
        if args in self._instance_cache:
            return self._instance_cache[args]
        else:
            newobj=super().__call__(*args)
            self._instance_cache[args]=newobj
            return newobj

class test(metaclass=Cached):
    def __init__(self,name):
        print("test")

a=test('name1')       
b=test('name1') 
# 创建两个相同实例的时候，只会创建一个
```

