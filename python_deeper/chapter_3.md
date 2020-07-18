### 文件 与 IO

##### 文件读写

```python
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
> 但是python中的换行符是统一的 ```\n```
>
> linux -> ```\n```   windows -> ```\r\n```

- 文本文件      rt		wt		at
- 读写二进制文件  		rb		wb
- 禁止覆盖写       xt      xb

> 直接将字节写入文本文件
>
> ```python
> import sys
> ✖ sys.stdout.write()
> ✔ sys.stdout.buffer.write(b'test') 
> # 直接写入buffer即可
> ```



##### 补充 ，with 上下文管理

当出现 with语句的时候，对象的 __enter__() 方法被触发，它返回的值 (如果有的话) 会被赋值给as 声明的变量。然后，with 语句块里面的代码开始执行。最后，__exit__() 方法被触发进行清理工作。

`让对象支持上下文管理`

```python
class Test:
    def __init__(self):
        pass
    
    def __enter__(self):
        
        pass						#
    
    def __exit__(self):
        what.close()
        pass						#
```









##### 压缩文件处理

```python
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

```python
import os
path='/path/test/to/file.csv'
os.path.basename(path)				# file.csv
os.path.dirname(path)				# /path/test/to
os.path.join('new','path','to',os.path.basename(path))
# '/new/path/to/file.csv'

# 路径存在与否
os.path.exists('/etc/test')
# 创建目录
os.mkdir('test')				# 生成一级目录
os.makedirs()					# 多级目录

# 获取文件夹文件列表
os.listdir('/etc/test')
[name for name in os.listdir('/etc/test') if 		           os.path.isfile(os.path.join('/etc/test',name))]
[name for name in os.listdir('/etc/test') if name.endswith('.py')]
# 使用os.listdir()获取目录下的信息
```

现在推荐的路径处理方案（3.6之后使用）

```python
from pathlib import Path			# 面向对象式
p=Path('/path/test/to/file.csv')		
# PosixPath('/path/test/to/file.csv')	表明是一个PosixPath对象

# 文件名字
p.name
p.suffix					# 后缀
p.suffixs					# 后缀们
p.stem						# 文件名字
# 获取父级目录属性
p.parents[0]				# /path/test/to
p.parents[2]				# /path
p.parent					# /path/test/to
p.parent.parent				# /path/test
# 目录拼接
Path('/').joinpath('path','test/test')			# PosixPath('/path/test/test')
Path('/')/'home'/'test/test'
Path('home') / 'dongwm/code'					# PosixPath('home/dongwm/code')
# 路径检测
p.is_file()
p.is_dir()
# 创建目录
Path('1/2/3').mkdir()		# 可以直接创建多级目录
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

```python
import serial
```

> 具体有了应用场景再深入



##### 序列化对象

###### pickle

```python
import pickle

data=python对象
f=open('file','wb')
pickle.dump(data,f)				    # dump进文件
f=open('file','rb')
data=pickle.load(f)					# 从文件恢复对象

s=pickle.dumps(data)				# 对象转成字符串
data=pickle.loads(s)				# 字符串转成对象

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

```python
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



> ==在数据库和存档文件中存储数据时，你最好使用更加标准的数据编码格式如 XML，CSV 或 JSON==
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

```python
import json

d = dict(name='Bob', age=20, score=88)
json_str=json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'

json.loads(json_str)
{'age': 20, 'score': 88, 'name': 'Bob'}
```

类的序列化/反序列化方案

```python
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
n=json.loads(json_string,object_hook=get)  		 	# 钩子函数
>>> n
<__main__.test object at 0x7f9ef3c74190>
>>> n.name
'jack'
>>> n.age
20
```









参考  [廖雪峰的python3教程](https://www.liaoxuefeng.com/wiki/1016959663602400/1017624706151424)



###     补充 ：is 和 ==

- **is **: 同一性算符，比较对象间的唯一身份标识
- **==** ：比较值是否相等

> Python中引用的思想：
>
> ​		python中万物皆对象，
>
> ​		变量是对内存中对应值这个对象的引用，当引用为0时，内存释放
>
> ```python
> a=[1,2,3]
> b=[1,2,3]
> a==b
> True
> a is b
> False
> ```



