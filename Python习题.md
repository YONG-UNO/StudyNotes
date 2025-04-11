# 1-1 买苹果 
## 题目:
```bash
阿福在水果店买了 w 公斤苹果，价格为每公斤 p 元，老板心情不错，大气地抹去小数点后的零头。 请问阿福要付多少钱，老板让利多少钱钱？

输入格式:
输入分两行。第1行是一个整数w，表示重量，第2行是一个实数，表示价格p。

输出格式:
在一行中输出两个数：实付（整数）和让利（实数，保留2位小数），之间用一个空格分隔。

输入样例:
3
9.88
输出样例:
29 0.64
```
## 答案

```python
w = int(input())
p = float(input())

total = w * p
real_cost = int(total)
give_away = total - real_cost

print(f"{real_cost} {give_away:.2f}")
```

# 1-2 时间转换输出
## 题目
```bash
输入一个以秒为单位的整数，转换为小时、分和秒输出。输出格式见样例。(以24小时制显示）

输入格式:
输入一个整数。

输出格式:
输出小时 ：分 ：秒，各部分输出之间有一个空格

输入样例:
在这里给出一组输入。例如：

11730
输出样例:
在这里给出相应的输出。例如：

3 : 15 : 30
```
## 答案
```bash
total_seconds = int(input())
#采用模块化设计
hours = (total_seconds // 3600) % 24
#一定要取模24，时钟不能取超过24的小时数
minutes = (total_seconds % 3600 ) // 60
seconds = total_seconds % 60
print(f"{hours} : {minutes} : {seconds}")
```

# 1-3 苹果与虫子
## 题目
>箱子里有n个苹果，还混进了一条虫子（虫子是免费的）。虫子每x小时能吃掉一个苹果，假设虫子在吃完一个苹果之前不会吃另一个（这虫子不错哦），那么经过y小时你还有多少个完整的苹果？

### 输入格式:
输入仅一行，包括三个整数，n，x和y。

### 输出格式:
剩余的完整苹果的个数。

### 输入样例:
在这里给出一组输入。例如：

>10 3 10
### 输出样例:
在这里给出相应的输出。例如：

>6

## 答案
```bash
#读取一行输入用map()
n,x,y = map(int, input().split())
eaten = y  // x
if eaten >= n:
    #刚好全部吃完,结果为0
    result = 0
else:
    #只吃了一小口也会少一个完整的苹果
    if (y/x - y//x) > 0:
        result = n - eaten - 1
    else:
        result = n - eaten
print(result) 
```


# 1-4计算存款利息
## 题目
>本题目要求计算存款利息，计算公式为interest = money×(1+rate) ** year − money，其中interest为存款到期时的利息（税前），money是存款金额，year是存期，rate是年利率。

## 输入格式：
输入在一行中顺序给出三个正实数money、year和rate，以空格分隔。

## 输出格式：
在一行中按“interest = 利息”的格式输出，其中利息保留两位小数。

## 输入样例：
>1000 3 0.025

## 输出样例：
>interest = 76.89

## 答案
```bash
money, year, rate = map(float, input().split())
interest = money * (1+rate) ** year - money
print(f"interest = {interset:.2f}")
```

# 1-5 重要的事情说N遍
## 题目
>对重要的事情，阿福老师会反复说N多遍。
提示：用字符串 * 和 +

### 输入格式:
>输入包含2行，第一行是阿福要重复说多遍的一句话，也就是阿福想强调的重要事情；第2行为一个整数N（0<N<10）

### 输出格式:
>将阿福要强调的重要事情，反复输出N遍，具体格式参见样例。

### 输入样例:
>Go
3
### 输出样例:
>Go!Go!Go!

## 答案
```bash
string = input()
N = int(input())

print((string + "!") * N)
```


# 1-6 阿福的年龄
## 题目
>公元2080的某一天，是阿福生日。大家要给阿福准备生日蜡烛，需要多少？这得看阿福的年龄。还好你有阿福的身份证号码，来算吧~

### 输入格式:
>一个字符串，表示阿福的身份证号码。

### 输出格式:
>一个整数，表示阿福的年龄。

### 输入样例:
>320102201003076937
### 输出样例:
>70

## 答案
```bash
id = input()
year = int(id[6:10])
print(2080 - year)
```

