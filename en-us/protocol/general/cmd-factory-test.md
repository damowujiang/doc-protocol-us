# 工厂测试 Factory Test(0x80)

## 命令说明

所有设备应当在出厂前进行测试，收集设备工作状态、性能等数据。因此为设备定义一个工作状态：工厂测试模式。

在这个模式下，正常的应用功能可能无法正常运行，因此应用功能应当关闭（比如 SOS 报警、跌倒识别、计步等），需要退出或逐步关闭正在运行的应用程序。

同时，由于工厂测试模式 应用功能关闭，因此工厂测试模式应当加以限制，避免正常使用过程中进入，影响用户正常使用。

+ 对工厂测试模式 加权限，需要获取权限才能进入
+ 对工厂测试模式 限制 时间，开机后 5 分钟内允许 进入
+ 对工厂测试模式 设置存活时长，超过时长后自动退出

对于模块/传感器的 测试命令定义，大多时候遵从：

+ 控制命令，用于设置 模块或传感器 的工作状态
+ 型号信息，用于查询 模块或传感器 当前型号信息
+ 状态数据，用于查询/接收 模块或传感器的 测试模式下的数据变化
+ 其他，根据不同模块或传感器 增加额外的控制或参数配置

注意：在正式版本中，模块/传感器的控制命令 需要  `设备处于工厂测试状态` 才能使用。

---

## KEY列表

