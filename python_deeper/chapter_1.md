*建议：这章的内容，草草过一下就好，具体需求的时候再回来查阅*



### 数据结构



#### 内建数据结构：

```mermaid
classDiagram
基本数据结构 --> list
基本数据结构 --> dict
基本数据结构 --> tuple
基本数据结构 --> set
list: [1,2,3]
dict: {num1:1,num2:2}
tuple: (1,2,3) 
set: 无序不重复元素集
set: set(),frozenset()
```

> ###### 操作方法

```python
# list:					Sequence Types			可变类型
list1=["test",2,4]				# 初始化
list1[0]						# 索引
slice1=list1[1:]				# 切片
slice2=list1[1:5:2]				# 从1到5按步数2摘出
list1.index('2')				# 查询
list1[1]=5						# 修改
list1.append('new')				# 追加元素
list1.insert(1,'insert1')		# 插入元素
del list1[1]
list1.remove['test']
list1.pop()						# 删除元素
list1.extend(['add1','add2'])	# 拓展列表
list1.count('test')				# 统计
list1.sort()					# 排序  
list1.reverse()					# .sort(reverse=True)
list2=list1.copy()				# copy
								# 深拷贝，浅拷贝的区别---import copy
list1=list(set(list1))			# 用set()实现 去重
list2=[]
[list2.append(i) for i in list1 if i not in list2]	# 去重
if 'a' in list:
if 'a' not in list:

# dict:										可变类型
dict1={'a':1,'b':2,'c':3}		# 初始化
dict1.keys()			>>> dict_keys(['a', 'b', 'c'])
dict1.values()			>>> dict_values([1, 2, 3])
dict1.items()			>>> dict_items([('a', 1), ('b', 2), ('c', 3)])
list(dict1.keys())		>>> ['a', 'b', 'c']
dict2=dict1.copy()				# copy 此处既有深拷贝也有浅拷贝
dict1.update({'b':4,'d':5})		# b的键值更新，新加d
dict1.pop('c')					# 删除指定键值对
dict1.popitem()					# ”随机“删除键值对,本质删除顺序存储最后一键
dict1.setdefault('new1',8)		# new1不存在则，加入new1并给值8
dict1.setdefault('new2')		# 默认是None，加入new2赋None
dict1.setdefault('a',10)		# a存在，则返回a原来的值，不会覆盖指定的值

# tuple:				Sequence Types		不可变类型
tuple1=tuple1+tuple2
# 元组中的元素不可以更改，即不能通过tuple[2]=a,来修改元素，但是元组内可以放任意数据结构，即如果元素是列表，那列表内的元素是可以改变的。
del tuple1
len(tuple1)
# 其他用法，同sequence types通用用法
# 比较两个元组的元素，可以导入operator模块

# set:
# 创建方法
set1=set()								# 可以用来创建空集合
set2={'a','b'}							# 不能用来创建空集合
in / not in
a,b=set('slice'),set('test')
c=a-b									# a含b不含
c=a|b									# a含或b含
c=a&b									# a且b
c=a^b									# 不同时含于ab的集合
c={x for x in a if x not in b}
a.add()									# 添加元素
a.update()								# 添加元素，其他数据结构
a.remove(x)
a.discard(x)							# 删除元素
a.pop()									# “随机”删除，存储顺序出栈
len(a)
a.clear()								# 清空集合

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
>     ==python3.6==~的优化，改用一个一维散列表和一个二维数组来实现，计算hash值，对哈希表目数（此处是一位散列表的长度）求余，得到在表二中保存的位置下标，将[hash值,指向key的指针,指向value的指针]保存到二维数组中，并在一维表中对应下标位置，存入此时对应的元素序号~
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
>     ```python
>     # zip() 本质迭代器，只生效一次
>     p=min(zip(price.values(),price.keys()))		#利用zip()反转key												和value
>     # 上面方法，在值相同时，键的比较结果决定返回值
>     
>     # 利用min(),max()中提供的key函数
>     min(dict1 ,key=lambda k: dict1[k])		# key通过lambda遍历了所有的键,迭代器
>     ```
>
>     [click to zip()迭代器](#zip()迭代器)
>
> - 删除指定键构造新字典
>
>     ```python
>     c={key:dict1[key] for key in dict1.keys() - {'z','w'}}
>     # 利用生成器，按字典的key进行差预算
>     ```
>
>     ==字典的键也是可以进行并差交运算的==



#### collections拓展模块

- defaultdict			--> 多值字典

    ```python
    from collections import defaultdict
    							# 构造一对多字典
    d=defaultdict(list)			# 亦可以是元组
    d['a'].append(1)
    d['b'].append(2)
    
    # 此处对标一对一标准字典中.setdefault('key',value)方法，添加键对时，无则创建赋值
    ```

    [应用](#根据字段将记录分组)

- ~~OrdereDict~~ 

    ```python
    # 控制字典中元素的顺序
    from collections import OrderedDict
    
    d=OrderedDict()
    d['a']=1
    d['b']=2
    
    # 这样的字典会保持插入时的顺序		本质双向链表
    ```

    ps，python3.6之后，由于字典的底层结构的更改，自动生成的字典就是有序的

    再者，OrdereDict这个方法，由于维护双向链表，所以大小是正常字典的二倍，加之3.6的优化，==这种方法已经被淘汰了==

- deque

    ```python
    # 队列
    from collections import deque
    q = deque(maxlen=5)					# 不指定，默认无长度限制
    q.append(2)							
    q.appendleft(4)						>>> deque([4,2],maxlen=5)
    q.pop()								>>> deque([4],maxlen=5)
    ```

- ChainMap

- Counter

    ```python
    # 找出序列中出现次数最多的元素
    from collections import Counter
    words=['a','b','c']
    word_counts=Counter(words)
    top_three=word_counts.most_common(3)
    # top_three=[('a',1),('b',1),('c',1)]
    ```

    

- namedtuple                   --> 命名元组

    使用方法

    ```python
    from collections import namedtuple
    Stock=namedtuple('Stock',['name','shares','price'])
    s=Stock('ACME',100,123.23)
    # 元组的值无法修改，所以得用重写一个元组
    s= s._replace(shares=75)
    ```

    通用思路

    ```python
    # 构筑命名元组原型，通过_replace()初始化
    Stock=namedtuple('Stock',['name','shares','price','date'])
    stock_list=Stock('',0,0.0,None)	#实例原型
    def dict_to_stock(s):
        return stock_list._replace(**s)		#将字典导入
    ```

    <a href="#动态参数:*与**">*&** 用法</a>

    以往的优点是作为字典的替代品，使用命名元组会占用更少的空间，助于摆脱下标解码，提升可维护，




#### queue 

【多线程编程，任务队列，[内联回调](#内联回调)】

提供两种队列对象，FIFO > Queue , LIFO > LifoQueue , PriorityQueue(优先级队列) ，deque(双向队列)

```python
# Queue
from queue import Queue
test=Queue(maxsize=10)
# test=LifoQueue(maxsize=5)
# test=PriorityQueue(maxsize=5)
exp=deque

