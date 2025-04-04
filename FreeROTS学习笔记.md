# FreeRTOS学习笔记
## 1.准备工程和FreeRTOS源码
### 1.1打开现有 stm32f103 基础工程
>确保工程已经正确配置时钟,gpio等外设
创建文件夹**freertos**里面包含**inc,src,port**三个子文件夹
##  1.2 下载freertos源码
从[FreeRTOS官网](https://www.freertos.org/)下载最新版本的源码，并解压到本地。将解压后的文件按照以下结构整理到工程的**freertos**文件夹中：
- **inc**: 包含FreeRTOS的头文件。
- **src**: 包含FreeRTOS的核心源码文件。
- **port**: 包含与目标硬件相关的移植文件。