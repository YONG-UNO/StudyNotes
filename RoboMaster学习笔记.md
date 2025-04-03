# c8t6 pwm 呼吸灯
## 1.配置
### 1.1选择芯片
>打开cubemx,选择c8t6
### 1.2 配置时钟
>进入**RCC**选项卡,外部高速时钟(HSE)选择**crystal/cetamic resonator**(晶体谐振器/陶瓷谐振器)
`Bypass Clock Source(旁路时钟):不使用外部晶体与芯片内部时钟驱动组件配合产生时钟信号,而是直接从外界导入时钟信号,就好像芯片内部的时钟驱动组件被旁路了一样`
### 1.3配置GPIO
>配置可以使用定时器的引脚如**PA3,PB12**,
将其配置为**TIM_CH**(如使用定时器3的通道1:**TIM3_CH1**)
### 1.4 配置定时器
#### 1.4.1 **Timers**选项卡
>进入相应定时器"**TIMx**",**使能**相应定时器
#### 1.4.2 配置定时器的基本参数
>**Prescaler（预分频器）**：设置预分频值，用于降低定时器的时钟频率。例如，若系统时钟为 72MHz，设置预分频器为 71，则定时器的时钟频率为72MHz/(71+1)=1MHz

>**Counter Period（计数器周期）**：设置计数器的最大值，决定了 PWM 的周期。例如，设置为 999，则 PWM 周期为1/1MHz×(999+1)=1ms，即 PWM 频率为 1kHz。
#### 配置PWM模式:
`1.选择 “PWM Generation CH1”（PWM 生成通道 1）。
2.设置 “Pulse”（脉冲宽度）为初始值，例如 0，它决定了 PWM 信号的初始占空比。
3.选择 “PWM Mode 1”（PWM 模式 1），在这种模式下，当计数器值小于脉冲宽度时，输出高电平，否则输出低电平。`
### 1.5 生成代码
`点击 “Generate Code” 生成项目代码`

## 2.编写代码实现呼吸灯效果
```bash
#include "main.h"
#include "stm32f1xx_hal.h"

TIM_HandleTypeDef htim3;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM3_Init(void);

int main(void)
{
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
  MX_TIM3_Init();

  // 启动PWM输出
  HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_1);

  uint16_t pulse_value = 0;
  uint8_t direction = 1; // 1表示亮度增加，0表示亮度降低

  while (1)
  {
    if (direction) {
      pulse_value++;
      if (pulse_value >= 999) {
        direction = 0;
      }
    } else {
      pulse_value--;
      if (pulse_value == 0) {
        direction = 1;
      }
    }

    // 设置PWM脉冲宽度
    __HAL_TIM_SET_COMPARE(&htim3, TIM_CHANNEL_1, pulse_value);

    HAL_Delay(1); // 延时1ms，控制呼吸灯变化速度
  }
}

void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSI_DIV2;
  RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL9;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV2;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
  {
    Error_Handler();
  }
}

static void MX_TIM3_Init(void)
{
  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_OC_InitTypeDef sConfigOC = {0};

  htim3.Instance = TIM3;
  htim3.Init.Prescaler = 71;
  htim3.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim3.Init.Period = 999;
  htim3.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim3.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim3, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_PWM_Init(&htim3) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim3, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigOC.OCMode = TIM_OCMODE_PWM1;
  sConfigOC.Pulse = 0;
  sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
  sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
  if (HAL_TIM_PWM_ConfigChannel(&htim3, &sConfigOC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }
  HAL_TIM_MspPostInit(&htim3);
}

static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOA_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_RESET);

  /*Configure GPIO pin : PA5 */
  GPIO_InitStruct.Pin = GPIO_PIN_5;
  GPIO_InitStruct.Mode = GPIO_MODE_AF_PP;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
}
```
## 3 代码讲解与逻辑控制
### 3.1 初始化部分
`在main函数中，首先调用HAL_Init()进行 HAL 库的初始化，然后进行系统时钟配置、GPIO 初始化和定时器初始化。`
### 3.2 启动PWM输出
`使用HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_1)启动定时器 3 的通道 1 的 PWM 输出。`
### 3.3 呼吸灯逻辑
>1.定义一个变量pulse_value用于存储 PWM 脉冲宽度，direction用于表示亮度变化的方向（增加或降低）。
2.在while(1)循环中，根据direction的值增加或减少pulse_value。
3.当pulse_value达到最大值（999）时，将direction设置为 0，表示亮度开始降低；当pulse_value达到最小值（0）时，将direction设置为 1，表示亮度开始增加。
4.使用__HAL_TIM_SET_COMPARE(&htim3, TIM_CHANNEL_1, pulse_value)设置 PWM 的脉冲宽度，从而改变 LED 的亮度。
5.通过HAL_Delay(1)延时 1ms，控制呼吸灯的变化速度。
