# 利用PWM实现LED呼吸灯
>作者：丁勇
## 目录
- 一、PWM是什么？
- 二、呼吸灯实现的原理
  - 1. LED灯的快速响应
  - 2. 人眼的视觉暂留效应
- 三、参数设置
  - 1. 实现的要求
  - 2. 设计思路
  - 3. CubeMx配置
    - 3.1 选择芯片型号
    - 3.2 System Core配置
    - 3.3 Timer配置
    - 3.4 时钟树设置（Clock Configuration）
    - 3.5 Project Manager配置
  - 4. 代码编写
    - 4.1 打开main文件
    - 4.2 将程序设置为自启动模式
    - 4.3 代码部分
- 总结

## 一、PWM是什么？
PWM是Pulse Width Modulation的缩写，中文意思是**脉冲宽度调制**，简称脉宽调制。PWM用于调节脉宽，进而控制电压。其控制电压的原理是依据一个周期内高电平所占该周期的比值。例如，输入电压为5V，若高电平在一个周期内持续时间占比为20%，此时输出电压等效为5V×20% = 1V。这里20%即（一个周期内）高电平持续时间与一个周期的时间之比，该比值被命名为占空比。调节脉冲宽度（即调节占空比），能够改变输出电压。

## 二、呼吸灯实现的原理
PWM实现LED呼吸灯亮度调节**基于LED灯对电流的瞬时响应以及人眼的视觉暂留效应。**
### 1. LED灯的快速响应
LED灯作为半导体器件，对电流变化响应迅速。当PWM信号为高电平时，二极管导通，有电流流过，LED灯获得足够电流发光；当PWM信号为低电平时，二极管截止，无电流流过，LED灯不发光。由于LED响应时间极短，几乎能立即响应电流变化 。
### 2. 人眼的视觉暂留效应
人眼在观察快速变化的图像时，存在“视觉暂留”的生理现象。当光信号快速闪烁，人眼不会立即感知到闪烁，而是将多个短暂光信号在视觉上进行平均或融合，从而感知到一个**平均亮度**水平。

综合以上两点，当PWM信号占空比（高电平时间与整个周期时间的比例）变化时，尽管LED灯实际在不断开关（发光和不发光快速切换），但由于人眼视觉暂留效应，我们感知到的是**平均亮度**。占空比越高，LED灯在一个周期内发光时间越长，平均亮度越高；占空比越低，平均亮度越低。

注：**电流大小控制LED灯亮度**。对于非线性元件二极管（LED是发光二极管），调节占空比可控制高电平和低电平的持续时间，进而控制二极管的导通和截止时间，导通时有电流，截止时无电流。类似PWM调节输出固定电压，调节占空比也能使流过的电流大小改变。假设输出电流为5A，占空比为20%，此时**等效输出电流**为5A×20% = 1A。所以，调节占空比越大，LED灯越亮。

## 三、参数设置
### 1. 实现的要求
在STM32F103C8T6单片机的A0引脚上实现LED的**双向呼吸灯**（灯的亮度变化为：暗 -> 亮 -> 暗）。设置PWM周期为20ms，占空比从0%开始，步进为20%，递增到100%后，再逐级减为0%，并重复该过程。
### 2. 设计思路
占空比从0%开始，所以写入捕获/比较寄存器CCR的初值为0。之后在while循环中调用宏函数`__HAL_TIM_SET_COMPARE`修改CCR的内容，从0开始，逐渐增加到200（依据公式`Duty = （CCR）/（ARR + 1）`可得CCR为200 ），步进值为40。
## 占空比相关计算的原理及过程

要理解占空比、捕获/比较寄存器值（CCR:Crystal/Ceramic Resonator）和自动重装载寄存器值（ARR:Auto-Reload Register）之间是如何计算的，我们先从占空比的定义入手，然后结合公式来分析为什么这里 CCR 能从计算得出。

## 占空比的定义
占空比（Duty Cycle）指的是在一个脉冲周期内，高电平持续时间与整个周期时间的比值，通常用百分比表示。在PWM（脉冲宽度调制）信号中，占空比决定了信号的平均功率，广泛应用于电机调速、灯光亮度控制等场景。

## 计算公式及推导
占空比的计算公式为：
$$
Duty = \frac{t_{high}}{T} \times 100\%
$$
其中 $t_{high}$ 是高电平持续时间，$T$ 是整个脉冲周期。