| Value(HEX)     | Parameter          | 功能描述                 | 可写 | 可读 | 通知 |
|----------------|--------------------|----------------------|----|----|----|
| [0x10](#_0x10) | AccelGyroMagCtrl   | 加速度/陀螺仪/地磁计 控制命令     | ✅  | ❌  | ❌  |
| [0x11](#_0x11) | AccelGyroMagInfo   | 加速度/陀螺仪/地磁计 传感器信息    | ❌  | ✅  | ❌  |
| [0x12](#_0x12) | AccelGyroMagStatus | 加速度/陀螺仪/地磁计 状态数据     | ❌  | ✅  | ❌  |
|                |                    |                      |    |    |    |
| [0x20](#_0x20) | PressureCtrl       | 气压传感器 控制命令           | ✅  | ❌  | ❌  |
| [0x21](#_0x21) | PressureInfo       | 气压传感器 信息             | ❌  | ✅  | ❌  |
| [0x22](#_0x22) | PressureStatus     | 气压传感器 状态数据           | ❌  | ✅  | ❌  |
|                |                    |                      |    |    |    |
| [0x28](#_0x28) | PpgEcgCtrl         | PPG/ECG 传感器控制命令      | ✅  | ❌  | ❌  |
| [0x29](#_0x29) | PpgEcgInfo         | PPG/ECG 传感器信息        | ❌  | ✅  | ❌  |
| [0x2A](#_0x2A) | PpgEcgResult       | PPG/ECG 测试结果         | ❌  | ❌  | ✅  |
|                |                    |                      |    |    |    |
| [0x30](#_0x30) | TempCtrl           | 温度传感器 控制命令           | ✅  | ❌  | ❌  |
| [0x31](#_0x31) | TempInfo           | 温度传感器 信息             | ❌  | ✅  | ❌  |
| [0x32](#_0x32) | TempStatus         | 温度传感器 状态数据           | ❌  | ✅  | ❌  |
|                |                    |                      |    |    |    |
| [0x38](#_0x38) | BatteryCtrl        | 电量计/电池 控制命令          | ✅  | ❌  | ❌  |
| [0x39](#_0x39) | BatteryInfo        | 电量计/电池 信息            | ❌  | ✅  | ❌  |
| [0x3A](#_0x3A) | BatteryStatus      | 电量计/电池 状态数据          | ❌  | ✅  | ❌  |
|                |                    |                      |    |    |    |
| [0x50](#_0x50) | KeyCtrl            | 按键 命令                | ✅  | ❌  | ❌  |
| [0x51](#_0x51) | KeyInfo            | 按键 信息                | ❌  | ✅  | ❌  |
| [0x52](#_0x52) | KeyStatus          | 按键状态                 | ❌  | ❌  | ✅  |
|                |                    |                      |    |    |    |
| [0x54](#_0x54) | TpCtrl             | TP 控制命令              | ✅  | ❌  | ❌  |
| [0x55](#_0x55) | TpInfo             | TP 信息                | ❌  | ✅  | ❌  |
| [0x56](#_0x56) | TpStatus           | TP 状态数据              | ❌  | ❌  | ✅  |
|                |                    |                      |    |    |    |
| [0x58](#_0x58) | LcdOledCtrl        | LCD/OLED 控制命令        | ✅  | ❌  | ❌  |
| [0x59](#_0x59) | LcdOledInfo        | LCD/OLED 信息          | ❌  | ✅  | ❌  |
|                |                    |                      |    |    |    |
| [0x5C](#_0x5C) | LedCtrl            | LED 控制命令             | ✅  | ❌  | ❌  |
| [0x5D](#_0x5D) | LedInfo            | LED 信息               | ❌  | ✅  | ❌  |
|                |                    |                      |    |    |    |
| [0x60](#_0x60) | MotorCtrl          | 马达 控制命令              | ✅  | ❌  | ❌  |
| [0x61](#_0x61) | MotorInfo          | 马达 信息                | ❌  | ✅  | ❌❌ |
|                |                    |                      |    |    |    |
| [0x80](#_0x80) | FlashInfo          | 外部 Flash 信息          | ❌  | ✅  | ❌  |
|                |                    |                      |    |    |    |
| [0x90](#_0x90) | NfcCtrl            | NFC 控制命令             | ✅  | ❌  | ❌  |
| [0x91](#_0x91) | NfcInfo            | NFC 信息               | ❌  | ✅  | ❌  |
| [0x92](#_0x92) | NfcData            | NFC 读数据              | ✅  | ❌  | ❌  |
|                |                    |                      |    |    |    |
| [0x98](#_0x98) | BleCtrl            | BLE 控制命令             | ✅  | ❌  | ❌  |
| [0x99](#_0x99) | BleInfo            | BLE 信息               | ❌  | ✅  | ❌  |
| [0x9A](#_0x9A) | BleParam           | BLE 设置参数             | ✅  | ❌  | ❌  |
| [0x9B](#_0x9B) | BleAdvScan         | BLE 广播扫描数据           | ❌  | ✅  | ✅  |
|                |                    |                      |    |    |    |
| [0xA0](#_0xA0) | WifiCtrl           | WiFi  控制命令           | ✅  | ❌  | ❌  |
| [0xA1](#_0xA1) | WifiInfo           | WiFi 信息              | ❌  | ✅  | ❌  |
| [0xA2](#_0xA2) | WifiApReport       | WiFi AP 报告           | ❌  | ❌  | ✅  |
|                |                    |                      |    |    |    |
| [0xA8](#_0xA8) | GpsCtrl            | GPS 控制命令             | ✅  | ❌  | ❌  |
| [0xA9](#_0xA9) | GpsInfo            | GPS 信息               | ❌  | ✅  | ❌  |
| [0xAA](#_0xAA) | GpsLocation        | GPS 定位信息             | ❌  | ❌  | ✅  |
| [0xAB](#_0xAB) | GpsSatellites      | GPS 卫星信息             | ❌  | ❌  | ✅  |
|                |                    |                      |    |    |    |
| [0xB0](#_0xB0) | GsmCtrl            | GSM 控制命令             | ✅  | ❌  | ❌  |
| [0xB1](#_0xB1) | GsmInfo            | GSM 信息               | ❌  | ✅  | ❌  |
| [0xB2](#_0xB2) | GsmStatus          | GSM 状态               | ❌  | ✅  | ❌  |
| [0xB3](#_0xB3) | GsmMicSpeakerCtrl  | GSM MiC 和 SPEAKER 控制 | ✅  | ❌  | ❌  |
| [0xB4](#_0xB4) | GsmAudioPlay       | GSM 音频播放             | ✅  | ❌  | ❌  |
|                |                    |                      |    |    |    |
| [0x90](#_0x90) | LoraSub1G          | Lora/Sub 1G          |    |    |    |
|                |                    |                      |    |    |    |
| [0xF0](#_0xF0) | FactoryTestMode    | 进入/退出 工厂测试模式         | ✅  | ✅  | ❌  |

---

## 命令描述

查询命令请使用 `len-key`格式进行查询。参考功能 `命令章节` 中说明。

---

### (0x10)加速度/陀螺仪/磁力计 控制 :id=_0x10

**功能描述**

提供命令控制，打开或关闭 传感器。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                           |
|----------|-----------|------|-----------|---------------------------------------|
| 1        | length    | byte |           | length                                |
| 2        | key       | byte |           | key of Accelerator/Gyroscrope/Magneto |
| 3        | command   | byte |           | command <br/>0 - stop<br/>1 - start   |

---

### (0x11) 加速度/陀螺仪/磁力计 信息 :id=_0x11

**功能描述**

用于查询当前传感器的 id 和型号，用于判断设备当前型号是否一致有效。

**读回复数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                                         |
|----------|-----------|--------|-----------|---------------------------------------------------------------------|
| 1        | length    | byte   |           | length                                                              |
| 2        | key       | byte   |           | key of Accelerator/Gyroscrope/Magneto info                          |
| 3-6      | chipId    | uint32 |           | chip id<br/>传感器 id 数值，一般指从传感器中读取到的 `who_am_i`                       
| 7-n      | model     | String | utf8      | model string,**String** type <br/>传感器模型型号，字符串表示<br/>比如 `"lis2dw\0"` |

---

**异常处理**

+ chipid 为 0，传感器通讯失败，可能未正确焊接
+ chipid 非预期值，传感器型号错误，未支持的型号
+ chipid 正确，但 model 不一致，设备对传感器初始化异常

---

### (0x12) 加速度/陀螺仪/磁力计 状态数据 :id=_0x12

**功能描述**

用于获取加速度/陀螺仪/磁力计 轴的数据。可用于识别传感器工作是否正常。

**读回复数据格式**

| Byte No. | Parameter | Type  | Converter | Description                                  |
|----------|-----------|-------|-----------|----------------------------------------------|
| 1        | length    | byte  |           | length                                       |
| 2        | key       | byte  |           | key of Accelerator/Gyroscrope                |
| 3        | type      | byte  | bits      | type 传感器 id<br/>   bit0： 0-加速度数据无效，1-加速度数据有效 |
| 4-5      | accX      | short |           | Linear acceleration Axis X<br/>X 轴的线性加速度值    |
| 6-7      | accY      | short |           | Linear acceleration Axis Y<br/>Y 轴的线性加速度值    |
| 8-9      | accZ      | short |           | Linear acceleration Axis Z<br/>Z 轴的线性加速度值    |
| 10-11    | gyroPitch | short |           | Angular rate pitch axis X<br/>X 轴上的角速度，俯仰角   |
| 12-13    | gyroRoll  | short |           | Angular rate roll axis Y<br/>Y 轴上的角速度，横滚角    |
| 14-15    | gyroYaw   | short |           | Angular rate yaw axis Z<br/>Z 轴上的角速度，偏航角     |
| 16-17    | magX      | short |           | X 轴上的磁感应强度                                   |
| 18-19    | magY      | short |           | Y 轴上的磁感应强度                                   |
| 20-21    | magZ      | short |           | Z 轴上的磁感应强度                                   |

---

### (0x20) 气压传感器控制 :id=_0x20

**功能描述**

控制气压传感器工作模式。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                                           |
|----------|-----------|------|-----------|-------------------------------------------------------|
| 1        | length    | byte |           | length                                                |
| 2        | key       | byte |           | key of Pressure sensor Control                        |
| 3        | cmd       | byte |           | cmd <br/>   0- stop 停止气压传感器<br/>   1- start 启动气压传感器工作 |

---

### (0x21) 气压传感器信息 :id=_0x21

**功能描述**

查询获取气压传感器的信息。

**读回复数据格式**

| Byte No. | Parameter | Type   | Converter | Description                        |
|----------|-----------|--------|-----------|------------------------------------|
| 1        | length    | byte   |           | length                             |
| 2        | key       | byte   |           | key of Pressure sensor Info        |
| 3-6      | chipId    | uint32 |           | chip id<br/>传感器 ID                 |
| 7-n      | model     | String | utf8      | model, String type<br/>传感器芯片型号 字符串 |

---

### (0x22) 气压传感器状态数据 :id=_0x22

**功能描述**

查询气压传感器状态数据变化

**读回复数据格式**

| Byte No. | Parameter   | Type   | Converter   | Description                                             |
|----------|-------------|--------|-------------|---------------------------------------------------------|
| 1        | length      | byte   |             | length                                                  |
| 2        | key         | byte   |             | key of Pressure sensor state                            |
| 3-6      | pressure    | uint32 |             | pressure value, unit: hPa<br/>气压值，单位为：hPa                  |
| 7-10     | temperature | float  | temperature | temperature, float type, unit: ℃<br/>温度，单精度符号数类型，单位：摄氏度 |

---

### (0x28)PPG 心率/血氧传感器控制 :id=_0x28

**功能描述**

用于启动或停止 PPG。

**读回复数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                                        |
|----------|-----------|--------|-----------|--------------------------------------------------------------------|
| 1        | length    | byte   |           | length                                                             |
| 2        | key       | byte   |           | key of PPG control                                                 |
| 3        | cmd       | byte   |           | cmd 控制命令 <br/>0: 停止<br/>1:  启动心率测量 <br/>2:  启动血氧测量<br/>255： 启动工厂测试 |
| 4-n      | testParam | byte[] |           | 工厂测试携带参数                                                           |

+ 不同的传感器携带的工厂测试参数不一样，根据不同传感器选择对应的工厂测试参数

**工厂测试参数**

+ GH3011 工厂测试参数

工厂测试具体使用，参考 `GH3011整机测试说明 `文档

| Byte(s) | Parameter   | Type | Converter | Description   |
|---------|-------------|------|-----------|---------------|
| 0x01    | commTest    | byte |           | 通讯测试          |
| 0x02    | otpTest     | byte |           | OTP 测试        |
| 0x04    | ctrTest     | byte |           | CTR 测试        |
| 0x08    | leakTest    | byte |           | LEAK 测试       |
| 0x1C    | ctrLeakTest | byte |           | CTR 和 LEAK 测试 |
| 0x1F    | allTest     | byte |           | 全部测试          |

---

+ PAH8151 工厂测试参数

| Byte(s) | Parameter   | Type | Converter | Description |
|---------|-------------|------|-----------|-------------|
| 0x01    | capTest     | byte |           | cap测试       |
| 0x02    | intTest     | byte |           | INT测试       |
| 0x04    | fpcGrayTest | byte |           | fpc整机灰卡测试   |
| 0x08    | leakTest    | byte |           | 整机漏光测试      |
| 0x10    | grayTest    | byte |           | 整机灰卡测试      |
| 0x20    | touchTest   | byte |           | touch测试     |
| 0x3F    | allTest     | byte |           | 全部测试        |

---

### (0x29) PPG 心率/血氧 传感器信息 :id=_0x29

**功能描述**

查询 PPG 心率/血氧 传感器 信息。

**读回复数据格式**

| Byte No. | Parameter | Type   | Converter | Description      |
|----------|-----------|--------|-----------|------------------|
| 1        | length    | byte   |           | length           |
| 2        | key       | byte   |           | key of PPG state |
| 3-6      | chipId    | uint32 |           | chipid           |
| 7-n      | model     | String | utf8      | model            |

---

### **(0x2A) PPG 心率/血氧 传感器通知** :id=_0x2A

**功能描述**

当启动 PPG 工作模式后，通过当前 `key`来异步通知测试过程变化。

当接收到 PPG 停止命令后，不会使用当前 `key` 来通知。

**通知数据格式**

+ 心率测量数据格式

| Byte No. | Parameter     | Type   | Converter | Description                                                                       |
|----------|---------------|--------|-----------|-----------------------------------------------------------------------------------|
| 1        | length        | byte   |           | length                                                                            |
| 2        | key           | byte   |           | key of PPG state                                                                  |
| 3        | type          | byte   |           | type = 1                                                                          |
| 4-7      | dateTime      | uint32 | unixTime  | 时间戳，单位：秒<br/>_`<font style="color:#DF2A3F;">`用于判断数据是否有更新，相同的时间戳表示数据未变化 `</font>`_ |
| 8        | heartRate     | byte   |           | 心率值，单位为：bpm (beat per minute)                                                     |
| 9        | signalQuality | byte   |           | 当前信号质量，数值越大，信号质量越好                                                                |

+ 血氧测量数据格式

| Byte No. | Parameter     | Type   | Converter | Description                                                                       |
|----------|---------------|--------|-----------|-----------------------------------------------------------------------------------|
| 1        | length        | byte   |           | length                                                                            |
| 2        | key           | byte   |           | key of PPG state                                                                  |
| 3        | type          | byte   |           | type = 2                                                                          |
| 4-7      | dateTime      | uint32 | unixTime  | 时间戳，单位：秒<br/>_`<font style="color:#DF2A3F;">`用于判断数据是否有更新，相同的时间戳表示数据未变化 `</font>`_ |
| 8        | spo2          | byte   |           | 血氧值，表示血氧含量，百分比                                                                    |
| 9        | signalQuality | byte   |           | 当前信号质量，数值越大，信号质量越好                                                                |

+ GH3011 工厂测试数据格式

| Byte No. | Parameter  | Type   | Converter | Description                                                                                                                                                                     |
|----------|------------|--------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1        | length     | byte   |           | length                                                                                                                                                                          |
| 2        | key        | byte   |           | key of PPG state                                                                                                                                                                |
| 3        | type       | byte   |           | type = 255                                                                                                                                                                      |
| 4-7      | dateTime   | uint32 | unixTime  | 时间戳，单位：秒<br/>_`<font style="color:#DF2A3F;">`用于判断数据是否有更新，相同的时间戳表示数据未变化 `</font>`_                                                                                               |
| 8        | testResult | byte   |           | 测试结果 <br/>0: OK<br/>1: 初始化错误 <br/>2: 顺序错误<br/>3: 通讯读操作错误 <br/>4: 通讯写操作错误<br/>5: OTP 读错误 <br/>6: CTR 未通过<br/>7: 原始数据未通过 <br/>8: 噪声未通过<br/>9: 漏光未通过 <br/>10: 漏光率 不通过<br/>11: 资源错误 |

+ PAH8151 工厂测试数据格式

| Byte No. | Parameter      | Type   | Converter | Description                                                                             |
|----------|----------------|--------|-----------|-----------------------------------------------------------------------------------------|
| 1        | length         | byte   |           | length                                                                                  |
| 2        | key            | byte   |           | key of PPG state                                                                        |
| 3        | type           | byte   |           | type = 255                                                                              |
| 4-7      | dateTime       | uint32 | unixTime  | 时间戳，单位：秒``_`<font style="color:#DF2A3F;">`用于判断数据是否有更新，相同的时间戳表示数据未变化 `</font>`_          |
| 8        | testResult     | byte   | bits      | 测试结果 0：0k，bit为1的代表错误，bit0：cap 失败，bit1：INT 失败， bit2：FPC 失败，bit3：漏光失败，bit4：灰卡失败，bit5：佩戴失败 |
| 9-10     | fpcGrayPpg1    | ushort |           | fpc灰卡ppg1                                                                               |
| 11-12    | fpcGrayPpg2    | ushort |           | fpc灰卡ppg2                                                                               |
| 13-14    | fpcGrayPpg3    | ushort |           | fpc灰卡ppg3                                                                               |
| 15-16    | deviceGrayPpg1 | ushort |           | 整机灰卡ppg1                                                                                |
| 17-18    | deviceGrayPpg2 | ushort |           | 整机灰卡ppg2                                                                                |
| 19-20    | deviceGrayPpg3 | ushort |           | 整机灰卡ppg3                                                                                |
| 21-22    | deviceLeakPpg1 | ushort |           | 整机漏光ppg1                                                                                |
| 23-24    | deviceLeakPpg2 | ushort |           | 整机漏光ppg2                                                                                |
| 25-26    | deviceLeakPpg3 | ushort |           | 整机漏光ppg3                                                                                |
| 27-30    | capIndex1      | float  |           | cap index1                                                                              |
| 31-34    | capIndex2      | float  |           | cap index2                                                                              |

---

### (0x30) 温度传感器控制 :id=_0x30

**功能描述**

用于开启或停止 温度传感器。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                              |
|----------|-----------|------|-----------|------------------------------------------|
| 1        | length    | byte |           | length                                   |
| 2        | key       | byte |           | key of temperature control               |
| 3        | cmd       | byte |           | cmd 控制命令 <br/>0: 停止温度传感器<br/>1:  启动温度传感器 |

---

### (0x31)温度传感器信息 :id=_0x31

**功能描述**

用于获取温度传感器型号相关信息。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                |
|----------|-----------|--------|-----------|--------------------------------------------|
| 1        | length    | byte   |           | length                                     |
| 2        | key       | byte   |           | key of temperature                         |
| 3-6      | chipId    | uint32 |           | 芯片 id <br/>0 -无效 id<br/>others - 芯片的 id 数据 |
| 7-n      | model     | String | utf8      | model 传感器型号, String 类型                     |

---

### (0x32)温度传感器状态&数据 :id=_0x32

---

**功能描述**

查询温度传感器工作过程中的状态和数据。

**写请求数据格式**

| Byte No. | Parameter   | Type  | Converter   | Description                                             |
|----------|-------------|-------|-------------|---------------------------------------------------------|
| 1        | length      | byte  |             | length                                                  |
| 2        | key         | byte  |             | key of temperature                                      |
| 3-6      | temperature | float | temperature | temperature, float type, unit: ℃<br/>温度，单精度符号数类型，单位：摄氏度 |

---

### (0x38)电量计/电源控制 :id=_0x38

---

**功能描述**

用于控制电源工作方式，比如断开充电、允许充电等。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                   |
|----------|-----------|------|-----------|-------------------------------|
| 1        | length    | byte |           | length                        |
| 2        | key       | byte |           | key                           |
| 3        | cmd       | byte |           | cmd <br/>0- stop<br/>1- start |

---

### (0x39)电量计/电源信息 :id=_0x39

**功能描述**

获取当前电量计/电源管理信息。对于使用 ADC 采集电压后转换为电量百分比的，也需要输出提示当前电源管理方式。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                                  |
|----------|-----------|--------|-----------|--------------------------------------------------------------|
| 1        | length    | byte   |           | length                                                       |
| 2        | key       | byte   |           | key                                                          |
| 3-6      | chipId    | uint32 |           | 芯片 id <br/>0 - 无效 id<br/>1 - ADC<br/>others - 电量计/PMIC 芯片 id |
| 7-n      | model     | String | utf8      | model 传感器型号, String 类型                                       |

---

### (0x3A)电量计/电源状态&数据 :id=_0x3A

**功能描述**

获取电量计/PMIC/电源管理 的状态及数据。

**写请求数据格式**

| Byte No. | Parameter     | Type   | Converter | Description                   |
|----------|---------------|--------|-----------|-------------------------------|
| 1        | length        | byte   |           | length                        |
| 2        | key           | byte   |           | key of battery                |
| 3        | batteryLevel  | byte   |           | 电量百分比                         |
| 4        | chargeStatus  | byte   |           | 状态 <br/>0 - 未在充电<br/>1 - 正在充电 |
| 5-8      | voltage       | uint32 |           | 当前电池电压，单位为:mV                 |
| 9-10     | current       | short  |           | 电流，单位为:mA (int16_t)           |
| 11-12    | chargeVoltage | ushort |           | 充电电压，单位为:mV                   |
| 13       | health        | byte   |           | 电池健康度 0-100                   |
| 14-15    | chargeCount   | ushort |           | 充电次数                          |

---

### (0x50)按键控制器控制 :id=_0x50

**功能描述**

控制按键芯片工作状态。如果当前只是通过外部中断进行按键识别，当前 `key`不使用。

当前 `key`预留给 `IO` 扩展芯片使用。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                   |
|----------|-----------|------|-----------|-------------------------------|
| 1        | length    | byte |           | length                        |
| 2        | key       | byte |           | key of battery                |
| 3        | cmd       | byte |           | cmd <br/>0- stop<br/>1- start |

---

### (0x51)按键控制器信息 :id=_0x51

**功能描述**

获取按键控制器的信息。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                             |
|----------|-----------|--------|-----------|---------------------------------------------------------|
| 1        | length    | byte   |           | length                                                  |
| 2        | key       | byte   |           | key                                                     |
| 3-4      | keyCount  | ushort |           | 按键数，表示当前设备上支持的按键数<br/>范围:[1,n] 最多支持 32 个按键              |
| 5-8      | chipId    | uint32 |           | 芯片 id <br/>0- 无效 id<br/>1- 外部中断<br/>others- 从芯片内部读取到 ID |
| 9-n      | model     | String | utf8      | model 传感器型号, String 类型                                  |

---

### (0x52)按键控制器状态&数据 :id=_0x52

**功能描述**

当启动按键工作时，在工厂测试模式下，按键状态数据会主动通知上位机。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                                            |
|----------|-----------|--------|-----------|------------------------------------------------------------------------|
| 1        | length    | byte   |           | length                                                                 |
| 2        | key       | byte   |           | key                                                                    |
| 3-6      | dateTime  | uint32 | unixTime  | timestamp 按键变化时间戳                                                      |
| 7-10     | state     | uint32 | bits      | state, 按键状态，每个 bit 表示一个按键 <br/>bit = 0, 表示当前按键松开<br/>bit = 1, 表示当前按键按下 |

---

### (0x54)TP 控制 :id=_0x54

**功能描述**

设置 TP 的工作方式。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                                                                                     |
|----------|-----------|------|-----------|-------------------------------------------------------------------------------------------------|
| 1        | length    | byte |           | length                                                                                          |
| 2        | key       | byte |           | key                                                                                             |
| 3        | cmd       | byte |           | cmd <br/>0- stop<br/>1- normal <br/>2- sleep<br/>3- brick test 内置的方块测试<br/>4- line test 内置的画线测试 |

+ brick test/line test ，设备端需要显示对应测试界面，让测试人员更方便操作。详细查看<< 触摸板TP测试说明文档>>

---

### (0x55)TP 信息 :id=_0x55

**功能描述**

用于查询 TP 芯片 的信息。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description             |
|----------|-----------|--------|-----------|-------------------------|
| 1        | length    | byte   |           | length                  |
| 2        | key       | byte   |           | key                     |
| 3-6      | chipId    | uint   |           | 芯片 id                  |
| 7-10     | fw_ver    | uint   |           | 固件版本                 |

| 5-n      | model     | String | utf8      | model 芯片具体型号, String 类型 |

---

### (0x56)TP 状态&数据 :id=_0x56

**功能描述**

当设置 TP 工作时，TP 主动通知按下或松开状态信息。

**写请求数据格式**

| Byte No. | Parameter | Type  | Converter | Description                 |
|----------|-----------|-------|-----------|-----------------------------|
| 1        | length    | byte  |           | length                      |
| 2        | key       | byte  |           | key                         |
| 3        | status    | byte  |           | status <br/>0- 松开<br/>1- 按下 |
| 4-5      | x         | short |           | X 坐标                        |
| 6-7      | y         | short |           | Y 坐标                        |

+ status = 0 时，此时坐标值无意义。

---

### (0x58)LCD/OLED 控制 :id=_0x58

---

**功能描述**

设置 LCD/OLED 等显示的工作方式。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                                                                               |
|----------|-----------|--------|-----------|-----------------------------------------------------------------------------------------------------------|
| 1        | length    | byte   |           | length                                                                                                    |
| 2        | key       | byte   |           | key                                                                                                       |
| 3        | cmd       | byte   |           | cmd <br/>0- power off<br/>1- power on <br/>2- 自动轮播内置纯色背景<br/>3- 手动轮播内置纯色背景<br/>255- 自定义颜色显示 （内置颜色为 红绿蓝白黑） |
| 4-7      | argb      | uint32 | leHex     | ARGB，十六进制颜色数值，仅自定义颜色显示使用，其他命令此参数不生效、不作用                                                                   |

+ 自定义颜色用于临时指定屏幕颜色。

---

### (0x59)LCD/OLED 信息 :id=_0x59

**功能描述**

获取 LCD/OLED 内部 LCM 的信息。

部分设备由于没有读接口，无法直接获取芯片 ID，可以在初始化参数时手动指定或者自填写。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                  |
|----------|-----------|--------|-----------|----------------------------------------------|
| 1        | length    | byte   |           | length                                       |
| 2        | key       | byte   |           | key                                          |
| 3-5      | chipId    | uint24 |           | LCM chip id <br/>0- 无效 id<br/>others- LCM id |
| 6-7      | width     | ushort |           | 分辨率宽度, 单位为: pixel                            |
| 8-9      | height    | ushort |           | 分辨率高度, 单位为: pixel                            |
| 10-n     | model     | String | utf8      | model 芯片具体型号, String 类型                      |

---

### (0x5C)LED 控制 :id=_0x5C

**功能描述**

设置及控制 LED 的工作方式。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                         |
|----------|-----------|--------|-----------|-----------------------------------------------------|
| 1        | length    | byte   |           | length                                              |
| 2        | key       | byte   |           | key                                                 |
| 3        | ledIndex  | byte   |           | LED 序号                                              |
| 4        | type      | byte   |           | type <br/>[0,254]- 使用内置的显示参数控制<br/>255- 自定义闪烁方式     |
| 5-6      | delay     | ushort |           | delay，unit: ms<br/>参数延时生效时长                         |
| 7-8      | on        | ushort |           | on，unit: ms<br/>LED 亮时长                             |
| 9-10     | off       | ushort |           | off，unit: ms<br/>LED 灭时长                            |
| 11-14    | repeat    | uint32 |           | repeat，unit: times<br/>LED on/off 重复次数              |
| 15-16    | sleep     | ushort |           | sleep，unit: ms<br/>LED 重复次数结束后，延时到下一个周期时长           |
| 17-20    | cycle     | uint32 |           | cycle，unit: times<br/>需要执行 repeat[on,off]-sleep 的次数 |

+ 自定义控制格式参数格式：1000, 100,200, 2, 500, 10

LED 的状态如下，方括号内的动作要执行 10 次：

off(1000ms)-[on(100ms)-off(200ms)-on(100ms)-off(200ms)-sleep(500)] * 10

---

### (0x5D)LED 信息 :id=_0x5D

**功能描述**

获取 LED 控制器的信息。如果没有 LED 扩展控制芯片，则手动填写。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                                                   |
|----------|-----------|--------|-----------|-------------------------------------------------------------------------------|
| 1        | length    | byte   |           | length                                                                        |
| 2        | key       | byte   |           | key                                                                           |
| 3-6      | chipId    | uint32 |           | chip id <br/>0- invalid id<br/>1- GPIO 控制 <br/>2- PWM 控制<br/>3- GPIO/PWM 共同控制 |
| 7-n      | model     | String | utf8      | model 芯片具体型号, String 类型 <br/>没有控制芯片，可填写<br/>"gpio\0" <br/>"pwm\0" <br/>"gpio  |

---

### (0x60)马达控制 :id=_0x60

**功能描述**

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                         |
|----------|-----------|--------|-----------|-----------------------------------------------------|
| 1        | length    | byte   |           | length                                              |
| 2        | key       | byte   |           | key                                                 |
| 3        | type      | byte   |           | type <br/>[0,254]- 使用内置的显示参数控制<br/>255- 自定义闪烁方式     |
| 4-5      | delay     | ushort |           | delay，unit: ms<br/>参数延时生效时长                         |
| 6-7      | on        | ushort |           | on，unit: ms<br/>振动时长                                |
| 8-9      | off       | ushort |           | off，unit: ms<br/>停止时长                               |
| 10-13    | repeat    | uint32 |           | repeat，unit: times<br/>on/off 重复次数                  |
| 14-15    | sleep     | ushort |           | sleep，unit: ms<br/> 重复次数结束后，延时到下一个周期时长              |
| 16-19    | cycle     | uint32 |           | cycle，unit: times<br/>需要执行 repeat[on,off]-sleep 的次数 |

+ 自定义振动参数可参考 `(0x5C)LED 控制`中说明，原理是一样的。

---

### (0x61)马达信息 :id=_0x61

**功能描述**

获取马达控制器的信息。

马达控制有以下方式：

+ GPIO
+ PWM
+ 线性马达（总线通讯控制）

**写请求数据格式**

| Byte No. | Parameter    | Type   | Converter | Description                                                          |
|----------|--------------|--------|-----------|----------------------------------------------------------------------|
| 1        | length       | byte   |           | length                                                               |
| 2        | key          | byte   |           | key                                                                  |
| 3-6      | controllerId | uint32 |           | 控制器 id <br/>0- invalid id<br/>1- GPIO <br/>2- PWM<br/>others- 控制器 id |
| 7-n      | model        | String | utf8      | model 芯片具体型号, String 类型 <br/>没有控制芯片，可填写<br/>"gpio\0" <br/>"pwm\0"    |

---

### (0x80)外部存储信息 :id=_0x80

**功能描述**

查询获取外部存储器信息。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description             |
|----------|-----------|--------|-----------|-------------------------|
| 1        | length    | byte   |           | length                  |
| 2        | key       | byte   |           | key                     |
| 3-6      | chipId    | uint32 |           | chip id                 |
| 7-n      | model     | String | utf8      | model 芯片具体型号, String 类型 |

---

### (0x90)NFC 控制 :id=_0x90

**功能描述**

控制 NFC 的工作模式。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                                        |
|----------|-----------|------|-----------|----------------------------------------------------|
| 1        | length    | byte |           | length                                             |
| 2        | key       | byte |           | key                                                |
| 3        | cmd       | byte |           | cmd <br/>0- 关闭 NFC 通讯，由线圈供电<br/>1- 打开 NFC 通讯，由内部供电 |

---

### (0x91)NFC 信息 :id=_0x91

**功能描述**

获取 NFC 芯片的信息。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description             |
|----------|-----------|--------|-----------|-------------------------|
| 1        | length    | byte   |           | length                  |
| 2        | key       | byte   |           | key                     |
| 3-6      | chipId    | uint32 |           | 芯片                      |
| 7-n      | model     | String | utf8      | model 芯片具体型号, String 类型 |

---

### (0x92)NFC 数据 :id=_0x92

---

**功能描述**

提供接口，访问 NFC 内部数据。在使用此接口前，请先使用控制命令打开 NFC 内部供电。

**写请求数据格式**

| Byte No. | Parameter  | Type   | Converter | Description              |
|----------|------------|--------|-----------|--------------------------|
| 1        | length     | byte   |           | length                   |
| 2        | key        | byte   |           | key of nfc               |
| 3        | cmd        | byte   |           | cmd <br/>0- 读取<br/>1- 写入 |
| 4-7      | dataLength | uint32 |           | length 写数据长度             |
| 8-n      | data       | byte[] |           | hex data 写到的十六进制数据       |

**写返回数据格式**

| Byte No. | Parameter  | Type   | Converter | Description              |
|----------|------------|--------|-----------|--------------------------|
| 1        | length     | byte   |           | length                   |
| 2        | key        | byte   |           | key of nfc               |
| 3        | cmd        | byte   |           | cmd <br/>0- 读取<br/>1- 写入 |
| 4-7      | dataLength | uint32 |           | datalength ()读数据长度         |
| 8-n      | data       | byte[] |           | hex data 读到的十六进制数据       |

---

### (0x98)BLE 命令 :id=_0x98

**功能描述**

控制 BLE 的工作方式。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                  |
|----------|-----------|------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------|
| 1        | length    | byte |           | length                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                          |
| 3        | cmd       | byte |           | cmd <br/>0- stop adv & scan 同时停止广播和扫描<br/>1- start advertising 开始广播   2- stop advertising 停止广播 <br/>3- start scan 开始扫描<br/>4- stop scan 停止扫描 |

---

### (0x99)BLE 信息 :id=_0x99

**功能描述**

查询获取当前 BLE 模组信息。BLE 一般方式：

+ SOC 自带 BLE 射频模块
+ 外挂的 BLE 模组

**读返回数据格式**

| Byte No. | Parameter | Type   | Converter | Description             |
|----------|-----------|--------|-----------|-------------------------|
| 1        | length    | byte   |           | length                  |
| 2        | key       | byte   |           | key                     |
| 3-8      | mac       | MAC    | leMac     | 设备 MAC 地址               |
| 9-n      | model     | String | utf8      | model 芯片具体型号, String 类型 |

---

### (0x9A)BLE 参数设置 :id=_0x9A

**功能描述**

临时设置 BLE 的工作参数。可设置的参数有：

+ phy- BLE 的数据编码速率
+ rssi- BLE 扫描的 rssi 阈值
+ advdata- 广播数据，只允许设置广播数据，不允许设置 scan response data

**写请求数据格式**

+ 设置 phy

| Byte No. | Parameter | Type | Converter | Description                                                        |
|----------|-----------|------|-----------|--------------------------------------------------------------------|
| 1        | length    | byte |           | length                                                             |
| 2        | key       | byte |           | key                                                                |
| 3        | type      | byte |           | type = 0                                                           |
| 4        | phy       | byte |           | phy <br/>0- auto Phy<br/>1- 1M Phy <br/>2- 2M Phy<br/>4- coded Phy |

+ 设置 rssi

| Byte No. | Parameter | Type  | Converter | Description              |
|----------|-----------|-------|-----------|--------------------------|
| 1        | length    | byte  |           | length                   |
| 2        | key       | byte  |           | key                      |
| 3        | type      | byte  |           | type = 1                 |
| 4        | rssi      | sbyte |           | rssi, 有符号数，范围: [-127, 0] |

+ 设置 advdata

| Byte No. | Parameter  | Type   | Converter | Description                                    |
|----------|------------|--------|-----------|------------------------------------------------|
| 1        | length     | byte   |           | length                                         |
| 2        | key        | byte   |           | key                                            |
| 3        | type       | byte   |           | type = 2                                       |
| 4-n      | advRawData | byte[] |           | adv raw data，**当前限制最大 31 字节**<br/>BLE 广播原始数据格式 |

---

### (0x9B)BLE 广播包 :id=_0x9B

**功能描述**

设备扫描到的广播通知上位机。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description       |
|----------|-----------|--------|-----------|-------------------|
| 1        | length    | byte   |           | length            |
| 2        | key       | byte   |           | key               |
| 3        | advType   | byte   |           | type 广播数据类型，当前不解析 |
| 4        | rssi      | sbyte  |           | rssi 广播信号强度       |
| 5-10     | mac       | MAC    | leMac     | mac 广播设备的 MAC 地址  |
| 11-n     | advData   | byte[] |           | advdata 广播数据包     |

---

### (0xA0)WiFi 控制 :id=_0xA0

**功能描述**

设置 WiFi 模块的工作方式。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                                          |
|----------|-----------|------|-----------|------------------------------------------------------|
| 1        | length    | byte |           | length                                               |
| 2        | key       | byte |           | key                                                  |
| 3        | cmd       | byte |           | cmd <br/>0- power off<br/>1- power on<br/>2- scan AP |

---

### (0xA1)WiFi 信息 :id=_0xA1

**功能描述**

获取 WiFi 模块的信息。WiFi 的一般方式为：

+ SOC，芯片内部自带 WiFi 模组
+ 外挂 WiFi 模块，比如 ESP32/ESP8266 等。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                                        |
|----------|-----------|--------|-----------|--------------------------------------------------------------------|
| 1        | length    | byte   |           | length                                                             |
| 2        | key       | byte   |           | key                                                                |
| 3-6      | chipId    | uint32 |           | chip id <br/>0- invalid id<br/>0x8266- esp8266<br/>0x32C2- esp32c2 |
| 7-n      | model     | String | utf8      | model 芯片具体型号, String 类型                                            |

---

### (0xA2)WiFi 扫描到的AP :id=_0xA2

**功能描述**

设置 WiFi 模块扫描 AP 时，设备在接收到 AP 更新时，主动通知上位机 AP 更新。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                          |
|----------|-----------|--------|-----------|------------------------------------------------------|
| 1        | length    | byte   |           | length                                               |
| 2        | key       | byte   |           | key                                                  |
| 3        | rssi      | sbyte  |           | rssi, 有符号数, 范围:[-127,0]<br/>AP 的信号强度                 |
| 4-9      | mac       | MAC    | leMac     | MAC<br/>AP 的 MAC 地址                                  |
| 10-n     | ssid      | String | utf8      | SSID, utf8 编码, String 类型<br/>MAC 的 ssid 字符串, utf8 编码 |

---

### (0xA8)GPS 控制 :id=_0xA8

**功能描述**

设置 GPS 工作方式。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                                                                 |
|----------|-----------|------|-----------|-----------------------------------------------------------------------------|
| 1        | length    | byte |           | length                                                                      |
| 2        | key       | byte |           | key                                                                         |
| 3        | cmd       | byte |           | cmd <br/>0- power off<br/>1- cold start <br/>2- hot start<br/>3- warm start |

---

### (0xA9)GPS 信息 :id=_0xA9

**功能描述**

获取 GPS 模块的信息。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description              |
|----------|-----------|--------|-----------|--------------------------|
| 1        | length    | byte   |           | length                   |
| 2        | key       | byte   |           | key                      |
| 3-6      | chipId    | uint32 |           | chip id<br/>GPS 模组/芯片 id |
| 7-n      | model     | String | utf8      | model 芯片具体型号, String 类型  |

---

### (0xAA)GPS 最新定位数据 :id=_0xAA

**功能描述**

GPS 上电后，一般会自动进行定位更新。在 GPS 更新到有效时间，则开始主动上报定位信息。

**通知数据格式**

+ 仅更新时间

| Byte No. | Parameter | Type   | Converter | Description                     |
|----------|-----------|--------|-----------|---------------------------------|
| 1        | length    | byte   |           | length                          |
| 2        | key       | byte   |           | key                             |
| 3-6      | dateTime  | uint32 | unixTime  | timestamp，utc 时间<br/>GPS 接收到的时间 |

+ 更新到定位信息

| Byte No. | Parameter               | Type   | Converter | Description                 |
|----------|-------------------------|--------|-----------|-----------------------------|
| 1        | length                  | byte   |           | length                      |
| 2        | key                     | byte   |           | key                         |
| 3-6      | dateTime                | uint32 | unixTime  | timestamp，utc 时间，GPS 接收到的时间 |
| 7-10     | lat                     | float  | loc       | latitude, float             |
| 11-14    | lng                     | float  | loc       | longitude, float            |
| 15-18    | speed                   | float  |           | speed, unit: km/h           |
| 19-20    | degree                  | ushort |           | degree                      |
| 21-22    | direction               | ushort |           | direction                   |
| 23-24    | altitude                | ushort |           | altitude                    |
| 25-26    | haaccuracy              | ushort |           | haaccuracy                  |
| 27       | visibleSatelitesNumbers | byte   |           | visible satelites numbers   |
| 28       | numberOfSatelitesUsed   | byte   |           | number of satelites used    |
| 29       | pdop                    | byte   |           | PDOP                        |

---

### (0xAB)GPS 卫星数据 :id=_0xAB

**功能描述**

当更新到定位信息时，除上报最新的定位信息，同时上报卫星信息。

**通知数据格式**

| Byte No. | Parameter | Type   | Converter | Description |
|----------|-----------|--------|-----------|-------------|
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3        | prn       | byte   |           | prn         |
| 4        | elevation | byte   |           | elevation   |
| 5-6      | azimuth   | ushort |           | azimuth     |
| 7-8      | snr       | ushort |           | snr         |
| ...      | ...       | ...    | ...       | ...         |
| n        | prn       | byte   |           | PRN         |
| n+1      | elevation | byte   |           | elevation   |
| n+2-n+3  | azimuth   | ushort |           | azimuth     |
| n+4-n+5  | snr       | ushort |           | snr         |

+ 当前数据包中包含多个卫星信息，卫星信息是每次 NMEA 更新时从 GSV 中获取到的。

---

### (0xB0)GSM控制 :id=_0xB0

---

**功能描述**

设置 GSM 的工作模式。

**写请求数据格式**

+ 电源控制写请求数据格式

| Byte No. | Parameter | Type | Converter | Description                                                                                                                  |
|----------|-----------|------|-----------|------------------------------------------------------------------------------------------------------------------------------|
| 1        | length    | byte |           | length                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                          |
| 3        | cmd       | byte |           | cmd <br/>0- power off<br/>1- power on <br/>2- enter flight mode<br/>3- exit flight mode <br/>4- reset<br/>0x20- rf test mode |

+ 射频测试 写请求数据格式

| Byte No. | Parameter | Type   | Converter | Description                                                 |
|----------|-----------|--------|-----------|-------------------------------------------------------------|
| 1        | length    | byte   |           | length                                                      |
| 2        | key       | byte   |           | key                                                         |
| 3        | cmd       | byte   |           | cmd = 0x20-rf test mode                                     |
| 4        | mode      | byte   |           | mode <br/>0- stop all<br/>1- start rf tx<br/>2- start rf rx |
| 5-8      | band      | uint32 |           | band                                                        |
| 9-12     | channel   | uint32 |           | channel                                                     |
| 13-16    | power     | uint32 |           | power                                                       |
| 17-20    | slotOrBw  | uint32 |           | slotnumber/bandwidth                                        |

参考文档：

1. SIM7600_CTBURST_Application_Note_V0.01.pdf
2. A76XX Series_CTBURST_Application Note_V1.04.pdf
3. Quectel_LTE_Standard(U)系列_RF_FTM_应用指导_V1.2.pdf

---

### (0xB1)GSM信息 :id=_0xB1

---

**功能描述**

获取 GSM 模块的硬件信息、固件信息，以及 SIM 卡信息。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description           |
|----------|-----------|--------|-----------|-----------------------|
| 1        | length    | byte   |           | length                |
| 2        | key       | byte   |           | key                   |
| 3-n      | imei      | String | ascii     | IMEI, String type     |
|          | iccid     | String | ascii     | ICCID, String type    |
|          | model     | String | utf8      | model, String type    |
|          | revision  | String | utf8      | revision, String type |
|          | version   | String | utf8      | version, String type  |
|          | qmbn      | String | utf8      | qmbn, String type     |

由于 `String` 类型 带结束符，需要根据结束符来解析每个字符串的值。需要注意的是，有些数据未能获取成功，字符串仅有 `"\0"`。比如 ICCID，当 SIMCARD 未插入时，则 ICCID 字符串无实际字符。

---

### (0xB2)GSM状态 :id=_0xB2

**功能描述**

提示获取 GSM 状态信息，在 GSM 开机后的网络注册状态，以及注册到的基站信息等。

**写请求数据格式**

| Byte No. | Parameter     | Type   | Converter | Description                                                     |
|----------|---------------|--------|-----------|-----------------------------------------------------------------|
| 1        | length        | byte   |           | length                                                          |
| 2        | key           | byte   |           | key                                                             |
| 3        | state         | byte   |           | state <br/>0- not ready<br/>1- ready                            |
| 4        | creg          | byte   |           | creg                                                            |
| 5        | cgreg         | byte   |           | cgreg                                                           |
| 6        | cereg         | byte   |           | cereg                                                           |
| 7        | rssi          | byte   |           | rssi of csq                                                     |
| 8        | ber           | byte   |           | ber of csq                                                      |
| 9        | networkStatus | byte   |           | network status <br/>2- 2G<br/>3- 3G <br/>4- 4G<br/>255- unknown |
| 10-11    | mcc           | ushort |           | MCC                                                             |
| 12-13    | mnc           | ushort |           | MNC                                                             |
| 14-17    | lac           | uint32 |           | LAC                                                             |
| 18       | cellId        | byte   |           | CELL ID                                                         |
| 19       | rsrp          | byte   |           | RSRP                                                            |
| 20-n     | band          | String | utf8      | Band, String Type                                               |
|          | operator      | String | utf8      | Operator, String Type                                           |

---

### (0xB3)Mic&Speaker 控制 :id=_0xB3

---

**功能描述**

通过 GSM 模块控制 Mic 和 Speaker 的工作方式。

**写请求数据格式**

| Byte No. | Parameter   | Type | Converter | Description                                                                                                                                                                                                |
|----------|-------------|------|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1        | length      | byte |           | length                                                                                                                                                                                                     |
| 2        | key         | byte |           | key                                                                                                                                                                                                        |
| 3        | cmd         | byte |           | cmd <br/>0- 关闭 mic & speaker<br/>1- 打开 mic & speaker <br/>2- 仅打开 speaker<br/>3- 仅打开 mic <br/>4- 来电时的 mic & speaker 控制<br/>5- 拨号时的 mic & speaker 控制 <br/>6- 通话时的 mic & speaker 控制<br/>7- mic & speaker 回环测试 |
| 4        | micGain     | byte |           | mic 增益，0-100                                                                                                                                                                                               |
| 5        | speakerGain | byte |           | speaker 增益，0-100                                                                                                                                                                                           |

---

### (0xB4)电话测试 :id=_0xB4

**功能描述**

测试 GSM 的电话功能。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                   |
|----------|-----------|--------|-----------|-----------------------------------------------|
| 1        | length    | byte   |           | length                                        |
| 2        | key       | byte   |           | key                                           |
| 3        | cmd       | byte   |           | cmd <br/>0- hangup<br/>1- dial<br/>2- answer  |
| 4-n      | number    | String | ascii     | number, String type<br/>仅拨号时需要传入，其他命令不需要传入此参数 |

+ 挂断/接听 写请求数据格式

| Byte No. | Parameter | Type | Converter | Description                               |
|----------|-----------|------|-----------|-------------------------------------------|
| 1        | length    | byte |           | length                                    |
| 2        | key       | byte |           | key                                       |
| 3        | cmd       | byte |           | cmd = 0, hangup <br/>cmd = 2, answer<br/> |

+ 拨号写请求数据格式

| Byte No. | Parameter | Type   | Converter | Description                                   |
|----------|-----------|--------|-----------|-----------------------------------------------|
| 1        | length    | byte   |           | length                                        |
| 2        | key       | byte   |           | key                                           |
| 3        | cmd       | byte   |           | cmd: 1-dial                                   |
| 4-n      | number    | String | ascii     | number, String type<br/>仅拨号时需要传入，其他命令不需要传入此参数 |

---

### (0xB5)播放音频文件 :id=_0xB5

**功能描述**

控制 GSM 的音频播放，或停止音频播放。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                                            |
|----------|-----------|--------|-----------|------------------------------------------------------------------------|
| 1        | length    | byte   |           | length                                                                 |
| 2        | key       | byte   |           | key                                                                    |
| 3        | cmd       | byte   |           | cmd <br/>0- stop<br/>1- play 播放内部固定编号的音频文件<br/>2- file play 播放指定路径下的文件 |
| 4-7      | times     | uint32 |           | times, 播放次数                                                            |
| 8-11     | delay     | uint32 |           | delay, 每次播放后的延时，单位:ms                                                  |
| 12-n      | params    | byte[] |           | params                                                                 |

+ 停止播放

| Byte No. | Parameter | Type | Converter | Description   |
|----------|-----------|------|-----------|---------------|
| 1        | length    | byte |           | length        |
| 2        | key       | byte |           | key           |
| 3        | cmd       | byte |           | cmd = 0- stop |

+ 播放指定编号

| Byte No. | Parameter | Type   | Converter | Description                |
|----------|-----------|--------|-----------|----------------------------|
| 1        | length    | byte   |           | length                     |
| 2        | key       | byte   |           | key                        |
| 3        | cmd       | byte   |           | cmd =1- play 播放内部固定编号的音频文件 |
| 4-7      | times     | uint32 |           | times, 播放次数                |
| 8-11     | delay     | uint32 |           | delay, 每次播放后的延时，单位:ms      |
| 9-12     | type      | uint32 |           | type, 范围：[0, 65535]        |

+ 播放指定文件

| Byte No. | Parameter    | Type   | Converter | Description                                           |
|----------|--------------|--------|-----------|-------------------------------------------------------|
| 1        | length       | byte   |           | length                                                |
| 2        | key          | byte   |           | key                                                   |
| 3        | cmd          | byte   |           | cmd = 2- file play 播放指定路径下的文件                         |
| 4-7      | times        | uint32 |           | times, 播放次数                                           |
| 8-11     | delay        | uint32 |           | delay, 每次播放后的延时，单位:ms                                 |
| 12-n      | fileFullName | String | utf8      | file full name， 指定文件全路径, String Type<br/>比如: e:/1.mp3 |

---

### 工厂测试 进入/退出

**功能描述**

于控制设备进入工厂测试模式，或退出工厂测试模式。

注意，设备对 此命令的有效时间有限制，超过时间限制将回复负响应。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                             |
|----------|-----------|------|-----------|-----------------------------------------|
| 1        | length    | byte |           | length                                  |
| 2        | key       | byte |           | key                                     |
| 3        | mode      | byte |           | 控制方式 <br/>0 - 退出工厂测试模式<br/>1 - 进入工厂测试模式 |
---

**读返回数据格式**

| Byte No. | Parameter | Type | Converter | Description                                                 |
|----------|-----------|------|-----------|-------------------------------------------------------------|
| 1        | length    | byte |           | length                                                      |
| 2        | key       | byte |           | key                                                         |
| 3        | status    | byte |           | 状态 <br/>0 - 未进入工厂测试模式<br/>1 - 工厂测试模式正在初始化<br/>2 - 工厂测试模式已就绪 |

---

## 扩展应用

---

### 老化测试

当前协议中并未提供单个命令来直接进行老化测试，但可以组合命令来进行。

+ 进入工厂测试模式，设置时间为最大值
+ 设置马达振动方式
+ 设置屏幕工作方式（如果有屏幕）
+ 设置 LED 工作方式（如果有 LED）
+ 设置 GSM 模块播放音频参数

---

### 按键老化

按键老化使用外部治具来控制，上位机可以接收按键状态变化来统计 `按键的按下/松开` 状态变化。
