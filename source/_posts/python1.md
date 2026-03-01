---
title: python格式化
date: 2026-03-01 14:47:39
categories: Python
tags: 
  - Python基础
  - Python
password: 3g45d
abstract: 私密笔记
---
# Python格式化

Python中常见的格式化方法：
1. %符号+格式指定法
2. format()函数
3. f-string

```py
# 【案例】在控制台输出：【张三】今年【18】岁
name = "张三"
age = 18

# 最原始提供的字符串替换方法，使用了%运算符和经典字符串格式指定，如%s %d等
print("【%s】今年【%d】岁" %(name,age))

# Python 3.0新增了format()函数，可以提供高级的格式化选项
print("【{}】今年【{}】岁".format(name,age))

# Python 3.6 f-string出现，使得格式化方法更加灵活，字符串前缀为f，并使用{}评估值
print(f"【{name}】今年【{age}】岁")
```

## f-string常见用法

```py
# f-string中接收表达式
num = 12
price = 6

print(f"【{num}】个苹果，每个【{price}】元，一共要花【{num*price}】元")
# ---------
# 控制台输出：【12】个苹果，每个【6】元，一共要花【72】元

# ---------------------------------------------------------------

# f-string可以对字典取值
user = {"name":"Alice","job":"teacher"}
print(f'【{user["name"]}】的工作是【{user["job"]}】')
# ---------
# 控制台输出：【Alice】的工作是【teacher】

# -------------------------------------------------------

# f-string针对多行数据进行格式化
name = "李四"
age = 28
job = "码农"
msg = (
    f'Name：{name}\n'
    f'Age：{age}\n'
    f'Job：{job}\n'
)# 注意msg使用了()进行包裹
print(msg)
# ----------
# 控制台输出：
# Name：李四
# Age：28
# Job：码农

# ------------------------------------------------------

# f-string中调用函数
def my_max(x,y):
    """
    三目运算符比较两个数字大小
    :param x: int x
    :param y: int y
    :return: x和y中较大的数字
    """
    return x if x>6 else y

a = 3
b = 4
print(f'【{a}】和【{b}】中较大的是【{my_max(a,b)}】')
# ----------
# 控制台输出：【3】和【4】中较大的是【4】

# -------------------------------------------

# f-string转义用法
print(f"Python使用{{}}来计算f-string中的变量")
print(f'你真的很\'厉害\'') # 注意：单引号中想继续使用单引号，那就需要进行转义
# ----------
# 控制台输出：
# Python使用{}来计算f-string中的变量
# 你真的很'厉害'

# -----------------------------------------------

# f-string格式化浮点数
val = 11.34567

# 通过后跟浮点数标识，可以实现格式化浮点数
print(f'{val:.3f}')
print(f'{val:.4f}')
# -----------
# 控制台输出：
# 11.346
# 11.3457

# ----------------------------------------------------

# f-string格式化宽度
# i:02表示以0开头，两位
for i in range(1,11):
    print(f'{i:02} {i * i:3} {i * i * i:4}')
# ------------
# 控制台输出：
# 01   1    1
# 02   4    8
# 03   9   27
# 04  16   64
# 05  25  125
# 06  36  216
# 07  49  343
# 08  64  512
# 09  81  729
# 10 100 1000

# ------------------------------------------------------

# f-string对齐字符串
s1 = 'a'
s2 = 'ab'
s3 = 'abc'
s4 = 'abcd'

# 将输出的宽度设置为十个字符。使用 > 符号，让输出结果右对齐。
# 实际上，只要大于最大的字符串长度，就可以实现右对齐
print(f'{s1:>10}')
print(f'{s2:>10}')
print(f'{s3:>10}')
print(f'{s4:>10}')
# 控制台输出：
    #      a
    #     ab
    #    abc
    #   abcd

# -----------------------------------------------------------

# f-string格式化时间
import datetime
now = datetime.datetime.now()
print(f'{now:%Y-%m-%d %H:%M}')
# ----------
# 控制台输出：2026-03-01 15:33

# -----------------------------------------------------------

# f-string接收对象
# 注意：对象必须定义了__str__()或__repr__()函数
class User:
    def __init__(self,name,job):
        self.name = name
        self.job = job

    def __repr__(self):
        return f"{self.name} is a {self.job}"

u = User('Ace','teacher')

print(f'{u}')
# 机制：这是一个 f-string（格式化字符串）。在 Python 中，f-string 在解析 {u} 这个表达式时，也是调用对象的 __str__ 方法（如果未定义 __str__，同样回退到 __repr__）。

# 在本案例中由于没有 __str__，f'{u}' 同样调用了 u.__repr__()，生成了相同的字符串，然后 print 将其输出。

# ---------
print(u)
# 机制：print 函数会直接调用对象的 __str__ 方法（如果未定义 __str__，则调用 __repr__ 作为回退方案）来获取要打印的字符串。

# 本案例中User 类没有定义 __str__，只定义了 __repr__。因此，print(u) 实际上调用的是 u.__repr__()，返回 "Ace is a teacher"。


# --------

# 关键点：__str__ vs __repr__
# 为了更透彻地理解，可以看看这两个魔法方法的区别：

# __repr__：目标是对开发者友好。通常输出一个无歧义的字符串，理想情况下甚至可以直接用来重新创建该对象（例如 User('Ace','teacher')）。
# __str__：目标是对用户友好。输出一个可读性强的、漂亮的字符串。

# ----------

# f-string接收对象
# 注意：对象必须定义了__str__()或__repr__()函数
class User:
    def __init__(self,name,job):
        self.name = name
        self.job = job

    def __str__(self):
        return f"{self.name} is a {self.job}"

u = User('Ace','teacher')
print(u)
# 控制台输出：Ace is a teacher
```