在定时器产生PWM信号时，定时器计数器从 0 开始计数，当计数值达到 CCR 时，输出电平发生变化；当计数值达到 ARR 时，计数器重新归零，开始下一个周期。因此，高电平持续时间 $t_{high}$ 与 CCR 成正比，整个周期时间 $T$ 与 ARR + 1 成正比（因为计数器从 0 开始计数到 ARR，总共 ARR + 1 个计数值）。由此可以得到：
$$
Duty = \frac{CCR}{ARR + 1} \times 100\%
$$

## 结合具体例子计算
假设我们要实现占空比从 0% 开始逐渐增加，并且已知 ARR 的值（这里假设 ARR = 200），要计算不同占空比对应的 CCR 值。

### 1. 占空比为 0% 时
当占空比 $Duty = 0\%$ 时，代入公式 $\frac{CCR}{ARR + 1} \times 100\% = 0\%$，即 $\frac{CCR}{200 + 1} = 0$，可得出 $CCR = 0$。这意味着在一个周期内，高电平持续时间为 0，也就是没有高电平输出。

### 2. 逐步增加占空比
步进值为 40，意味着每次 CCR 的值增加 40。我们来计算不同 CCR 值对应的占空比：
 - 当 $CCR = 40$ 时，占空比 $Duty=\frac{40}{200 + 1}\times100\%\approx 19.9\%$
 - 当 $CCR = 80$ 时，占空比 $Duty=\frac{80}{200 + 1}\times100\%\approx 39.8\%$
 - 当 $CCR = 120$ 时，占空比 $Duty=\frac{120}{200 + 1}\times100\%\approx 59.7\%$
 - 当 $CCR = 160$ 时，占空比 $Duty=\frac{160}{200 + 1}\times100\%\approx 79.6\%$
 - 当 $CCR = 200$ 时，占空比 $Duty=\frac{200}{200 + 1}\times100\%\approx 99.5\%$ 

## 代码中的体现
在代码里，CCR 从 0 开始，以 40 为步进值逐步增加，直到达到 200，就是按照上述原理来调整占空比的。通过 `__HAL_TIM_SET_COMPARE` 函数修改 CCR 的值，从而改变 PWM 信号的占空比。

综上所述，通过占空比的计算公式以及定时器的工作原理，我们就能算出不同占空比对应的 CCR 值。 