#对于前三种
test.put(item)
test.get()		# remove and return one item
test.empty()    # empty return True,else False
test.full()		# if full return True,else Flase
test.qsize()	# return size of test
# 
test.get_nowait()	# 等价于 test.get(Flase) 非阻塞方法
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

---

##### 解压序列

> ==解压序列==不仅可以用在列表和元组，亦可以用在字符串，文件对象，迭代器和生成器

- 解压部分变量时，可以使用占位变量名

    ```python
    >>> data=[1,2,3,4]
    >>> _,a,_,b=data
    >>> _
    3
    >>> b
    4
    ```

- 星号-匹配元素

    ```python
    >>> first,*middle,last=data
    >>> middle
    [2,3]
    ```

    ```python
    # 分割字符的一种新方法，简单的场景可以取代正则
    >>> line="··· name :test: where : dir :/var/test"
    >>> _,name,*_,dir=line.split(":")
    # 元组结合的案例
    >>> record=[2,3,3,(4,5,6)]
    >>> *_,num1,(*_,num2)=record
    ```

    

##### 索引切片

> 个人用==索引切片==会更好

```
>>> data=[2,3,4,5,6,7]
>>> data[2:]
[4,5,6,7]
>>> data[-2:]
[6,7]
>>> data[::2]			# 步进器
[2,4,6]
>>> data[::-1]
[7,6,5,4,3,2]
>>> data[::-3]
[7,4]
```

