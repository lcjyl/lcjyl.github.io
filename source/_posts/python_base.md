---
title: Python的一些基础与语法
aplayer: true
cover: https://cdn.jsdelivr.net/gh/lcjyl/images@master/img/202207100305013.jpg
---
{% meting "1383927243" "netease" "song" "autoplay" %}

# format函数的基本用法总结

```python
a = 1
b = 2
c = 3
print("我是{} 哈 {} 哈 {}".format(a, b, c))  # 没有标注指定位置，按默认a-> b -> c
print("我是{2}哈{1}{0} {3}".format(a, b, c, "world")) # 标注了指定位置先输出c -> b -> a ->world

d = 3.1314
print("d保留两位小数位:{:.2f}".format(d))  # 输出3.13
print("d带保留两位小数位(带符号):{:+.2f}".format(d)) # 输出+3.13

print函数默认结尾换行，可通过如下形式修改
print(1, end=" ")
print(2)


e = 10
print("{:0>3d}".format(e)) # 输出001  宽度为3，左边补0。大于3宽度照常输出 0的位置可以为其它字符或数字
print("{:a<3d}".format(e)) # 输出10a  道理同上只不过是在右边补齐  长度足够时候不需要补

f = 102050800
print("{:,}".format(f)) # 输出为 100,000,000 三位三位以逗号分开的形式
print("{:.5e}".format(f)) # 指数计法

g = 0.37565
print("{:.2%}".format(g)) # 输出37.56% 百分比输出.2位保留百分点后两位

h = 1
g = 123456789
print("{:>10d}".format(h)) # 左对齐宽度为10 相当于上面的前面补齐，这里补的是空格
print("{:<10d}".format(h)) # 右对齐

# 居中对齐(恰好左右补空格时候正中间，若多出一个空格会放在后面)如下输出为
# 123456789
#  1  66
#  1 66
print(g)
print("{:^4d}66".format(h)) 
print("{:^3d}66".format(h))

# %格式输出（类似C格式的输出）  不推荐使用
num = 3.131444
print('%0.2f' %num)      # 保留两位小数
print('%5.1f' %num)     # 占5个空格, 右对齐
print('%-5.2f' %num)    # 占5个空格, 左对齐


```



# f-string基本用法

```python
name = "小沐"
age = 20
print(f'我叫{name}，今年{age}岁了。')
# 多行使用
print(f'我叫{name}, '
        f'今年{age}岁了。')
```

#运算符


```python
# +-*类似C
a = 10
b = 4
print(a/b) # 2.5  全输出
print(a//b) # 2取整类似C的/
print(a**b) # 10000 a的b次幂

# a and b 都为真为真    
# a or b 有真为真
# not a 非真为真

# in 和 not in in：如果在指定的序列中找到值返回 True，否则返回 False。 not in：如果在指定的序列中没有找到值返回 True，否则返回 False。
ls = [1,2,3,4,5]
a = 4
print(a not in ls)  # False
print(a in ls) # True

# is 和  is not  is：is 是判断两个标识符是不是引用自一个对象   is not：is not 是判断两个标识符是不是引用自不同对象
a = 2
b = 3
print(a is b) # False
print(a is not b) # True
```



- 注意：python有着及其严谨的缩进要求，代码中若出现空格则不要出现tab对齐（避免移植时候出错，因为Tab对齐不一直为4个空格）， 尽量使用空格 

# 条件语句

```python
a = 6
if a > 5:
    print("a > 5")
elif a < 3:
    print("a < 3")
else:
    print("a在区间[3,5]")
```

# 循环语句

```python
# while循环 while后接判断条件
i = 10
while i>0:
    print(i, end=" ")
    i-=1
# for循环  range[0,10) 左闭右开
for i in range(10):
    print(i)

for s in "abcdefg":
    print(s)
```

- break语句和continue语句类似C中

# pass语句

```python
#  pass 是空语句，是为了保持程序结构的完整性。 pass不做任何事情
for i in "abcdefg":
    if i == 'c':
        pass
        print("this is pass")
    print("The current letter is:{}".format(i))
# 注意在2.x版本中空函数是需要pass语句，在3.x中空函数pass语句可以不写
```

