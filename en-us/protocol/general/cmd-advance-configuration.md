# 高级配置

---
---

## 介绍

高级配置中的参数对设备系统硬件参数进行修改，修改不当可能会引起设备硬件工作异常/配对设备无法正常通讯等问题，无相关经验知识人员请谨慎使用。

包含以下配置：

* 硬件的工作参数，比如BLE的phy,  Sub1G使用的频率等。
* 功能的使用，比如GSM短信使用 `CS`还是 `PS`， 锁定网络类型等。
* 硬件提示：LED灯的开关提示

---

---
## KEY列表

## key 列表

| Value(HEX)  | Parameter   | 功能描述               | 可写 | 可读 | 通知 |
| ----------- | ----------- | ---------------------- | ---- | ---- | ---- |
|             |             | **设备基础信息** |      |      |      |
| [0x01](#_0x01) | DeviceId    | 设备ID                 | ❌   | ✅   | ❌   |
| [0x10](#_0x10) | SMSSendConfig | 设备ID                 | ✅   | ✅   | ❌   |
## key 列表

| Value(HEX)  | Parameter      | 功能描述               | 可写 | 可读 | 通知 |
| ----------- | -------------- | ---------------------- | ---- | ---- | ---- |
|             |                | **设备基础信息** |      |      |      |
| [0x01](#_0x01) | DeviceId       | 设备ID                 | ❌   | ❌   | ✅   |
| [0x10](#_0x10) | SMSSendConfig  | 设备ID                 | ✅   | ✅   | ❌   |
|             |                |                        |      |      |      |
| [0x20](#_0x20) | NFCSwitch      | NFC开关                | ✅   | ✅   | ❌   |
| [0x21](#_0x21) | BLESwitch      | BLE开关                | ✅   | ✅   | ❌   |
| [0x22](#_0x22) | DataSaveOption | 历史数据开关           | ✅   | ✅   | ❌   |

## 子命令描述

---

### (0x01)设备ID :id=0x01

**功能描述**

设备ID用于向上层提供设备唯一标识。当使用设备的 `IMEI`, 参考[配置命令中-IMEI](cmd-configuration.md)。

**写请求数据格式**

| byte No. | Parameter | Type   | Converter | Description                        |
| -------- | --------- | ------ | --------- | ---------------------------------- |
| 1        | length    | byte   |           | key+value                          |
| 2        | key       | byte   |           | key                                |
| 3-n      | IMEI      | String | ascii     | IMEI, Type: String, length:[1, 32] |

Note:

1. 上报数据时, IMEI号必须是第一个的 `length-key-value`, 以方便服务器快速解析判断当前设备是否合法。

---

### (0x10)SMS发送设置 :id=0x10

**功能描述**

用于设置短信的发送配置参数。

**写请求数据格式**

| byte No. | Parameter    | Type | Converter | Description                                     |
| -------- | ------------ | ---- | --------- | ----------------------------------------------- |
| 1        | length       | byte |           | key+value                                       |
| 2        | key          | byte |           | key                                             |
| 3        | mode         | byte |           | 使用方式：<br />0:默认/不修改/模块自识别 <br />1: CS <br />2:PS |
| 4        | longSMSSplit | byte | bool      | 长短信是否分割：<br />0:不分割 <br />1:分割     |


Note

1. 短信发送使用`CS`还是`PS`域，同样根据不同运营商、模块配置来设定。
2. 长短信分割，用于部分运营商不支持长短信的情况，需要将长短信分割为多个短信发送，同时注意短信内容中的链接不要截断。

---

### (0x11)移动数据开关 :id=0x11

**功能描述**

用于打开/关闭移动数据功能。


**写请求数据格式**

| byte No. | Parameter    | Type | Converter | Description                                     |
| -------- | ------------ | ---- | --------- | ----------------------------------------------- |
| 1        | length       | byte |           | key+value                                       |
| 2        | key          | byte |           | key                                             |
| 3        | enable       | byte | bool      | 移动数据开关：<br />0:关闭 <br />1:开启 |


Note

1. 移动数据功能，指GSM是否允许联网。
---

### (0x01)设备ID :id=0x01

**功能描述**

设备ID用于向上层提供设备唯一标识。当使用设备的 `IMEI`, 参考[配置命令中-IMEI](cmd-configuration.md)。

**写请求数据格式**

| byte No. | Parameter | Type   | Converter | Description                        |
| -------- | --------- | ------ | --------- | ---------------------------------- |
| 1        | length    | byte   |           | key+value                          |
| 2        | key       | byte   |           | key                                |
| 3-n      | IMEI      | String | ascii     | IMEI, Type: String, length:[1, 32] |

Note:

1. 上报数据时, IMEI号必须是第一个的 `length-key-value`, 以方便服务器快速解析判断当前设备是否合法。

---

### (0x10)SMS发送设置 :id=0x10

**功能描述**

用于设置短信的发送配置参数。

**写请求数据格式**

| byte No. | Parameter    | Type | Converter | Description                                                     |
| -------- | ------------ | ---- | --------- | --------------------------------------------------------------- |
| 1        | length       | byte |           | key+value                                                       |
| 2        | key          | byte |           | key                                                             |
| 3        | mode         | byte |           | 使用方式：<br />0:默认/不修改/模块自识别 <br />1: CS <br />2:PS |
| 4        | longSMSSplit | byte | bool      | 长短信是否分割：<br />0:不分割 <br />1:分割                     |

Note

1. 短信发送使用 `CS`还是 `PS`域，同样根据不同运营商、模块配置来设定。
2. 长短信分割，用于部分运营商不支持长短信的情况，需要将长短信分割为多个短信发送，同时注意短信内容中的链接不要截断。

---

### (0x20)NFC功能配置 NFC config :id=0x20

**功能描述**

打开/关闭 NFC功能。当前NFC仅定义 `tag`功能。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                                            |
| -------- | --------- | ---- | --------- | ------------------------------------------------------ |
| 1        | length    | byte |           | length                                                 |
| 2        | key       | byte |           | key                                                    |
| 3        | enable    | byte | bool      | enable<br />0: 关闭NFC tag功能 <br />1:开启NFC tag功能 |

Note

1. 后续NFC的其他控制功能在这里扩展。

---

### (0x21)BLE功能开关 :id=0x21

**功能描述**

打开/关闭 `BLE` 从机功能。当关闭 `BLE`从机功能时，BLE不广播。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                                            |
| -------- | --------- | ---- | --------- | ------------------------------------------------------ |
| 1        | length    | byte |           | length                                                 |
| 2        | key       | byte |           | key                                                    |
| 3        | enable    | byte | bool      | enable<br />0: 关闭BLE从机功能 <br />1:开启BLE从机功能 |

Note

1. 当前仅关闭BLE广播功能，让 `central`无法扫描发现到。
2. 其他功能定位开启时，需要 `BLE`参与，则此配置不生效。

---

### (0x22)历史数据开关 Data save option :id=0x22

**功能描述**

是否开启设备将设备使用过程中产生的数据持久化到设备内部存储中。

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br />0: 关闭历史数据持久化 <br />1:开启历史数据持久化 |

Note

1. 关闭历史数据持久化后，设备使用过中产生的位置&报警信息不会保存到设备内部存储中。