### 3. CubeMx配置
#### 3.1 选择芯片型号
打开CubeMx软件
![](https://i-blog.csdnimg.cn/direct/0a8324a8f6084c2fb445a9ad96442254.png)

在搜索框中输入芯片型号“STM32F103C8”，并选择“STM32F103C8T6”型号

![](https://i-blog.csdnimg.cn/direct/b7b5a668a426459d86ea238eafdb56e7.png)

#### 3.2 System Core配置
1. 点击“SYS”，将Debug选择为“Serial Wire”，其余保持默认设置。

![](https://i-blog.csdnimg.cn/direct/6342c1ed85ff43e78d4109cc4e32df8a.png)

2. 点击“RCC”，将“High Speed Clock（HSE）”选择为“Crystal/Ceramic Resonator”，其余保持默认设置。
![](https://i-blog.csdnimg.cn/direct/a6eb7ce6d76f4b998d5c0b095aa9e68d.png)
##### 2.2 配置时钟
>进入**RCC**选项卡,外部高速时钟(HSE)选择**crystal/cetamic resonator**(晶体谐振器/陶瓷谐振器)
`Bypass Clock Source(旁路时钟):不使用外部晶体与芯片内部时钟驱动组件配合产生时钟信号,而是直接从外界导入时钟信号,就好像芯片内部的时钟驱动组件被旁路了一样`

#### 3.3 Timer配置
1. 在Timers中点击“TIM2”，选择时钟资源“Clock Source”为内部时钟“Internal Clock”。
![](https://i-blog.csdnimg.cn/direct/f1f4e44ffb3847788e66c1b14c50c12e.png)

2. 选择通道1。
![](https://i-blog.csdnimg.cn/direct/b4fecce34dd94be9b469cd8923244fb3.png)

3. 根据公式计算，取PSC为7199，取ARR为199
![](https://i-blog.csdnimg.cn/direct/36e5714e3ff549e3963369ae0719c515.png)

以得到20ms的定时时间。在PSC中填入7199，在ARR中填入199（捕获比较寄存器CCR的初始值为0，无需更改）。
![](https://i-blog.csdnimg.cn/direct/cf6f194eb19a4bcb9d198b6a3b052863.png)

4. 点击“NVIC Settings”，勾选“Enabled”使能定时器2。
![](https://i-blog.csdnimg.cn/direct/bb1777d568894af7abceb94c1e369947.png)

#### 3.4 时钟树设置（Clock Configuration）
点击“Clock Configuration”，将框中的“8”修改为“72”，**按下回车键**
![](https://i-blog.csdnimg.cn/direct/652cb9319f414fc0aeb9e0832fc75323.png)

在弹出的框中选择“OK”。
![](https://i-blog.csdnimg.cn/direct/a1d7952d899d4e209caf6177d2f2211e.png)

#### 3.5 Project Manager配置
1. 输入项目名称“PWM”，将“EWARM”改为“MDK-ARM”。
![](https://i-blog.csdnimg.cn/direct/f35ff84cbe014feb94cf963c5e166c06.png)

2. 将版本更改为V5。
![](https://i-blog.csdnimg.cn/direct/a6dfd982927e4df083740d80efe1f801.png)

3. 点击“Code Generator”，勾选第二个框，然后点击“GENERATE CODE”生成代码
![](https://i-blog.csdnimg.cn/direct/737a40544a044bd4b49fbbc1f17bf8da.png)


点击“Open Project”打开keil5。
![](https://i-blog.csdnimg.cn/direct/3411ea529d28420b98e655dd55e375cd.png)

### 4. 代码编写
#### 4.1 打开main文件
打开“PWM”文件夹，再打开其子文件夹“Application/User”，双击“main.c”开始编写程序。
![](https://i-blog.csdnimg.cn/direct/0a7b73070e6e4f39beafdd32ac4723ce.png)

#### 4.2 将程序设置为自启动模式
1. 点击菜单栏中的“魔术棒”。
![](https://i-blog.csdnimg.cn/direct/92133563c8c24c45b6684efde2650438.png)

2. 点击“Debug”，然后点击“Settings”。
![](https://i-blog.csdnimg.cn/direct/8ce1a9b999894d9baef9b621ed28cbe0.png)

3. 选择“ST-Link Debugger”，点击“Flash Download”，勾选选项“Reset and Run”，再点击“确定”。
![](https://i-blog.csdnimg.cn/direct/d7943873b41f438ba7c0526e4554c156.png)

#### 4.3 代码部分
1. 在“ /* USER CODE BEGIN PTD */ ”和 “ /* USER CODE END PTD */ ”之间添加代码“typedef unsigned char u8;”，将“unsigned char”修改为“u8”，用于方便后续书写。（不修改也行，如果不修改的话要把本文代码中的“u8”写成“unsigned char”）
```c
/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */
typedef unsigned char u8;
/* USER CODE END PTD */
```


2. 以数组的形式定义控制LED灯亮度的变量，使LED亮度变化为：暗 -> 亮 -> 暗；再定义用于锁定数组序列号的变量标志。在“ /* USER CODE BEGIN PV */ ”和“ /* USER CODE END PV */ ”之间添加“u8 DutyStep[] = {0,40,80,120,160,200,160,80,40,0},DutyStep_Index;”
```c
/* USER CODE BEGIN PV */
u8 DutyStep[] = {0,40,80,120,160,200,160,80,40,0},DutyStep_Index;
/* USER CODE END PV */
```

3. 开启定时器2的通道1输出PWM信号，在“ /* USER CODE BEGIN 2 */ ”和“ /* USER CODE END 2 */ ”之间添加“HAL_TIM_PWM_Start(&htim2,TIM_CHANNEL_1);”
```c
/* USER CODE BEGIN 2 */
HAL_TIM_PWM_Start(&htim2,TIM_CHANNEL_1);
/* USER CODE END 2 */
```

4. 每隔200ms（HAL_Delay）修改一次占空比，在“ /* USER CODE BEGIN 3 */ ”和“ /* USER CODE END 3 */ ”间添加代码。


```c
/* USER CODE BEGIN 3 */
for(DutyStep_Index = 0;DutyStep_Index <= 9;DutyStep_Index++){
    __HAL_TIM_SET_COMPARE(&htim2,TIM_CHANNEL_1,DutyStep[DutyStep_Index]);
    HAL_Delay(200);
/* USER CODE END 3 */
}
```
## 总结
以上就是PWM控制LED双向呼吸灯的程序，通过学习，我学会了定时器的配置，理解了STM32和51单片机的相似点和不同点，STM32的HAL库可以用CubeMx进行初始化参数的配置，配置完后将会自动帮你生成代码，很方便，CubeMx有单片机的框图，可以知道哪些引脚被使用、用于什么功能，更直观。