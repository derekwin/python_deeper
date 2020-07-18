

#### 单例

单例的作用在于保证系统中一个类只有一个实例且该实例易于被外界访问，便于控制实例个数，节约系统资源。

常用命名规范：**XXXXManger**

三个需求：

1. 只能有一个实例
2. 必须自己创建这个实例
3. 自行向整个系统提供这个实例





#### 单例的python实现

==基础实现==

[【基于__new__】](#默认父类object)

```python
class Singleton(object):
	_instance=None
	def __new__(cls,*args,**kwargs):
		if not _instance:
            cls._instance=super(Singleton,cls).__new__(cls,*args,**kwargs)
        return cls._instance    
```

基础方法实现的单例，在多线程并发场景会出现多个实例的问题

==加锁==

```
线程进程使用方法复习 + 线程进程锁 的学习
```

==模块的本质：完美的单例==

使用import引入模块的方式，本质就是单例



#### 发布/订阅模式

发送者：将发送的信息分类发送，不关心接收者

接收者：按订阅的类别接受特定的消息

中间人：负责将分类的信息按‘’路由表‘’转发给接收者

【python-message 模块】做程序库和日志模块的解耦

```python
import message
```

程序库的日志使用message模块，将报错的信息pub出来，再在接受日志的程序处，使用sub进行接收，统一处理。

[实例与细节 参考 或者 effective python](http://www.pythontip.com/blog/post/2032/)





#### 状态模式 优化代码

###### 【state 模块】

优化模块，使得模块的具备状态切换能力

一个使用场景是应用的登录验证环节，可以定义用户的类，其中未登录时，是一个状态，登陆后，切换至登陆状态。

ps.目前中文世界搜不到state模块的使用情况，鉴于状态模式是可以自己比较方便实现的，所以是否使用state模块目前不做定夺。

补充 state模块 多年未维护，py3已经没法导入，以下代码已经不能使用   ==实现一个状态库？==

```python
from state import curr,switch,stateful,State,behavior
@stateful
class User:
    class NeededSignin(State):
        default=True
        @behavior
        def signin(self,user,passwd):
            # code
            switch(self,Player.Signin)
    class Signin(State):
        @behavior
        def move(self,dst):
        	# move
        @behavior
        def atk(self,other):
            # atk
```



###### 【状态机实现】

原理：将不同状态下类的功能实现写成不同的类，调用类的时候，依据状态实例化不同的类

```python
# 状态机
database={'user1':['1@qq.com','123456'],}
# 用户创建的逻辑是 setdefault(key,value),无返回值则创建成功，有返回值说明已经注册了

# 基类
class base:
    @staticmethod
    def signup(conn):
        raise NotImplementedError("choose state with set_state")
    @staticmethod
    def login(conn):
        raise NotImplementedError("choose state with set_state")
    @staticmethod
    def do_things(conn):
        raise NotImplementedError("choose state with set_state")

class needsignup(base):
    @staticmethod
    def signup(conn,*args):
        if args[0] in list(database.keys()):
            print("{}已经被创建".format(args[0]))
            print("pls sign up again with another username")
        else:
            database.setdefault(args[0],[args[1],args[2]])
            print("{}创建成功".format(args[0]))
            conn.set_state(needlogin)
            print("please login in")
    @staticmethod
    def login(conn,*args):
        print("need signup")
    @staticmethod
    def do_things(conn):
        print("need signup from do_things")

class needlogin(base):
    @staticmethod
    def signup(conn,*args):
        print("pls sign up")
        conn.set_state(needsignup)
    @staticmethod
    def login(conn,*args):
        if args[0] in list(database.keys()) and database[args[0]]==[args[1],args[2]]:
            print("login succeed")
            conn.set_state(do_things)
        else:
            print("login failed")
    @staticmethod
    def do_things(conn):
        print("need login from do_things")

class do_things(base):
    @staticmethod
    def signup(conn,*args):
        print("already signup")
    @staticmethod
    def login(conn,*args):
        print("already login")
    @staticmethod
    def do_things(conn):
        print("do things")

class User():
    def __init__(self):
        self._state=base

    def set_state(self,object_state):
        self._state=object_state

    def signup(self,*args):
        self._state.signup(self,*args)
    def login(self,*args):
        self._state.login(self,*args)
    def do_things(self):
        self._state.do_things(self)

if __name__=="__main__":
    userconnected=User()

    # userconnected.set_state(needsignup)
    # userconnected.login('user2','2.@qq.com','43523')
    # userconnected.signup('user1','2.@qq.com','43523')
    # userconnected.do_things()

    # print("-------")

    # userconnected.set_state(needlogin)
    # userconnected.signup('user2','2.@qq.com','43523')
    # userconnected.login('user2','2.@qq.com','43523')
    # userconnected.do_things()  

    # print("-------")

    # userconnected.set_state(do_things)
    # userconnected.signup('user2','2.@qq.com','43523')
    # userconnected.login('user2','2.@qq.com','43523')
    # userconnected.do_things()

    print("============")

    userconnected.set_state(needlogin)
    userconnected.signup()
    userconnected.signup('user2','2.@qq.com','43523')
    userconnected.login('user2','2.@qq.com','43523')
    userconnected.do_things()
```

###### 【有限状态机】

transitions库       https://github.com/pytransitions/transitions

https://www.cnblogs.com/21207-iHome/p/6085334.html



#### 依赖注入

松耦合，模块化

常用在框架级别的项目

eg. fastapi 的数据库，认证和鉴权实现



*过度松耦合加剧了项目的可读性下降*