## 1-1 过几天是星期几

>告诉你今天是星期几，请你计算一下过几天后是星期几。程序读入今天的星期数（0、6，分别表示星期日、星期六）和所过的天数，计算并输出未来那一天的星期数。

### 输入格式:
>输入两个整数，分别表示今天星期几（0~6）和所过的天数，两个整数之间用空格隔开。

### 输出格式:
>输出星期数的缩写形式（首字母大写，三个字母末尾有句点）。

### 输入样例:
>6 8
### 输出样例:
>Sun.

## 答案
```python
tuple = ("sun","mon","tue","wed","thu","fri","sat")
today,day = map(int,input().split())
total_day = today + day
week = total_day % 7
print(f"{tuple[week].title()}.")
```

# 1-8号码牌的制作
>打印一个号码牌。号码牌由边框和号码构成，组成边框的字符分为角落字符，水平字符和垂直字符。

## 输入格式:
>输入一个字符（如：+-|2008161876）串，字符串的前三个字符分别代表组成边框的角落字符，水平字符和垂直字符，从第四个字符开始到最后表示号码数字。

## 输出格式:
打印出由边框包围的号码。如：

号码牌1.JPG

## 输入样例:
在这里给出一组输入。例如：

#=$2020
## 输出样例:
>在这里给出相应的输出。例如：

>#====#
$2020$
#====#
## 输入样例:
在这里给出一组输入。例如：

>+-*20191234
## 输出样例:
>在这里给出相应的输出。例如：

>+--------+
*20191234*
+--------+

## 答案
```python
string = input()
eage_up = string[0]
medium_up = string[1]
eage = string[2]
number = string[3:]
print(eage_up + medium_up*len(number) + eage_up)
print(eage + number + eage)
print(eage_up + medium_up*len(number) + eage_up)
```

# 7-9 然后是几点
`有时候人们用四位数字表示一个时间，比如 1106 表示 11 点零 6 分。现在，你的程序要根据起始时间和流逝的时间计算出终止时间。
读入两个数字，第一个数字以这样的四位数字表示当前时间，第二个数字表示分钟数，计算当前时间经过那么多分钟后是几点，结果也表示为四位数字。当小时为个位数时，没有前导的零，例如 5 点 30 分表示为 530；0 点 30 分表示为 030。注意，第二个数字表示的分钟数可能超过 60，也可能是负数。`
### 输入格式：
>输入在一行中给出 2 个整数，分别是四位数字表示的起始时间、以及流逝的分钟数，其间以空格分隔。注意：在起始时间中，当小时为个位数时，没有前导的零，即 5 点 30 分表示为 530；0 点 30 分表示为 030。流逝的分钟数可能超过 60，也可能是负数。

### 输出格式：
>输出不多于四位数字表示的终止时间，当小时为个位数时，没有前导的零。题目保证起始时间和终止时间在同一天内。

### 输入样例：
>1120 110
### 输出样例：
>1310

## 答案
```python
start_time, minutes = map(int, input().split())
start_hour = start_time // 100
start_minute = start_time % 100

total_minutes = start_hour * 60 + start_minute + minutes
new_hour = total_minutes // 60
new_minute = total_minutes % 60

if new_hour < 10:
    new_hour_str = str(new_hour)
else:
    new_hour_str = str(new_hour)

if new_minute < 10:
    new_minute_str = '0' + str(new_minute)
else:
    new_minute_str = str(new_minute)

print(new_hour_str + new_minute_str)
```
# 1-10
>输入3行字符串，然后对其按照说明进行格式化输出

### 输入格式:
>第1行：一个浮点数字符串
第2行：一个整数字符串
第3行：一个非数值型字符串

### 输出格式:
>对浮点数字符串:
第1行： 保留2位小数输出
第2行： 分别输出浮点数的小写字母e的指数形式，大写字母e的指数形式， 百分数形式且其小数部分为2位。每个输出的元素之间以一个空格分隔。

>对于整数：
第3行：在一行分别输出其二进制与小写十六进制，之间以一个空格分隔。

>对非数值型字符串：
首先，去除掉字符串得左右空格。然后输出3行：
第4行，将全部字符转化为大写并输出。
第5行，将字符串右对齐输出，宽度为20。
第6行，将字符串居中输出，宽度20，两侧使用*填充。

