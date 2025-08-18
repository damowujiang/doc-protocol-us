# 配置命令 Configuration(0x02)

---
## 命令说明 Command Description

配置命令提供: <br />Configuration commands provided: 


+ 获取设备的基础信息 <br />Obtain basic device information
+ 设置设备的工作参数 <br />Set device operating parameters
+ 设置/查询 功能应用功能的配置参数 <br />Set/query configuration parameters for functional applications

相对 `V0`版本有以下修改：<br />The following changes have been made relative to the `V0` version:

+ `报警设置` 拆分为两个部分：检测设置和执行设置。检测设置用于设置算法或识别过程的阈值等，但识别到对应结果时，生成一个事件; 执行设置根据配置执行所配置好的顺序动作行为等。 <br />`Alarm Settings` has been split into two parts: Detection Settings and Execution Settings. Detection Settings are used to set thresholds for algorithms or recognition processes, etc., but when the corresponding result is recognized, an event is generated; Execution Settings execute the configured sequence of actions, etc., according to the configuration.
+ 新增 `String`类型，务必仔细阅读协议文档。<br />A new `String` type has been added. Please read the protocol documentation carefully.
+ 重新定义了命令行为: `write`/`read`/`notify`，同一个 `key`不同操作所使用的数据格式可能不相同。<br />Command behaviors have been redefined: `write`/`read`/`notify`. The data format used for different operations with the same `key` may vary.
+ 部分功能参数移往 `高级配置命令`中。<br />Some functional parameters have been moved to the `Advanced Configuration Commands` section.

---

## 子命令列表key list