##### 补充：切片对象

```python
a=slice(2,10,2)
a.start
a.stop
a.step
# 消除硬编码，创建切片对象，使得代码可维护性提升
record="......sss.sss."
get=record[a]
```





---

#### 过滤元素

##### 删除序列相同元素

1. 不需要保持顺序（不稳定方法）

    ```python
    a=[1,6,3,4,1]
    a1=[{'a':1,'b':2},{'a':1,'b':3}]
    b=list(set(a))							# 集合
    ```

2. 保持顺序（稳定）

    ```python
    # 构造生成器
    
    # 一般序列
    def dedupe(items):
    	seen=set()
    	for item in items:
    		if item not in seen:
    			yield item
    			seen.add(item)
    # 含有字典等hash的序列结构
    def dedupe2(items,key=None):
        seen=set()
        for item in items:
            var=item if key is not None key(item) 	# key需要传入一个函数
            if var not in seen:
                yield item
                seen.add(var)
    
    b=list(dedupe(a))
    b1=list(dedupe(a1,key=lambda d:(d['a'],d['b'])))
    ```

##### 过滤元素

- 列表推导

    [ f(x) for x in list if x is not None ]

- 生成器表达式，迭代产生

    pos=( n for n in list if n > 0 )

    此时的pos是一个生成器

    ```python
    >>> for x in pos:
    ···		print(x)
    ···
    1
    4
    10
    >>> 
    ```

- filter( ) 过滤

    ```python
    #filter()本质创建一个迭代器
    values=['1','2','no','5','a']
    def is_int(var):
        try: 
            x=int(var)
            return True
        except ValueError:
            return False
    
    ivals=list[filter(is_int,values)]
    # 因为是迭代器，所以这块用list接收值
    ```





---

#### 实用字典序列操作

##### 通过某关键字排序字典列表

operator 模块 itemgetter( )			==更快的方法==

```python
rows=[
	{'fname':'test1','lname':'test2','uid':1},
	{'fname':'test11','lname':'test22','uid':2},	
    {'fname':'test1','lname':'test22','uid':3},
]
```

```python
from operator import itemgetter
rows_by_fname=sorted(rows,key=itemgetter('fname'))
rows_by_uid=sorted(rows,key=itemgetter('uid'))
```



传统方法,lambda自定函数

```python
row_by_fname=sorted(rows,key=lambda x: x['fname'])
```

> 补充：对对象排序  [引申概念点]()
>
> ```python
> class User:
> 	def __init__(self,user_id):
> 		self.user_id=user_id
> 	
> 	def __repr__(self):
> 		return 'User({})'.format(self.user_id)
> def sort_class():
> 	users=[User(23),User(2),User(4)]
> 	sorted_by_user_id=sorted(users,key= lambda k: k.user_id)
> 
>  # or 采用attrgetter()模块
> 	from operator import attrgetter
>  sorted_by_user_id_2=sorted(users,key=attrgetter('user_id','first_name'))
>  # attrgetter支持多参数排序
> ```

##### 根据字段将记录分组

itertools.groupby( ) 模块

```python
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

```python
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











