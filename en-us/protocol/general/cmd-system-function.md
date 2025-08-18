# 系统功能 System Control(0x04)

系统功能命令用于控制和查询系统功能的状态。

## KEY列表

| Value(HEX)  | Parameter    | 功能描述     | 可写 | 可读 | 通知 |
| ----------- | ------------ | ------------ | ---- | ---- | ---- |
| [0x01](#_0x01) | DeviceId     | Device ID    | ❌   | ❌   | ❌   |
| [0x10](#_0x10) | FindDevice   | 查找设备     | ✅   | ❌   | ❌   |
| [0x12](#_0x12) | CheckUpdate  | 检查更新     | ✅   | ❌   | ❌   |
| [0x20](#_0x20) | FactoryReset | 恢复出厂设置 | ✅   | ❌   | ❌   |
| [0x21](#_0x21) | Shutdown     | 关机         | ✅   | ❌   | ❌   |
| [0x22](#_0x22) | Reboot       | 重启         | ✅   | ❌   | ❌   |
| [0x23](#_0x23) | ShippingMode | 船运模式     | ✅   | ❌   | ❌   |

## KEY功能描述

---

### (0x01)Device ID :id=0x01

**功能描述**

`Device ID`用于配合其他 `key`与服务器通讯时组合使用。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3-n      | deviceId  | String | utf8      |             |

---

### (0x10)查找设备 Find :id=0x10

**功能描述**

**写请求数据格式**

| Byte No. | Parameter | Type | Converter | Description                          |
| -------- | --------- | ---- | --------- | ------------------------------------ |
| 1        | length    | byte |           | length                               |
| 2        | key       | byte |           | key                                  |
| 3        | cmd       | byte |           | 0- 停止查找<br />1- 启动设备查找提醒 |

Note

1. 查找设备 功能 用于立刻向设备发送命令，要求设备执行查找提醒流程或停止当前查找提醒流程。

---

### (0x12)检查更新 :id=0x12

**功能描述**

发送此命令，设备将通过网络向服务器查询当前固件&文件是否为最新。如果服务器文件有更新，则自动完成文件更新流程。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                |
| -------- | --------- | ------ | --------- | -------------------------- |
| 1        | length    | byte   |           | length                     |
| 2        | key       | byte   |           | key                        |
| #string  | updateUrl | String | utf8      | url/ip+port 远端服务器网址 |

Note(待讨论):

1. url如果为 `空`, 则表示使用内部默认网址去查询检查更新
2. url如果为 `http url`,则表示使用 `http`查询, 如 `http://www.aaa.com/ds09342990`
3. url如果为 `ip:port`, 则表示使用 `tcp/ip`查询, 如 `192.168.1.199:3000`
4. url参数不会保存到本地。

---

### (0x20)恢复出厂设置 :id=0x20

**功能描述**

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description    |
| -------- | --------- | ------ | --------- | -------------- |
| 1        | length    | byte   |           | length         |
| 2        | key       | byte   |           | key            |
| 3-n      | cmd       | string | ascii     | 固定"recovery" |

Note

1. 命令需要带 `recovery`字符串，避免做命令扫描时触发恢复出厂设置。

---

### (0x21)设备关机 :id=0x21

**功能描述**

设置设备进行关机状态，大部分功能关闭。小部分后台功能仍在工作，比如闹钟，或者模块定时检测短信开机等。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                       |
| -------- | --------- | ------ | --------- | ------------------------------------------------- |
| 1        | length    | byte   |           | length                                            |
| 2        | key       | byte   |           | key                                               |
| 3-n      | mode      | string |           | "normal" -正常关机流程<br />"force" -强制关机流程 |

Note

1. `normal`关机流程指正常等待关闭任务与外设。
2. `force`强制复位后进入关机流程。

---

### (0x22)设备重启 :id=0x22

**功能描述**

设置设备进行重启。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description                                 |
| -------- | --------- | ------ | --------- | ------------------------------------------- |
| 1        | length    | byte   |           | length                                      |
| 2        | key       | byte   |           | key                                         |
| 3-n      | mode      | string |           | "safe" - 安全重启 `<br>`"force" -强制重启 |

Note:

1. 安全重启，指设备在复位前，依次关闭应用功能，保存关键数据
2. 强制重启，指设备直接调用系统复位命令，强制复位。中间数据可能会丢失。仅建议在测试过程中使用。用户端慎用!!!

---

### (0x23)运输模式 :id=0x23

**功能描述**

运输模式，也称冬眠模式,指设备进入最低电量消耗状态。

**写请求数据格式**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| #string  | ship      | String | utf8      | "ship"      |