| Value(HEX)  | Parameter                    | Description                                                       | Writable | Readable | Notifiable |
| ----------- | ---------------------------- | -------------------------------------------------------------- | ---- | ---- | ---- |
|             |                              | 设备基础信息 device basic information                           |      |      |      |
| [0x01](#_0x01) | DeviceModel                  | 设备模型 model                                                 | ❌   | ✅   | ❌   |
| [0x02](#_0x02) | FirmwareVersion              | 固件版本号 firmware version                                    | ❌   | ✅   | ❌   |
| [0x03](#_0x03) | IMEI                         | GSM 模块 IMEI                                                  | ❌   | ✅   | ❌   |
| [0x04](#_0x04) | ICCID                        | SIM 卡 ICCID                                                   | ❌   | ✅   | ❌   |
| [0x05](#_0x05) | MAC                          | BLE/BT MAC                                                     | ❌   | ✅   | ❌   |
| [0x06](#_0x06) | DeviceName                   | 设备名称 device name                                           | ❌   | ✅   | ❌   |
| [0x07](#_0x07) | CustomFirmwareVersion        | 定制版本字符串 custom firmware version                         | ❌   | ✅   | ❌   |
| [0x08](#_0x08) | DeviceFirmwareInfo           | 设备固件信息 device firmware information                       | ❌   | ✅   | ❌   |
| [0x09](#_0x09) | DeviceHardwareVersion        | 设备硬件版本 device hardware version                           | ❌   | ✅   | ❌   |
| [0x0A](#_0x0A) | WiFiMac                      | WiFi MAC                                                       | ❌   | ✅   | ❌   |
| [0x0B](#_0x0B) | BatteryStatus                | 电池状态 battery status                                        | ❌   | ✅   | ❌   |
| [0x0C](#_0x0C) | DeviceRunTime                | 开机时长 device run time                                       | ❌   | ✅   | ❌   |
| [0x0D](#_0x0D) | ProtocolApiVersion           | 协议API版本 protocol API version                                | ❌   | ✅   | ❌   |
| [0x0E](#_0x0E) | SDKApiVersion                | SDK API版本 sdk api version                                    | ❌   | ✅   | ❌   |
|             |                              | 扩展基础信息 extended basic information                        |      |      |      |
| [0x10](#_0x10) | GsmModuleExtraInfo           | GSM 模块其他信息 GSM extra information                         | ❌   | ✅   | ❌   |
| [0x11](#_0x11) | SimCardExtraInfo             | SIM 卡其他信息 SIM Card extra information                      | ❌   | ✅   | ❌   |
|             |                              | 基础设置 basic settings                                       |      |      |      |
| [0x20](#_0x20) | DeviceCustomName             | 设备自定义名称 device custom name                              | ✅   | ✅   | ❌   |
| [0x21](#_0x21) | DeviceCustomId               | 自定义设备 ID device custom id                                 | ✅   | ✅   | ❌   |
| [0x22](#_0x22) | NfcRecord                    | NFC 记录 nfc record                                            | ✅   | ✅   | ❌   |
| [0x23](#_0x23) | UserInfo                     | 用户信息 user profile                                          | ✅   | ✅   | ❌   |
| [0x24](#_0x24) | ApplicationScene             | 场景模式 application scence                                    | ✅   | ✅   | ❌   |
| [0x25](#_0x25) | DoNotDisturbMode             | 勿扰模式 do not disturb mode                                   | ✅   | ✅   | ❌   |
| [0x26](#_0x26) | ButtonShortcut               | 按键快捷功能 shortcut of buttons                               | ✅   | ✅   | ❌   |
| [0x27](#_0x27) | SleepMode                    | 睡眠模式 sleep mode                                            | ✅   | ✅   | ❌   |
| [0x28](#_0x28) | AutoLowPowerMode             | 自动低电量模式 auto low power mode                             | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0x30](#_0x30) | SystemSetting                | 系统设置 system setting                                        | ✅   | ✅   | ❌   |
| [0x31](#_0x31) | FormatPreference             | 格式偏好 format prefercence                                    | ✅   | ✅   | ❌   |
| [0x32](#_0x32) | LanguagePreference           | 语言偏好 language                                              | ✅   | ✅   | ❌   |
| [0x33](#_0x33) | DateTime                     | 日期时间 datetime                                              | ✅   | ✅   | ❌   |
| [0x34](#_0x34) | TimeZone                     | 时区 time zone                                                 | ✅   | ✅   | ❌   |
| [0x35](#_0x35) | DateTimeExtraSetting         | 日期时间扩展 datetime extra setting                            | ✅   | ✅   | ❌   |
|             |                              | 网络设置 network settings                                      |      |      |      |
| [0x40](#_0x40) | ServerConfig                 | 服务器配置 server config                                       | ✅   | ✅   | ❌   |
| [0x41](#_0x41) | ApnList                      | APN 配置 APN list                                              | ✅   | ✅   | ❌   |
| [0x42](#_0x42) | WiFiNetworkApList            | WiFi 网络 AP 列表  WLAN AP list                                | ✅   | ✅   | ❌   |
| [0x43](#_0x43) | WiFiAutoConnectSetting       | WiFi 自动联网设置  WLAN WiFi connection setting                | ✅   | ✅   | ❌   |
| [0x44](#_0x44) | HeartbeatToServer            | 心跳包设置 heartbeat to server                                 | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0x48](#_0x48) | RingToneVolume               | 来电响铃音量 ring tone volume                                  | ✅   | ✅   | ❌   |
| [0x49](#_0x49) | MicGain                      | mic 增益 gain of mic                                           | ✅   | ✅   | ❌   |
| [0x4A](#_0x4A) | SpeakerGain                  | speaker 增益 gain of speaker                                   | ✅   | ✅   | ❌   |
| [0x4B](#_0x4B) | PromptVolume                 | 提示音音量 prompt volume                                       | ✅   | ✅   | ❌   |
| [0x4C](#_0x4C) | PromptEnable                 | 语音提示开关 prompt enable                                     | ✅   | ✅   | ❌   |
|             |                              |                                                                | ✅   | ✅   |      |
| [0x50](#_0x50) | GeoApplicationCfg            | GEO 参数 GEO application config                                | ✅   | ✅   | ❌   |
| [0x51](#_0x51) | FalldownCfg                  | 跌倒算法参数 falldown algorithm config                         | ✅   | ✅   | ❌   |
| [0x52](#_0x52) | MotionCfg                    | 活动检测算法参数 motion algorithm config                       | ✅   | ✅   | ❌   |
| [0x53](#_0x53) | StaticCfg                    | 静止检测算法参数 static algorithm config                       | ✅   | ✅   | ❌   |
| [0x54](#_0x54) | TiltCfg                      | 倾斜检测算法参数 tilt algorithm config                         | ✅   | ✅   | ❌   |
| [0x55](#_0x55) | OverspeedCfg                 | 超速检测算法参数 overspeed algorithm config                    | ✅   | ✅   | ❌   |
| [0x56](#_0x56) | LowPowerCfg                  | 低电量检测算法参数 low power algorithm config                  | ✅   | ✅   | ❌   |
| [0x57](#_0x57) | LeaveGoHomeCfg               | 离家/回家 参数 leave/go home algorithm config                  | ✅   | ✅   | ❌   |
| [0x58](#_0x58) | FatigueCfg                   | 久坐/疲劳 检测 fatigue algorithm config                        | ✅   | ✅   | ❌   |
| [0x59](#_0x59) | WearDetectionCfg             | 佩戴检测 wear status config                                    | ✅   | ✅   | ❌   |
| [0x5A](#_0x5A) | WelfareCfg                   | Welfare功能检测配置 welfare detected config                    | ✅   | ✅   | ❌   |
| [0x5A](#_0x5A) | WelfareCfg                   | Welfare功能检测配置 welfare detected config                    | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0x60](#_0x60) | Geo                          | GEO 提醒执行设置  actions of GEO reminder                      | ✅   | ✅   | ❌   |
| [0x61](#_0x61) | Sos                          | SOS 提醒执行设置  actions of  SOS reminder                     | ✅   | ✅   | ❌   |
| [0x62](#_0x62) | Falldown                     | 跌倒 提醒执行设置 actions of  falldown reminder                | ✅   | ✅   | ❌   |
| [0x63](#_0x63) | Motion                       | 活动 提醒执行设置 actions of motion reminder                   | ✅   | ✅   | ❌   |
| [0x64](#_0x64) | Static                       | 静止 提醒执行设置 actions of static reminder                   | ✅   | ✅   | ❌   |
| [0x65](#_0x65) | Tilt                         | 倾斜 提醒执行设置 actions of tilt reminder                     | ✅   | ✅   | ❌   |
| [0x66](#_0x66) | Overspeed                    | 超速 提醒执行设置 actions of overspeed reminder                | ✅   | ✅   | ❌   |
| [0x67](#_0x67) | LowPower                     | 低电量 提醒执行设置 actions of low power reminder              | ✅   | ✅   | ❌   |
| [0x68](#_0x68) | DevicePowerOn                | 开机 提醒执行设置 actions of device power on                   | ✅   | ✅   | ❌   |
| [0x69](#_0x69) | DevicePowerOff               | 关机 提醒执行设置 actions of device power off                  | ✅   | ✅   | ❌   |
| [0x6A](#_0x6A) | LeaveHomeGoHome              | 回家/离家 提醒执行设置 actions of leave home/go home reminder  | ✅   | ✅   | ❌   |
| [0x6B](#_0x6B) | Fatigue                      | 久坐/疲劳 提醒执行设置   actions of fatigue reminder           | ✅   | ✅   | ❌   |
| [0x6C](#_0x6C) | FindDevice                   | 查找设备 执行设置 actions of find device                       | ✅   | ✅   | ❌   |
| [0x6D](#_0x6D) | WelfareExecution             | Welfare提醒执行设置 actions of welfare reminder                | ✅   | ✅   | ❌   |
| [0x6D](#_0x6D) | WelfareExecution             | Welfare提醒执行设置 actions of welfare reminder                | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0x90](#_0x90) | EmergencyContacts            | 紧急联系人列表 emergency contacts                              | ✅   | ✅   | ❌   |
| [0x91](#_0x91) | Contacts                     | 联系人列表 contacts                                            | ✅   | ✅   | ❌   |
| [0x92](#_0x92) | IncomingCallExtraConfig      | 来电设置 incoming call extra config                            | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0x95](#_0x95) | DtmfEnabled                  | DTMF 开关 DTMF enabled                                         | ✅   | ✅   | ❌   |
| [0x96](#_0x96) | DtmfKeysExecution            | DTMF 按键功能 DTMF key's execution                             | ✅   | ✅   | ❌   |
| [0x97](#_0x97) | SmsPermission                | 短信命令权限设置 permission of SMS command                     | ✅   | ✅   | ❌   |
| [0x98](#_0x98) | SmsPwd                       | 短信命令密码设置 password of SMS command                       | ✅   | ✅   | ❌   |
| [0x99](#_0x99) | SmsPrefix                    | 短信命令回复前缀 prefix text for SMS command's response        | ✅   | ✅   | ❌   |
| [0x9B](#_0x9B) | GpsLocationLinkFormatter     | GPS 定位链接格式化字符串 GPS location link formatter           | ✅   | ✅   | ❌   |
| [0x9C](#_0x9C) | WiFiLbsLocationLinkFormatter | WiFi/LBS 定位链接格式化字符串 WiFi/LBS location link formatter | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0xB0](#_0xB0) | AgpsConfig                   | AGPS 配置 AGPS config                                          | ✅   | ✅   | ❌   |
| [0xB1](#_0xB1) | LocateContinousConfig        | 连续定位配置 locate continous config                           | ✅   | ✅   | ❌   |
| [0xB3](#_0xB3) | BleScanSchedule              | BLE 定时工作参数 BLE scan schedule                             | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0xB8](#_0xB8) | BleList                      | BLE 设备列表 BLE device list binding location                  | ✅   | ✅   | ❌   |
| [0xB9](#_0xB9) | WiFiList                     | WiFi 设备列表 WiFi AP list binding location                    | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0xD0](#_0xD0) | WatchfaceId                  | 表盘 id watchface id                                           | ✅   | ✅   | ❌   |
| [0xD1](#_0xD1) | WatchfaceListManagement      | 设置表盘列表管理 watchface list management                     | ✅   | ✅   | ❌   |
| [0xD2](#_0xD2) | WatchfaceAccessLink          | 云端表盘服务器链接 watchface access link                       | ✅   | ✅   | ❌   |
| [0xD3](#_0xD3) | ScreenWorkParameter          | 屏幕设置 screen work parameter                                 | ✅   | ✅   | ❌   |
| [0xD4](#_0xD4) | PrimaryUiVisible             | 一级界面设置 primary UI's visible                              | ✅   | ✅   | ❌   |
| [0xD5](#_0xD5) | HomeUiAutoReturn             | Home 界面设置 Home UI auto return                              | ✅   | ✅   | ❌   |
| [0xD6](#_0xD6) | WatchfaceDataVisibility      | 表盘元素设置 Watchface data's visibility                       | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0xE0](#_0xE0) | AlarmclockList               | 闹钟列表 alarmclock list                                       | ✅   | ✅   | ❌   |
| [0xE1](#_0xE1) | Heartrate                    | 心率定时测量 heartrate measure schedule                        | ✅   | ✅   | ❌   |
| [0xE2](#_0xE2) | Bloodoxyge                   | 血氧定时测量 bloodoxygen measure schedule                      | ✅   | ✅   | ❌   |
| [0xE3](#_0xE3) | Temperature                  | 温度定时测量 temperature measure schedule                      | ✅   | ✅   | ❌   |
| [0xE4](#_0xE4) | WeatherApplication           | 天气功能参数 weather application config                        | ✅   | ✅   | ❌   |
| [0xE5](#_0xE5) | ThirdPartyAccessory          | 第三方配件接入 config of 3rd accessories intergrated           | ✅   | ✅   | ❌   |
| [0xE6](#_0xE6) | HealthDataUpload             | 数据定时上报 data upload schedule                              | ✅   | ✅   | ❌   |
| [0xE8](#_0xE8) | Mileage                      | 里程 mileage                                                   | ✅   | ✅   | ❌   |
| [0xE9](#_0xE9) | StepFunctionConfig           | 计步功能配置                                                   | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0xF0](#_0xF0) | ReadKey                      | 读功能 read key                                                | ✅   | ❌   | ❌   |
| [0xF1](#_0xF1) | ReadKeyRange                 | 读key区间 read key range                                       | ✅   | ❌   | ❌   |

---

## 子命令描述 Subcommand description

---

### (0x01) 设备模型 model number :id=0x01

**功能描述 Function description**

查询当前设备模型编号。<br />Query the current device model number.

**读返回数据格式 Read return data format**

| Byte No. | Parameter | Type | Converter | Description     |
| -------- | --------- | ---- | --------- | --------------- |
| 1        | length    | byte |           | length          |
| 2        | key       | byte |           | key             |
| 3-6      | model     | uint |           | model |

Note

1. model 数据在固件中对应使用的是 `project code`, model的对应关系参考项目标准文档中的 `project code`对应列表。由于文档持续更新，请向研发获取设备对应的 `model`编号。<br />The model data used in the firmware corresponds to the `project code`. For the correspondence between models and project codes, refer to the `project code` correspondence list in the `project standard documentation`. As the documentation is continuously updated, please contact R&D to obtain the `model` number corresponding to your device.

---

### (0x02) 固件版本号 firmware version :id=0x02

**功能描述 Function description**

查询固件的标准版本号。<br />Query the standard version number of the firmware.

**读返回数据格式 Read return data format**

| Byte No. | Parameter       | Type | Converter | Description                             |
| -------- | --------------- | ---- | --------- | --------------------------------------- |
| 1        | length          | byte |           | length                                  |
| 2        | key             | byte |           | key                                     |
| 3        | compiledVersion | byte | version   | complied/modified version, range:[0-255] |
| 4        | hotfixVersion   | byte | version   | hotfix version, range:[0-255]            |
| 5        | minorVersion    | byte | version   | minor version, range:[0-255]             |
| 6        | majorVersion    | byte | version   | major version, range:[0-255]             |

备注：

1. major 版本号变化：包含程序架构、流程与前一版本无法兼容时，版本号升级。<br />Major version number change: When the program architecture and processes are incompatible with the previous version, the version number is upgraded.
2. minor 版本号变化：新功能添加、功能重构、功能移除。<br />Minor version number change: New features are added, features are restructured, or features are removed.
3. hotfix 版本号变化：对版本 bug 进行修复。<br />Hotfix version number change: Bugs in the version are fixed.
4. complied 版本号变化：释放给内部测试人员来区别测试版本。<br />Compiled version number change: Released to internal testers to distinguish test versions.
5. 版本号可转换为 `int`数据来进行比较。<br />Version numbers can be converted to `int` data for comparison.

---

### (0x03) IMEI :id=0x03

**功能描述 Function description**

获取 GSM 模块的 IMEI 字符串。<br />Get the IMEI string for the GSM module.


**读返回数据格式 Read return data format**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3-n      | imei      | String | ascii     | IMEI string |

Note:

1. IMEI 号需要在 GSM 模块正常开机后才能识别到。未识别到 `IMEI`时返回空字符串。<br />The IMEI number can only be identified after the GSM module has booted up normally. If the `IMEI` is not identified, an empty string is returned.

---

### (0x04)ICCID :id=0x04

**功能描述 Function description**

获取 SIM 卡的 ICCID 字符串。<br />Get the ICCID string of the SIM card.

**读返回数据格式 Read return data format**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3-n      | iccid     | String | ascii     | ICCID       |

Note:

1. SIM 卡插入设备后，需要正确识别到才能获取 ICCID。<br />After inserting the SIM card into the device, it must be correctly identified in order to obtain the ICCID.
2. SIM 卡移除后，ICCID 应当为空，不能显示上次识别到的 ICCID。<br />After removing the SIM card, the ICCID should be empty and should not display the last identified ICCID.

---

### (0x05)BLE/BT MAC :id=0x05

**功能描述 Function description**

获取蓝牙功能模块的 MAC 地址。<br />Obtain the MAC address of the Bluetooth function module.

**读返回数据格式 Read return data format**

+ 设备仅支持 BLE 读返回数据格式 <br />The device only supports BLE read return data format

| Byte No. | Parameter | Type | Converter | Description |
| -------- | --------- | ---- | --------- | ----------- |
| 1        | length    | byte |           | length      |
| 2        | key       | byte |           | key         |
| 3-8      | bleMac    | MAC  | leMac     | BLE MAC     |

+ 设备同时支持 BLE&BT 读返回数据格式 <br />The device also supports BLE&BT read return data format

| Byte No. | Parameter | Type | Converter | Description |
| -------- | --------- | ---- | --------- | ----------- |
| 1        | length    | byte |           | length      |
| 2        | key       | byte |           | key         |
| 3-8      | bleMac    | MAC  | leMac     | BLE MAC     |
| 9-15     | btMac     | MAC  | leMac     | BT MAC      |

Note:

1. 由于部分平台IC内部没有自带 `MAC`,需要在生产时手动配置 `public address`。注意 `public address`类型的地址要求。<br />Since some platform ICs do not have a built-in MAC, the public address must be manually configured during production. Please note the requirements for public address types.

---

### (0x06)设备名称 device name :id=0x06

**功能描述 Function description**

获取当前设备名称。<br />Get the current device name.

**读返回数据格式 Read return data format**

| Byte No. | Parameter  | Type   | Converter | Description           |
| -------- | ---------- | ------ | --------- | --------------------- |
| 1        | length     | byte   |           | length                |
| 2        | key        | byte   |           | key                   |
| 3-n      | deviceName | String | utf8      |  设备名称 device name|

---

### (0x07)版本字符串 custom firmware version string :id=0x07

**功能描述 Function description**

获取当前固件版本字符串描述。定制固件使用定制固件版本字符串。<br />Get the current firmware version string description. Custom firmware uses a custom firmware version string.

**读返回数据格式 Read return data format**

| Byte No. | Parameter     | Type   | Converter | Description                |
| -------- | ------------- | ------ | --------- | -------------------------- |
| 1        | length        | byte   |           | length                     |
| 2        | key           | byte   |           | key                        |
| 3-n      | versionString | String | utf8      | 版本字符串 version string,  |

---

### (0x08)设备固件信息 device firmware information :id=0x08

**功能描述 Function description**

用于获取设备的固件信息描述。不同的固件版本、平台，此信息格式有区别。<br />Used to obtain the firmware information description of the device. The format of this information varies depending on the firmware version and platform.

**读返回数据格式 Read return data format**

| Byte No. | Parameter    | Type  | Converter | Description                 |
| -------- | ------------ | ----- | --------- | --------------------------- |
| 1        | length       | byte  |           | length                      |
| 2        | key          | byte  |           | key                         |
| 3-n      | firmwareInfo | array |           | device firmware information |

* **EV-101读返回数据格式 magic=0xa991c0d0 Read return data format**

| Byte No. | Parameter       | Type   | Converter  | Description                                                                      |
| -------- | --------------- | ------ | ---------- | -------------------------------------------------------------------------------- |
| 1        | length          | byte   |            | length                                                                           |
| 2        | key             | byte   |            | key                                                                              |
| 3-6      | magic           | uint   |            | magic                                                                            |
| 7-10     | projectCode     | uint   |            | project code 项目代号                                                            |
| 11-14    | firmwareVersion | uint   | version    | firmware version 固件版本                                                        |
| 15-19    | hardwareVersion | uint   | version    | hardware version 最小要求的硬件版本                                              |
| 20-23    | icType          | uint   |            | IC type 硬件平台                                                                 |
| 24-27    | customFlag      | uint   |            | custom flag 自定义标识/定制标识                                                  |
| 28-39    | compileDate     | String | asciiFixed | date of complied string 固件编译日期字符串, 使用 `ASCII` 编码                  |
| 40-47    | compileTime     | String | asciiFixed | time of complied string 固件编译时间字符串，使用 `ASCII` 编码                  |
| 48-59    | description     | String | utf8       | description string 固件字符串描述，使用 `utf8` 编码                            |
| 60-77    | hash            | array  |            | 固件bin格式内容的hash, 追加在固件最后的。非编译生成, 调试固件时此值为全 `0xff`<br />The hash of the firmware bin format content, appended to the end of the firmware. Not generated by compilation; this value is all `0xff` when debugging the firmware. |

note:

1. 设备固件信息为详细信息，其本身是用于工具打包使用。<br />The device firmware information is detailed information, which is used for tool packaging.
2. 在跟踪固件问题时用于获取更详细的固件信息，平常时不需要使用此信息。<br />Used to obtain more detailed firmware information when tracking firmware issues. This information is not normally required.
3. `magic`在打包时，区分识别当前的信息结构格式。`magic`的选择不要与指令码相同，避免编译固件中有多个相同导致难以自动定位到固件信息位置。`magic` distinguishes and identifies the current information structure format during packaging. The selection of `magic` should not be the same as the instruction code to avoid multiple identical entries in the compiled firmware, which would make it difficult to automatically locate the firmware information.
4. 设备固件信息在程序内部不再使用 `绝对或相对定位`,同时禁止使用 `绝对或相对定位`避免链接过程段区分割。<br />Device firmware information is no longer used in the program internally with `absolute or relative location`, and the use of `absolute or relative location` is prohibited to avoid link segment segmentation.

---

### (0x09)设备硬件生产信息 device hardware production information(待评估) :id=0x09

**功能描述 Function description**

用于查询当前设备硬件生产信息, 比如硬件的生产批次，外壳颜色等。<br />Used to query the current device hardware production information, such as hardware production batch, shell color, etc.

**读返回数据格式 Read return data format**

| Byte No. | Parameter    | Type   | Converter | Description    |
| -------- | ------------ | ------ | --------- | -------------- |
| 1        | length       | byte   |           | length         |
| 2        | key          | byte   |           | key            |
| 3-6      | optional | uint   |           | parameters       |


Note:

1. 写入操作放到其他命令中，需要更高权限来操作写入，防止误修改。<br />Write operations are placed in other commands and require higher permissions to prevent accidental modifications.
2. 待工程部门反馈需求后完善。<br />To be refined after receiving feedback from the engineering department.

---

### (0x0A) WiFi MAC :id=0x0A

**功能描述 Function description**

获取 WiFi 功能模组的MAC。<br />Obtain the MAC address of the WiFi function module.

**读返回数据格式 Read return data format**

| Byte No. | Parameter | Type | Converter | Description |
| -------- | --------- | ---- | --------- | ----------- |
| 1        | length    | byte |           | length      |
| 2        | key       | byte |           | key         |
| 3-8      | mac       | MAC  | leMac     | MAC         |

---

### (0x0B)电池状态 battery status :id=0x0B

**功能描述 Function description**

获取电池的状态信息。<br />Obtain battery status information.

**读返回数据格式 Read return data format**

| Byte No. | Parameter           | Type | Converter | Description                                         |
| -------- | ------------------- | ---- | --------- | --------------------------------------------------- |
| 1        | length              | byte |           | length                                              |
| 2        | key                 | byte |           | key                                                 |
| 3        | level               | byte |           | 电量百分比，范围：[0, 100]<br />Battery percentage, range: [0, 100]                          |
| 4        | isCharging          | byte | bool      | 是否在充电<br />0- 未在充电 <br />1- 正在充电 <br />Charging status <br />0- Not charging <br />1- Charging      |
| 5-8      | voltage             | uint |           | unit: mV 电池当前电压，单位:mV                      |
| 9-12     | capacity            | uint |           | unit: mAh 电池容量，单位: mAh                       |
| 13-16    | chargerInputVoltage | uint |           | optional, unit: mV 充电头当前充电输入电压           |
| 17-20    | chargerInputCurrent | uint |           | optional, unit: mA 充电头当前充电输入电流, unit: mA |

Note:

1. `optional`为可填写参数，不支持设备不填写。<br />`optional` is an optional parameter that can be filled in, but devices cannot be left blank.
2. 电池容量可能识别不准，评估可能删除<br />Battery capacity may not be accurately identified, and the assessment may be deleted.

---

### (0x0C)设备运行时长 device run time :id=0x0C

**功能描述 Function description**

获取设备上电后已经运行的时长。<br />Get the duration that the device has been running since it was powered on.

**读返回数据格式 Read return data format**

| Byte No. | Parameter | Type | Converter | Description                                   |
| -------- | --------- | ---- | --------- | --------------------------------------------- |
| 1        | length    | byte |           | length                                        |
| 2        | key       | byte |           | key                                           |
| 3-6      | runTime   | uint |           | run time, unit: second 设备运行时长，单位: 秒 |

---

### (0x0D)协议API版本 Protocol API Version :id=0x0D

**功能描述 Function description**

获取当前设备已经实现的 API 版本，从而识别当前设备支持的API接口协议参数。<br />Obtain the API version currently implemented on the device to identify the API interface protocol parameters supported by the device.

**读返回数据格式 Read return data format**

| Byte No. | Parameter  | Type | Converter | Description |
| -------- | ---------- | ---- | --------- | ----------- |
| 1        | length     | byte |           | length      |
| 2        | key        | byte |           | key         |
| 3-6      | apiVersion | uint |           | api version |

Note:

1. api version 的计算方式：`major` * 1000000 + `minor` * 1000 + `hotfix`, 显示格式为v1.0.0, v2.0.0<br />Calculation method for API version: `major` * 1000000 + `minor` * 1000 + `hotfix`, displayed in the format v1.0.0, v2.0.0
2. api version 的 `major`为大版本修改，与前面版本不兼容。`minor`为次版本，添加新的协议功能描述 Function description。`hotfix`对接口进行修订或更新描述。<br />The `major` version indicates a major version change, which is incompatible with previous versions. The `minor` version indicates a minor version change, adding new protocol functionality descriptions. The `hotfix` version indicates revisions or updates to the interface description.

---

### (0x0E)SDK API版本 SDK API Version :id=0x0E

**功能描述 Function description**

获取当前设备SDK API 版本，获取当前设备中SDK所支持的功能。为二次开发 `SDK`预留。
<br />Retrieve the current device SDK API version and the features supported by the SDK on the current device.
Reserved for secondary development of the SDK.

**读返回数据格式 Read return data format**

| Byte No. | Parameter     | Type | Converter | Description     |
| -------- | ------------- | ---- | --------- | --------------- |
| 1        | length        | byte |           | length          |
| 2        | key           | byte |           | key             |
| 3-6      | SdkApiVersion | uint |           | SDK api version |

Note:

1. api version 的计算方式：`major` * 1000000 + `minor` * 1000 + `hotfix`, 显示格式为v1.0.0, v2.0.0<br />API version calculation method: `major` * 1000000 + `minor` * 1000 + `hotfix`, displayed in the format v1.0.0, v2.0.0
2. api version 的 `major`为大版本修改，与前面版本不兼容。`minor`为次版本，添加新的协议功能描述 Function description。`hotfix`对接口进行修订或更新描述。<br />The `major` version indicates a major version change, which is incompatible with previous versions. The `minor` version indicates a minor version, adding new protocol functionality descriptions. The `hotfix` version indicates revisions or updates to the interface description.
3. 考虑到不同客户使用的 `SDK`功能不一样（功能裁剪不一样），`SDK API`版本号更多是作为一个标准来使用。<br />Considering that different customers use different `SDK` functionalities (with varying functionalities), the `SDK API` version number is primarily used as a standard.

---
### (0x10) GSM 模块额外信息 GSM Module Extra Information :id=0x10


**功能描述 Function description**

提供或补充 GSM 模块的额外信息。<br />Provide or supplement additional information about GSM modules.

**读返回数据格式 Read return data format**

| Byte No.  | Parameter | Type   | Converter | Description |
| --------- | --------- | ------ | --------- | ----------- |
| 1         | length    | byte   |           | length      |
| 2         | key       | byte   |           | key         |
| #string 1 | model     | String | utf8      | Model       |
| #string 2 | fwVersion | String | utf8      | FW version  |
| #string 3 | revision  | String | utf8      | Revision    |
| #string 4 | qmbn      | String | utf8      | QMBN        |
| #string 5 | svn       | String | utf8      | SVN         |

Note:

* 1. Model 指的是模块的型号，例如: `SIMCOM_SIM7500A`。<br />Model refers to the module model number, for example: `SIMCOM_SIM7500A`.
* 2. FW version 指模块固件信息, 例如: `SIM7500A_B04V02_240227`。<br />FW version refers to the module firmware information, for example: `SIM7500A_B04V02_240227`.
* 3. Revsion 指模块的修改版本信息，例如: `LE11B04SIM7500A_CUS_YW03`。<br />Revision refers to the module's revision version information, for example: `LE11B04SIM7500A_CUS_YW03`.
* 4. SVN(Software Version Number)通常是由通信模块厂商在模块固件里预设的一个软件版本号，属于模块自身固有的信息。<br />SVN (Software Version Number) is typically a software version number predefined by the communication module manufacturer in the module's firmware, and is inherent to the module itself.
* 5. QMBN(Qualcomm Module Build Number)是 Qualcomm 公司在模块固件里预设的一个构建版本号，属于模块自身固有的信息。例如:`9X07_SIM7500A_P1.02_20220706`。<br />QMBN (Qualcomm Module Build Number) is a build revision number predefined by Qualcomm in the module firmware, which is inherent to the module itself. For example: `9X07_SIM7500A_P1.02_20220706`.
* 6. SVN的获取方式为向厂家获取，暂时没有提供通过软件方式直接从模块获取。<br />The SVN is obtained from the manufacturer; currently, there is no software-based method to directly retrieve it from the module.

**模块额外信息获取方式 命令方式 How to obtain additional module information Command method**

```plain
// Query Module Information
AT+SIMCOMATI\r\n

Manufacturer: SIMCOM INCORPORATED\r\n
Model: SIMCOM_SIM7500A\r\n
Revision: LE11B04SIM7500A_CUS_YW03\r\n
SIM7500A_B04V02_240227\r\n
QCN: \r\n
IMEI: 016485002491632\r\n
MEID: \r\n
+GCAP: +CGSM\r\n
DeviceInfo: 173,170\r\n
\r\n
OK\r\n

// Query Module QMBN
AT+CQCNV\r\n
+CQCNV: "9X07_SIM7500A_P1.02_20220706"\r\n
\r\n
OK\r\n
```

---

### (0x11)SIM 卡额外信息 SimCard extra information :id=0x11

**功能描述 Function description**

查询 SIM 卡的额外信息。<br />Check additional information about your SIM card.

**读返回数据格式 Read return data format**

| Byte No. | Parameter | Type   | Converter | Description   |
| -------- | --------- | ------ | --------- | ------------- |
| 1        | length    | byte   |           | length        |
| 2        | key       | byte   |           | key           |
| #string1 | msisdn    | String | utf8      | MSISDN string |
| #string2 | ismi      | String | utf8      | ISMI string   |

Note:

1. MSISDN 指simcard中用户手机号码的完整国际形式，例如中国用户的 `MSISDN`格式为: `+86135-xxxx-xxxx`之类的。<br />MSISDN refers to the complete international format of the user's mobile phone number on the SIM card. For example, the MSISDN format for Chinese users is: +86135-xxxx-xxxx.
2. ISMI是simcard的唯一标识符，存储中SIMCARD中,用于区分不同的SIM卡。<br />ISMI is the unique identifier of the SIM card, stored on the SIM card, used to distinguish between different SIM cards.

**数据获取方式 Data acquisition method**

```plain
// Query MSISDN
AT+CNUM\r\n

+CNUM: "","12300010055",177\r\n

OK\r\n
```

```plain
// Query ISMI
AT+CIMI\r\n

001010000000055\r\n
OK\r\n

AT+CIMIM\r\n

001010000000055\r\n

OK
```

---

### (0x20)设备自定义名称 device custom name :id=0x20

**功能描述 Function description**

可以设置/查询 设备的自定义名称。<br />You can set/query the custom name of the device.

**写请求/读返回数据格式 Write request/read response data format**


| Byte No. | Parameter  | Type   | Converter | Description                      |
| -------- | ---------- | ------ | --------- | -------------------------------- |
| 1        | length     | byte   |           | length                           |
| 2        | key        | byte   |           | key                              |
| #3-n     | customName | String | utf8      | 自定义设备名称字符串, 最大32字节<br />Custom device name string, maximum 32 bytes |

Note:

1. 设备自定义名称用来更新蓝牙广播名称时，当名称过长时，截断数据来进行广播。比如 `hello world`，可能只广播 `hello`。<br />When using a custom device name to update the Bluetooth broadcast name, if the name is too long, the data will be truncated for broadcasting. For example, `hello world` may only broadcast `hello`.
2. 带屏幕显示的设备，可以选择在合适的位置显示 `设备自定义的名称`。<br />Devices with screens can display a custom name in a suitable location.
3. `legacy`广播最大只支持31个字节，加上 `len-type`占用，允许最大名称为字节长度为 `29字节`。但在 `advertising extend`中可以扩展到 `250+`字节。<br />The `legacy` broadcast supports a maximum of 31 bytes, plus the space occupied by `len-type`, allowing for a maximum name length of `29 bytes`. However, in `advertising extend`, this can be extended to `250+` bytes.
4. `BLE central`在扫描设备时，如果是只能通过 `name`来识别设备类型，则不建议使用自定义名称来更新广播数据。<br />When scanning devices, if the device type can only be identified by its name, it is not recommended to use a custom name to update the broadcast data.

---

### (0x21)设备自定义识别码 device custom identify :id=0x21

**功能描述 Function description**

用于设置或查询设备的自定义识别码，自定义识别码可选用于：<br />Used to set or query the device's custom identification code. The custom identification code can be used for:

+ 与服务器通讯时携带 <br />Carry when communicating with the server
+ 其他应用 <br />Other applications


**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                     |
| -------- | --------- | ----- | --------- | ------------------------------- |
| 1        | length    | byte  |           | length                          |
| 2        | key       | byte  |           | key                             |
| 3-n      | customId  | array |           | custum id,  length: [2-32]bytes |

Note:

1. 无开关取消 <br />No switch cancellation
2. 长度定义至少 `2bytes`, 少于2字节表示不启用。<br />The length must be at least 2 bytes, and less than 2 bytes means it is not enabled.
3. 自定义识别码由用户自定义数据结构格式，比如customId[0]表示 `encodingType`，后续内容按编码填充即可。客户自行维护编码。<br />The custom identification code is a user-defined data structure format, such as customId[0] representing `encodingType`, and the subsequent content is filled in according to the encoding. The customer is responsible for maintaining the encoding.
4. 编写工具方便客户按自定义规则来批量更新设备的自定义识别码。(后续建议)<br />The tool allows customers to batch update device identification codes according to custom rules. (Follow-up suggestion)
5. 上位机展示当前字段时，应当允许切换显示格式，比如: HEX/UTF8/Unicode<br />When the host computer displays the current field, it should allow switching between display formats, such as HEX/UTF8/Unicode.
6. customId内容由客户或服务器端根据需求自定义，设备不对此数据进行解析/修改/展示。建议客户或服务器端根据需求自定义数据结构, 但考虑是否对设备分类、分组、分客户?<br />The customId content is customized by the customer or server side according to requirements. The device does not parse, modify, or display this data. It is recommended that customers or server sides customize the data structure according to requirements, but consider whether to classify, group, or categorize customers.

---

### (0x22)NFC 自定义数据设置 NFC records:id=0x22

**功能描述 Function description**

用于设置 NFC 的消息内容。<br />Used to set the message content of NFC.


**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                                                                                                                                               |
| -------- | --------- | ----- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte  |           | length                                                                                                                                                    |
| 2        | key       | byte  |           | key                                                                                                                                                       |
| 3        | index     | byte  |           | record index, range:[0,9]<br /> 最多10条记录数据                                                                                                                 |
| 4        | type      | byte  |           | record类型,非自定义类型使用 `NDEF`数据格式<br />0:清除record <br />1:url/uri <br />2:自定义url/uri <br />3: 文本 <br />4: 蓝牙oob <br />255: 自定义数据<br /><br />record type, `NDEF` format<br />0:clear record <br />1:url/uri <br />2:custom url/uri <br />3: plain text <br />4: bluetooth oob <br />255: custom data |
| 5-n      | content   | array |           | record内容<br />record content                                                                                                                                                |

Note:

1. NFC tag需要设置为只读，不允许使用 `NFC`方式来修改 `tag`内容。<br />The NFC tag must be set to read-only mode, and it is not permitted to modify the tag content using the NFC method.
2. 由于NFC reader不同平台处理 `NDEF`内容优先机制不一样，请谨慎使用不同的 `record`数据。<br />Since NFC readers on different platforms have different priority mechanisms for processing NDEF content, please use different record data with caution.
3. 如果是自定义的 `record`内容也使用 `NDEF`格式，注意手动标志为 `ME`标志。同时使用自定义数据时，优先使用倒序使用索引，避免对前面记录产生影响。即 `自定义数据`从 `index=9`开始使用。<br />If the `record` content is custom, it also uses the `NDEF` format. Note that it is manually marked as the `ME` flag. When using custom data, prioritize reverse indexing to avoid affecting previous records. That is, `custom data` starts from `index=9`.
4. 在NFC中存放固件基本信息，当设备出现问题无法开机时，通过读取NFC内容来获取固件基本信息。固件基本信息可作为默认 `record`，不占用当前协议的索引。<br />Store basic firmware information in NFC. When the device encounters a problem and cannot boot up, read the NFC content to obtain the basic firmware information. The basic firmware information can be used as the default `record` and does not occupy the index of the current protocol.

!> record类型及对应内容待补充完整, 涉及到 `NDEF`格式单独文件介绍。<br />The record types and corresponding content are pending completion, involving a separate file description for the `NDEF` format.

---

### (0x23)用户信息 user information :id=0x23


**功能描述 Function description**

用于设置或查询用户的基本个人信息。<br />Used to set or query basic personal information about users.

**读返回数据格式 Read return data format**

| Byte No.  | Parameter  | Type   | Converter | Description                                                                                                                                                     |
| --------- | ---------- | ------ | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1         | length     | byte   |           | length                                                                                                                                                          |
| 2         | key        | byte   |           | key                                                                                                                                                             |
| 3         | gender     | byte   |           | gendar 性别<br />0-male<br />1-female<br />2-unknown                                                                                                            |
| 4         | bloodType  | byte   | bits      | blood type<br />`bit0-3:`<br />0- O 型 <br />1- A 型 <br />2- B 型 <br />3- AB 型 <br />15-default,空<br /> `bit4:`<br />0- Rh negative<br />1- Rh positive |
| 5-6       | height     | ushort |           | height, unit: cm/10, range:[480-2300]                                                                                                                           |
| 7-8       | weight     | ushort |           | weight, unit: kg/10, range:[200-1500]                                                                                                                           |
| 9-10      | birthYear  | ushort |           | year in date of birth, range:[1900-2100]                                                                                                                        |
| 11        | birthMonth | byte   |           | month in date of birth, range:[1,12]                                                                                                                            |
| 12        | birthDay   | byte   |           | day in date of birth, range: [1-31]                                                                                                                             |
| #string 1 | name       | String | utf8      | name string, Type: String, Encode: utf8 名字字符串，类型为字符串，使用 utf8 编码, 最大长度为 32 字节                                                            |
| #string 2 | image      | String | utf8      | image fullname Type: String, Encode: utf8 头像图片路径，类型为字符串, 使用 utf8 编码，最大长度为 64 字节                                                        |

Note:

1. day in date of birth 出生日需要满足月份有效日期。<br />day in date of birth The date of birth must meet the valid date for the month.
2. 头像图片路径，可以为本地存储文件名，也可以为 URL/URI。<br />Avatar image path, which can be a local storage file name or a URL/URI.

---

### (0x24)使用场景 usage scenario :id=0x24

**功能描述 Function description**

对应之前的工作模式(work mode)，为区别于模式称呼，现修改为使用场景，针对不同的使用场景，加载默认的工作参数。
<br />使用场景通常描述：面向用户的行为描述、关注外部环境因素、描述具体应用情况、强调用户体验需求。
<br />工作模式通常描述：面向设备状态描述、关注内部运行参数、描述系统工作状态、强调性能与功耗。
<br />Corresponding to the previous work mode, in order to distinguish it from the mode name, it has been changed to usage scenario. For different usage scenarios, default work parameters are loaded.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter  | Type | Converter | Description                                                                                  |
| -------- | ---------- | ---- | --------- | -------------------------------------------------------------------------------------------- |
| 1        | length     | byte |           | length                                                                                       |
| 2        | key        | byte |           | key                                                                                          |
| 3        | type       | byte |           | type, range:[0-6], default: 1<br />0- 不使用预设场景/No preset scenario, RFU<br />1/2/3/4/5/6- 启对应用场景参数/Setup preset scenario parameters |
| 4-7      | parameters | uint |           | parameters 参数，不同的场景参数定义不一样                                                    |


+ **type=0 不支持通过协议将场景类型设置为 0，Does not support setting the scene type to 0 via protocol.**
+ type=1/2/3/6 写请求/读返回数据格式 type=1/2/3/6 Write request/read response data format

| Byte No. | Parameter | Type | Converter | Description                |
| -------- | --------- | ---- | --------- | -------------------------- |
| 1        | length    | byte |           | length                     |
| 2        | key       | byte |           | key                        |
| 3        | type      | byte |           | type = 1                   |
| 4-7      | meanless  | uint |           | meanless data 无意义的参数 |

+ type=4 写请求/读返回数据格式 type=1/2/3/6 Write request/read response data format

| Byte No. | Parameter      | Type | Converter | Description                                                                                                                                                                               |
| -------- | -------------- | ---- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length         | byte |           | length                                                                                                                                                                                    |
| 2        | key            | byte |           | key                                                                                                                                                                                       |
| 3        | type           | byte |           | type = 4                                                                                                                                                                                  |
| 4-7      | uploadInterval | uint |           | interval of upload data to server schedule, unit: seconds， range:[10minutes, 7days]<br />上传数据到服务器计划间隔, 单位: 秒，范围:[10 分钟, 7 天 ] 0- 表示关闭定时上传数据到服务器功能。 |

+ type=5 写请求数据格式

| Byte No. | Parameter      | Type | Converter | Description                                                                                                                                                                             |
| -------- | -------------- | ---- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length         | byte |           | length                                                                                                                                                                                  |
| 2        | key            | byte |           | key                                                                                                                                                                                     |
| 3        | type           | byte |           | type = 5                                                                                                                                                                                |
| 4-7      | uploadInterval | uint |           | interval of upload data to server schedule, unit: seconds， range:[10minutes, 7days]<br />上传数据到服务器计划间隔, 单位: 秒，范围:[10 分钟, 7 天 ] 0- 表示关闭定时上传数据到服务器功能 |

Note:

1. 各场景的功能定义及其影响的参数，需要参考产品功能规格书。<br />The functional definitions of each scenario and the parameters that affect them can be found in the product functional specifications.

!> todo: 待重新修改参数(把对应功能模块参数提取出来), 心跳包功能/动静修改上报周期功能/定时定位功能/TCP常连接/GSM待机/GPS常开<br />Todo: Parameters need to be revised (extract parameters corresponding to functional modules), heartbeat packet function/dynamic modification reporting cycle function/timed positioning function/TCP constant connection/GSM standby/GPS constant on。


---

### (0x25) 勿扰模式 do not disturb mode :id=0x25

**功能描述 Function description**

勿扰模式用于在指定时间段内，禁止部分功能不进行声、光、振动方式来影响使用者。<br />Do Not Disturb mode is used to disable certain functions during specified time periods so that they do not disturb the user with sound, light, or vibration.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type  | Converter | Description                                       |
| -------- | ----------- | ----- | --------- | ------------------------------------------------- |
| 1        | length      | byte  |           | length                                            |
| 2        | key         | byte  |           | key                                               |
| 3        | index       | byte  |           | 索引，范围：0-1                                   |
| 4        | enable      | byte  | bool      | 是否启用勿扰模式Enable Do Not Disturb mode<br />0- disabled<br />1- enabled |
| 5        | startHour   | byte  |           | 开始时间 start hour, range:[0, 23]                     |
| 6        | startMinute | byte  |           | 开始时间 start minute, range:[0, 59]                    |
| 7        | endHour     | byte  |           | 结束时间 end hour, range:[0, 23]                      |
| 8        | endMinute   | byte  |           | 结束时间 end minute, range:[0, 59]                    |
| 9-n      | options     | array |           | 额外的参数选项，可选. 以4字节对齐来扩展<br />Additional parameter options, optional. Extend with 4-byte alignment.           |

Note:

1. 开始时间小于结束时间，则时间段在同一天内。<br />If the start time is earlier than the end time, the time period is within the same day.  
2. 开始时间大于结束时间，则时间段跨越天。<br /> If the start time is later than the end time, the time period spans multiple days.  
3. 开始时间等于结束时间，则强制关闭勿扰模式。<br />If the start time is equal to the end time, the Do Not Disturb mode is forced to be disabled.  
4. 额外的选项参数为可选，未下发则维持原来值不变。但 `读取操作`会返回额外的参数。可用作勿扰期间功能应用开关，具体应用参数对应的行为由另外的应用来确定，即 `options`参数允许不同设备修改为不同功能使用。<br />Additional option parameters are optional; if not specified, the original values remain unchanged. However, the `read operation` will return additional parameters. These can be used as toggle switches for features during Do Not Disturb mode, with the specific behavior of each parameter determined by another application. That is, the `options` parameter allows different devices to be modified for different functionalities.

---

### (0x26)按键快捷功能 button shortcut function :id=0x26

**功能描述 Function description**

按键功能设置，可以自定义按键按下时的任务。<br />Key function settings allow you to customize the tasks performed when a key is pressed or double click.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description                                                                                                                               |
| -------- | --------- | ------ | --------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte   |           | length                                                                                                                                    |
| 2        | key       | byte   |           | key                                                                                                                                       |
| 3        | index     | byte   |           | 索引, 范围根据设备当前支持的按键<br />0- SOS 按键 <br /><br />Index, range based on keys currently supported by the device<br />0- SOS key                                                                                  |
| 4        | group     | byte   |           | 分组，允许按键组合绑定<br />0- 无效分组 <br />others- 分组编号<br /><br />Grouping, allowing key combinations to be bound<br />0- Invalid grouping <br />others- Group number                                                                      |
| 5        | enable    | byte   |           | 使能<br />0- 不使用功能绑定<br />1- 使用功能绑定 <br /><br />Enable<br />0- Do not use feature binding<br />1- Use feature binding                                                                                       |
| 6        | feedback  | byte   | bits      | 按键反馈<br />bit0: 0- 不振动 1-振动反馈 <br />bit1: 0- 不使用提示音 2-使用提示音 <br />bit2: 0- 不使用 LED 提示 2-使用 LED 提示<br /><br />feedback <br />bit0: 0- no vibrate 1-vibratiion feedback <br />bit1: 0- no prompt audio 2-audio prompt <br />bit2: 0- no LED alert  2-LED alert |
| 7        | mode      | byte   |           | 模式<br />0- 长按 <br />1- 双击 <br /><br />mode<br />0- long press <br />1- double click                                                                                                               |
| 8-9      | timeLimit | ushort |           | 时间要求， 单位: ms<br />mode=0, 长按时长 <br />mode=1, 双击最大间隔<br /><br />time limit. unit: ms<br />mode=0, time of pressed  <br />mode=1, max interval of double click               |
| 10       | action    | byte   |           | task/action 执行的任务或动作<br />0- 无动作<br />1-10：拨打联系人号码(1..10)  <br /><br />task/action <br />0- no task<br />1-10：dial contact number(1..10)                                                            |

Note:

1. index-0 默认为 SOS 按键<br />index-0 Default SOS button
2. feedback 中，注意设备有无对应输出，无对应输出能力的则不起作用。<br />In feedback, pay attention to whether the device has a corresponding output. If it does not have the corresponding output capability, it will not work.
3. 分组编号用于组合按键功能，在启用分组后，要满足所有按键触发时才会执行任务或动作。（预留功能扩展）Group numbers are used to combine key functions. After enabling grouping, all keys must be pressed before a task or action is executed. (Reserved for future functionality expansion)

**型号对应的按键索引 Key index corresponding to model number**

+ W101 按键索引表，按键数: 1 <br />W101 Key Index Table, Number of Keys: 1

TODO: 添加对应图片标注 <br />TODO: Add corresponding image annotations

| index | button name |
| ---- | -------- |
| 0    | SOS 按键/SOS button |

---
### (0x27) 睡眠模式 sleep mode :id=0x27

**功能描述 Function description**

睡眠模式用于在指定时间段内，禁止部分功能不进行声、光、振动方式来影响使用者。
同时考虑是否对需要开启助眠功能(声/光/电方式主动调弱一个等级)
<br />Sleep mode is used to disable certain functions during a specified time period so that they do not affect the user through sound, light, or vibration.
At the same time, consider whether to enable the sleep aid function (actively reducing the sound/light/electricity level by one level).

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type  | Converter | Description                                                                                        |
| -------- | ----------- | ----- | --------- | -------------------------------------------------------------------------------------------------- |
| 1        | length      | byte  |           | length                                                                                             |
| 2        | key         | byte  |           | key                                                                                                |
| 3        | index       | byte  |           | 索引，范围：0-1   <br /><br />Index, range: 0-1                                                                                 |
| 4        | enable      | byte  | bool      | enable<br />0- disabled<br />1- enabled                                                  |
| 5        | startHour   | byte  |           | 开始时间 start hour, range:[0, 23]                                                                      |
| 6        | startMinute | byte  |           | 开始时间 start minute, range:[0, 59]                                                                     |
| 7        | endHour     | byte  |           | 结束时间 end hour, range:[0, 23]                                                                       |
| 8        | endMinute   | byte  |           | 结束时间 end minute, range:[0, 59]                                                                     |
| 9        | sleepAid    | byte  |           | optional: 是否使用睡眠辅助,提前指定时提示且调低声光电提示等级<br />0:不启用 <br />1-60：提前分钟数<br /><br />Whether to use sleep assistance, set a reminder in advance, and lower the sound and light reminder level<br />0: Disabled <br />1-60: Number of minutes in advance |
| 10-n     | options     | array |           | opiotional: 额外的参数选项，可选. 以4字节对齐来扩展 <br />optional: extra parameter options, optional. Aligned to 4 bytes for extension |

Note:

1. 开始时间小于结束时间，则时间段在同一天内。<br />If the start time is earlier than the end time, the time period is within the same day.  
2. 开始时间大于结束时间，则时间段跨越天。<br />If the start time is later than the end time, the time period spans multiple days.  
3. 开始时间等于结束时间，则强制关闭睡眠模式。<br />If the start time is equal to the end time, sleep mode is forced to close.  
4. 额外的选项参数为可选，未下发则维持原来值不变。但 `读取操作`会返回额外的参数。可用作睡眠期间功能应用开关，具体应用参数对应的行为由另外的应用来确定，即 `options`参数允许不同设备修改为不同功能使用。<br />Additional option parameters are optional; if not specified, the original values remain unchanged. However, the `read operation` will return additional parameters. These can be used as functional application switches during sleep mode, with the specific behavior of each parameter determined by another application. That is, the `options` parameter allows different devices to be modified for different functional uses.
5. 在 `睡眠模式`中，当检测到达 `睡眠开始时间段`时，提示用户作息，在后期数据分析时结合此时间段来统计用户每天的睡眠达成率。<br />In `sleep mode`, when the system detects that the `sleep start time period` has been reached, it prompts the user to adjust their schedule. During subsequent data analysis, this time period is used to calculate the user's daily sleep achievement rate.

---

### (0x28) 自动低电量模式 auto low power mode :id=0x28

**功能描述 Function description**

低电量模式用于在指定时间段内，自动进入低电量模式，其表现为关闭耗电模式的工作，比如 `GSM的网络连接/WiFi的网络连接/GPS定位`等。<br />Low power mode is used to automatically enter low power mode during a specified time period, which manifests itself by turning off power-consuming functions such as GSM network connection, WiFi network connection, GPS positioning, etc.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type | Converter | Description                                             |
| -------- | ----------- | ---- | --------- | ------------------------------------------------------- |
| 1        | length      | byte |           | length                                                  |
| 2        | key         | byte |           | key                                                     |
| 3        | index       | byte |           | 索引，范围：0-1 <br /><br />Index, range: 0-1                                         |
| 4        | enable      | byte | bool      | enable <br />0- disabled<br />1- enabled |
| 5        | startHour   | byte |           | 开始时间 start hour, range:[0, 23]                           |
| 6        | startMinute | byte |           | 开始时间 start minute, range:[0, 59]                          |
| 7        | endHour     | byte |           | 结束时间 end hour, range:[0, 23]                            |
| 8        | endMinute   | byte |           | 结束时间 end minute, range:[0, 59]                          |

Note:

1. 开始时间小于结束时间，则时间段在同一天内。<br />If the start time is earlier than the end time, the time period is within the same day.  
2. 开始时间大于结束时间，则时间段跨越天。<br />If the start time is later than the end time, the time period spans multiple days.  
3. 开始时间等于结束时间，则强制关闭自动低电量模式。<br />If the start time is equal to the end time, force close the automatic low power mode.
4. `自动低电量模式` 指设备在指定时间段中，自动进入低电量模式，其表现为关闭耗电模式的工作，比如 `GSM的网络连接/WiFi的网络连接/GPS定位`等。<br />`Automatic low-power mode` refers to the device automatically entering low-power mode during a specified time period, which involves disabling power-consuming functions such as GSM network connections, Wi-Fi network connections, and GPS location services.
5. 当前模式需要增加限制条件，比如检测在家时。且需要设备执行完任务后，才能关闭耗电应用。<br />The current mode requires additional conditions, such as detecting when the device is at home. Additionally, power-consuming applications should only be disabled after the device has completed its tasks.

---

### (0x31)格式偏好 format preference :id=0x31

**功能描述 Function description**

设置/查询当前设备的单位格式喜好：<br />Set/check the unit format preferences for the current device:

+ 长度/高度 单位 <br />Length/height units
+ 重量 单位 <br />Weight units
+ 温度 单位 <br />Temperature units
+ 小时 格式 <br />Hour format
+ 年月日格式<br />Year/month/day format

在其他 `key`中存在上述类型的显示的，可通过获取 `格式偏好` 来更新页面单位的展示。<br />If the above type of display exists in other `keys`, you can update the display of page units by obtaining the `format preference`.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter       | Type | Converter | Description                                                          |
| -------- | --------------- | ---- | --------- | -------------------------------------------------------------------- |
| 1        | length          | byte |           | length                                                               |
| 2        | key             | byte |           | key                                                                  |
| 3        | lengthUnit      | byte |           | length/height unit, 长度或高度 单位 <br />0- cm 厘米/米 <br />1- ft-in 英寸/英尺 |
| 4        | weightUnit      | byte |           | weight unit, 重量单位 <br />0- kilogram, 公斤 <br />1- lbs 英磅                  |
| 5        | temperatureUnit | byte |           | temperature unit, 温度单位 <br />0- 摄氏度 Celsius  <br />1- 华氏度Fahrenheit                       |
| 6        | hourFormat      | byte |           | hour format 小时格式 <br />0- 24 小时格式/24 hour format <br />1- 12 小时格式/12 hour format                   |
| 7        | dateFormat      | byte |           | date format 日期格式 <br />0- 年月日/Year-Month-Day <br />1- 月日年/Month-Day-Year <br />2- 日月年/Day-Month/Year                   |


Note:

1. date format 日期格式，在展示时应当使用对应显示语言进行展示。<br />Date format: The date format should be displayed in the corresponding display language.
2. 格式偏好 在 `自定义表盘中` 不生效。<br />Format preferences do not apply in the `Custom Watchface`
3. 格式偏好为系统全局默认格式。<br />Format preferences are the system-wide default format.
4. 部分应用中有其本身的格式偏好设定，比如天气应用使用它自带的单位设置。<br />Some apps have their own format preferences, such as weather apps that use their own unit settings.

---

### (0x32)语言偏好 language preference :id=0x32


**功能描述 Function description**

设置/获取 设备需要使用的语言类型<br />Set/get the language type that the device needs to use.

+ 音频提示语言 <br />Audio prompt language
+ 文本显示语言 <br />Text display language

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter           | Type   | Converter | Description                                                  |
| -------- | ------------------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length              | byte   |           | length                                                       |
| 2        | key                 | byte   |           | key                                                          |
| #string1 | voicePromptLanguage | String | utf8      | audio prompt language, max length 16bytes<br /> 音频提示语言，字符串类型，最长16 字节  |
| #string2 | textDisplayLanguage | String | utf8      | text display language, max length 16bytes<br /> 文本显示语言，字符串类型，最长 16 字节 |

Note:

1. 语言字符串的格式参考附录。或在线文档([https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r1/core/res/res/values/locale_config.xml](https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r1/core/res/res/values/locale_config.xml))<br />Refer to the appendix for language string formats. Or online documentation([https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r1/core/res/res/values/locale_config.xml](https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r1/core/res/res/values/locale_config.xml))
2. 需要增加更多语言支持时，请从参考附录中增加。<br />When additional language support is required, please add it from the reference appendix.

---

### (0x33)日期时间 date & time :id=0x33

**功能描述 Function description**

查询或设置 设备的时间日期。<br />Check or set the device's time and date.

**写请求/读返回 数据格式**

| Byte No. | Parameter | Type | Converter | Description                |
| -------- | --------- | ---- | --------- | -------------------------- |
| 1        | length    | byte |           | length                     |
| 2        | key       | byte |           | key                        |
| 3-6      | dateTime  | uint | unixTime  | unix 时间，由 1970-1-1 00:00:00 加上 `UTC`的秒数组成<br />Unix time consists of the number of seconds since January 1, 1970, 00:00:00 UTC. |


Note:

1. 前面章节已经提到过，unixTime时间为无符号的32bits数据。<br />As mentioned in previous chapters, unixTime is an unsigned 32-bit data type.
2. 时间的转换规则: 从 1970-1-1 00:00:00 加上 `UTC`的秒数<br />Time conversion rules: From 1970-1-1 00:00:00, add the number of seconds in UTC.

```csharp
DateTime dt = new DateTime(1970, 1, 1, 0, 0, 0);
dt = dt.AddSeconds(Utc);
return dt;
```

---

### (0x34)时区 time zone :id=0x34

**功能描述 Function description**

设置或查询 当前设备所使用的时区。<br />Set or query the time zone currently used by the device.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                                                                                                           |
| -------- | --------- | ----- | --------- | --------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte  |           | length                                                                                                                |
| 2        | key       | byte  |           | key                                                                                                                   |
| 3        | timeZone  | sbyte | timeZone  | time zone, Type:sbyte, unit: quarter, range:[-48, 56]<br />时区值，类型为有符号字节，单位: 一刻钟，取值范围:[-48, 56] |

Note:

1. 时区对应的有效值范围为：[-48 * 15, 56 * 15], 对应 [UTC -12:00，UTC +14:00 ]<br />
   Time zone value range: [-48 * 15, 56 * 15], corresponding to [UTC -12:00, UTC +14:00]
2. 关于UTC+13:00 [https://en.wikipedia.org/wiki/UTC+13:00](https://en.wikipedia.org/wiki/UTC+13:00) <br />About UTC+13:00 [https://en.wikipedia.org/wiki/UTC+13:00](https://en.wikipedia.org/wiki/UTC+13:00)

---

### (0x35)日期&时间 额外配置 date & time extra 配置 :id=0x35

**功能描述 Function description**

查询或设置 日期时间的自动更新。<br /> Check or set automatic updates for date and time.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter               | Type | Converter | Description                                                                                           |
| -------- | ----------------------- | ---- | --------- | ----------------------------------------------------------------------------------------------------- |
| 1        | length                  | byte |           | length                                                                                                |
| 2        | key                     | byte |           | key                                                                                                   |
| 3        | dayLightSavingTime      | byte |           | 使用的国家夏令时规则<br />0:不使用<br />1:美国 <br />2:欧洲<br />3:澳洲(澳大利亚)<br />4:澳洲(新西兰) <br /><br />National daylight saving time rules in use<br />0:disable<br />1:American <br />2:Europe<br />3:Australia<br />4:Australia (New Zealand)|
| 4        | winterTime              | byte |           | 使用的国家冬令时规则<br />0:不使用 <br />others:RFU    <br /><br />Winter time rules used in the country <br />0:disable <br />others:RFU                                              |
| 5-8      | baseStationSyncInterval | uint |           | 基站校时间隔, 单位:秒  <br />Base station calibration interval, unit: second                                                                                         |
| 9-12     | gpsSyncInterval         | uint |           | GPS 校时间隔, 单位:秒 <br />GPS time synchronization interval, unit: seconds                                                                                         |
| 13       | serverSync              | byte | bool      | 是否允许 服务器校时 <br />allow server time synchronization                                                                                  |
| 14       | bleSync                 | byte | bool      | 是否允许 BLE 校时 <br />allow BLE time synchronization                                                                                    |
| 15       | pcSync                  | byte | bool      | 是否允许上位机校时<br />aloow Upper computer time calibration                                                                                   |

---

### (0x40)远程服务器配置 remote server config :id=0x40

**功能描述 Function description**

通过设置远程服务器，提供设备访问连接并进行数据交互。<br />By setting up a remote server, provide device access connections and perform data interaction.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description                              |
| -------- | --------- | ------ | --------- | ---------------------------------------- |
| 1        | length    | byte   |           | length                                   |
| 2        | key       | byte   |           | key                                      |
| 3        | index     | byte   |           | index range:[0,3]，default: 0      |
| 4        | type      | byte   |           | type <br />0- TCPIP <br />1- UDP |
| 5-6      | port      | ushort |           | port 端口号                              |
| 7-n      | server    | String | utf8      | server's IP or domain<br /> 服务器 IP 地址或域名 字符串              |

Note:

1. 通过其他方式查询获取服务器配置时，未指定索引则默认使用 `index = 0` 的服务器配置。<br />When querying server configurations through other means, if no index is specified, the server configuration with `index = 0` is used by default.

---

### (0x41)APN 配置列表 APN list :id=0x41

**功能描述 Function description**

APN 的核心作用是作为移动设备连接蜂窝网络时的配置参数，用于标识和确定设备通过哪种方式访问特定网络。<br />
The core function of APN is to serve as a configuration parameter for mobile devices connecting to cellular networks, used to identify and determine how devices access specific networks.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description                                          |
| -------- | --------- | ------ | --------- | ---------------------------------------------------- |
| 1        | length    | byte   |           | length                                               |
| 2        | key       | byte   |           | key                                                  |
| 3-4      | index     | byte   |           | index                                            |
| 5-8      | plmn      | uint   |           | PLMN，由 MCC 和 MNC 组合，用于标识不同移动网络运营商<br />PLMN, composed of MCC and MNC, is used to identify different mobile network operators. |
| #string1 | apn       | String | ascii     | APN string,  length:[0, 32]                          |
| #string2 | userName  | String | ascii     | user name string,  length:[0, 32]                    |
| #string3 | password  | String | ascii     | password string, length:[0, 32]                      |

**读请求数据格式-查询索引对应的APN参数 read response data format-query with index**

| Byte No. | Parameter | Type | Converter | Description    |
| -------- | --------- | ---- | --------- | -------------- |
| 1        | length    | byte |           | length         |
| 2        | key       | byte |           | key            |
| 3-4      | index     | byte |           | index  |

**读请求数据格式-查询PLMN对应的APN参数 read response data format-query with plmn**


| Byte No. | Parameter | Type | Converter | Description                                          |
| -------- | --------- | ---- | --------- | ---------------------------------------------------- |
| 1        | length    | byte |           | length                                               |
| 2        | key       | byte |           | key                                                  |
| 3-4      | index     | byte |           | index will be ignored                                     |
| 5-8      | plmn      | uint |           | PLMN，由 MCC 和 MNC 组合，用于标识不同移动网络运营商<br />PLMN, composed of MCC and MNC, is used to identify different mobile network operators.|

Note:

1. PLMN用于自适配APN<br />PLMN used for self-adaptive APN

---

### (0x42)WiFI 联网热点列表 WLAN AP list :id=0x42

**功能描述 Function description**

用于设置或查询，当前设备可以连接的 WiFi 热点，以用于访问互联网络或内部网络以进行数据交换。<br />Used to set up or query the WiFi hotspots that the current device can connect to in order to access the Internet or internal networks for data exchange.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description                            |
| -------- | --------- | ------ | --------- | -------------------------------------- |
| 1        | length    | byte   |           | length                                 |
| 2        | key       | byte   |           | key                                    |
| 3        | index     | byte   |           | index, range:[0, 9]               |
| 4-7      | rfu       | uint   |           | RFU 后续可能需要指定认证方式和加密方式 <br />Subsequent steps may require specifying authentication and encryption methods. |
| #string1 | ssid      | String | utf8      | ssid , length:[0, 32]                  |
| #string2 | password  | String | utf8      | password, length:[0, 32]               |
|          | mac       | MAC    | leMac     | MAC，optional 可选参数                          |

Note:

1. MAC 作为可选参数，增加 `MAC`来作为补充项，避免相同的 `ssid`但 `password`不同时导致连接失败。<br />MAC as an optional parameter, add `MAC` as a supplementary item to avoid connection failures when the `ssid` is the same but the `password` is different.

---

### (0x43)WiFi 联网设置 WLAN connection setting :id=0x43

**功能描述 Function description**

设置或查询 WiFi 自动联网的工作参数。<br />Set or query the working parameters for automatic WiFi connection.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                                            |
| -------- | --------- | ----- | --------- | ------------------------------------------------------ |
| 1        | length    | byte  |           | length                                                 |
| 2        | key       | byte  |           | key                                                    |
| 3        | enable    | byte  | bool      | Allow Wi-Fi connection<br />0- disable <br />1- enable |
| 4        | rssi      | sbyte |           | rssi, range:[-127, 0]                                  |
| 5        | tryCount  | byte  |           | try count if connect fail <br />连接失败时尝试重连次数       |
| 6-9      | timeout   | uint  |           | timeout, unit: second <br />连接失败超时时长                 |

Note:

1. 当启用 WiFi 联网时，上报数据优先选择使用 WLAN。<br />When WiFi networking is enabled, data reporting will prioritize the use of WLAN.
2. timeout 无法设定 WiFi 模块扫描超时，可作为下次尝试连接的间隔延时。<br />timeout Cannot set the WiFi module scan timeout, which can be used as the delay interval for the next connection attempt.

---

### (0x44)与服务器保活心跳包设置 heartbeat keepalive to server :id=0x44

**功能描述 Function description**

心跳包，作为与服务器保持通讯的一种保活机制，有时也能防止长时间无数据时，基站将设备踢出网络。<br />Heartbeat packets, as a keepalive mechanism for maintaining communication with the server, can sometimes prevent the base station from kicking the device out of the network when there is no data for a long time.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                     |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                          |
| 2        | key       | byte |           | key                                                                             |
| 3        | enable    | byte | bool      | enable<br />0- Do not use heartbeat packets to keep the server alive. <br />1- Use heartbeat packets to keep the server alive. |
| 4-7      | interval  | uint |           | interval, range:[60, 86400], unit: second <br />定时间隔，单位:s, 范围:[60, 86400] (1分钟 - 24小时 )                            |


---

### (0x48)来电铃声音量 volume of ring tone :id=0x48

**功能描述 Function description**

用来设置或才查询设备接到来电时的铃声音量。<br />Used to set or query the ring volume when the device receives a call.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter      | Type | Converter | Description                     |
| -------- | -------------- | ---- | --------- | ------------------------------- |
| 1        | length         | byte |           | length                          |
| 2        | key            | byte |           | key                             |
| 3        | ringToneVolume | byte |           | ring tone volume, range:[0,100] |

Note:

1. 音量为0时，需要关闭 `speaker`的输出，不能简单将音量等级设置为0. 在 `codec`中，`0`对应的增益低但并非没有输出。<br />When the volume is set to 0, the `speaker` output must be turned off; simply setting the volume level to 0 is not sufficient. In the `codec`, `0` corresponds to low gain but not no output.
2. 来电铃声音量为全局来电铃声音量。<br />The incoming call ringtone volume is the global incoming call ringtone volume.
3. 独立应用如果有指定联系人号码对应的音量，则优先级覆盖全局设置。<br />If an independent application has a specified volume for a contact number, it takes precedence over the global settings.
4. 不同 `codec`对应的 `gain`分级不一样，由内部进行转换。<br />Different `codec`s have different `gain` levels, which are internally converted.
5. 前端可以分出更少的等级来进行音量调节，比如10个等级。<br />The frontend can divide the volume into fewer levels, such as 10 levels.


---

### (0x49)mic 增益 gain of mic :id=0x49

**功能描述 Function description**

设置或查询麦克风增益。<br />Set or check the microphone gain.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                 |
| -------- | --------- | ---- | --------- | --------------------------- |
| 1        | length    | byte |           | length                      |
| 2        | key       | byte |           | key                         |
| 3        | micGain   | byte |           | gain of mic, range:[0, 100] |

Note:

1. 此处 `mic`的增益为全局设置。<br />The gain for `mic` here is a global setting.
2. 独立应用中指定联系人号码对应的mic增益，则优先级覆盖全局设置。<br />Specify the mic gain corresponding to the contact number in the standalone app, which will override the global settings.


---

### (0x4A)speaker 音量 volume of speaker :id=0x4A

**功能描述 Function description**

设置或查询扬声器音量。<br />Set or check the speaker volume.
speaker音量影响功能：<br />Speaker volume affects the following functions:

* 通话过程音量 <br />volume during call talking 
* 回环音量<br />volume during speaker & mic loopback 


**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type | Converter | Description                       |
| -------- | ----------- | ---- | --------- | --------------------------------- |
| 1        | length      | byte |           | length                            |
| 2        | key         | byte |           | key                               |
| 3        | speakerGain | byte |           | volume of speaker, range:[0, 100] |

Note:

1. 此处的 `speaker`音量为全局设置。<br />The volume of the speaker here is a global setting.
2. 独立应用中指定联系人使用不同的 `speaker`增益时，则优先级覆盖全局设置。<br />When specifying contacts in a standalone application that use different `speaker` gain settings, the priority overrides the global settings.


---

### (0x4B)提示音音量 volume of audio prompt :id=0x4B

**功能描述 Function description**

设置或查询提示音的音量。<br />Set or check the volume of the audio prompt.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter    | Type | Converter | Description                      |
| -------- | ------------ | ---- | --------- | -------------------------------- |
| 1        | length       | byte |           | length                           |
| 2        | key          | byte |           | key                              |
| 3        | promptVolume | byte |           | volume of prompt, range:[0, 100] |

Note:

1. 提示音音量为全局提示音音量。<br />The audio volume is the global alert volume.
2. 独立应用中指定联系人使用不同的音频提示音量时，则优先级覆盖全局设置。比如查找设备功能，播放音量应该最较大的。<br />When different audio prompt volumes are specified for contacts in a standalone application, the priority overrides the global settings. For example, the playback volume should be the loudest when searching for devices.

---

### (0x4C)提示音开关 prompt function enable :id=0x4C

**功能描述 Function description**

打开/关闭音频提示开关。<br />Turn the audio prompt switch on/off. 

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                    |
| -------- | --------- | ---- | --------- | ------------------------------ |
| 1        | length    | byte |           | length                         |
| 2        | key       | byte |           | key                            |
| 3      | enable     | uint |           | main switch, 0- disable, 1- enable |



Note

1. `main switch`是总开关，`disable`后全局音频提示不生效。<br />The `main switch` is the master switch. When `disabled`, global audio prompts will not work.
2. 不同音频提示由提示执行流程配置来关闭。<br />Different audio prompts are disabled by the prompt execution process configuration.
3. 后续可在这里扩展`音频提示文件`快捷开关。<br />You can expand the `audio prompt file` quick switch here later.

---

### (0x50)GEO 检测配置 GEO detection config :id=0x50

**功能描述 Function description**

GEO，俗称电子围栏，用于画定指定区域，检测设备是否进入或离开区域范围。<br />GEO, commonly known as an electronic fence, is used to define a designated area and detect whether a device has entered or left the area.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type   | Converter | Description                                                                                                          |
| -------- | ----------- | ------ | --------- | -------------------------------------------------------------------------------------------------------------------- |
| 1        | length      | byte   |           | length                                                                                                               |
| 2        | key         | byte   |           | key                                                                                                                  |
| 3        | index       | byte   |           | index                                                                                                          |
| 4        | enable      | byte   | bool      | enable, 0- disable 1- enabled                                                                      |
| 5        | flag        | byte   | bits      | 标志<br />bit0: 0-不检测离开，1-检测离开 <br />bit1: 0- 不检测进入，1-检测进入 <br />bit7: 0- 圈形区域，1-多边形区域<br /><br />flag<br />bit0 left detectioin : 0-disable，1-enable <br />bit1  enter detection: 0- disable，1-enabled <br />bit7 area shape: 0- circle，1-polygon |
| 6        | points      | byte   |           | 位置点数 points                                                                                                            |
| 7-8      | radius      | ushort |           | 圆形区域半径，单位:米 radius, unit: meters                                                                                               |
| 9-12     | lat         | float  | loc       | 位置点 0 -latitude                                                                                                   |
| 13-16    | lng         | float  | loc       | 位置点 0 -longitude                                                                                                  |
| ...      | ...         | ...    | ...       | ...                                                                                                                  |
|          | todo        | float  | loc       | 位置点 n, n 最大为 8，即最大支持 8 个顶点绘制的多边形 <br />Position point `n`, where `n` is at most 8, meaning that polygons with up to `8` vertices can be drawn.                                                                 |
| string   | description | string |           | GEO描述字符串，utf8编码，不超过32个字节。 <br />description, utf8 encoding, max 32 bytes bytes.                                                                           |


+ 电子围栏圆形区域 数据格式 flag.bit7 = 0 <br />GEO fence circular area Data format flag.bit7 = 0 

| Byte No. | Parameter   | Type   | Converter | Description                                                                                             |
| -------- | ----------- | ------ | --------- | ------------------------------------------------------------------------------------------------------- |
| 1        | length      | byte   |           | length                                                                                                  |
| 2        | key         | byte   |           | key                                                                                                     |
| 3        | index       | byte   |           | index                                                                                                  |
| 4        | enable      | byte   | bool      | enable, 0- disable 1- enabled                                                        |
| 5        | flag        | byte   | bits      | 标志<br />bit0: 0-不检测离开，1-检测离开 <br />bit1: 0- 不检测进入，1-检测进入 <br />bit7= 0- 圈形区域<br /><br />flag<br />bit0 left detectioin : 0-disable，1-enable <br />bit1  enter detection: 0- disable，1-enabled <br />bit7 area shape = 0- circle |
| 6        | points      | byte   |           | 位置点数 points = 1                                                                                            |
| 7-8      | radius      | ushort |           | 圆形区域半径，单位:米 radius, unit: meters                                                                                     |
| 9-12     | lat         | float  | loc       | 位置点 0 -latitude                                                                                      |
| 13-16    | lng         | float  | loc       | 位置点 0 -longitude                                                                                     |
| 17-48    | description | string |           | GEO描述字符串，utf8编码，不超过32个字节。 <br />description, utf8 encoding, max 32 bytes                                                              |

+ 电子围栏多边形区域 数据格式 flag.bit7 = 1 <br />GEO fence polygon area data format flag.bit7 = 1

| Byte No. | Parameter   | Type   | Converter | Description                                                                                              |
| -------- | ----------- | ------ | --------- | -------------------------------------------------------------------------------------------------------- |
| 1        | length      | byte   |           | length                                                                                                   |
| 2        | key         | byte   |           | key                                                                                                      |
| 3        | index       | byte   |           | index                                                                                                   |
| 4        | enable      | byte   | bool      | enable, 0- disable 1- enabled                                                          |
| 5        | flag        | byte   | bits      | 标志<br />bit0: 0-不检测离开，1-检测离开 <br />bit1: 0- 不检测进入，1-检测进入 <br />bit7=1-多边形区域<br /><br />flag<br />bit0 left detectioin : 0-disable，1-enable <br />bit1  enter detection: 0- disable，1-enabled <br />bit7 area shape = 1-polygon |
| 6        | points      | byte   |           | 位置点数 points range:[3, 8]                                                                                          |
| 7-8      | radius      | ushort |           | ~~radius 圆形区域半径，单位:米 ~~任意值，不解析  meanless                                                        |
| 9-12     | lat         | float  | loc       | 位位置点 0 -latitude                                                                                     |
| 13-16    | lng         | float  | loc       | 位置点 0 -longitude                                                                                      |
| ...      | ...         | ...    | ...       | ...                                                                                                      |
|          | todo        | float  | loc       | 位置点 n, n 最大为 8，即最大支持 8 个顶点绘制的多边形<br />Position point `n`, where `n` is at most 8, meaning that polygons with up to `8` vertices can be drawn.                                                     |
|          | description | string |           | GEO描述字符串，utf8编码，不超过32个字节。  <br />description, utf8 encoding, max 32 bytes                                                               |

Note:

1. `radius` 半径只有在设置围栏形为圆形时有效<br />The `radius` is only effective when the fence shape is set to circular.
2. `points` 在设置围栏区域为圆形时强制为 1; 在围栏区域为多边形时最小为 3，最大为 8<br />`points` is forced to 1 when setting the fence area as a circle; when the fence area is a polygon, the minimum is 3 and the maximum is 8.
3. 位置点是用于提供在地图上绘制形状所需要的经纬度位置<br />A location point is used to provide the latitude and longitude coordinates required to draw shapes on a map.
4. 位置点 0 在选择围栏形为圆形时，表示圆形区域的中心点位置<br />Position point 0 When selecting a circular fence shape, this represents the center point of the circular area.
5. 当设置 `GEO`同时检测进入与离开时，那么在检测到进入/离开时，输出对应事件。<br />When `GEO` is set to detect both entry and exit, the corresponding event is output when entry/exit is detected.

---

### (0x51)跌倒检测配置 falldown detection config :id=0x51

**功能描述 Function description**

跌倒检测 用于设置跌倒算法的一些工作参数，以更贴合实际使用场景。<br />Fall detection: Used to set some working parameters for the fall algorithm to better suit actual usage scenarios.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type | Converter | Description                                              |
| -------- | ----------- | ---- | --------- | -------------------------------------------------------- |
| 1        | length      | byte |           | length                                                   |
| 2        | key         | byte |           | key                                                      |
| 3        | enable      | byte | bool      | 使能<br />0- 关闭跌倒算法检测<br />1- 启用跌倒算法检测 <br /><br />enable<br />0- disable fall detection<br />1- enable fall detection  |
| 4        | sensitivity | byte |           | 灵敏度, 范围:[1-10] <br /><br />sensitivity, range:[1-10]                                     |

Note:

1. 灵敏度数值越大，对于模型匹配要求更加模糊，越容易触发跌倒检测，但同时也比较容易误报。<br />The higher the sensitivity value, the more vague the model matching requirements are, making it easier to trigger fall detection, but also increasing the likelihood of false positives.

?> todo: 1.增加延时计算窗口，用于跌倒检测匹配到尖峰波形后，继续延时匹配，以优化提高跌倒识别率。移到别的参数设置中。<br />Add a delay calculation window to continue delay matching after fall detection matches a spike waveform, in order to optimize and improve fall recognition accuracy. Move to other parameter settings.

---

### (0x52)活动检测配置 motion detection config :id=0x52

**功能描述 Function description**

活动检测一般用于检测设备是否在设置时间段时满足活动阈值。<br />Activity detection is generally used to detect whether a device meets the activity threshold during a set time period.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------- |
| 1        | length    | byte |           | length                                                     |
| 2        | key       | byte |           | key                                                        |
| 3        | enable    | byte | bool      | 使能<br />0- 不启用活动检测功能<br />1- 启用活动检测功能 <br /><br />enable<br />0- disable activity detection <br />1- enable activity detection  |
| 4        | interval  | byte |           | interval, unit: seconds 活动检测间隔                       |
| 5        | threshold | byte |           | threshold, unit: seconds 活动持续时长                      |

Note:

1. 检测逻辑为：设备处于非静止状态时间，满足持续时长，则认为设备一直在活动中。<br />The detection logic is: when the device is not in a static state for a certain duration, it is considered that the device is active.


---

### (0x53)静止检测配置 static detection config :id=0x53

**功能描述 Function description**

静止检测指设备持续一段时间都没有进行移动，认为设备处于静止状态。<br />Stationary detection refers to a situation where the device has not moved for a certain period of time and is considered to be in a stationary state.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                        |
| -------- | --------- | ---- | --------- | -------------------------------------------------- |
| 1        | length    | byte |           | length                                             |
| 2        | key       | byte |           | key                                                |
| 3        | enable    | byte | bool      | 使能<br />0- 不启用静止检测<br />1- 启用静止检测 <br /><br />enable<br />0- disable static detection <br />1- enable static detection |
| 4        | threshold | byte |           | threshold, unit: second 静止时间阈值               |

Note:

1. 充电过程中，设备应当停止静止检测，或者对检测到静止事件不处理。<br />During charging, the device should stop detecting stationary events or ignore detected stationary events.

---

### (0x54)倾斜检测配置 tilt detection config :id=0x54

**功能描述 Function description**

倾斜检测，指设备位置角度超过设定角度持续超过了指定时长，认为设备处于倾斜状态。<br />Tilt detection refers to a situation where the angle of the device exceeds the set angle for a specified period of time, indicating that the device is in a tilted state.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter      | Type   | Converter | Description                                        |
| -------- | -------------- | ------ | --------- | -------------------------------------------------- |
| 1        | length         | byte   |           | length                                             |
| 2        | key            | byte   |           | key                                                |
| 3        | enable         | byte   | bool      | 使能<br />0- 不启用倾斜检测<br />1- 启用倾斜检测 <br /><br />enable<br />0- disable tilt detection <br />1- enable tilt detection |
| 4        | angleThreshold | byte   |           | angle threshold 角度阈值                           |
| 5-6      | timeThreshold  | ushort |           | time threshold 持续时间阈值                        |

---

### (0x55)超速检测配置 overspeed detection config :id=0x55

**功能描述 Function description**

超速检测，指通过 GPS 检测当前设备的速度变化，当超过设置的速度且速度保持了指定时长时，认为设备处于超速状态。<br />Overspeed detection refers to detecting changes in the current device's speed via GPS. When the speed exceeds the set speed and remains above the specified speed for a certain period of time, the device is considered to be speeding.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter      | Type | Converter | Description                                      |
| -------- | -------------- | ---- | --------- | ------------------------------------------------ |
| 1        | length         | byte |           | length                                           |
| 2        | key            | byte |           | key                                              |
| 3        | enable         | byte | bool      | 使能<br />0- 关闭超速检测<br />1- 启用超速检测 <br /><br />enable<br />0- disable overspeed detection <br />1- enable overspeed detection |
| 4-7      | speedThreshold | uint |           | speed threshold, unit: km/h 速度阈值             |
| 8-11     | timeThreshold  | uint |           | time threshold, unit: seconds 超速持续时长       |

---

### (0x56)低电量检测配置 low power detection config :id=0x56

**功能描述 Function description**

低电量检测，指设备所使用电池电量低于设定阈值，且未在充电状态时，认为设备处于低电量状态。<br />Low battery detection refers to when the battery power of a device falls below a set threshold and the device is not charging, at which point the device is considered to be in a low battery state.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter        | Type | Converter | Description                                          |
| -------- | ---------------- | ---- | --------- | ---------------------------------------------------- |
| 1        | length           | byte |           | length                                               |
| 2        | key              | byte |           | key                                                  |
| 3        | enable           | byte | bool      | 使能<br />0- 关闭低电量检测<br />1- 开启低电量检测 <br /><br />enable<br />0- disable low power detection <br />1- enable low power detection |
| 4        | levelThreshold   | byte |           | 低电量百分比阈值 <br />low battery percentage threshold |
| 5        | triggerThreshold | byte |           | 电量降低百分比再次触发 <br />percentage of battery drop to trigger again |

---

### (0x57)离家/在家检测配置 leave/at home detection config :id=0x57

**功能描述 Function description**

离家/在家检测，通过 BLE 扫描周围设备，或通过 WiFi 扫描周围 `AP`，通过对比判断设备是否在指定区域内。<br />Away/at home detection: Scan nearby devices via BLE or nearby `APs` via WiFi, and determine whether the device is within the specified area by comparison.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter      | Type | Converter | Description                                                  |
| -------- | -------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length         | byte |           | length                                                       |
| 2        | key            | byte |           | key                                                          |
| 3        | enable         | byte | bool      | enable<br />0- 关闭离家/在家 检测<br />1- 打开离家/在家 检测 <br /><br />enable<br />0- disable away/at home detection <br />1- enable away/at home detection |
| 4-7      | scanInterval   | uint |           | interval of scan 扫描间隔,单位:second                        |
| 8        | leaveThreshold | byte |           | 离家设备数据阈值 <br />away device data threshold |
| 9        | homeThreshold  | byte |           | 在家设备数量阈值 <br />home device number threshold |

---

### (0x58)久坐/疲劳检测配置 fatigue detection config :id=0x58

**功能描述 Function description**

疲劳提醒，指设备在设定时间内没有完成运动目标时，提醒用户活动。<br />Fatigue reminder: When the device does not complete the exercise goal within the set time, it reminds the user to exercise.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter  | Type | Converter | Description                                      |
| -------- | ---------- | ---- | --------- | ------------------------------------------------ |
| 1        | length     | byte |           | length                                           |
| 2        | key        | byte |           | key                                              |
| 3        | enable     | byte | bool      | 使能<br />0- 关闭久坐检测<br />1- 打开久坐检测 <br /><br />enable<br />0- disable sedentary detection <br />1- enable sedentary detection |
| 4-7      | period     | uint |           | 检测周期，单位：s, 范围:[30 分钟, 4 小时 ] <br />detect period, unit: s, range: [30 minutes, 4 hours] |
| 8-11     | stepTarget | uint |           | 目标步数, 范围:[30, 1000] <br />target step, range: [30, 1000] |

Note:

1. 久坐提醒，在专注模式期间不工作。<br />Reminder to take breaks from sitting for extended periods of time. Do not work during focus mode.

---

### (0x59)佩戴检测配置 wear detection config :id=0x59

**功能描述 Function description**

佩戴检测，指设备通过综合数据判断当前设备检测到的传感器数据是否满足佩戴条件。<br />Wear detection refers to the device using comprehensive data to determine whether the sensor data detected by the device currently meets the wear conditions.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter      | Type | Converter | Description                                                  |
| -------- | -------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length         | byte |           | length                                                       |
| 2        | key            | byte |           | key                                                          |
| 3        | enable         | byte | bool      | 使能<br />0- 关闭佩戴检测<br />1- 打开佩戴检测 <br /><br />enable<br />0- disable wear detection <br />1- enable wear detection |
| 4-7      | period         | uint |           | 检测周期，单位：s, 范围:[1 分钟, 4 小时 ]  <br />Testing cycle, unit: s, range: [1 minute, 4 hours]                  |
| 8        | retryCondition | byte |           | 以下条件满足时，重新检测<br />bit0: 设备由静止转为活动状态时 <br />When the following conditions are met, re-detect<br />bit0: When the device transitions from a stationary state to an active state |

Note:

1. 此处的佩戴检测开关不影响心率、血氧算法开始执行的佩戴检测逻辑判断。<br />The wear detection switch here does not affect the wear detection logic judgment that starts the heart rate and blood oxygen algorithms.
2. 此处的佩戴检测，后续根据技术能力，综合多个传感器数据进行判断。当前判断仅来源于 `PPG`的佩戴检测。<br />The wear detection here is based on technical capabilities and combines data from multiple sensors for judgment. The current judgment is based solely on wear detection from PPG.
3. 佩戴检测逻辑在设备活动时，检测到未佩戴，需要定时检测多几遍。<br />Wear detection logic: When the device is active, if it detects that it is not being worn, it needs to perform periodic checks multiple times.

---

### (0x5A)Welfare检测配置 welfare detection config :id=0x5A

**功能描述 Function description**

设置/获取welfare的功能检测参数。<br />Set/obtain welfare function detection parameters.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type | Converter | Description                                            |
| -------- | ----------- | ---- | --------- | ------------------------------------------------------ |
| 1        | length      | byte |           | length                                                 |
| 2        | key         | byte |           | key                                                    |
| 3        | enable      | byte | bool      | 使能<br />0- 关闭welfare功能<br />1- 打开welfare功能 <br /><br />enable<br />0- disable welfare function <br />1- enable welfare function |
| 4-7      | duration    | uint |           | 倒计时时长，单位：s, 范围:[600, 3600000 ] <br />countdown duration, unit: s, range: [600, 3600000] |
| 8-11     | warningTime | uint |           | 警告时长，单位:s,范围:[120， 600] <br />warning duration, unit: s, range: [120, 600]                     |



Note:

1. warefare功能启动后，倒计时时长>警告时长。<br />After the warefare function is activated, the countdown time is longer than the warning time.
2. 此功能会产生多个事件：时间准备消耗完/checkin/停止/报警。<br />This function generates multiple events: time preparation exhausted/check-in/stop/alarm.

---

### (0x60)GEO 提醒执行设置 :id=0x60

**功能描述 Function description**

电子围栏在检测到设定事件时，进入或离开围栏区域，设置：<br />When an electronic fence detects a set event, entering or leaving the fence area, settings:

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |


Note：

1. `enable` 中内置提醒流程，指标准固件版本中的提醒业务逻辑代码。当前版本中，在打开 GEO 检测时，enable 强制打开。对应 UI 界面不需要勾选两个开关。<br />The built-in reminder process in `enable` refers to the reminder business logic code in the standard firmware version. In the current version, when GEO detection is enabled, `enable` is forced to be enabled. The corresponding UI interface does not require checking two switches.
2. 允许发送短信/允许拨号的指定联系人，需要联系人设置有效号码。<br />Specified contacts who are allowed to send text messages/make calls must have valid numbers set in their contact settings.

?> todo: 1. 添加拨号前提前/提醒执行完成提示?<br />Todo: 1. Add a prompt to remind users to complete the task before dialing?

---

### (0x61)SOS 执行设置 :id=0x61

**功能描述 Function description**

SOS 在检测到设定事件时，设置：<br />SOS settings when detecting a set event:

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x62)跌倒提醒执行设置 :id=0x62

**功能描述 Function description**

在检测到跌倒事件时，设置： <br />When a fall event is detected, set:

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |


---

### (0x63)活动提醒执行设置 :id=0x63

**功能描述 Function description**

在检测到活动事件时，设置：<br />When an activity event is detected, set

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x64)静止提醒执行设置 :id=0x64

**功能描述 Function description**

在检测到静止事件时，设置：<br />When a stationary event is detected, set:

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x65)倾斜提醒执行设置 :id=0x65

**功能描述 Function description**

在检测到倾斜事件时，设置：<br />When a tilt event is detected, set:


+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x66)超速提醒执行设置 Speed alert execution settings:id=0x66

**功能描述 Function description**

在检测到超速事件时，设置：<br />When an over speed event is detected, set:


+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x67)低电量提醒执行设置 Low battery alert settings:id=0x67

**功能描述 Function description**

在检测到低电量事件时，设置：<br />When a low battery event is detected, set:

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |
---

### (0x68)开机提醒执行设置 Power-on reminder execution settings:id=0x68

**功能描述 Function description**

在检测到开机事件时，设置：<br />When a power-on event is detected, set:

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x69)关机提醒执行设置 Shutdown reminder execution settings:id=0x69

**功能描述 Function description**

在检测到关机事件时，设置：<br />When a shutdown event is detected, set:

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x6A)回家/在家提醒执行设置 Set up reminders for going home/staying at home:id=0x6A

**功能描述 Function description**

在检测到 回家/在家 事件时，设置：<br />When a “return home/at home” event is detected, set:

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x6B)久坐/疲劳提醒执行设置 Set up reminders for prolonged sitting/fatigue:id=0x6B

**功能描述 Function description**

在检测到久坐事件时，设置：<br />When detecting a sedentary/fatigue event, set:


+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send text message to contact

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x6C)查找设置提醒执行设置 FindMe Reminder Execute settings :id=0x6C

**功能描述 Function description**

在检测到查找设备需求时，设置：<br />When detecting a device search request, set:

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter     | Type   | Converter | Description                                                                                |
| -------- | ------------- | ------ | --------- | ------------------------------------------------------------------------------------------ |
| 1        | length        | byte   |           | length                                                                                     |
| 2        | key           | byte   |           | key                                                                                        |
| 3-4      | cancelSource  | byte   |           | 可取消源<br />bit0: SOS按键， bit1: 按键1， bit2: 按键2, bit3: 按键3 <br /> bit8: 触摸取消<br /><br />alert can be canceled by:<br />bit0: SOS button， bit1: button1， bit2: button2, bit3: button3 <br /> bit8: touchpanel |
| 5        | screenLedMode | byte   |           | 屏幕/LED等 发光提示方式, <br />0-不使用发光提示<br />1-使用发光提示(闪烁) <br /><br />Light indication method, <br />0-No light indication<br />1-Light indication (flashing)                           |
| 6        | vibrate       | byte   |           | 振动提示, 0-不振动， others-内置振动方式<br /><br />Vibration alert, <br />0-No vibration<br />others-Built-in vibration mode                                                   |
| 7        | voicePrompt   | byte   |           | 音频提示，0-不提示， others-指定音频序号 <br /><br />Audio prompt, <br />0-no prompt<br />others-specify audio sequence number                                                  |
| 8        | voiceVolume   | byte   |           | 语音提示音量  <br /><br />Audio prompt volume, range:[0, 100]                                                                             |
| 9-10     | duration      | ushort |           | 提示时长，单位:s  <br /><br />alert duration, unit: s                                                                         |

---

### (0x6D)Welfare提醒执行设置 :id=0x6D

**功能描述 Function description**

在检测到welfare报警事件时，设置：<br />When a welfare alarm event is detected, set:

+ 设备端声、光、电提醒<br />Sound, light, and electrical alerts on the device end
+ 向服务器上传事件<br />Upload events to the server
+ 向联系人拨打电话<br />Call emergency contacts
+ 向联系人发送短信<br />Send SMS to emergency contacts

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                  |
| -------- | --------- | ---- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                       |
| 2        | key       | byte |           | key                                                                                                                                                                          |
| 3        | enable    | byte | bool      | 使能<br />0- 不执行内置提醒流程<br />1- 执行内置提醒流程 <br /><br />enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 振动提醒<br />0- 不振动<br />1- 内置振动提醒方式 <br />vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | 音频/声音 提醒<br />0- 不提醒<br />others- 指定声音序号<br />255- 默认声音序号 <br />sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | 通知联系人或上报服务器<br />bit0- 0: 不通知服务器; 1:通知服务器<br />bit1- 0:  不发送短信; 1:发送短信给紧急联系人<br />bit2- 0: 不拨打电话; 1:拨打紧急联系人电话 <br /><br />notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | 允许发送短信到 紧急联系人<br />bit0: 0-不发送; 1-允许发送给紧急联系人1 <br />bit1: 0-不发送; 1-允许发送给紧急联系人2 <br />...<br />bit9: 0-不发送; 1-允许发送给紧急联系人10 <br /><br />smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | 允许拨打电话 的紧急联系人<br />bit0: 0-不拨号; 1-允许拨打紧急联系人1 <br />bit1: 0-不拨号; 1-允许拨打给紧急联系人2 <br />...<br />bit9: 0-不拨号; 1-允许拨打给紧急联系人10 <br /><br />callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

?> 1. 由于welfare存在多个事件，针对其他提示事件可能需要增加额外配置。<br />1. Since welfare involves multiple stage events, additional configuration may be required for other prompt events.

---

### (0x90)紧急联系人列表 emergency contact list :id=0x90

**功能描述 Function description**

紧急联系人列表，仅用于紧急事件时设备主动拨打或发送短信的联系人号码。在带屏幕显示的设备端中，不允许直接选择此列表中号码进行拨号。<br />Emergency contact list, which is only used for emergency situations when the device actively dials or sends text messages to the contact numbers in the list. On devices with a display screen, it is not permitted to directly select numbers from this list for dialing.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter     | Type   | Converter | Description                                   |
| -------- | ------------- | ------ | --------- | --------------------------------------------- |
| 1        | length        | byte   |           | length                                        |
| 2        | key           | byte   |           | key                                           |
| 3        | index         | byte   |           | no 序号                                       |
| 4-7      | talkTimeLimit | byte   |           | 通话时长限制, 单位:s, 0表示不启用通话时长限制 <br /><br />Call duration limit, unit: s, 0 indicates that call duration is unlimit. |
| 8        | micGain       | byte   |           | MIC 增益, 范围：[0, 100]   <br /><br />MIC gain, range : [0, 100]         |
| 9        | speakerVolume | byte   |           | Speaker 音量, 范围:[0, 100]   <br /><br />Speaker volume, range : [0, 100]     |
| #string1 | name          | String | utf8      | Name string length:[0, 32]                    |
| #string2 | number        | String | ascii     | number string, Encode: ASCII, length: [0, 32] |
| #string3 | imageUrl      | String | utf8      | image url/fullname, [0, 64]                   |

---

### (0x91)普通联系人列表 contact list :id=0x91

**功能描述 Function description**

普通联系人列表，在带显示设备中允许用户选择拨号的联系人。<br />A list of regular contacts that allows users to select contacts for dialing on devices with displays.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter     | Type   | Converter | Description                                   |
| -------- | ------------- | ------ | --------- | --------------------------------------------- |
| 1        | length        | byte   |           | length                                        |
| 2        | key           | byte   |           | key                                           |
| 3        | index         | byte   |           | no 序号                                       |
| 4-7      | talkTimeLimit | byte   |           | 通话时长限制, 单位:s, 0表示不启用通话时长限制 <br /><br />Call duration limit, unit: s, 0 indicates that call duration is unlimit. |
| 8        | micGain       | byte   |           | MIC 增益, 范围：[0, 100]   <br /><br />MIC gain, range : [0, 100]         |
| 9        | speakerVolume | byte   |           | Speaker 音量, 范围:[0, 100]   <br /><br />Speaker volume, range : [0, 100]     |
| #string1 | name          | String | utf8      | Name string length:[0, 32]                    |
| #string2 | number        | String | ascii     | number string, Encode: ASCII, length: [0, 32] |
| #string3 | imageUrl      | String | utf8      | image url/fullname, [0, 64]                   |

---

### (0x92)来电设置 incoming call setting :id=0x92

**功能描述 Function description**

设备端在来电时，可以设置：<br />When a call comes in, the device can be set with:

+ 自动接听 <br />Answer automatically
+ 接听方式 <br />Answer mode
+ 挂断方式 <br />Hang up mode
+ 启用白名单 <br />Enable whitelist
+ 提示方式<br />Alert mode

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter        | Type | Converter        | Description                                                                                                                                               |
| -------- | ---------------- | ---- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length           | byte |                  | length                                                                                                                                                    |
| 2        | key              | byte |                  | key                                                                                                                                                       |
| 3        | autoAnswer       | byte |                  | enable auto answer<br />0- 关闭自动接听<br />1- 开启自动接听                                                                                              |
| 4        | autoAnswerDelay  | byte |                  | auto answer after rings 响铃后多久自动接听, 单位:s                                                                                                        |
| 5        | answerMethod     | byte |                  | 接听方式<br />bit0: 按键1接听<br />bit1: 按键2接听<br />bit2:按键2接听<br />bit3:按键3接听 <br />bit4:触摸接听<br /><br />answer method:<br />bit0: button1<br />bit1: button12<br />bit2:button13<br />bit3:button3 <br />bit4:touchpanel |
| 6        | hangupMethod     | byte |                  | 挂断方式<br />bit0: 按键1接听<br />bit1: 触摸挂断<br />bit2:触摸挂断<br />bit3:触摸挂断 <br />bit4:触摸挂<br /><br />hangup/reject method<br />bit0: button1<br />bit1: button2<br />bit2:button3<br />bit3:button4 <br />bit4:touchpanel                                               |
| 7        | whitelistMode    | byte |                  | 是否启用来电白名单<br />0- 不启动，所有来电均提醒<br />1- 仅允许紧急联系人列表中号码 <br />2- 仅允许联系人列表中号码<br />3- 仅允许紧急联系人&联系人 列表<br /><br />Enable call whitelist<br />0- all incoming calls will be notified<br />1- Only numbers in the emergency contact list are allowed. <br />2- Only numbers in the contact list are allowed.<br />3- Only emergency contacts & contact list allowed |
| 8        | vibration        | byte |                  | 来电振动提示<br />0:不振动提示 <br />others: 选择振动提示方式 <br /><br />vibration:<br />0: no vibration<br />others: select vibration mode |
| 9        | ringtone         | byte |                  | 来电响铃提示<br />0:使用默认铃声 <br />others: 选择内置可选择铃声文件 <br /><br />ringtone:<br />0: default ringtone<br />others: select ringtone |
| 10-13    | emegencyContacts | uint |                  | 允许呼入提醒的紧急联系人列表 <br />List of emergency contacts allowed to receive incoming call alerts<br />bit0: emergency contact 1 <br />bit1: emergency contact 2 ... <br />bit9: emergency contact 10 |
| 14-17    | normalContacts   | uint |                   |允许呼入提醒的联系人列表 <br />List of contacts allowed to receive incoming call alerts<br />bit0: contact 1 <br />bit1: contact 2 ... <br />bit9: contact 10|

                                                                                                                                                     |

Note


1. 接听方式或挂断方式，根据实际情况代码预留 单击/短按/滑动接听 方式判断。<br />Answering or hanging up method: Reserve code based on actual circumstances. Determine method based on click/short press/slide to answer.
2. 未接听前挂断，预留首次静音，二次挂断业务逻辑。<br />Hang up before answering: Reserve first mute, second hang up function logic.

---

### (0x95)DTMF 开关 DTMF enable :id=0x95

**功能描述 Function description**

在通话期间，`DTMF` 功能允许远端可以通过虚拟拨号按键向设备发送按键指令。<br />During a call, the remote end can send key commands to the device via virtual dial keys using the DTMF function.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter            | Type | Converter | Description                                                                                                                                                     |
| -------- | -------------------- | ---- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length               | byte |           | length                                                                                                                                                          |
| 2        | key                  | byte |           | key                                                                                                                                                             |
| 3        | enable               | byte |           | 使能<br />0:关闭<br />1:仅紧急联系人可用<br />2:仅普通联系人使用<br />3:仅联系人可用<br />255:所有通话均打开 <br /><br />enable:<br />0: close<br />1: only emergency contacts are allowed<br />2: only normal contacts are allowed<br />3: only contacts are allowed<br />255: all calls are allowed |
| 4-7      | emergencyContactMask | uint |           | 指定可以使用的紧急联系人<br />bit0: 允许发送给紧急联系人1使用dtmf <br />bit1: 允许发送给紧急联系人2使用dtmf <br />...<br />bit9: 允许发送给紧急联系人10使用dtmf <br /><br />Emergency contacts who can use this feature<br />bit0: emergency contact 1<br />bit1: emergency contact 2 <br />... <br />bit9: emergency contact 10 |
| 8-11     | contactMask          | uint |           | 指定可以使用的普通联系人<br />bit0: 允许发送给普通联系人1使用dtmf <br />bit1: 允许发送给普通联系人2使用dtmf <br />...<br />bit9: 允许发送给普通联系人10使用dtmf<br /><br />Normal contacts who can use this feature<br />bit0: contact 1<br />bit1: contact 2 <br />... <br />bit9: contact 10 |


---

### (0x96)DTMF 按键功能 DTMF key's function :id=0x96

**功能描述 Function description**

允许`DTMF`虚拟键盘，自定义按键的功能。<br />Customize the function of the `DTMF` virtual keyboard.


**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type  | Converter | Description                                                                                                                                                                                              |
| -------- | ----------- | ----- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length      | byte  |           | length                                                                                                                                                                                                   |
| 2        | key         | byte  |           | key                                                                                                                                                                                                      |
| 3        | index       | byte  |           | index, range:[0,9]                                                                                                                                                                                        |
| 4        | action      | byte  |           | 任务/动作<br />0:无动作<br />1:音量增加<br />2：音量减少<br />3：打开静音 <br />4:关闭静音<br />5:mic增益递加<br />6:mic增益减少<br />7: 挂断电话<br />8: 取消SOS报警 <br />9: 取消SOS报警并挂断当前电话 <br /><br />task/action<br />0:no action<br />1.speaker's volume up<br />2:speaker's volume down<br />3:mute on<br />4: mute off<br />5:mic volume up<br />6:mic volume down<br />7:end call<br />8:cancel SOS alert |
| 5-n      | keySequence | array |           | key/combine, 0/1/2/3/4/5/6/7/8/9/*/#                                                                                                                                                                    |

---

### (0x97)短信命令权限设置 Permission of SMS command :id=0x97

**功能描述 Function description**

短信命令，允许通过发送短信到设备，设置或查询设备的配置。<br />SMS commands allow you to set or query device configurations by sending text messages to the device.

当前命令用于设置短信命令权限，过滤非信任号码发来的短信命令。<br />The current command is used to set SMS command permissions and filter SMS commands sent from untrusted numbers.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter            | Type | Converter | Description                                                                                                                                                          |
| -------- | -------------------- | ---- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length               | byte |           | length                                                                                                                                                               |
| 2        | key                  | byte |           | key                                                                                                                                                                  |
| 3        | permissionLevel      | byte |           | 权限<br />0:关闭短信命令权限，所有号码短信均处理<br />1: 仅处理紧急联系人发来的短信 <br />2: 仅处理普通联系发来的短信 <br />3: 仅处理紧急联系人&普通联系人发来的短信<br /><br />permission level:<br />0: close<br />1: only emergency contacts are allowed<br />2: only normal contacts are allowed<br />3: only emergency contacts and normal contacts are allowed |
| 4-7      | emergencyContactMask | int  |           | 具体指定紧急联系人<br />bit0: 是否处理紧急联系人1发来的短信 <br />bit1: 是否处理紧急联系人2发来的短信 <br />...<br />bit9: 是否处理紧急联系人10发来的短信<br /><br />specific emergency contacts:<br />bit0: emergency contact 1<br />bit1: emergency contact 2 <br />... <br />bit9: emergency contact 10 |
| 8-11     | contactMask          | int  |           | 具体指定普通联系人<br />bit0: 是否处理普通联系人1发来的短信 <br />bit1: 是否处理普通联系人2发来的短信 <br />...<br />bit9: 是否处理普通联系人10发来的短信<br /><br />specific normal contacts:<br />bit0: contact 1<br />bit1: contact 2 <br />... <br />bit9: contact 10 |


---

### (0x98)短信命令密钥设置 Password for SMS command :id=0x98

**功能描述 Function description**

为防止他人使用随意使用允许的联系人号码发送短信命令，添加短信命令。<br />To prevent others from sending text message commands using permitted contact numbers, add text message commands.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description       |
| -------- | --------- | ----- | --------- | ----------------- |
| 1        | length    | byte  |           | length            |
| 2        | key       | byte  |           | key               |
| 3        | enable    | byte  | bool      | enable            |
| 4-n      | password  | array |           | 数字密码/字符密码<br />numeric password / text password |


+ 启用数字密码 numeric password

| Byte No. | Parameter       | Type | Converter | Description |
| -------- | --------------- | ---- | --------- | ----------- |
| 1        | length          | byte |           | length      |
| 2        | key             | byte |           | key         |
| 3        | enable          | byte | bool      | enable = 1  |
| 4-7      | numericPassword | int  |           | 数字密码 <br />numeric password |


+ 启用字符密码 text password

| Byte No. | Parameter    | Type   | Converter | Description |
| -------- | ------------ | ------ | --------- | ----------- |
| 1        | length       | byte   |           | length      |
| 2        | key          | byte   |           | key         |
| 3        | enable       | byte   | bool      | enable = 2  |
| 4-9      | textPassword | String | utf8      | 密码字符串 <br />text password |


---

### (0x9B)GPS 定位链接格式化字符串 GPS location URL for format :id=0x9B

**功能描述 Function description**

GPS 定位链接，提供用户点击跳转到地图查看位置。这个字符串用于提供链接格式化生成方式。<br />GPS location link, allowing users to click and jump to the map to view the location. This string is used to provide the link formatting generation method.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| #string1 | urlFormat | String | utf8      | URL format  |

---

### (0x9C)WiFi&LBS 定位链接格式化字符串 WiFi&LBS location URL for format :id=0x9C

**功能描述 Function description **

提供设备生成 WiFi/LBS 的位置访问链接。<br />Provide equipment to generate WiFi/LBS location access links.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description              |
| -------- | --------- | ------ | --------- | ------------------------ |
| 1        | length    | byte   |           | length                   |
| 2        | key       | byte   |           | key                      |
| #string  | urlFormat | String | utf8      | URL format, Type: String |

---

### (0xB0)AGPS 的使用 AGPS's usage :id=0xB0

**功能描述 Function description**

GPS 模组在使用过程中，通过更新定位辅助信息，可以让GPS加快定位时间和提升定位精度。<br />During use, GPS modules can speed up positioning time and improve positioning accuracy by updating positioning assistance information.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                                                                                            |
| -------- | --------- | ----- | --------- | ------------------------------------------------------------------------------------------------------ |
| 1        | length    | byte  |           | length                                                                                                 |
| 2        | key       | byte  |           | key                                                                                                    |
| 3        | enable    | byte  | bool      | 使能<br />0- 不使用 GPS 定位辅助数据<br />1- 使用 GPS 定位辅助数据<br />2- GPS位置更新时更新参考位置<br /><br />enable:<br />0: close<br />1: open<br />2: update reference position when GPS position is updated |
| 4-7     | lat       | float | loc       | 参考纬度  <br /><br />Reference latitude                                                                                             |
| 8-11      | lng       | float | loc       | 参考经度   <br /><br />Reference longitude                                                                                        |



Note：

1. 参考经纬度可以在向服务器申请辅助数据时，更快速提供更准确的辅助数据。<br />Reference latitude and longitude can provide more accurate auxiliary data more quickly when requesting auxiliary data from the server.

---

### (0xB1)持续定位配置 locating schedule :id=0xB1

**功能描述 Function description**

持续定位，指设备在连续时间段时执行定位功能，直到满足设定的工作时长。<br />Continuous positioning refers to a device performing positioning functions continuously for a set period of time until the specified working time is reached.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                                                     |
| 2        | key       | byte |           | key                                                                                        |
| 3-6      | duration  | int  |           | duration, unit: second 工作时长, 单位：秒                                                                |
| 7-10     | interval  | int  |           | interval, unit: second 工作间隔, 单位: 秒                                                                |
| 11-13    | timeout   | int  |           | timeout,unit: second <br />Location timeout refers to a situation where location information has not been updated beyond a specified time limit, which is considered a location timeout, resulting in the premature termination of location tracking.<br /><br />定位超时，指超过指定时长定位信息没有更新，视为定位超时，提前结束定位。 |


---

### (0xB3)BLE 定时扫描参数 BLE Scan schedule :id=0xB3

**功能描述 Function description**

BLE 定时扫描，常用于检测离家/在家。可以设定 BLE 扫描的计划。<br />BLE periodic scanning, commonly used to detect when someone is away from home or at home. You can set a schedule for BLE scanning.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                   |
| -------- | --------- | ---- | --------- | ----------------------------- |
| 1        | length    | byte |           | length                        |
| 2        | key       | byte |           | key                           |
| 3-6      | scanTime  | int  |           | scan time, unit: second<br /><br />扫描时长，单位：秒  |
| 7-10     | sleepTime | int  |           | sleep time, unit: second <br /><br />休眠时长，单位：秒 |

---

### (0xB8)BLE 带定位信息设备列表 BLE device list with location :id=0xB8

**功能描述 Function description**

带定位信息的 BLE 设备列表，通过 BLE 扫描到对应设备数量及对比其他位置信息，可大概确定设备所在位置。<br />A list of BLE devices with location information. By scanning BLE to determine the number of corresponding devices and comparing other location information, the approximate location of the device can be determined.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type   | Converter | Description               |
| -------- | ----------- | ------ | --------- | ------------------------- |
| 1        | length      | byte   |           | length                    |
| 2        | key         | byte   |           | key                       |
| 3        | index       | byte   |           | index                |
| 4-9      | mac         | MAC    | leMac     | MAC                       |
| 11-13    | lat         | float  |           | latitude, Type:float      |
| 14-17    | lng         | float  |           | longitude, Type:float     |
| #string1 | description | String | utf8      | description, Type: String |

---

### (0xB9)WiFi 带定位信息 AP 列表 WiFi AP list with location :id=0xB9

**功能描述 Function description**

带定位信息的 WiFi AP列表，通过 WiFi 模块扫描到对应设备数量及对比其他位置信息，可大概确定设备所在位置。<br />A list of WiFi access points with location information, scanned by the WiFi module, along with the corresponding number of devices and comparison with other location information, can be used to roughly determine the location of the device.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type   | Converter | Description               |
| -------- | ----------- | ------ | --------- | ------------------------- |
| 1        | length      | byte   |           | length                    |
| 2        | key         | byte   |           | key                       |
| 3        | index       | byte   |           | index                 |
| 4-9      | mac         | MAC    | leMac     | MAC                       |
| 11-13    | lat         | float  |           | latitude, Type:float      |
| 14-17    | lng         | float  |           | longitude, Type:float     |
| #string1 | description | String | utf8      | description, Type: String |

---

### (0xD0)表盘 Watchface :id=0xD0

**功能描述 Function description**

指定当前设备所使用的表盘。<br />Specify the watchface used by the current device.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter     | Type   | Converter | Description                 |
| -------- | ------------- | ------ | --------- | --------------------------- |
| 1        | length        | byte   |           | length                      |
| 2        | key           | byte   |           | key                         |
| #string1 | watchFaceName | String | utf8      | name, length: [1, 32] |

?> todo: 表盘名称是否可以使用字符串表示id, 从服务器下载表盘时，通过 `id`来获取表盘信息，决定是否下载 `表盘`内容。<br />Can the Watchface name be represented by a string ID? When downloading the Watchface from the server, use the `id` to retrieve the Watchface information and determine whether to download the `Watchface` content.

---

### (0xD3)屏幕设置 Screen setting :id=0xD3

**功能描述 Function description**

设置屏幕的工作参数。<br />Set the working parameters on the screen.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter      | Type | Converter | Description                                                                                         |
| -------- | -------------- | ---- | --------- | --------------------------------------------------------------------------------------------------- |
| 1        | length         | byte |           | length                                                                                              |
| 2        | key            | byte |           | key                                                                                                 |
| 3        | autoOffTimeout | byte |           | 自动息屏时长, 单位：秒  <br /><br />Auto-screen-off duration, unit: seconds, range:[3, 240]                                                                            |
| 4        | brightness     | byte |           | 屏幕亮度, 范围:[0, 100] <br /><br />Screen brightness, range: [0, 100]                                                                              |
| 5        | touchWakeUp    | byte |           | 触摸亮屏,<br />0:关闭 <br />1：触摸亮屏 <br />2:长按亮屏(1s) <br /><br />Touch to wake up the screen<br />0:disable <br />1：touch <br />2:long press(1s)                                       |
| 6        | buttonWakeUp   | byte |           | 按键亮屏,<br />0:关闭 <br />1: 短按亮屏 <br />2:长按亮屏(1s) <br /><br />Press button to turn on screen<br />0:disable <b r/>1:click <br />2:long press(1s)                                        |
| 7        | wristWakeUp    | byte |           | 翻腕亮屏,<br />0:关闭翻腕亮屏功能<br />1:打开翻腕亮屏功能  <br /><br />Flip wrist to wake screen<br />0:disable<br />1:enable |

---

### (0xD4)一级界面设置 Navigation Page Settings:id=0xD4

**功能描述 Function description**

带显示屏幕的设备，可以设置一级界面(导航页面)的可见性。<br />Devices with display screens can set the visibility of the primary interface(navigation page).


**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                                                                                                                                                                   |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length    | byte |           | length                                                                                                                                                                                                        |
| 2        | key       | byte |           | key                                                                                                                                                                                                           |
| 3-6      | hideMask  | int  |           | 一级界面的隐藏 bit=0表示显示，bit=1表示隐藏<br />bit0：计步; <br />bit1:心率; <br />bit2: 血氧; <br />bit3:紧急联系人; <br />bit4: 联系人; <br />bit5: 天气; <br />bit6: 闹钟; <br />bit7: 提醒; <br />bit8: 设置<br /><br />bit=0 is visible ，bit=1 is hide<br />bit0：step; <br />bit1:heartrate; <br />bit2: bloodoxygen; <br />bit3:emergency contacts; <br />bit4: contacts; <br />bit5: weather; <br />bit6: alarmclock; <br />bit7: reminder; <br />bit8: settings |

---

### (0xD5)Home 界面设置 Home page settings :id=0xD5

**功能描述 Function description**

表盘界面又称为 `Home Page`，设置屏幕非活动时长超过设定时间，再次亮屏自动返回 `Home Page`界面功能。<br />The watch face interface is also known as the “Home Page.” When the screen is inactive for longer than the set time, it will automatically return to the “Home Page” interface when the screen is turned on again.

非活动指屏幕息屏后的时长。<br />Inactivity refers to the length of time after the screen has been turned off.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter       | Type | Converter | Description                    |
| -------- | --------------- | ---- | --------- | ------------------------------ |
| 1        | length          | byte |           | length                         |
| 2        | key             | byte |           | key                            |
| 3        | enableHomePage  | byte |           | enable                         |
| 4-7      | homePageTimeout | int  |           | inactive time screen , unit: s, range: [5, 240] |


---

### (0xD6)表盘元素设置 Watchface Element Settings:id=0xD6

**功能描述 Function description**

一般的表盘上有其多种数据元素显示，可以设定选择表盘界面允许显示的数据类型。<br />The standard Watchface displays a variety of data elements, and you can set the Watchface to display the data types you want.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type | Converter | Description                                                                                 |
| -------- | ----------- | ---- | --------- | ------------------------------------------------------------------------------------------- |
| 1        | length      | byte |           | length                                                                                      |
| 2        | key         | byte |           | key                                                                                         |
| 3-6      | elementMask | int  |           | 数据类型，0-关闭 1-显示<br />bit0: 计步;<br />bit1: 心率;<br />bit2: 血氧;<br />bit3: 天气; <br /><br />data types, bit=0 is hide, bit=1 is show <br />bit0: step;<br />bit1: heartrate;<br />bit2: bloodoxygen;<br />bit3: weather;|


Note:

1. 表盘元素仅在系统表盘中有效，自定义表盘元素中此配置无效更改。<br />The dial elements are only valid in the system dial; this configuration is not valid in custom dial elements.

---

### (0xE0)闹钟列表 Alarm clock list :id=0xE0

**功能描述 Function description**

设定指定时间，提醒用户。<br />Set a specific time to remind users.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type   | Converter | Description                                                                                                                                               |
| -------- | ----------- | ------ | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length      | byte   |           | length                                                                                                                                                    |
| 2        | key         | byte   |           | key                                                                                                                                                       |
| 3        | alarmIndex  | byte   |           | index                                                                                                                                               |
| 4        | enable      | byte   | bool      | 使能<br />0- 关闭闹钟功能 <br />1- 开启闹钟功能 <br /><br />enable<br />0- disable <br />1- enable                                                                                                        |
| 5        | alarmType   | byte   |           | 闹钟类型<br />0:闹钟<br />1:吃药提醒<br />2:运动提醒 <br /><br />type<br />0- alarmclock<br />1- medicine<br />2- exercise                                                                                                 |
| 6        | hour        | byte   |           | 设定时间小时 <br /><br />hour  range:[0, 23]                                                                                                                          |
| 7        | minute      | byte   |           | 设置时间分钟<br /><br />minute ， range:[0, 59]                                                                                                                       |
| 8        | repeatMask  | byte   | bits      | 重复星期，0-关闭，1-开启<br />bit0:  星期一<br />bit1: 星期二 <br />bit2: 星期三<br />bit3: 星期四 <br />bit4: 星期五<br />bit5: 星期六<br />bit6: 星期天 <br /><br />repeat week, bit=0 is disable, bit=1 is enable <br />bit0: monday; <br />bit1: tuesday; <br />bit2: wednesday; <br />bit3: thursday; <br />bit4: friday; <br />bit5: saturday; <br />bit6: sunday |
| 9        | vibrateMode | byte   |           |  振动提醒<br />0- 关闭振动提醒<br />others- 选择振动提醒方式 <br /><br />vibrate<br />0- disable <br />others- enable                                                                      |
| 10       | soundMode   | byte   |           | 音频提醒<br />0- 关闭声音提醒 <br />others: 其他他音频文件序号 <br /><br />audio prompt<br />0- disable <br />others: select audio file                                                                                      |
| 11       | duration    | byte   |           | 提醒时长, 单位: s, 范围: [5, 120]  <br /><br />duration, unit: seconds, range:[5, 120]                                                                                                                       |
| 12-n     | description | String | utf8      | 描述字符串 <br /><br />description: length:[0, 32]                                                                                                                                |

Note

1. 单次闹钟的在执行后，要关闭使能位，保存参数，相关工作参数也要同步更新（比如记录的单次闹钟时间戳）。<br />After executing a One-time alarmclock, disable the enable bit, save the parameters, and synchronously update the relevant working parameters (such as the recorded One-time alarmclock timestamp).
2. 提醒时长在执行过程中不需要严格卡时间，可以在一个提醒动作周期后判断是否满足提醒时长。即 `实际执行提醒时长`>`设置提醒时长`, 避免比如播放音频过程中断影响体验。<br />The reminder duration does not need to be strictly enforced during execution. Instead, it can be determined whether the reminder duration is met after one reminder action cycle. That is, `actual reminder duration` > `set reminder duration`, to avoid interruptions such as audio playback interruptions that may affect the user's experience.


---

### (0xE1)心率定时测量 Periodic heart rate measurement :id=0xE1

**功能描述 Function description**

心率的定时测量，同时使设备定时记录使用过程的心率测量结果。<br />Regular heart rate measurements, while allowing the device to record heart rate measurements during use at regular intervals.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                        |
| -------- | --------- | ---- | --------- | -------------------------------------------------- |
| 1        | length    | byte |           | length                                             |
| 2        | key       | byte |           | key                                                |
| 3        | enable    | byte | bool      | enable periodic measurement <br />0- disable<br />1- enabled   |
| 4-7      | interval  | int  |           | interval, unit: second, range:[180=3mins, 60x60x8=8 hours] <br /><br />定时测量间隔，单位: s, 允许范围:[ 3 分钟, 8 小时 ] |


Note

1. 定时测量，由于不同时间段内，测量环境存在差异，其结果时间不能保证在相同分钟内得到结果。<br />Timed measurements: Due to differences in the measurement environment at different times, the results cannot be guaranteed to be obtained within the same minute.
2. 充电状态下，不执行定时测量。<br />No periodic measurements are performed while charging.
3. 未佩戴状态下，定时测量时间到，不执行测量。<br />Not wearing status: when the timed measurement time expires, the measurement is not executed.

---

### (0xE2)血氧定时测量 Periodic blood oxygen measurement :id=0xE2

**功能描述 Function description**

定时测量血氧，同时记录血氧测量结果。<br />Periodic blood oxygen measurement, while allowing the device to record blood oxygen measurements during use at regular intervals.


**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                        |
| -------- | --------- | ---- | --------- | -------------------------------------------------- |
| 1        | length    | byte |           | length                                             |
| 2        | key       | byte |           | key                                                |
| 3        | enable    | byte | bool      | enable periodic measurement <br />0- disable<br />1- enabled   |
| 4-7      | interval  | int  |           | interval, unit: second, range:[180=3mins, 60x60x8=8 hours] <br /><br />定时测量间隔，单位:秒, 允许范围:[ 3 分钟, 8 小时 ] |

Note

1. 定时测量，由于不同时间段内，测量环境存在差异，其结果时间不能保证在相同分钟内得到结果。<br />Timed measurements: Due to differences in the measurement environment at different times, the results cannot be guaranteed to be obtained within the same minute.
2. 充电状态下，不执行定时测量。<br />No periodic measurements are performed while charging.
3. 未佩戴状态下，定时测量时间到，不执行测量。<br />Not wearing status: when the timed measurement time expires, the measurement is not executed.

---

### (0xE3)温度定时测量 Periodic temperature measurement :id=0xE3

**功能描述 Function description**

定时测量温度，同时记录温度测量结果。<br />Periodic temperature measurement, while allowing the device to record temperature measurements during use at regular intervals.


**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                         |
| -------- | --------- | ---- | --------- | --------------------------------------------------- |
| 1        | length    | byte |           | length                                              |
| 2        | key       | byte |           | key                                                 |
| 3        | enable    | byte | bool      | enable periodic measurement <br />0- disable<br />1- enabled   |
| 4-7      | interval  | int  |           | interval, unit: second, range:[180=3mins, 60x60x8=8 hours] <br /><br />定时测量间隔，单位: 秒, 允许范围:[ 3 分钟, 8 小时 ] |

---

### (0xE4)天气功能设置 Weather function configuration :id=0xE4


**功能描述 Function description**

设置天气应用的工作参数。<br />Set the working parameters of the weather app.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter            | Type | Converter | Description                                    |
| -------- | -------------------- | ---- | --------- | ---------------------------------------------- |
| 1        | length               | byte |           | length                                         |
| 2        | key                  | byte |           | key                                            |
| 3        | enable               | byte | bool      | enable weather function <br />0- disable<br />1- enabled   |
| 4        | tempUnit             | byte |           | temperature unit <br />0: Celsius <br />1: Fahrenheit          |
| 5-8      | autoUpdateInterval   | int  |           | weather auto update interval, unit: s                      |
| 9-12     | manualUpdateInterval | int  |           | weather manual update interval, unit: s                       |

Note:

1. 增加获取天气的来源方式？比如访问url获取，还是从服务器获取?<br />Add the way to get weather? such as accessing url or from server?


---

### (0xE5)第三方配件接入配置 Third-party accessory access configuration :id=0xE5

**功能描述 Function description**

第三方配件接入设置，打开或关闭第三方配件的功能。<br />Third-party accessory access configuration, to enable or disable the function of third-party accessories.

第三方配件必须是当前设备已经适配的。<br />The third-party accessory must be already adapted to the current device.


**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter   | Type  | Converter | Description                                    |
| -------- | ----------- | ----- | --------- | ---------------------------------------------- |
| 1        | length      | byte  |           | length                                         |
| 2        | key         | byte  |           | key                                            |
| 3-6      | accessoryId | int   |           | accessory id, range: [0, 0xFFFF]                                 |
| 7        | enable      | byte  | bool      | enable accessory function <br />0- disable<br />1- enabled   |
| 8-n      | config      | array |           | accessory configuration                                 |

**支持的第三方配件列表 Supported third-party accessory list**

+ TBD(to be determined)


---

### (0xE6)数据定时上报 Periodic data reporting :id=0xE6

**功能描述 Function description**

设置设备定期将测量到的健康数据上传服务器保存。<br />Set the device to regularly upload the measured health data to the server for storage.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter            | Type | Converter | Description                                                                                                                       |
| -------- | -------------------- | ---- | --------- | --------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length               | byte |           | length                                                                                                                            |
| 2        | key                  | byte |           | key                                                                                                                               |
| 3        | enable               | byte |           | enable periodic data reporting <br />0- disable<br />1- enabled <br />2- fixed hour and minute reporting                                            |
| 3-6      | uploadInterval       | int  |           | upload interval, unit: s, range: [1, 3600 * 24]<br />when enable=2, the value must be: 5min/10min/20min/30min/1hour/2hour/3hour/4hour/6hour/12hour/24hour |
| 7-10     | motionUploadInterval | int  |           | upload interval when device is moving, unit: s                                                                                            |
| 11-14    | staticUploadInterval | int  |           | upload interval when device is static, unit: s                                                                                            |
| 15-18    | dataTypeMask         | int  | bits      | data type mask <br />bit0: step<br />bit1: heart rate <br />bit2: blood oxygen<br />bit3: sleep record                               |

Note

1. `motionUploadInterval`不为0时，表示启用活动数据上报功能，此时数据上报间隔覆盖 `uploadInterval`。<br />When `motionUploadInterval` is not 0, it means that the activity data reporting function is enabled, and the data reporting interval will override `uploadInterval`.
2. `staticUploadInterval`不为0时，表示启用静止数据上报功能，此时数据上报间隔覆盖 `uploadInterval`。<br />When `staticUploadInterval` is not 0, it means that the static data reporting function is enabled, and the data reporting interval will override `uploadInterval`.
3. `motionUploadInterval` & `staticUploadInterval` 都为0时，数据上报间隔使用 `uploadInterval`。<br />When both `motionUploadInterval` and `staticUploadInterval` are 0, the data reporting interval will use `uploadInterval`.
4. 当使能类型为 `2` 时，`uploadInterval` 表示固定小时分钟上报。比如设定为5minutes时， xx:00/xx:05/xx:10/xx:15/xx:20/xx:25/xx:30/xx:35/xx:40/xx:45/xx:50/xx:55 (xx=hour)时，进行数据上传。其他同理。<br />When the enable type is set to `2`, `uploadInterval` indicates fixed hourly and minute reporting. For example, when set to 5 minutes, data is uploaded at xx:00/xx:05/xx:10/xx:15/xx:20/xx:25/xx:30/xx:35/xx:40/xx:45/xx:50/xx:55 (where xx represents the hour). The same applies to other cases.

---

### (0xE8)初始化里程 Initialize mileage :id=0xE8

**功能描述 Function description**

用于记录设备在正常使用过程中，总共行进的距离。<br />Used to record the total distance traveled by the device during normal use.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description      |
| -------- | --------- | ---- | --------- | ---------------- |
| 1        | length    | byte |           | length           |
| 2        | key       | byte |           | key              |
| 3-6      | mileage   | int  |           | mileage, unit: m |

Note:

1. 这个功能从 EV07B 开始存在，应用场景在上报 GPS 数据中携带里程记录。<br />This feature has been available since EV07B and is used to carry mileage records in GPS data reports.
2. 里程数允许设置指定里程，还是只允许复位为 `0`更合适呢?<br />Is it more appropriate to allow the mileage to be set to a specified value, or to only allow it to be reset to `0`?
3. 还有哪些应用场景呢?<br />What are some other application scenarios?


---

### (0xE9)计步功能配置 Step counting function configuration:id=0xE9

**功能描述 Function description**

用于设定计步功能的相关参数。<br />Used to set parameters related to the step counting function.

**写请求/读返回数据格式 Write request/read response data format**

| Byte No. | Parameter          | Type | Converter | Description                                                            |
| -------- | ------------------ | ---- | --------- | ---------------------------------------------------------------------- |
| 1        | length             | byte |           | length                                                                 |
| 2        | key                | byte |           | key                                                                    |
| 3        | enable             | byte | bool      | 计步功能开关:<br />0:关闭 <br />1:打开 <br /><br />step function enable<br />0:disable<br />1:enabled                                |
| 4-7      | targetStepDaily    | byte |           | 每天目标步数<br />0: 不设定<br />每天步数目标:[1_000, 100_000]<br /><br />target step daily<br />0:disable<br />target steps:[1_000, 100_000]    |
| 8-11     | targetCalorieDaily | int  |           | 每天目标消耗的卡路里, 单位: kcal<br />0:不设定<br />消耗卡路里目标:[10, 999]<br /><br />target calorie burned daily<br />0:disable<br />target:[10, 999] |

Note

1. 当计步功能关闭时，只是不上传计步，内部还是记录。同步影响到UI的计步界面计步显示为0。<br />Used to set parameters related to the step counting function.
2. 当计步功能开启时，才会上传计步数据。<br />Step count data will only be uploaded when the step count function is enabled.

---

### (0xF0)读功能readkey :id=0xF0

**功能描述 Function description**

用于配合读取其他 `key`的信息或配置参数。<br />Used to read other `key` information or configuration parameters.

**写请求数据格式 Write request data format**

| Byte No. | Parameter | Type  | Converter | Description       |
| -------- | --------- | ----- | --------- | ----------------- |
| 1        | length    | byte  |           | length            |
| 2        | readkey   | byte  |           | readkey           |
| 3-n      | keys      | array |           | 所需要读取的 keys <br />keys list to read |

Note

1. 如果 `readkey`后面不带任何参数，即表示读所有参数。但可能内存限制，只会上传前面部分数据。<br />If `readkey` is not followed by any parameters, it means to read all parameters. However, due to memory limitations, only the first part of the data will be uploaded.

### (0xF1)读指定key区间read key range :id=0xF1

**功能描述 Function description**

给定两个`key`, 读到两个`key`之间存在的`key`列表信息或配置。<br />Given two `keys`, read the list of `keys` or configuration information that exists between the two `keys`.


**写请求数据格式 Write request data format**

| Byte No. | Parameter | Type | Converter | Description |
| -------- | --------- | ---- | --------- | ----------- |
| 1        | length    | byte |           | length      |
| 2        | readkey   | byte |           | readkey     |
| 3        | keystart  | byte |           | 开始key     |
| 4        | keyend    | byte |           | 结束key     |

Note

1. 指定读取 `keystart`到 `keyend`之间的参数。<br />Specify parameters to read between `keystart` and `keyend`.
2. 程序内部调整 `keystart`和 `keyend`最近的参数。<br />Adjust the parameters of `keystart` and `keyend` within the program.
3. 如果 `keystart` > `keyend`， 程序自动交换 `keystart`和 `keyend`的值。<br />If `keystart` > `keyend`, the program automatically swaps the values of `keystart` and `keyend`.
4. 如果 `keystart`到 `keyend`之间的参数超过了 `packet`最大长度，只回复前面的参数。<br />If the parameters between `keystart` and `keyend` exceed the maximum length of `packet`, only the preceding parameters will be replied.
5. 用于分段读取设备配置信息。<br />Used to read device configuration information in segments.


