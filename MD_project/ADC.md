### 常量说明

|常量|说明|适用平台|
|----|----|-------|
|ADC.ADC0|ADC通道0| |
|ADC.ADC1|ADC通道1| |
|ADC.ADC2|ADC通道2| |
|ADC.ADC3|ADC通道3| |

### 创建一个ADC对象
>adc = ADC()

- 示例


	>>> from misc import ADC
	>>> adc = ADC()

### ADC功能初始化
>adc.open()

ADC功能初始化。

- 参数

无

- 返回值

成功返回整型0，失败返回整型-1。

### 读取通道电压值
>adc.read(ADCn)

读取指定通道的电压值，单位mV。

- 参数

|参数|参数类型|参数说明|
|----|-------|-------|
|ADCn|int|  |

- 返回值

成功返回指定通道电压值，错误返回整型-1。

- 示例


	>>>adc.read(ADC.ADC0)  #读取ADC通道0电压值
	613
	>>>adc.read(ADC.ADC1)  #读取ADC通道1电压值
	605

### 关闭ADC
>adc.close()

关闭ADC。

- 参数

无

- 返回值

0关闭成功，-1关闭失败。


