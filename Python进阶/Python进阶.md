《Python进阶》

---

### *args 和 **kwargs

后面的参数并不重要，重要的是一`*`和两个`**`的区别和作用，主要用于函数不定参数的定义。***args**是用来发送一个非键值对的可变数量的参数列表给一个函数。

```python
def test_var_args(f_arg, *args):
    print("first normal arg:", f_arg)
    for arg in args:
        print("another arg through *args:", arg)

# 测试
test_var_args('php', 'java', 'python', 'c++')
# output
# first normal arg: php
# another arg through *args: java
# another arg through *args: python
# another arg through *args: c++
```

而**\**args** 允许使用不定长度的键值对，作为参数传递给函数。

```python
def greet_me(**args):
    for key, value in args.items():
        print("{0}=={1}".format(key, value))


# 测试
greet_me(name="waldeincheng")
# output
# name==waldeincheng
```

标准参数与***args**、**\**args**在使⽤时的顺序 那么如果你想在函数⾥同时使⽤所有这三种参数， 顺序是这样的： 

```python
some_func(fargs, *args, kwargs)
```

### 调试Debug

使用任何开发语言中，也在实际工作中，调试都是必不可少的一步。

从命令行进入

```bash
python -m pdb my_script.py
```

从脚本内部运⾏

```python
import pdb
def make_bread():
pdb.set_trace() # 从此处断点
return "I don't have time"
print(make_bread())
```

debugger模式下的常用命令：

- c: 继续执⾏ -
- w: 显⽰当前正在执⾏的代码⾏的上下⽂信息 -
- a: 打印当前函数的参数列表 s: 执⾏当前代码⾏，并停在第⼀个能停的地⽅（相当于单步进⼊）
- n: 继续执⾏到当前函数的下⼀⾏，或者当前⾏直接返回（单步跳过） 

单步跳过（next）和单步进⼊（step）的区别在于， 单步进⼊会进⼊当前⾏调⽤的函数内 部并停在⾥⾯， ⽽单步跳过会（⼏乎）全速执⾏完当前⾏调⽤的函数，并停在当前函数的 下⼀⾏。

### 生成器Generators