>最后:
第7行，将浮点数与整数以浮点数 + 整数 = 结果的形式输出

### 输入样例:
>3.14159265
10
      abc 123     
### 输出样例:
>3.14
3.141593e+00 3.141593E+00 314.16%
1010 a
ABC 123
             abc 123
\*\*\*\*\*\*abc 123*******
3.14159265 + 10 = 13.14159265
## 答案
```python
float_str = input()
int_str = input()
non_num_str = input()

# 处理浮点数
float_num = float(float_str)
print(f"{float_num:.2f}")
print(f"{float_num:e} {float_num:E} {float_num:.2%}")

# 处理整数
int_num = int(int_str)
print(f"{bin(int_num)[2:]} {hex(int_num)[2:]}")

# 处理非数值型字符串
non_num_str = non_num_str.strip()
print(non_num_str.upper())
print(f"{non_num_str:>20}")
print(f"{non_num_str:*^20}")

# 输出浮点数和整数的运算结果
print(f"{float_str} + {int_str} = {float_num + int_num}")
```

# 2-1分段计算居民水费




# 4-1 99乘法表
## 题目:
```bash
1*1=1   
1*2=2   2*2=4   
1*3=3   2*3=6   3*3=9   
1*4=4   2*4=8   3*4=12  4*4=16  
1*5=5   2*5=10  3*5=15  4*5=20  5*5=25  
1*6=6   2*6=12  3*6=18  4*6=24  5*6=30  6*6=36  
1*7=7   2*7=14  3*7=21  4*7=28  5*7=35  6*7=42  7*7=49  
```
>输入在一行中给出一个正整数N（1≤N≤9）。

## 答案
```bash
N = int(input())
#外层循环
for i in range(1,N+1):
    for j in range(1,i+1):
        product = i * j
        #格式化输出,等号左对齐,等号右边占四位
        print(f"{j}*{i}={product:<4}",end="")
    print()
```

# 4-2 查找最后一个250
## 题目
>对方不想和你说话，并向你扔了一串数…… 而你必须从这一串数字中找到“250”这个高大上的感人数字。

## 输入格式:
输入在一行中给出不知道多少个绝对值不超过1000的整数。

## 输出格式:
在一行中输出最后一次出现的“250”是对方扔过来的第几个数字（计数从1开始）。如果没有出现“250”这个数，输出为0。

## 输入样例:
>888 666 123 -233 250 13 250 -222

## 输出样例:
>7

## 答案
```bash
#输入处理：借助 input().split() 读取一行输入，再使用 map(int, ...) 把输入的字符串转换为整数，最终存储在列表 numbers 中。
numbers = list(map(int, input().split()))
#初始化最后一次出现 250 的位置为-1,表示未找到
last_index = 0
#遍历整数列表
for i, num in enumerate(numbers):
    if num == 250:
        last_index = i + 1 # 位置 = 索引 + 1
print(last_index)
```

# 4-3 判断一个整数是否为素数
## 题目
>本题要求编写程序，判断一个给定的整数是否为素数。素数就是只能被1和自身整除的正整数，1不是素数，2是素数。

## 输入格式:
输入在一行中给出一个需要判断的整数 M（−2^31≤M≤2^32 - 1  )

## 输出格式:

如果M是素数，则在一行中输出Yes，否则输出No。如果输入了非正整数，也要输出No。

## 输入样例1:
> 11

## 输出样例1:

>Yes
## 输入样例2:
>9
## 输出样例2:
>No
## 输入样例3:
>-2
## 输出样例3:
>No

## 答案
```bash
# 获取用户输入的整数
M = int(input())

# 判断输入的数是否为非正整数
if M <= 1:
    print('No')
else:
    # 从 2 开始到该数的平方根进行遍历
    # 当M=2时，range（2，2），是一个空范围YES，循环不会执行，输出YES
    for i in range(2, int(M**0.5) + 1):
        # 如果该数能被 2 到其平方根之间的某个数整除
        if M % i == 0:
            print('No')
            break
    else:
        print('Yes')
```

# 4-4 判断素数
>本题的目标很简单，就是判断一个给定的正整数是否素数。

## 输入格式：
输入在第一行给出一个正整数N（≤ 10），随后N行，每行给出一个小于2^31的需要判断的正整数。

