## cubemx配置
### 1.进入sys
>选择serial wire(swd,串行线调试)
timebase source给一个时钟
![](.\images\1.jpg)

`
Serial Wire：Serial Wire 调试接口通常仅需两根引脚，即 SWDIO（串行数据输入 / 输出）和 SWCLK（串行时钟） 。例如在 STM32 芯片中，常见的是 PA13 用作 SWDIO，PA14 用作 SWCLK。这种较少的引脚占用，对于引脚资源紧张的开发板或项目来说非常有利，能让更多引脚用于其他功能，如连接传感器、执行器等。
`
`
FreeRTOS 是一个实时操作系统内核，它的核心功能之一就是对任务进行调度管理。任务调度依赖于精确的时间度量，时间基准源能为系统提供一个稳定、准确的时间参考。借助时间基准源，系统能够精准地确定任务的执行时间点、执行时长以及任务之间的时间间隔，从而保证任务按照预设的优先级和时间顺序依次执行。
例如，在一个包含多个任务的系统中，有一个周期性执行的传感器数据采集任务和一个显示任务。时间基准源可以确保传感器数据采集任务按照固定的周期执行，并且在采集到数据后，显示任务能及时更新显示内容。
`
### 配置freertos
>在 Mode 页面下，选择 Interface 的版本，这里选择 CMSIS_V1。 CMSIS 是由 Keil 提供的
一套特殊的函数接口，他对 freeRTOS 的功能函数进行了封装，使其变得更加易用， 在使用
freeRTOS 时，不需要再去直接调用 freeRTOS 的函数，只需要调用 CMSIS 为我们提供的
函数即可，目前 CMSIS 有 V1 和 V2 两个版本。
![](.\images\2.jpg)

### 创建任务
![](.\images\3.png)
![](.\images\4.png)
>由于选择了通过__weak 修饰符创建一个弱函数，可以再在别处实现该任务函数。程序执行
时会自动寻找到这个另外实现的任务函数
```c
void red_led_task(void const * argument)
{
    while(1)
    {
        HAL_GPIO_WritePin(LED_R_GPIO_Port, LED_R_Pin, GPIO_PIN_SET);
        osDelay(500);
        HAL_GPIO_WritePin(LED_R_GPIO_Port, LED_R_Pin, GPIO_PIN_RESET);
        osDelay(500);
        HAL_GPIO_WritePin(LED_R_GPIO_Port, LED_R_Pin, GPIO_PIN_RESET);
        osDelay(500);
    }
}
```

### 添加文件
>在工程文件夹下创建User文件夹,并添加相应.c,.h文件
![](.\images\5.png)
![](.\images\6.png)
>创建User Group,添加相应文件与路径
![](.\images\7.png)

### 编写任务函数
![](.\images\7.png)
>默认引脚
![](.\images\8.png)
>自定义引脚名称
![](.\images\9.png)

### 程序流程
![](.\images\10.png)