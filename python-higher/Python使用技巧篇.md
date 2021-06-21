Python使用技巧篇| 实际应用中的函数利用

---

### 如何在集合，列表，字典中筛选数据？

```python
"""
如何在集合，列表和字典中筛选数据
"""
from random import randint
# 过滤掉列表中的负数
list = [randint(-10, 10) for _ in range(10)]
# 1 使用filter.Python3中filter()返回的是一个可迭代的对象
result = filter(lambda x: x >= 0, list)
for i in result:
    print(i)
# 2 使用列表解析的方式，一般现在常用
result1 = [x for x in list if x >= 0]
print(result1)


# 筛选出字典里值大于80的项
dict = {x: randint(60, 100) for x in range(1, 21)}
# 使用字典解析
result2 = {k: v for k, v in dict.items() if v > 80}
print(result2)


# 筛选出集合中能被三整除的元素
s = set(list)
result3 = {x for x in s if x%3==0}
print(result3)
```

### 如何统计序列中元素出现的频率？

```python

```