## 输出格式：
对每个需要判断的正整数，如果它是素数，则在一行中输出Yes，否则输出No。

## 输入样例：
>2
11
111

## 输出样例：
>Yes
No

## 答案
>全部输入完后，再一起判断
```bash
#定义判断素数函数
def is_prime(num):
    if num <= 1:
        return False
    else:
        for i in range(2, int(num ** 0.5)+1):
            if num % i == 0:
                return False
                break
        else:
            return True

#读取需要判断的数的个数
N = int(input())
results = []
for i in range(N):
    #逐行读取需要判断的数字
    num = int(input())
    if is_prime(num):
        results.append("Yes")
    else:
        results.append("No")

#最后一起输出判断结果
for result in results:
    print(result)
```
>输入一个数字判断一个数字
```bash
#定义判断素数函数
def is_prime(num):
    if num <= 1:
        return False
    else:
        for i in range(2, int(num**0.5)+1):
            if num % i == 0:
                return False
                break
        else:
            return True

#需要判断的素数个数
N = int(input())

#逐个判断是否为素数并依次返回结果
for i in range(N):
    num = int(input())
    if is_prime(num):
        print("Yes")
    else:
        print("No")
```

## 题目
>数学领域著名的“哥德巴赫猜想”的大致意思是：任何一个大于2的偶数总能表示为两个素数之和。比如：24=5+19，其中5和19都是素数。本实验的任务是设计一个程序，验证20亿以内的偶数都可以分解成两个素数之和。

### 输入格式：
输入在一行中给出一个(2, 2 000 000 000]范围内的偶数N。

### 输出格式：
在一行中按照格式“N = p + q”输出N的素数分解，其中p ≤ q均为素数。又因为这样的分解不唯一（例如24还可以分解为7+17），要求必须输出所有解中p最小的解。

## 答案
```python
def is_prime(num):
    if num < 2:
        return False
    for i in range(2, int(num**0.5) + 1):
        if num % i == 0:
            return False
    return True


n = int(input())
for p in range(2, n // 2 + 1):
    q = n - p
    if is_prime(p) and is_prime(q):
        print(f"{n} = {p} + {q}")
        break
```

## 题目
![图片](https://github.com/GG-GIRL/StudyNotes/blob/main/images/3.png)

## 答案
```python
n = int(input())
invalid_isbns = []

for _ in range(n):
    isbn = input()
    if len(isbn) != 10 or not isbn[:9].isdigit():
        invalid_isbns.append(isbn)
        continue
    last_digit = isbn[-1]
    if last_digit == 'X':
        last_digit = 10
    elif not last_digit.isdigit():
        invalid_isbns.append(isbn)
        continue
    else:
        last_digit = int(last_digit)

    total = sum((i + 1) * int(isbn[i]) for i in range(9)) + 10 * last_digit
    if total % 11 != 0:
        invalid_isbns.append(isbn)

if not invalid_isbns:
    print("All passed")
else:
    for isbn in invalid_isbns:
        print(isbn)
```

## 题目
![](https://github.com/GG-GIRL/StudyNotes/blob/main/images/4.png)

## 答案
```python
def digit_sum(num):
    return sum(int(digit) for digit in str(num))


n = int(input())
for _ in range(n):
    num = int(input())
    first_sum = digit_sum(num * 2)
    is_unchanged = True
    for i in range(3, 10):
        current_sum = digit_sum(num * i)
        if current_sum != first_sum:
            is_unchanged = False
            break

    if is_unchanged:
        print(first_sum)
    else:
        print("NO")
```

## 题目
![](https://github.com/GG-GIRL/StudyNotes/blob/main/images/Screenshot%202025-04-11%20230149.png)
## 答案
```python
T = int(input())
for _ in range(T):
    s = input()
    length = len(s)
    has_upper = False
    has_lower = False
    has_digit = False
    has_special = False
    if 6 <= length <= 16 and s[0].isupper():
        for char in s:
            if char.isupper():
                has_upper = True
            elif char.islower():
                has_lower = True
            elif char.isdigit():
                has_digit = True
            elif char in "@#$%^&*":
                has_special = True
        if has_upper and has_lower and has_digit and has_special:
            print("Yes")
        else:
            print("No")
    else:
        print("No")
```
