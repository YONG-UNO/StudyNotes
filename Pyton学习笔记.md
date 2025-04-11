# Python学习笔记
## 1.end参数
### 默认情况
```bash
print("Hello")
print("World")
```
>未对end参数进行设置时,print()函数默认以换行符\n结尾
即:每次调用print后,自动换行
>输出：
```bash
Hello
World
```
### 使用end参数
>通过设置**end**参数，来改变**print()**函数结束时的字符，**end**参数的值可以时任意字符
#### 以空格结尾
```bash
print("Hello", end="")
print("World")
```
>***print()***的**end**参数被设置为一个空格，
故：打印“Hello”后不换行，而是打印一个空格，接着打印World

>输出:
```bash
Hello World
```

#### 以自定义字符串结尾
```bash
print("Apple", end = "-")
print("Banana")
```
>输出
```bash
Apple-Banana
```
### 应用场景
#### 水平打印
>在需要将多个**print()**的输出内容在同一行时,使用end
```bash
fruits = ["Apple","Banana","Cherry"]

for fruit in furits:
    print(fruit, end=",")
```

>输出:
```bash
Apple,Banana,Cherry
```

## 格式化
### 对齐
>{product:<4} 左对齐4位
```bash
1*1=1···
```
>{product:>4} 右对齐4位
```bash
1*1=···1
```
查询对字符串的操作
dir(str)

# 函数function
>函数定义+函数呼叫
def+函数名+(形参):
....函数体(返回值)
函数呼叫(实参)

### 函数的定义
- 1.函数没有形参(不需要传递数据),但要保留**圆括号**.调用时没有实参,也要空括号
- 2.占位语句pass:待完善内容
- 3.函数可以没有返回值.无返回值或retrun后无表达式,则返回**None**

```python
def myprint(): #函数定义,无形参
    print("i'm ok")
    pass #占位语句,内容待完善

myprint()  #函数调用,无实参

#output
i'm ok
None
```

>函数用return可以返回多个值
```python
def f(x,y):
    return x**2,y*3

a,b = f(10)
print(a,b)

#output
(20,30) #以元组形式返回多值
```

>**例**:打印生日歌,过生日要为朋友唱生日歌,歌词为:Happy birthday to [name]
```python
def happy(name):
    print(f"Happy birthday to {name}")
happy("Make")

#output
Happy birthday to Make
```

>**例**:输入a和b,找到[a,b]范围内的所有素数
```python
import math
def isprime(n): #定义函数
    if n == 1: #1不是素数
        return Fales
    for i in range(2,int(math.sqrt(n))+1): #循环找因子
        if n%i == 0:
            return False #有因子,故非素数
    return True #没有因子,是素数

a,b = map(int, input().split())
for n in range(a,b+1):
    if isprime(n):  #调用isprime()函数
        print(n,end=" ")
```

### 匿名函数
>自定义函数的另一种方式:用**lambda表达式**定义一个匿名函数:<函数对象名> = lambda <形参列表> : <表达式>
```python
a,b = 3,5
f = lambda x,y: x**2 + y**2
v = f(a,b)
print(v)

def f(x,y):
    value = x**2 + y**2
    return value

#等价于
def f(x,y):
    value = x**2 + y**2
    return value
```
- 1.lambda是一个**表达式**,而不是一个语句
- 2.作为表达式,lambda返回一个值(即一个新的函数对象)
```python
#lambda允许在不允许def出现的场合(终端环境),来编写函数

>>>(lambda x,y:x+y)(4,5)
>>>9
```

>匿名函数可以作为列表或字典的元素
```python
>>> ls = [lambda x,y: x+y, lambda x,y: x-y, lambda x,y: x*y]
>>> ls[0](3,5)
8
>>> ls[2](3,5)
15
```

>匿名函数可以作为其他函数的参数
```py
>>> ls = [('a', 23), ('b', 14), ('c', 20)]
>>> sorted(ls, key=lambda x:x[1])
[('b',14), ('c',20), ('a',23)] #设置参数key,按元素的第二维数据来排序
```

### 函数的参数
![](.\images\1.png)
>可变函数:dis(1, 3, 5, 6, 7)，在调用时除了给 x 传递 1，给 y 传递 3 外，还传入了 5、6、7，这些额外的值会被 z 以元组的形式接收，即 z 的值为 (5, 6, 7)

- 1.位置参数--函数调用时,按顺序依次传入数据
```py
def f(x,y,z,k):
    print(f"x={x},y={y},z={z},k={k}")
f(1,2,3,4)  #按顺序依次传入
```
- 2.关键字参数--指定参数名

```python
def hello():
    print("hello") #只有包含在缩进的才是函数内容

hello() #呼叫函数
```

>传入参数函数
```python
def hello(name):
    print("hello" + name)

hello("小白") #不写参数会报错

def hello(name,age):
    print("hello" + name + "你今年" + str(age) + "岁")

hello("小白",87)

```

>两个数相加的函数
```python
def add(num1,num2):
    print(num1 + num2)

add(2,3) #输出5
``` 

>回传函数 函数呼叫完,再回传一个值
```python
def add(num1,num2):
    add = num1 + num2
    return add

add(2,3) #函数之间覆盖add(2,3),直接变成数字10

print(add(2,3)) #得到10
```
```python
def add(num1,num2):
    print(num1 + num2)
    return 10

value = add(3,4)
print(value)

#输出7和10
```

>无返回值的函数
```python
def add(num1,num2):
#没有return,python预设None
#函式碰到return直接结束,后面不会进行

print(add(1+2)) #返回None
```

## enumerate()函数  (枚举)
>在 Python 中，enumerate() 是一个内置函数，用于将一个可迭代对象（如列表、元组、字符串等）组合为一个索引序列，同时列出数据和数据下标。它返回的是一个枚举对象，一般会通过 for 循环来遍历使用，语法格式如下：
```python
enumerate(iterable, start=0)
```
>其中，iterable 是必需参数，表示要遍历的可迭代对象；start 是可选参数，表示索引的起始值，默认值为 0。
以下是一些示例代码，展示了 enumerate() 的用法：
```python
# 示例1：遍历列表并获取元素和索引
fruits = ['apple', 'banana', 'cherry']
for index, fruit in enumerate(fruits):
    print(f"索引 {index} 对应的水果是 {fruit}")

# 示例2：指定索引起始值为1
numbers = [10, 20, 30]
for index, num in enumerate(numbers, start=1):
    print(f"第 {index} 个数字是 {num}")

# 示例3：遍历字符串并获取字符和索引
s = "hello"
for index, char in enumerate(s):
    print(f"索引 {index} 对应的字符是 {char}")
```