# eval函数

```python
# eval函数将字符串当成有效的表达式，并返回计算结果
a = 10
b = eval('a * 5')
print(b) # 此时输出结果50
```

# input函数

```python
num = input("Please enter a number:") # 里面的文字有没有不影响，是为了提示用户使用
# 注意input默认返回值为字符串，若需转换为数字则需要使用eval函数
print(type(num)) #str
num = eval(num)
print(type(num)) # int
```

# del删除对象

```python
# del删除一个或多个对象
a=1
b=2
print(a)
del a, b
print(a) # error 因为a已删除
```

- ()代表tuple元祖数据类型，元祖是一种不可变序列。创建方法很简单，大多数时候都是小括号括起来的。
  []代表list列表数据类型，列表是一种可变序列。创建方法既简单又特别。
  {}代表dict字典数据类型，没有键值对是集合，字典是Python中唯一内建的映射类型。 
  字典中的值没有特殊的顺序，但都是存储在一个特定的键（key）下。键可以是数字、字符串甚至是元祖。 

# 字典、元组...


```python
s="abcd"

print(s[1:3]) # 输出bc 左闭右开

# 元组中只包含一个元素时，需要在元素后面添加逗号 , ，否则括号会被当作运算符使用： 元组可正反向索引反向索引时最后一个为-1第一个为-len(列表、元组...)
a = (5)
print(type(a))  # int
a = (4,)
print(type(a)) # tuple

t = (1,2,30)
print(t[0])
# t[0] = 2 #error该修改方法是非法的

# 字典 不允许键同名，若键同名，则取最后一个键对应的值
dic={"haha": 6, "dddd":"aaaa"}
print(dic)
print(dic["dddd"])

dic["haha"]=8 # 修改字典
print(dic["haha"])

dic.clear() # 清空字典
del dic # 删除字典

# 集合 创建空集合使用set() 而不是{}  （这表示为空字典) 集合不支持下标索引（为无效集合）

a = {'a', 'a', 'b', 'c'}
print(a) # b c a不重复
a.add(12) # 增加一个元素
print(a)
b = [1, 2, 3]
a.update(b) # 增加元素 b还可以是字典，元组，列表等....
print(a) 
a.remove(1) # 移除1
print(a) 
c = a.pop() # 随机移除一个元素
print(c) # 打印移除的元素
a.clear() # 清空集合

a = [1, 2, 2, 2, 3]
a = set(a) #集合不支持重复 可用来去重
a = list(a)
print(a)

ls=[1, 2, 3]
ln=ls # 公用空间 修改ln时候ls也跟着改变 若不想公用，可创建新的列表用循环append添加到列表中
ln[0] = 11 
print(ls) 
```

# 斐波那契数列示例：第三个数等于前两个数之和，其中第一个0，第一个1

```python
a, b = 0, 1
while b < 100:
    print(b, end=" ")
    a, b = b, a + b
```

# 键值对调示例：字典中键变为值，值变为键

```python
dic = {'a':1, 'b':2, 'c':3}
dic_new = {}
for key, val in dic.items():
    # print(key)  # 打印键的值
    # print(val)  # 打印键对应的值得值 
    dic_new[val] = key
print(dic_new)
```

# 文件管理（简单打开关闭）

```python
textFile = open("E:\编程类\Python学习\\notes\\test.txt", "rt", encoding= 'utf-8') # 表示文本文件方式打开
print(textFile.readline())
textFile.close()

binFile = open("E:\编程类\Python学习\\notes\\test.txt", "rb") # 表示以二进制文件方式打开
print(binFile.readline()) #readline从文件中读入一行内容，如果给出参数，读入该行前size长度的字符串或者字节流
binFile.close()
```

# 函数使用

```python
'''
def 函数名(参数1,参数2,...):
    内容
    return..

调用与直接写函数名和参数
'''

# 三个数相加
def Sum(a, b, c):
  return a + b + c

s = Sum(1, 2, 3)
print(s)
```

# 参考文章

<https://www.runoob.com/python/python-tutorial.html>