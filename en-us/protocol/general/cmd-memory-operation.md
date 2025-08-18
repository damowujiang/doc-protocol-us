# 内存操作命令

当前命令为内部使用命令，用于测试&调试:

1. 查询内存状态变量。
2. 设置传感器数据上报
3. 传入传感器数据，测试算法运行结果。
4. 用于实时跟踪数据变化。
5. 为方便快速打开/关闭对应数据的上报，可以直接操作 `key`写入开关值。

## KEY列表

| Value(HEX)  | Parameter                  | 功能描述                  | 可写 | 可读 | 通知 |
| ----------- | -------------------------- | ------------------------- | ---- | ---- | ---- |
| [0x01](#_0x01) | FlashWrite                 | 外部Flash写入             | ✅   | ❌   | ❌   |
| [0x02](#_0x02) | FlashRead                  | 外部Flash读取             | ✅   | ❌   | ❌   |
| [0x03](#_0x03) | FlashGetCRC32              | 外部Flash获取Crc32        | ✅   | ❌   | ❌   |
|             |                            |                           |      |      |      |
| [0x08](#_0x08) | EMFlashRead                | 读取内部Memory内容        | ✅   | ❌   | ❌   |
|             | **传感器原始数据**   |                           |      |      |      |
| [0x10](#_0x10) | SensorReqEnabled           | 订阅/使能传感器的数据上报 | ✅   | ✅   | ❌   |
| [0x11](#_0x11) | AccelDataReport            | 加速度传感器数据报告      | ✅   | ✅   | ✅   |
| [0x12](#_0x12) | GyroscopeDataReport        | 陀螺仪数据报告            | ✅   | ✅   | ✅   |
| [0x13](#_0x13) | CompassDataReport          | 磁力计数据报告            | ✅   | ✅   | ✅   |
| [0x14](#_0x14) | PPGRawDataReport           | PPG的原始数据报告         | ✅   | ✅   | ✅   |
| [0x15](#_0x15) | PressureDataReport         | 气压计数据报告            | ✅   | ✅   | ✅   |
| [0x16](#_0x16) | TemperatureDataReport      | 温度数据报告              | ✅   | ✅   | ✅   |
| [0x17](#_0x17) | batteryFuelGaugeDataReport | 电量计数据上报            | ✅   | ✅   | ✅   |
|             | **算法处理后的数据** |                           |      |      |      |
| [0x80](#_0x80) | heartrateReport            | 心率测量数据上报          | ✅   | ✅   | ✅   |
| [0x81](#_0x81) | spo2Report                 | 血氧测量数据上报          | ✅   | ✅   | ✅   |
| [0x82](#_0x82) | hrvReport                  | HRV测量数据上报           | ✅   | ✅   | ✅   |

---

## 功能描述

### (0x01) Flash 写入数据  :id=0x01

**功能描述**

用于向外部存储写入数据。

!> 注意当前操作可能会误修改已分配区域中的参数或配置。请谨慎使用!!!

**写请求数据格式**

| Byte No. | Parameter | Type  | Converter | Description      |
| -------- | --------- | ----- | --------- | ---------------- |
| 1        | length    | byte  |           | length           |
| 2        | key       | byte  |           | key              |
| 3-6      | address   | uint  |           | 写入内存的地址   |
| 7-n      | data      | array |           | 待写入的内容数据 |

---

### (0x02) Flash读取数据 :id=0x02

**功能描述**

用于从外部存储读取数据。

**读请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description      |
| -------- | --------- | ------ | --------- | ---------------- |
| 1        | length    | byte   |           | length           |
| 2        | key       | byte   |           | key              |
| 3-6      | address   | uint   |           | 读取内存的地址   |
| 7-8      | size      | ushort |           | 待读取的内容长度 |

**读返回数据格式**

| Byte No. | Parameter | Type  | Converter | Description      |
| -------- | --------- | ----- | --------- | ---------------- |
| 1        | length    | byte  |           | length           |
| 2        | key       | byte  |           | key              |
| 3-6      | address   | uint  |           | 读取内存的地址   |
| 7-n      | data      | array |           | 待写入的内容数据 |

---

### (0x03) 外部Flash获取Crc32 :id=0x03

**功能描述**

用于获取外部存储中指定地址的内容的Crc32校验值。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description      |
| -------- | --------- | ------ | --------- | ---------------- |
| 1        | length    | byte   |           | length           |
| 2        | key       | byte   |           | key              |
| 3-6      | address   | uint   |           | 读取内存的地址   |
| 7-8      | size      | ushort |           | 待读取的内容长度 |

**写返回数据格式**

| Byte No. | Parameter | Type   | Converter | Description      |
| -------- | --------- | ------ | --------- | ---------------- |
| 1        | length    | byte   |           | length           |
| 2        | key       | byte   |           | key              |
| 3-6      | address   | uint   |           | 读取内存的地址   |
| 7-8      | size      | ushort |           | 待读取的内容长度 |
| 9-12     | crc32     | uint   |           | Crc32校验值      |

---

### (0x08) EMFlash读取数据 :id=0x08

**功能描述**

用于读取设备内部存储器的内容。

**读请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description               |
| -------- | --------- | ------ | --------- | ------------------------- |
| 1        | length    | byte   |           | length                    |
| 2        | key       | byte   |           | key                       |
| 3-6      | address   | uint   |           | 读取内部 `memory`的地址 |
| 7-8      | size      | ushort |           | 待读取的内容长度          |

---

### (0x10) 订阅/使能传感器数据上报 :id=0x10

**功能描述**
用于订阅设备传感器数据上报。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                                             |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                                                  |
| 2        | key       | byte |           | key                                                                                                                                                                                                     |
| 3-6      | enable    | uint |           | 使能传感器数据上报<br />bit0: accelerator<br />bit1: gyscope<br />bit2: compass<br />bit3: ppg raw data<br />bit4: pressure<br />bit5: temperature<br />bit6: battery fuel gauge<br /><br />others: RFU |

---

### (0x11) 加速度传感器数据报告 :id=0x11

**功能描述**

用于设置或获取加速度传感器的数据上报。
用于上报加速度传感器的数据。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**通知数据格式**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3-10     | timestamp | uint64 |           | 时间戳      |
| 11-12    | x0        | int16  |           | x轴加速度   |
| 13-14    | y0        | int16  |           | y轴加速度   |
| 15-16    | z0        | int16  |           | z轴加速度   |
| ...      | ...       | ...    |           |             |
| n-n+1    | xn        | int16  |           | x轴加速度   |
| n+2-n+3  | yn        | int16  |           | y轴加速度   |
| n+4-n+5  | zn        | int16  |           | z轴加速度   |

note:

1. 注意三轴数据由1个或多个组成，这与传感器的采样率有关。
2. `timestamp`是 `64bits`的时间戳，单位为 `ns`,转为数值即可，表示最后一组数据采集的时间戳。如果需要计算第一组数据的时间戳，配合采样率来进行计算转换。本章中的`64bits`的`timestamp`均为相对时间值。

---

### (0x12)陀螺仪数据报告 :id=0x12

**功能描述**

用于设置或获取陀螺仪传感器的数据上报。
用于上报陀螺仪传感器的数据。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**通知数据格式**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3-10     | timestamp | uint64 |           | 时间戳      |
| 11-12    | x0        | int16  |           | x轴角速度   |
| 13-14    | y0        | int16  |           | y轴角速度   |
| 15-16    | z0        | int16  |           | z轴角速度   |
| ...      | ...       | ...    |           |             |
| n-n+1    | xn        | int16  |           | x轴角速度   |
| n+2-n+3  | yn        | int16  |           | y轴角速度   |
| n+4-n+5  | zn        | int16  |           | z轴角速度   |

note:

1. 注意三轴数据由1组或多组(x/y/z)组成，这与传感器的采样率有关。

---

### (0x13)磁力计数据报告 :id=0x13

**功能描述**

用于设置或获取磁力计传感器的数据上报。
用于上报磁力计传感器的数据。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**通知数据格式**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3-10     | timestamp | uint64 |           | 时间戳      |
| 11-12    | x0        | int16  |           | x轴磁通量   |
| 13-14    | y0        | int16  |           | y轴磁通量   |
| 15-16    | z0        | int16  |           | z轴磁通量   |
| ...      | ...       | ...    |           |             |
| n-n+1    | xn        | int16  |           | x轴磁通量   |
| n+2-n+3  | yn        | int16  |           | y轴磁通量   |
| n+4-n+5  | zn        | int16  |           | z轴磁通量   |

note:

1. 注意三轴数据由1组或多组(x/y/z)组成，这与传感器的采样率有关。

---

### (0x14) PPG原始数据报告 :id=0x14

**功能描述**

用于设置或获取PPG传感器的原始数据上报。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**通知数据格式**


| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3-10     | timestamp | uint64 |           | 时间戳      |
| 11       | ch_num    | byte   |           | 通道数量    |
| 12-n     | value0    | uint16 |           | PPG原始数据 |

!>不同测量模式使用的通道种类不一样，顺序按照G1 、R1、IR1、G2、R2、IR2....顺序排列。（心率使用三通道 G、R、IR，血氧使用二通道 G、IR）
---

### (0x15) 气压计数据报告 :id=0x15

**功能描述**

用于设置或获取气压计传感器的数据上报。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**通知数据格式**

| Byte No. | Parameter   | Type   | Converter | Description                |
| -------- | ----------- | ------ | --------- | -------------------------- |
| 1        | length      | byte   |           | length                     |
| 2        | key         | byte   |           | key                        |
| 3-10     | timestamp   | uint64 |           | 时间戳                     |
| 11-14    | pressure    | uint   |           | 气压计原始数据, 单位为:hPa |
| 15-18    | temperature | float  |           | 温度值, 单位为摄氏度       |

---

### (0x16) 温度数据报告 :id=0x16

**功能描述**

用于设置或获取温度传感器的数据上报。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**通知数据格式**

| Byte No. | Parameter   | Type   | Converter | Description          |
| -------- | ----------- | ------ | --------- | -------------------- |
| 1        | length      | byte   |           | length               |
| 2        | key         | byte   |           | key                  |
| 3-10     | timestamp   | uint64 |           | 时间戳               |
| 11-14    | temperature | float  |           | 温度值, 单位为摄氏度 |

---

### (0x17) 电量计数据上报 :id=0x17

**功能描述**

用于设置或获取电量计的数据上报。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**通知数据格式**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3-6      | timestamp | uint |           | 时间戳      |
| 7        | precent   | byte   |           |电量百分比    |
| 8        | is_charging | byte |           |1:充电，0：不充电  
| 9 -12    | voltage    | uint64 |          |电量电压       |
| 13 -14   | current    | uint16 |          |电流           |
| 15       | health     | byte   |          |电池健康值     |
| 16 -17   | cycle      | uint16 |          |循环次数       |

---

### (0x80) 心率测量上报 :id=0x80

**功能描述**

1. 打开/关闭心率测量过程数据上报。
2. PPG传感器在执行心率测量过程的数据结果上报。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**通知数据格式**

| Byte No. | Parameter | Type | Converter                                 | Description |
| -------- | --------- | ---- | ----------------------------------------- | ----------- |
| 1        | length    | byte |                                           | length      |
| 2        | key       | byte |                                           | key         |
| 3-6      | timestamp | uint |                                           | 时间戳      |
| 7        | status    | byte | 状态：0-已检测到佩带, others-其他错误状态 |             |
| 8        | bpm       | byte |                                           | 心率值      |

---

### (0x81) 血氧测量上报 :id=0x81

**功能描述**

1. 打开/关闭血氧测量过程数据上报。
2. PPG传感器在执行血氧测量过程的数据结果上报。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**通知数据格式**

| Byte No. | Parameter | Type | Converter                                 | Description |
| -------- | --------- | ---- | ----------------------------------------- | ----------- |
| 1        | length    | byte |                                           | length      |
| 2        | key       | byte |                                           | key         |
| 3-6      | timestamp | uint |                                           | 时间戳      |
| 7        | status    | byte | 状态：0-已检测到佩带, others-其他错误状态 |             |
| 8        | spo2      | byte |                                           | 血氧值      |

---

### (0x82) HRV测量上报 :id=0x82

**功能描述**

1. 打开/关闭HRV测量过程数据上报。
2. PPG传感器在执行HRV测量过程的数据结果上报。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                     |
| -------- | --------- | ---- | --------- | ------------------------------- |
| 1        | length    | byte |           | length                          |
| 2        | key       | byte |           | key                             |
| 3        | enabled   | byte |           | 使能<br />0:不上报 <br />1:上报 |

**通知数据格式**

| Byte No. | Parameter | Type | Converter                                 | Description |
| -------- | --------- | ---- | ----------------------------------------- | ----------- |
| 1        | length    | byte |                                           | length      |
| 2        | key       | byte |                                           | key         |
| 3-6      | timestamp | uint |                                           | 时间戳      |
| 7        | status    | byte | 状态：0-已检测到佩带, others-其他错误状态 |             |
| 8        | rri       | byte |                                           | rri         |

!> HRV的测量过程数据待补充。不同传感器携带的数据有区别。
