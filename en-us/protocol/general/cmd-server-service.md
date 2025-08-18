
# 服务器服务 Services

服务器服务提供与设备额外的增强功能。

## key列表

| Value(HEX) | 功能描述 | 可写 | 可读 | 通知 |
| --- | --- | --- | --- | --- |
| [0x10](#_0x10) | 心跳包 | | | |
| [0x12](#_0x12) | 时间同步 | | | |
| [0x21](#_0x20) | 基站定位解析 | | | |
| [0x22](#_0x21) | WiFi定位解析| | | |


## key功能描述



### (0x10)心跳包 :id=0x10

**功能描述**

心跳包是维持服务器和设备在线保活功能，在保活期间是可以正常读写数据流程。保活期间的时间由服务器特性去配置设备的心跳间隔上报达到保活

上传规则：服务command+设备ID+心跳包

**写请求数据格式**

| byte No. | Parameter | Type | Converter | Description    |
| -------- | --------- | ---- | --------- | -------------- |
| 1        | length    | byte |           | length + value |
| 2        | key       | byte |           | 0x10           |
| 3        | alive     | byte |           | 0x5A           |

---


Note:

1. 上个数据包帧间隔必须要超过配置的心跳包时间到了才能发送保活数据




### (0x12)时间同步 :id=0x12

**功能描述**

设备在无时钟芯片需要同步服务时间，该时间是utc同步，由设备结合本地时区计算出当地时间

上传规则：服务command+设备ID+时间同步

**写请求数据格式**

| byte No. | Parameter | Type | Converter | Description    |
| -------- | --------- | ---- | --------- | -------------- |
| 1        | length    | byte |           | length + value |
| 2        | key       | byte |           | 0x12           |

**应答服务数据**

| byte No. | Parameter | Type | Converter | Description |
| -------- | --------- | ---- | --------- | ----------- |
| 1        | length    | byte |           | length      |
| 2        | key       | byte |           | 0x12        |
| 3-6      | utc       | uint |           | 格林时间UTC |

------

Note:

1. 设备需要24小时同步一次服务器时间，来校准设备本地RTC时间误差

### (0x21)基站定位解析 :id=0x21

**功能描述**

基站定位解析是设备端需要基站定位经纬度来辅助设备定位。在下载agps的时候会用到辅助经纬度或者GPS无定位情况下能用模糊经纬度去发送短信的应用。精度相对来说比较低

上传规则：服务command+设备ID+基站定位解析

**写请求数据格式**

| byte No. | Parameter | Type   | Converter | Description                               |
| -------- | --------- | ------ | --------- | ----------------------------------------- |
| 1        | length    | byte   |           | length                                    |
| 2        | key       | byte   |           | key                                       |
| 3-4      | MCC       | ushort |           | Mobile Country Code,移动国家代码          |
| 5-6      | MNC       | ushort |           | 移动网络号码                              |
| 7        | rxl       | sbyte  |           | RX level value for base station selection |
| 8-9      | lac       | ushort |           | LAC（Location Area Code）是一个位置区域码 |
| 10-13    | cellid    | uint   |           | Service-cell Identify                     |

**应答服务数据**

| byte No. | Parameter | Type   | Converter | Description                      |
| -------- | --------- | ------ | --------- | -------------------------------- |
| 1        | length    | byte   |           | length                           |
| 2        | key       | byte   |           | 0x21                             |
| 3-4      | latitude  | float  |           | Mobile Country Code,移动国家代码 |
| 5-6      | longitude | float  |           | 移动网络号码                     |
| 7+n      | addr      | string | utf-8     | 最大字节为32字符,结束符'\0'      |

---

Note:

1. 服务器可以不用发具体地址，可选

### (0x22)WiFi定位解析 :id=0x22

**功能描述**

基站定位解析是设备端需要基站定位经纬度来辅助设备定位。在下载agps的时候会用到辅助经纬度或者GPS无定位情况下能用模糊经纬度去发送短信的应用。精度相对来说比较低

上传规则：服务command+设备ID+wifi定位解析

**写请求数据格式**

| byte No. | Parameter | Type  | Converter | Description |
| -------- | --------- | ----- | --------- | ----------- |
| 1        | length    | byte  |           | length      |
| 2        | key       | byte  |           | key         |
| 3-8      | mac       | array | leMac     |             |
| 9        | rssi      | sbyte |           | 信号值      |

**应答服务数据**

| byte No. | Parameter | Type   | Converter | Description                      |
| -------- | --------- | ------ | --------- | -------------------------------- |
| 1        | length    | byte   |           | length                           |
| 2        | key       | byte   |           | 0x21                             |
| 3-4      | latitude  | float  |           | Mobile Country Code,移动国家代码 |
| 5-6      | longitude | float  |           | 移动网络号码                     |
| 7+n      | addr      | string | utf-8     | 最大字节为32字符,结束符'\0'      |

Note:

1. 服务器可以不用发具体地址，可选