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