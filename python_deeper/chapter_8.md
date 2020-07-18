### 错误处理，调试与测试

#### try

```python
try:
	可能出错的代码，运行代码
except ValueError as e:
    print('ValueError',e)
except ZeroDivisionError:
    print('ZeroDivisionError')
else:
    print('everything is ok')
finally:
    print('finally')
```

> 一点注意，except的错误类型会包含子类，所以同类错误直接全部捕获

#### 调试

> python3.7之后 内置了breakpont()
>
> 所以除了print以外，breakpoint()就行了
>
> [breakpoint的实现](https://www.jianshu.com/p/5d3c52c5c2a2)

```python
代码块
breakpoint()
代码块
```



#### 测试代码

> Built-In model：unittest

```python
import unittest

from yourfile import yourmodel_neededtest

class Test_yourmodel(unittest.TestCase):		# 继承测试类
    
    def setUp():
        print('每个测试开始前，执行此函数内容先，代码复用')
        
    def tearDown():
        print('关闭setup打开的东西，比如数据库连接')
    
    
    def test_what1(self):						# 类名字前缀test的函数 会进行自动测试
        self.assertEqual(whatkey,whatvalue)		# unittest内置断言 assert。。。
        self.assertTrue(isinstance(d,dict))		# isinstance()判断类型
    
    def test_what2(self):
        with self.assertRaises(KeyError):
            value=d['emptykey']
    
# 常见断言


```

