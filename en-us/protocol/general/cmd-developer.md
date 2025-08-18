
# 开发者模式


## 命令说明
开发者模式，提供给开发者使用，用于调试和测试 模块。


## key列表

| Value(HEX) | Parameter | 功能描述 | 可写 | 可读 | 通知 |
| --- | --- | --- | --- | --- | --- |
| 0x30 | GsmModifyConfig | GSM修改配置 | | | |
| 0x31 | GsmQueryConfig | GSM查询配置 | | | |
| 0x32 | GsmWriteData | GSM写入数据 | | | |
| 0x33 | GsmReturnData | GSM返回数据 | | | |
| 0x40 | GpsModifyConfig | GPS修改配置 | | | |
| 0x41 | GpsQueryConfig | GPS查询配置 | | | |
| 0x42 | GpsSendData | GPS发送数据 | | | |
| 0x43 | GpsReceiveData | GPS接收数据 | | | |
| 0x50 | WifiModifyConfig | WiFi修改配置 | | | |
| 0x51 | WifiQueryConfig | WiFi查询配置 | | | |
| 0x52 | WifiSendData | WiFi发送数据 | | | |
| 0x53 | WifiReceiveData | WiFi接收数据 | | | |


## 功能描述

---
### GSM修改配置

**功能描述**

修改控制GSM工作参数：
* 开启/关闭 开发者模式
* 使能/禁止 模块休眠 
* 修改模块通讯参数，如`UART`的波特率
* 控制模块电源


**写请求数据格式**

| Byte No. | Parameter | Description |
| --- | --- | --- |
| 1 | length | length |
| 2 | key  | key |
| 3 | type | type-类型<br>0- 电源控制<br>1- 休眠控制<br>2- 串口波特率修改<br>3- IO控制<br>255- 开发者模式使能  |

---
### GSM查询配置

TBD

---
### GSM写入数据

**功能描述**

向GSM模块写入数据。

---
### GSM发送数据

**功能描述**

GSM模块通讯接口向外输出的数据，AT命令回复/URC回复等。

**通知数据格式**

| Byte No. | Parameter | Description |
| --- | --- | --- |
| 1 | length | length |
| 2 | key  | key |
| 3 - n | data | data |

Note:
1. 当前数据为通知主动返回，仅有一种类型数据。
   