# Configuration(0x02)

---
## Command Description

Configuration commands provided: 


+ Obtain basic device information
+ Set device operating parameters
+ Set/query configuration parameters for functional applications

The following changes have been made relative to the `V0` version:

+ `Alarm Settings` has been split into two parts: Detection Settings and Execution Settings. Detection Settings are used to set thresholds for algorithms or recognition processes, etc., but when the corresponding result is recognized, an event is generated; Execution Settings execute the configured sequence of actions, etc., according to the configuration.
+ A new `String` type has been added. Please read the protocol documentation carefully.
+ Command behaviors have been redefined: `write`/`read`/`notify`. The data format used for different operations with the same `key` may vary.
+ Some functional parameters have been moved to the `Advanced Configuration Commands` section.

---

## key list

| Value(HEX)  | Parameter                    | Description                                                       | Writable | Readable | Notifiable |
| ----------- | ---------------------------- | -------------------------------------------------------------- | ---- | ---- | ---- |
|             |                              | Device basic information                          |      |      |      |
| [0x01](#_0x01) | DeviceModel                  | Model                                               | ❌   | ✅   | ❌   |
| [0x02](#_0x02) | FirmwareVersion              | Firmware version                       | ❌   | ✅   | ❌   |
| [0x03](#_0x03) | IMEI                         | Cellular Module IMEI                                   | ❌   | ✅   | ❌   |
| [0x04](#_0x04) | ICCID                        | SIM Card ICCID                                                | ❌   | ✅   | ❌   |
| [0x05](#_0x05) | MAC                          | BLE/BT MAC                                                     | ❌   | ✅   | ❌   |
| [0x06](#_0x06) | DeviceName                   | Device name                                           | ❌   | ✅   | ❌   |
| [0x07](#_0x07) | CustomFirmwareVersion        | Custom firmware version                         | ❌   | ✅   | ❌   |
| [0x08](#_0x08) | DeviceFirmwareInfo           | Device firmware information                       | ❌   | ✅   | ❌   |
| [0x09](#_0x09) | DeviceHardwareVersion        | Device hardware version                           | ❌   | ✅   | ❌   |
| [0x0A](#_0x0A) | WiFiMac                      | WiFi MAC                                                       | ❌   | ✅   | ❌   |
| [0x0B](#_0x0B) | BatteryStatus                | Battery status                                        | ❌   | ✅   | ❌   |
| [0x0C](#_0x0C) | DeviceRunTime                | Device run time                                       | ❌   | ✅   | ❌   |
| [0x0D](#_0x0D) | ProtocolApiVersion           | Protocol API version                                | ❌   | ✅   | ❌   |
| [0x0E](#_0x0E) | SDKApiVersion                | SDK API version                             | ❌   | ✅   | ❌   |
|             |                              | extended basic information                        |      |      |      |
| [0x10](#_0x10) | GsmModuleExtraInfo           | Cellular extra information                | ❌   | ✅   | ❌   |
| [0x11](#_0x11) | SimCardExtraInfo             | SIM Card extra information                      | ❌   | ✅   | ❌   |
|             |                              | Basic settings                                      |      |      |      |
| [0x20](#_0x20) | DeviceCustomName             | Device custom name                              | ✅   | ✅   | ❌   |
| [0x21](#_0x21) | DeviceCustomId               | Device custom id                                | ✅   | ✅   | ❌   |
| [0x22](#_0x22) | NfcRecord                    | NFC record                                            | ✅   | ✅   | ❌   |
| [0x23](#_0x23) | UserInfo                     | User profile                                          | ✅   | ✅   | ❌   |
| [0x24](#_0x24) | ApplicationScene             | Application scence                                    | ✅   | ✅   | ❌   |
| [0x25](#_0x25) | DoNotDisturbMode             | Do not disturb mode                                   | ✅   | ✅   | ❌   |
| [0x26](#_0x26) | ButtonShortcut               | Shortcut of buttons                              | ✅   | ✅   | ❌   |
| [0x27](#_0x27) | SleepMode                    | Sleep mode                                            | ✅   | ✅   | ❌   |
| [0x28](#_0x28) | AutoLowPowerMode             | Auto low power mode                            | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0x30](#_0x30) | SystemSetting                | System setting                                        | ✅   | ✅   | ❌   |
| [0x31](#_0x31) | FormatPreference             | Format preference                          | ✅   | ✅   | ❌   |
| [0x32](#_0x32) | LanguagePreference           | Language                                              | ✅   | ✅   | ❌   |
| [0x33](#_0x33) | DateTime                     | Datetime                                              | ✅   | ✅   | ❌   |
| [0x34](#_0x34) | TimeZone                     | Time zone                                                 | ✅   | ✅   | ❌   |
| [0x35](#_0x35) | DateTimeExtraSetting         | Datetime extra setting                            | ✅   | ✅   | ❌   |
|             |                              | Network settings                                      |      |      |      |
| [0x40](#_0x40) | ServerConfig                 | Server config                                      | ✅   | ✅   | ❌   |
| [0x41](#_0x41) | ApnList                      | APN list                                              | ✅   | ✅   | ❌   |
| [0x42](#_0x42) | WiFiNetworkApList            | WLAN AP list                                | ✅   | ✅   | ❌   |
| [0x43](#_0x43) | WiFiAutoConnectSetting       | WLAN WiFi connection setting                | ✅   | ✅   | ❌   |
| [0x44](#_0x44) | HeartbeatToServer            | Heartbeat to server                                | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0x48](#_0x48) | RingToneVolume               | Ring tone volume                                  | ✅   | ✅   | ❌   |
| [0x49](#_0x49) | MicGain                      | Gain of mic                                           | ✅   | ✅   | ❌   |
| [0x4A](#_0x4A) | SpeakerGain                  | Gain of speaker                                   | ✅   | ✅   | ❌   |
| [0x4B](#_0x4B) | PromptVolume                 | Voice Prompt volume                                 | ✅   | ✅   | ❌   |
| [0x4C](#_0x4C) | PromptEnable                 | Voice prompt enable                                 | ✅   | ✅   | ❌   |
|             |                              |                                                                | ✅   | ✅   |      |
| [0x50](#_0x50) | GeoApplicationCfg            | GEO application config                                | ✅   | ✅   | ❌   |
| [0x51](#_0x51) | FalldownCfg                  | falldown algorithm config                         | ✅   | ✅   | ❌   |
| [0x52](#_0x52) | MotionCfg                    | motion algorithm config                       | ✅   | ✅   | ❌   |
| [0x53](#_0x53) | StaticCfg                    | static algorithm config                       | ✅   | ✅   | ❌   |
| [0x54](#_0x54) | TiltCfg                      | tilt algorithm config                         | ✅   | ✅   | ❌   |
| [0x55](#_0x55) | OverspeedCfg                 | overspeed algorithm config                    | ✅   | ✅   | ❌   |
| [0x56](#_0x56) | LowPowerCfg                  | low power algorithm config                  | ✅   | ✅   | ❌   |
| [0x57](#_0x57) | LeaveGoHomeCfg               | leave/go home algorithm config                  | ✅   | ✅   | ❌   |
| [0x58](#_0x58) | FatigueCfg                   | fatigue algorithm config                        | ✅   | ✅   | ❌   |
| [0x59](#_0x59) | WearDetectionCfg             | wear status config                                    | ✅   | ✅   | ❌   |
| [0x5A](#_0x5A) | WelfareCfg                   | welfare detected config                    | ✅   | ✅   | ❌   |
| [0x5A](#_0x5A) | WelfareCfg                   | welfare detected config                    | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0x60](#_0x60) | Geo                          | actions of GEO reminder                      | ✅   | ✅   | ❌   |
| [0x61](#_0x61) | Sos                          | actions of  SOS reminder                     | ✅   | ✅   | ❌   |
| [0x62](#_0x62) | Falldown                     | actions of  falldown reminder                | ✅   | ✅   | ❌   |
| [0x63](#_0x63) | Motion                       | actions of motion reminder                   | ✅   | ✅   | ❌   |
| [0x64](#_0x64) | Static                       | actions of static reminder                   | ✅   | ✅   | ❌   |
| [0x65](#_0x65) | Tilt                         | actions of tilt reminder                     | ✅   | ✅   | ❌   |
| [0x66](#_0x66) | Overspeed                    | actions of overspeed reminder                | ✅   | ✅   | ❌   |
| [0x67](#_0x67) | LowPower                     | actions of low power reminder              | ✅   | ✅   | ❌   |
| [0x68](#_0x68) | DevicePowerOn                | actions of device power on                   | ✅   | ✅   | ❌   |
| [0x69](#_0x69) | DevicePowerOff               | actions of device power off                 | ✅   | ✅   | ❌   |
| [0x6A](#_0x6A) | LeaveHomeGoHome              | actions of leave home/go home reminder  | ✅   | ✅   | ❌   |
| [0x6B](#_0x6B) | Fatigue                      | actions of fatigue reminder           | ✅   | ✅   | ❌   |
| [0x6C](#_0x6C) | FindDevice                   | actions of find device                       | ✅   | ✅   | ❌   |
| [0x6D](#_0x6D) | WelfareExecution             | actions of welfare reminder                | ✅   | ✅   | ❌   |
| [0x6D](#_0x6D) | WelfareExecution             | actions of welfare reminder                | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0x90](#_0x90) | EmergencyContacts            | emergency contacts                              | ✅   | ✅   | ❌   |
| [0x91](#_0x91) | Contacts                     | contacts                                            | ✅   | ✅   | ❌   |
| [0x92](#_0x92) | IncomingCallExtraConfig      | incoming call extra config                            | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0x95](#_0x95) | DtmfEnabled                  | DTMF enabled                                         | ✅   | ✅   | ❌   |
| [0x96](#_0x96) | DtmfKeysExecution            | DTMF key's execution                             | ✅   | ✅   | ❌   |
| [0x97](#_0x97) | SmsPermission                | permission of SMS command                     | ✅   | ✅   | ❌   |
| [0x98](#_0x98) | SmsPwd                       | password of SMS command                       | ✅   | ✅   | ❌   |
| [0x99](#_0x99) | SmsPrefix                    | prefix text for SMS command's response        | ✅   | ✅   | ❌   |
| [0x9B](#_0x9B) | GpsLocationLinkFormatter     | GPS location link formatter           | ✅   | ✅   | ❌   |
| [0x9C](#_0x9C) | WiFiLbsLocationLinkFormatter | WiFi/LBS location link formatter | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0xB0](#_0xB0) | AgpsConfig                   | AGPS config                                          | ✅   | ✅   | ❌   |
| [0xB1](#_0xB1) | LocateContinousConfig        | locate continous config                           | ✅   | ✅   | ❌   |
| [0xB3](#_0xB3) | BleScanSchedule              | BLE scan schedule                             | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0xB8](#_0xB8) | BleList                      | BLE device list binding location                  | ✅   | ✅   | ❌   |
| [0xB9](#_0xB9) | WiFiList                     | WiFi AP list binding location                    | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0xD0](#_0xD0) | WatchfaceId                  | watchface id                                           | ✅   | ✅   | ❌   |
| [0xD1](#_0xD1) | WatchfaceListManagement      | watchface list management                     | ✅   | ✅   | ❌   |
| [0xD2](#_0xD2) | WatchfaceAccessLink          | watchface access link                       | ✅   | ✅   | ❌   |
| [0xD3](#_0xD3) | ScreenWorkParameter          | screen work parameter                                 | ✅   | ✅   | ❌   |
| [0xD4](#_0xD4) | PrimaryUiVisible             | primary UI's visible                              | ✅   | ✅   | ❌   |
| [0xD5](#_0xD5) | HomeUiAutoReturn             | Home UI auto return                              | ✅   | ✅   | ❌   |
| [0xD6](#_0xD6) | WatchfaceDataVisibility      | Watchface data's visibility                       | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0xE0](#_0xE0) | AlarmclockList               | alarmclock list                                       | ✅   | ✅   | ❌   |
| [0xE1](#_0xE1) | Heartrate                    | heartrate measure schedule                        | ✅   | ✅   | ❌   |
| [0xE2](#_0xE2) | Bloodoxyge                   | bloodoxygen measure schedule                      | ✅   | ✅   | ❌   |
| [0xE3](#_0xE3) | Temperature                  | temperature measure schedule                      | ✅   | ✅   | ❌   |
| [0xE4](#_0xE4) | WeatherApplication           | weather application config                        | ✅   | ✅   | ❌   |
| [0xE5](#_0xE5) | ThirdPartyAccessory          | config of 3rd accessories intergrated           | ✅   | ✅   | ❌   |
| [0xE6](#_0xE6) | HealthDataUpload             | data upload schedule                              | ✅   | ✅   | ❌   |
| [0xE8](#_0xE8) | Mileage                      | mileage                                                   | ✅   | ✅   | ❌   |
| [0xE9](#_0xE9) | StepFunctionConfig           | Step couting setting                               | ✅   | ✅   | ❌   |
|             |                              |                                                                |      |      |      |
| [0xF0](#_0xF0) | ReadKey                      | read key                                                | ✅   | ❌   | ❌   |
| [0xF1](#_0xF1) | ReadKeyRange                 | read key range                                       | ✅   | ❌   | ❌   |

---

## Subcommand description

---

### (0x01) model number :id=0x01

**Function description**

Query the current device model number.

**Read return data format**

| Byte No. | Parameter | Type | Converter | Description     |
| -------- | --------- | ---- | --------- | --------------- |
| 1        | length    | byte |           | length          |
| 2        | key       | byte |           | key             |
| 3-6      | model     | uint |           | model |

Note

1. The model data used in the firmware corresponds to the `project code`. For the correspondence between models and project codes, refer to the `project code` correspondence list in the `project standard documentation`. As the documentation is continuously updated, please contact R&D to obtain the `model` number corresponding to your device.

---

### (0x02) firmware version :id=0x02

**Function description**

Query the standard version number of the firmware.

**Read return data format**

| Byte No. | Parameter       | Type | Converter | Description                             |
| -------- | --------------- | ---- | --------- | --------------------------------------- |
| 1        | length          | byte |           | length                                  |
| 2        | key             | byte |           | key                                     |
| 3        | compiledVersion | byte | version   | complied/modified version, range:[0-255] |
| 4        | hotfixVersion   | byte | version   | hotfix version, range:[0-255]            |
| 5        | minorVersion    | byte | version   | minor version, range:[0-255]             |
| 6        | majorVersion    | byte | version   | major version, range:[0-255]             |

备注：

1. Major version number change: When the program architecture and processes are incompatible with the previous version, the version number is upgraded.
2. Minor version number change: New features are added, features are restructured, or features are removed.
3. Hotfix version number change: Bugs in the version are fixed.
4. Compiled version number change: Released to internal testers to distinguish test versions.
5. Version numbers can be converted to `int` data for comparison.

---

### (0x03) IMEI :id=0x03

**Function description**

Get the IMEI string for the Cellular module.

**Read return data format**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3-n      | imei      | String | ascii     | IMEI string |

Note:

1. The IMEI number can only be identified after the Cellular module has booted up normally. Return an empty string  if the IEMI is failed to be identified.

---

### (0x04)ICCID :id=0x04

**Function description**

Obtain the ICCID string of the SIM card.

**Read return data format**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| 3-n      | iccid     | String | ascii     | ICCID       |

Note:

1. After inserting the SIM card into the device, The `ICCID` can be obtained only if the sim is actually identified.
2. After removing the SIM card, the `ICCID` status should be empty and should not display the last identified ICCID.

---

### (0x05)BLE/BT MAC :id=0x05

**Function description**

Obtain the MAC address of the Bluetooth function module.

**Read return data format**

+ The device only supports BLE read return data format

| Byte No. | Parameter | Type | Converter | Description |
| -------- | --------- | ---- | --------- | ----------- |
| 1        | length    | byte |           | length      |
| 2        | key       | byte |           | key         |
| 3-8      | bleMac    | MAC  | leMac     | BLE MAC     |

+ The device also supports BLE&BT read return data format

| Byte No. | Parameter | Type | Converter | Description |
| -------- | --------- | ---- | --------- | ----------- |
| 1        | length    | byte |           | length      |
| 2        | key       | byte |           | key         |
| 3-8      | bleMac    | MAC  | leMac     | BLE MAC     |
| 9-15     | btMac     | MAC  | leMac     | BT MAC      |



---

### (0x06)device name :id=0x06

**Function description**

Get the current device name.

**Read return data format**

| Byte No. | Parameter  | Type   | Converter | Description           |
| -------- | ---------- | ------ | --------- | --------------------- |
| 1        | length     | byte   |           | length                |
| 2        | key        | byte   |           | key                   |
| 3-n      | deviceName | String | utf8      | device name |

---

### (0x07)custom firmware version string :id=0x07

**Function description**

Get the current firmware version string description. Custom firmware uses a custom firmware version string.

**Read return data format**

| Byte No. | Parameter     | Type   | Converter | Description                |
| -------- | ------------- | ------ | --------- | -------------------------- |
| 1        | length        | byte   |           | length                     |
| 2        | key           | byte   |           | key                        |
| 3-n      | versionString | String | utf8      | version string,  |

---

### (0x08)device firmware information :id=0x08

**Function description**

Used to obtain the firmware information description of the device. The format of this information varies depending on the firmware version and platform.

**Read return data format**

| Byte No. | Parameter    | Type  | Converter | Description                 |
| -------- | ------------ | ----- | --------- | --------------------------- |
| 1        | length       | byte  |           | length                      |
| 2        | key          | byte  |           | key                         |
| 3-n      | firmwareInfo | array |           | device firmware information |

* **EV-101 Read return data format magic=0xa991c0d0 Read return data format**

| Byte No. | Parameter       | Type   | Converter  | Description                                                  |
| -------- | --------------- | ------ | ---------- | ------------------------------------------------------------ |
| 1        | length          | byte   |            | length                                                       |
| 2        | key             | byte   |            | key                                                          |
| 3-6      | magic           | uint   |            | magic                                                        |
| 7-10     | projectCode     | uint   |            | project code                                                 |
| 11-14    | firmwareVersion | uint   | version    | firmware version                                             |
| 15-19    | hardwareVersion | uint   | version    | hardware version                                             |
| 20-23    | icType          | uint   |            | IC type                                                      |
| 24-27    | customFlag      | uint   |            | custom flag                                                  |
| 28-39    | compileDate     | String | asciiFixed | date of complied string                                      |
| 40-47    | compileTime     | String | asciiFixed | time of complied string                                      |
| 48-59    | description     | String | utf8       | description string                                           |
| 60-77    | hash            | array  |            | The hash of the firmware bin format content, appended to the end of the firmware. Not generated by compilation; this value is all `0xff` when debugging the firmware. |

note:

1. The device firmware information is detailed information, which is used for tool packaging.
2. Used to obtain more detailed firmware information when tracking firmware issues. This information is not normally required.
3. `magic` distinguishes and identifies the current information structure format during packaging. The selection of `magic` should not be the same as the instruction code to avoid multiple identical entries in the compiled firmware, which would make it difficult to automatically locate the firmware information.
4. Device firmware information is no longer used in the program internally with `absolute or relative location`, and the use of `absolute or relative location` is prohibited to avoid link segment segmentation.

---

### (0x09)device hardware production information :id=0x09

**Function description**

Used to query the current device hardware production information, such as hardware production batch, shell color, etc.

**Read return data format**

| Byte No. | Parameter    | Type   | Converter | Description    |
| -------- | ------------ | ------ | --------- | -------------- |
| 1        | length       | byte   |           | length         |
| 2        | key          | byte   |           | key            |
| 3-6      | optional | uint   |           | parameters       |



---

### (0x0A) WiFi MAC :id=0x0A

**Function description**

Obtain the MAC address of the WiFi function module.

**Read return data format**

| Byte No. | Parameter | Type | Converter | Description |
| -------- | --------- | ---- | --------- | ----------- |
| 1        | length    | byte |           | length      |
| 2        | key       | byte |           | key         |
| 3-8      | mac       | MAC  | leMac     | MAC         |

---

### (0x0B)battery status :id=0x0B

**Function description**

Obtain battery status information.

**Read return data format**

| Byte No. | Parameter           | Type | Converter | Description                                             |
| -------- | ------------------- | ---- | --------- | ------------------------------------------------------- |
| 1        | length              | byte |           | length                                                  |
| 2        | key                 | byte |           | key                                                     |
| 3        | level               | byte |           | Battery percentage, range: [0, 100]                     |
| 4        | isCharging          | byte | bool      | Charging status <br />0- Not charging <br />1- Charging |
| 5-8      | voltage             | uint |           | Current battery voltage,unit: mV                        |
| 9-12     | capacity            | uint |           | Battery capacity, unit: mAh                             |
| 13-16    | chargerInputVoltage | uint |           | optional, Charger Input voltage, unit: mV               |
| 17-20    | chargerInputCurrent | uint |           | Charger Input Current, unit: mA                         |

Note:

1. `optional` is an optional parameter that can be filled in, but devices cannot be left blank.
2. Battery capacity may not be accurately identified, and the assessment may be deleted.

---

### (0x0C)device run time :id=0x0C

**Function description**

Get the duration that the device has been running since it was powered on.

**Read return data format**

| Byte No. | Parameter | Type | Converter | Description            |
| -------- | --------- | ---- | --------- | ---------------------- |
| 1        | length    | byte |           | length                 |
| 2        | key       | byte |           | key                    |
| 3-6      | runTime   | uint |           | run time, unit: second |

---

### (0x0D)Protocol API Version :id=0x0D

**Function description**

Obtain the API version currently implemented on the device to identify the API interface protocol parameters supported by the device.

**Read return data format**

| Byte No. | Parameter  | Type | Converter | Description |
| -------- | ---------- | ---- | --------- | ----------- |
| 1        | length     | byte |           | length      |
| 2        | key        | byte |           | key         |
| 3-6      | apiVersion | uint |           | api version |

Note:

1. Calculation method for API version: `major` * 1000000 + `minor` * 1000 + `hotfix`, displayed in the format v1.0.0, v2.0.0
2. The `major` version indicates a major version change, which is incompatible with previous versions. The `minor` version indicates a minor version change, adding new protocol functionality descriptions. The `hotfix` version indicates revisions or updates to the interface description.

---

### (0x0E)SDK API Version :id=0x0E

**Function description**

Get the current device SDK API version and the features supported by the SDK on the current device.
Reserved for secondary development of the SDK.

**Read return data format**

| Byte No. | Parameter     | Type | Converter | Description     |
| -------- | ------------- | ---- | --------- | --------------- |
| 1        | length        | byte |           | length          |
| 2        | key           | byte |           | key             |
| 3-6      | SdkApiVersion | uint |           | SDK api version |

Note:

1. API version calculation method: `major` * 1000000 + `minor` * 1000 + `hotfix`, displayed in the format v1.0.0, v2.0.0
2. The `major` version indicates a major version change, which is incompatible with previous versions. The `minor` version indicates a minor version, adding new protocol functionality descriptions. The `hotfix` version indicates revisions or updates to the interface description.
3. Considering that different customers use different `SDK` functionalities (with varying functionalities), the `SDK API` version number is primarily used as a standard.

---
### (0x10) Cellular Module Extra Information :id=0x10


**Function description**

提供或补充 GSM 模块的额外信息。<br />Provide or supplement additional information about GSM modules.

**Read return data format**

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

* 1. Model refers to the module model number, for example: `SIMCOM_SIM7500A`.
* 2. FW version refers to the module firmware information, for example: `SIM7500A_B04V02_240227`.
* 3. Revision refers to the module's revision version information, for example: `LE11B04SIM7500A_CUS_YW03`.
* 4. SVN (Software Version Number) is typically a software version number predefined by the communication module manufacturer in the module's firmware, and is inherent to the module itself.
* 5. QMBN (Qualcomm Module Build Number) is a build revision number predefined by Qualcomm in the module firmware, which is inherent to the module itself. For example: `9X07_SIM7500A_P1.02_20220706`.
* 6. The SVN is obtained from the manufacturer; currently, there is no software-based method to directly retrieve it from the module.

How to obtain additional module information Command method**

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

### (0x11)SimCard extra information :id=0x11

**Function description**

Check additional information about your SIM card.

**Read return data format**

| Byte No. | Parameter | Type   | Converter | Description   |
| -------- | --------- | ------ | --------- | ------------- |
| 1        | length    | byte   |           | length        |
| 2        | key       | byte   |           | key           |
| #string1 | msisdn    | String | utf8      | MSISDN string |
| #string2 | ismi      | String | utf8      | ISMI string   |

Note:

1. MSISDN refers to the complete international format of the user's mobile phone number on the SIM card. For example, the MSISDN format for Chinese users is: +86135-xxxx-xxxx.
2. ISMI is the unique identifier of the SIM card, stored on the SIM card, used to distinguish between different SIM cards.

 **Data acquisition method**

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

### (0x20)device custom name :id=0x20

**Function description**

Set up or query the custom name of the device.

**Write request/read response data format**


| Byte No. | Parameter  | Type   | Converter | Description                                 |
| -------- | ---------- | ------ | --------- | ------------------------------------------- |
| 1        | length     | byte   |           | length                                      |
| 2        | key        | byte   |           | key                                         |
| #3-n     | customName | String | utf8      | Custom device name string, maximum 32 bytes |

Note:

1. When using a custom device name to update the Bluetooth broadcast name, if the name is too long, the data will be truncated for broadcasting. For example, `hello world` may only broadcast `hello`.
2. Devices with screens can display a custom name in a suitable location.
3. The `legacy` broadcast supports a maximum of 31 bytes, plus the space occupied by `len-type`, allowing for a maximum name length of `29 bytes`. However, in `advertising extend`, this can be extended to `250+` bytes.
4. When scanning devices, if the device type can only be identified by its name, it is not recommended to use a custom name to update the broadcast data.

---

### (0x21)device custom identify :id=0x21

**Function description**

Used to set or query the device's custom identification code. The custom identification code can be used for:

+ Carry when communicating with the server
+ Other applications


**Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                     |
| -------- | --------- | ----- | --------- | ------------------------------- |
| 1        | length    | byte  |           | length                          |
| 2        | key       | byte  |           | key                             |
| 3-n      | customId  | array |           | custum id,  length: [2-32]bytes |

Note:

1. No switch cancellation
2. The length must be at least 2 bytes, and less than 2 bytes means it is not enabled.
3. The custom identification code is a user-defined data structure format, such as customId[0] representing `encodingType`, and the subsequent content is filled in according to the encoding. The customer is responsible for maintaining the encoding.
4. The tool allows customers to batch update device identification codes according to custom rules. (Follow-up suggestion)
5. When the host computer displays the current field, it should allow switching between display formats, such as HEX/UTF8/Unicode.
6. The customId content is customized by the customer or server side according to requirements. The device does not parse, modify, or display this data. It is recommended that customers or server sides customize the data structure according to requirements, but consider whether to classify, group, or categorize customers.

---

### (0x22)NFC records:id=0x22

**Function description**

Used to set the message content of NFC.


**Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                                                  |
| -------- | --------- | ----- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte  |           | length                                                       |
| 2        | key       | byte  |           | key                                                          |
| 3        | index     | byte  |           | record index, range:[0,9]                                    |
| 4        | type      | byte  |           | record type, `NDEF` format<br />0:clear record <br />1:url/uri <br />2:custom url/uri <br />3: plain text <br />4: bluetooth oob <br />255: custom data |
| 5-n      | content   | array |           | record content                                               |

Note:

1. The NFC tag must be set to read-only mode, and it is not permitted to modify the tag content using the NFC method.
2. Since NFC readers on different platforms have different priority mechanisms for processing NDEF content, please use different record data with caution.
3. If the `record` content is custom, it also uses the `NDEF` format. Note that it is manually marked as the `ME` flag. When using custom data, prioritize reverse indexing to avoid affecting previous records. That is, `custom data` starts from `index=9`.
4. Store basic firmware information in NFC. When the device encounters a problem and cannot boot up, read the NFC content to obtain the basic firmware information. The basic firmware information can be used as the default `record` and does not occupy the index of the current protocol.

!> The record types and corresponding content are pending completion, involving a separate file description for the `NDEF` format.

---

### (0x23) User information :id=0x23


**Function description**

Used to set or check the user's basic personal information.

**Read return data format**

| Byte No.  | Parameter  | Type   | Converter | Description                                                  |
| --------- | ---------- | ------ | --------- | ------------------------------------------------------------ |
| 1         | length     | byte   |           | length                                                       |
| 2         | key        | byte   |           | key                                                          |
| 3         | gender     | byte   |           | gender<br />0-male<br />1-female<br />2-unknown              |
| 4         | bloodType  | byte   | bits      | blood type<br />`bit0-3:`<br />0- O  <br />1- A  <br />2- B  <br />3- AB <br />15-default, in blank<br /> `bit4:`<br />0- Rh negative<br />1- Rh positive |
| 5-6       | height     | ushort |           | height, unit: cm/10, range:[480-2300]                        |
| 7-8       | weight     | ushort |           | weight, unit: kg/10, range:[200-1500]                        |
| 9-10      | birthYear  | ushort |           | year in date of birth, range:[1900-2100]                     |
| 11        | birthMonth | byte   |           | month in date of birth, range:[1,12]                         |
| 12        | birthDay   | byte   |           | day in date of birth, range: [1-31]                          |
| #string 1 | name       | String | utf8      | name string, Type: String, Encode: utf8                      |
| #string 2 | image      | String | utf8      | Avatar image path, Type: String, Encode: utf8                |

Note:

1. Date of birth requires valid information
2. The avatar image path can be a local storage file name or a URL/URI.

---

### (0x24) application scenario :id=0x24

**Function description**


Corresponding to the previous `work mode`, in order to distinguish it from the mode name, it has been changed to usage scenario. For different usage scenarios, default work parameters are loaded.

Application scenarios are usually described as follows: user-oriented behavior description, focus on external environmental factors, describe specific application situations, and emphasize user experience requirements.

Standard working mode description: device status description, focus on internal operating parameters, system
working status description, and emphasize performance and power consumption.

**Write request/read response data format**

| Byte No. | Parameter  | Type | Converter | Description                                                  |
| -------- | ---------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length     | byte |           | length                                                       |
| 2        | key        | byte |           | key                                                          |
| 3        | type       | byte |           | type, range:[0-6], default: 1<br />0- Do no preset scenario, RFU<br />1/2/3/4/5/6- Enable application scenario parameters |
| 4-7      | parameters | uint |           | Different scene parameter definitions are different          |


+ **type=0 Does not support setting the scene type to 0 via protocol.**
+ type=1/2/3/6 写请求/读返回数据格式 type=1/2/3/6 Write request/read response data format

| Byte No. | Parameter | Type | Converter | Description   |
| -------- | --------- | ---- | --------- | ------------- |
| 1        | length    | byte |           | length        |
| 2        | key       | byte |           | key           |
| 3        | type      | byte |           | type = 1      |
| 4-7      | meanless  | uint |           | meanless data |

+ type=1/2/3/6 Write request/read response data format

| Byte No. | Parameter      | Type | Converter | Description                                                  |
| -------- | -------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length         | byte |           | length                                                       |
| 2        | key            | byte |           | key                                                          |
| 3        | type           | byte |           | type = 4                                                     |
| 4-7      | uploadInterval | uint |           | interval of upload data to server schedule, unit: seconds， range:[10minutes, 7days] |

+ type=5 Write request/read response data format

| Byte No. | Parameter      | Type | Converter | Description                                                  |
| -------- | -------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length         | byte |           | length                                                       |
| 2        | key            | byte |           | key                                                          |
| 3        | type           | byte |           | type = 5                                                     |
| 4-7      | uploadInterval | uint |           | interval of upload data to server schedule, unit: seconds， range:[10minutes, 7days] |

Note:

1. The functional definitions of each scenario and the parameters that affect them can be found in the product functional specifications.


---

### (0x25)  do not disturb mode :id=0x25

**Function description**

Do Not Disturb mode is used to disable certain functions during specified time periods so that they do not disturb
the user with sound, light, or vibration.

**Write request/read response data format**

| Byte No. | Parameter   | Type  | Converter | Description                                                  |
| -------- | ----------- | ----- | --------- | ------------------------------------------------------------ |
| 1        | length      | byte  |           | length                                                       |
| 2        | key         | byte  |           | key                                                          |
| 3        | index       | byte  |           | index, range：0-1                                            |
| 4        | enable      | byte  | bool      | Enable Do Not Disturb mode<br />0- disabled<br />1- enabled  |
| 5        | startHour   | byte  |           | start hour, range:[0, 23]                                    |
| 6        | startMinute | byte  |           | start minute, range:[0, 59]                                  |
| 7        | endHour     | byte  |           | end hour, range:[0, 23]                                      |
| 8        | endMinute   | byte  |           | end minute, range:[0, 59]                                    |
| 9-n      | options     | array |           | Additional parameter options, optional. Extend with 4-byte alignment. |

Note:

1. If the start time is earlier than the end time, the time period is within the same day.  
2.  If the start time is later than the end time, the time period spans multiple days.  
3. If the start time is equal to the end time, the Do Not Disturb mode is forced to be disabled.  
4. Additional option parameters are optional; if not specified, the original values remain unchanged. However, the `read operation` will return additional parameters. These can be used as toggle switches for features during Do Not Disturb mode, with the specific behavior of each parameter determined by another application. That is, the `options` parameter allows different devices to be modified for different functionalities.

---

### (0x26)button shortcut function :id=0x26

**Function description**

Key function settings allow you to customize the tasks performed when a key is pressed or double click.

**Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description                                                  |
| -------- | --------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length    | byte   |           | length                                                       |
| 2        | key       | byte   |           | key                                                          |
| 3        | index     | byte   |           | Index, range based on keys currently supported by the device<br />0- SOS key |
| 4        | group     | byte   |           | Grouping, allowing key combinations to be bound<br />0- Invalid grouping <br />others- Group number |
| 5        | enable    | byte   |           | Enable<br />0- Do not use feature binding<br />1- Use feature binding |
| 6        | feedback  | byte   | bits      | feedback <br />bit0: 0- no vibrate 1-vibration feedback <br />bit1: 0- no prompt audio 2-audio prompt <br />bit2: 0- no LED alert  2-LED alert |
| 7        | mode      | byte   |           | mode<br />0- long press <br />1- double click                |
| 8-9      | timeLimit | ushort |           | time limit. unit: ms<br />mode=0, time of pressed  <br />mode=1, max interval of double click |
| 10       | action    | byte   |           | task/action <br />0- no task<br />1-10：dial contact number(1..10) |

Note:

1. index-0 Default SOS button
2. In feedback, pay attention to whether the device has a corresponding output. If it does not have the corresponding output capability, it will not work.
3. Group numbers are used to combine key functions. After enabling grouping, all keys must be pressed before a task or action is executed. (Reserved for future functionality expansion)

**Key index corresponding to model number**

+ W101 Key Index Table, Number of Keys: 1

TODO: Add corresponding image annotations

| index | button name |
| ---- | -------- |
| 0    | SOS button |

---
### (0x27)  sleep mode :id=0x27

**Function description**

Sleep mode is used to disable certain functions during a specified time period so that they do not affect the user through sound, light, or vibration.

At the same time, consider whether to enable the sleep aid function (actively reducing the sound/ light/ electricity level by one level).

**Write request/read response data format**

| Byte No. | Parameter   | Type  | Converter | Description                                                  |
| -------- | ----------- | ----- | --------- | ------------------------------------------------------------ |
| 1        | length      | byte  |           | length                                                       |
| 2        | key         | byte  |           | key                                                          |
| 3        | index       | byte  |           | Index, range: 0-1                                            |
| 4        | enable      | byte  | bool      | enable<br />0- disabled<br />1- enabled                      |
| 5        | startHour   | byte  |           | 开始时间 start hour, range:[0, 23]                           |
| 6        | startMinute | byte  |           | start minute, range:[0, 59]                                  |
| 7        | endHour     | byte  |           | end hour, range:[0, 23]                                      |
| 8        | endMinute   | byte  |           | end minute, range:[0, 59]                                    |
| 9        | sleepAid    | byte  |           | Whether to use sleep assistance, set a reminder in advance, and lower the sound and light reminder level<br />0: Disabled <br />1-60: Number of minutes in advance |
| 10-n     | options     | array |           | optional: extra parameter options, optional. Aligned to 4 bytes for extension |

Note:

1. If the start time is earlier than the end time, the time period is within the same day.  
2. If the start time is later than the end time, the time period spans multiple days.  
3. If the start time is equal to the end time, sleep mode is forced to close.  
4. Additional option parameters are optional; if not specified, the original values remain unchanged. However, the `read operation` will return additional parameters. These can be used as functional application switches during sleep mode, with the specific behavior of each parameter determined by another application. That is, the `options` parameter allows different devices to be modified for different functional uses.
5. In `sleep mode`, when the system detects that the `sleep start time period` has been reached, it prompts the user to adjust their schedule. During subsequent data analysis, this time period is used to calculate the user's daily sleep achievement rate.

---

### (0x28) auto low power mode :id=0x28

**Function description**

Low power mode is used to automatically enter low power mode during a specified time period, which manifests itself by turning off power-consuming functions such as GSM network connection, WiFi network connection, GPS positioning, etc.

**Write request/read response data format**

| Byte No. | Parameter   | Type | Converter | Description                              |
| -------- | ----------- | ---- | --------- | ---------------------------------------- |
| 1        | length      | byte |           | length                                   |
| 2        | key         | byte |           | key                                      |
| 3        | index       | byte |           | Index, range: 0-1                        |
| 4        | enable      | byte | bool      | enable <br />0- disabled<br />1- enabled |
| 5        | startHour   | byte |           | start hour, range:[0, 23]                |
| 6        | startMinute | byte |           | start minute, range:[0, 59]              |
| 7        | endHour     | byte |           | end hour, range:[0, 23]                  |
| 8        | endMinute   | byte |           | end minute, range:[0, 59]                |

Note:

1. If the start time is earlier than the end time, the time period is within the same day.  
2. If the start time is later than the end time, the time period spans multiple days.  
3. If the start time is equal to the end time, force close the automatic low power mode.
4. `Automatic low-power mode` refers to the device automatically entering low-power mode during a specified time period, which involves disabling power-consuming functions such as GSM network connections, Wi-Fi network connections, and GPS location services.
5. The current mode requires additional conditions, such as detecting when the device is at home. Additionally, power-consuming applications should only be disabled after the device has completed its tasks.

---

### (0x31)format preference :id=0x31

**Function description**

Set/check the unit format preferences for the current device:

+ Length/height units
+ Weight units
+ Temperature units
+ Hour format
+ Year/month/day format

If the above type of display exists in other `keys`, you can update the display of page units by obtaining the `format preference`.

**Write request/read response data format**

| Byte No. | Parameter       | Type | Converter | Description                                                  |
| -------- | --------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length          | byte |           | length                                                       |
| 2        | key             | byte |           | key                                                          |
| 3        | lengthUnit      | byte |           | length/height unit <br />0- cm/meter  <br />1- ft-in         |
| 4        | weightUnit      | byte |           | weight unit <br />0- kilogram <br />1- lbs                   |
| 5        | temperatureUnit | byte |           | temperature unit <br />0- Celsius  <br />1- Fahrenheit       |
| 6        | hourFormat      | byte |           | hour format  <br />0- 24 hour format <br />1- 12 hour format |
| 7        | dateFormat      | byte |           | date format  <br />0- Year-Month-Day <br />1- Month-Day-Year <br />2- Day-Month/Year |


Note:

1. Date format: The date format should be displayed in the corresponding display language.
2. Format preferences do not apply in the `Custom Watchface`
3. Format preferences are the system-wide default format.
4. Some apps have their own format preferences, such as weather apps that use their own unit settings.

---

### (0x32)language preference :id=0x32

**Function description**

Set/get the language type that the device needs to use.

+ Audio prompt language
+ Text display language

**Write request/read response data format**

| Byte No. | Parameter           | Type   | Converter | Description                               |
| -------- | ------------------- | ------ | --------- | ----------------------------------------- |
| 1        | length              | byte   |           | length                                    |
| 2        | key                 | byte   |           | key                                       |
| #string1 | voicePromptLanguage | String | utf8      | audio prompt language, max length 16bytes |
| #string2 | textDisplayLanguage | String | utf8      | text display language, max length 16bytes |

Note:

1. Refer to the appendix for language string formats. Or online documentation([https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r1/core/res/res/values/locale_config.xml](https://android.googlesource.com/platform/frameworks/base/+/android-8.1.0_r1/core/res/res/values/locale_config.xml))
2. When additional language support is required, please add it from the reference appendix.

---

### (0x33)date & time :id=0x33

**Function description**

Check or set the device's time and date.

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3-6      | dateTime  | uint | unixTime  | Unix time consists of the number of seconds since January 1, 1970, 00:00:00 UTC. |


Note:

1. As mentioned in previous chapters, unixTime is an unsigned 32-bit data type.
2. Time conversion rules: From 1970-1-1 00:00:00, add the number of seconds in UTC.

```csharp
DateTime dt = new DateTime(1970, 1, 1, 0, 0, 0);
dt = dt.AddSeconds(Utc);
return dt;
```

---

### (0x34)time zone :id=0x34

**Function description**

Set or check the time zone currently used by the device.

**Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                                            |
| -------- | --------- | ----- | --------- | ------------------------------------------------------ |
| 1        | length    | byte  |           | length                                                 |
| 2        | key       | byte  |           | key                                                    |
| 3        | timeZone  | sbyte | timeZone  | time zone, Type: sbyte, unit: quarter, range:[-48, 56] |

Note:

1. 
   Time zone value range: [-48 * 15, 56 * 15], corresponding to [UTC -12:00, UTC +14:00]
2. About UTC+13:00, please refer to this article:  [https://en.wikipedia.org/wiki/UTC+13:00](https://en.wikipedia.org/wiki/UTC+13:00)

---

### (0x35)date & time extra configuration :id=0x35

**Function description**

 Check or set automatic updates for date and time.

**Write request/read response data format**

| Byte No. | Parameter               | Type | Converter | Description                                                  |
| -------- | ----------------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length                  | byte |           | length                                                       |
| 2        | key                     | byte |           | key                                                          |
| 3        | dayLightSavingTime      | byte |           | Use national daylight saving time rules<br />0:disable<br />1:American <br />2:Europe<br />3:Australia<br />4:Australia (New Zealand) |
| 4        | winterTime              | byte |           | Winter time rules used in the country <br />0:disable <br />others:RFU |
| 5-8      | baseStationSyncInterval | uint |           | Base station calibration interval, unit: second              |
| 9-12     | gpsSyncInterval         | uint |           | GPS time synchronization interval, unit: seconds             |
| 13       | serverSync              | byte | bool      | allow server time synchronization or not                     |
| 14       | bleSync                 | byte | bool      | allow BLE time synchronization or not                        |
| 15       | pcSync                  | byte | bool      | allow Upper computer time calibration or not                 |

---

### (0x40)remote server config :id=0x40

**Function description**

By setting up a remote server, provide device access connections and perform data interaction.

**Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description                      |
| -------- | --------- | ------ | --------- | -------------------------------- |
| 1        | length    | byte   |           | length                           |
| 2        | key       | byte   |           | key                              |
| 3        | index     | byte   |           | index range:[0,3]，default: 0    |
| 4        | type      | byte   |           | type <br />0- TCPIP <br />1- UDP |
| 5-6      | port      | ushort |           | port                             |
| 7-n      | server    | String | utf8      | server's IP or domain            |

Note:

1. When querying server configurations through other means, if no index is specified, the server configuration with `index = 0` is used by default.

---

### (0x41)APN configuration list :id=0x41

**Function description**

The core function of APN is to serve as a configuration parameter for mobile devices connecting to Cellular networks, identify and determine how devices access specific networks.

**Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description                                                  |
| -------- | --------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length    | byte   |           | length                                                       |
| 2        | key       | byte   |           | key                                                          |
| 3-4      | index     | byte   |           | index                                                        |
| 5-8      | plmn      | uint   |           | PLMN, composed of MCC and MNC, is used to identify different mobile network operators. |
| #string1 | apn       | String | ascii     | APN string,  length:[0, 32]                                  |
| #string2 | userName  | String | ascii     | user name string,  length:[0, 32]                            |
| #string3 | password  | String | ascii     | password string, length:[0, 32]                              |

**read response data format-query with index**

| Byte No. | Parameter | Type | Converter | Description    |
| -------- | --------- | ---- | --------- | -------------- |
| 1        | length    | byte |           | length         |
| 2        | key       | byte |           | key            |
| 3-4      | index     | byte |           | index  |

**read response data format-query with plmn**


| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3-4      | index     | byte |           | index will be ignored                                        |
| 5-8      | plmn      | uint |           | PLMN, composed of MCC and MNC, is used to identify different mobile network operators. |

Note:

1. PLMN used for self-adaptive APN

---

### (0x42) WLAN AP list :id=0x42

**Function description**

Used to Configure or query available Wi-Fi hotspots for network access and data exchange. 

Configurefor accessing the Internet or an internal network to perform data exchange.

**Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description                            |
| -------- | --------- | ------ | --------- | -------------------------------------- |
| 1        | length    | byte   |           | length                                 |
| 2        | key       | byte   |           | key                                    |
| 3        | index     | byte   |           | index, range:[0, 9]               |
| 4-7      | rfu       | uint   |           | Subsequent steps may require specifying authentication and encryption methods. |
| #string1 | ssid      | String | utf8      | ssid , length:[0, 32]                  |
| #string2 | password  | String | utf8      | password, length:[0, 32]               |
|          | mac       | MAC    | leMac     | MAC，optional                          |

Note:

1. MAC is used as an optional parameter. By including the MAC address as a supplementary identifier, the system can prevent connection failures that may occur when multiple networks share the same SSID but have different passwords. including the identifier helps same SSID is associated with different passwords.

---

### (0x43)WLAN connection setting :id=0x43

**Function description**

Configure or query the working parameters for WiFi automatic connection.

**Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                                            |
| -------- | --------- | ----- | --------- | ------------------------------------------------------ |
| 1        | length    | byte  |           | length                                                 |
| 2        | key       | byte  |           | key                                                    |
| 3        | enable    | byte  | bool      | Allow Wi-Fi connection<br />0- disable <br />1- enable |
| 4        | rssi      | sbyte |           | rssi, range:[-127, 0]                                  |
| 5        | tryCount  | byte  |           | try count if connect fail                              |
| 6-9      | timeout   | uint  |           | timeout, unit: second                                  |

Note:

1. When WiFi networking is enabled, data reporting will prioritize the use of WLAN.
2. timeout Cannot set the WiFi module scan timeout, which can be used as the delay interval for the next connection attempt.

---

### (0x44)heartbeat keepalive to server :id=0x44

**Function description**

Heartbeat packets, as a keep-alive mechanism for maintaining communication with the server and can help prevent the device from being removed from the network by the base station after extended periods without data transmission.

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br />0- Do not use heartbeat packets to keep the server alive. <br />1- Use heartbeat packets to keep the server alive. |
| 4-7      | interval  | uint |           | interval, range:[60, 86400], unit: second                    |


---

### (0x48)volume of ring tone :id=0x48

**Function description**

Used to set or query the ring volume when the device receives a call.

**Write request/read response data format**

| Byte No. | Parameter      | Type | Converter | Description                     |
| -------- | -------------- | ---- | --------- | ------------------------------- |
| 1        | length         | byte |           | length                          |
| 2        | key            | byte |           | key                             |
| 3        | ringToneVolume | byte |           | ring tone volume, range:[0,100] |

Note:

1. When the volume is set to 0, the `speaker` output must be turned off; simply setting the volume level to 0 is not sufficient. In the `codec`, `0` corresponds to low gain but not no output.
2. The incoming call ringtone volume is the global incoming call ringtone volume.
3. If an independent application has a specified volume for a contact number, it takes precedence over the global settings.
4. Different `codec`s have different `gain` levels, which are internally converted.
5. The frontend can divide the volume into fewer levels, such as 10 levels.


---

### (0x49) gain of mic :id=0x49

**Function description**

Set or check the microphone gain.

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                 |
| -------- | --------- | ---- | --------- | --------------------------- |
| 1        | length    | byte |           | length                      |
| 2        | key       | byte |           | key                         |
| 3        | micGain   | byte |           | gain of mic, range:[0, 100] |

Note:

1. The gain for `mic` here is a global setting.
2. Specify the mic gain corresponding to the contact number in the standalone app, which will override the global settings.


---

### (0x4A)volume of speaker :id=0x4A

**Function description**

Configure or check the speaker volume.

Speaker volume affects the following functions:

* volume during call talking 
* volume during speaker & mic loopback 


**Write request/read response data format**

| Byte No. | Parameter   | Type | Converter | Description                       |
| -------- | ----------- | ---- | --------- | --------------------------------- |
| 1        | length      | byte |           | length                            |
| 2        | key         | byte |           | key                               |
| 3        | speakerGain | byte |           | volume of speaker, range:[0, 100] |

Note:

1. The volume of the speaker here is a global volume setting.
2. Specify a contact-specific speaker gain in the standalone app, which overrides the global volume setting


---

### (0x4B)volume of audio prompt :id=0x4B

**Function description**

Configure or check the volume of the audio prompt.

**Write request/read response data format**

| Byte No. | Parameter    | Type | Converter | Description                      |
| -------- | ------------ | ---- | --------- | -------------------------------- |
| 1        | length       | byte |           | length                           |
| 2        | key          | byte |           | key                              |
| 3        | promptVolume | byte |           | volume of prompt, range:[0, 100] |

Note:

1. The prompt tone volume is a global volume.
2. In standalone applications, when a specific contact is assigned a different audio prompt volume, that setting takes priority over the global configuration. For example, in the “find device” function, the playback volume should be set to the highest level.

---

### (0x4C)prompt function enable :id=0x4C

**Function description**

Enable/disable the audio prompt switch.

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                    |
| -------- | --------- | ---- | --------- | ------------------------------ |
| 1        | length    | byte |           | length                         |
| 2        | key       | byte |           | key                            |
| 3      | enable     | uint |           | main switch, 0- disable, 1- enable |



Note

1. The `main switch` is the master switch. When `disabled`, global audio prompts will not work.
2. Different audio prompts are disabled by the prompt execution process configuration.
3. Quick access switches for audio prompt files may be added here in future updates.

---

### (0x50) GEO detection config :id=0x50

**Function description**

GEO, commonly known as an electronic fence, is used to define a designated area and detect whether a device has entered or left the area.

**Write request/read response data format**

| Byte No. | Parameter   | Type   | Converter | Description                                                  |
| -------- | ----------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length      | byte   |           | length                                                       |
| 2        | key         | byte   |           | key                                                          |
| 3        | index       | byte   |           | index                                                        |
| 4        | enable      | byte   | bool      | enable, 0- disable 1- enabled                                |
| 5        | flag        | byte   | bits      | flag<br />bit0 left detection : 0-disable，1-enable <br />bit1  enter detection: 0- disable，1-enabled <br />bit7 area shape: 0- circle，1-polygon |
| 6        | points      | byte   |           | points                                                       |
| 7-8      | radius      | ushort |           | radius, unit: meters                                         |
| 9-12     | lat         | float  | loc       | Location point 0 -latitude                                   |
| 13-16    | lng         | float  | loc       | Location point 0 -longitude                                  |
| ...      | ...         | ...    | ...       | ...                                                          |
|          | todo        | float  | loc       | Position point `n`, where `n` is at most 8, meaning that polygons with up to `8` vertices can be drawn. |
| string   | description | string |           | description, utf8 encoding, max 32 bytes bytes.              |


+ GEO fence circular area Data format flag.bit7 = 0 

| Byte No. | Parameter   | Type   | Converter | Description                                                  |
| -------- | ----------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length      | byte   |           | length                                                       |
| 2        | key         | byte   |           | key                                                          |
| 3        | index       | byte   |           | index                                                        |
| 4        | enable      | byte   | bool      | enable, 0- disable 1- enabled                                |
| 5        | flag        | byte   | bits      | flag<br />bit0 left detectioin : 0-disable，1-enable <br />bit1  enter detection: 0- disable，1-enabled <br />bit7 area shape = 0- circle |
| 6        | points      | byte   |           | points = 1                                                   |
| 7-8      | radius      | ushort |           | radius, unit: meters                                         |
| 9-12     | lat         | float  | loc       | Location point 0 -latitude                                   |
| 13-16    | lng         | float  | loc       | Location point 0 -longitude                                  |
| 17-48    | description | string |           | description, utf8 encoding, max 32 bytes                     |

+ GEO fence polygon area data format flag.bit7 = 1

| Byte No. | Parameter   | Type   | Converter | Description                                                  |
| -------- | ----------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length      | byte   |           | length                                                       |
| 2        | key         | byte   |           | key                                                          |
| 3        | index       | byte   |           | index                                                        |
| 4        | enable      | byte   | bool      | enable, 0- disable 1- enabled                                |
| 5        | flag        | byte   | bits      | flag<br />bit0 left detectioin : 0-disable，1-enable <br />bit1  enter detection: 0- disable，1-enabled <br />bit7 area shape = 1-polygon |
| 6        | points      | byte   |           | points range:[3, 8]                                          |
| 7-8      | radius      | ushort |           | meanless                                                     |
| 9-12     | lat         | float  | loc       | Location point 0 -latitude                                   |
| 13-16    | lng         | float  | loc       | Location point 0 -longitude                                  |
| ...      | ...         | ...    | ...       | ...                                                          |
|          | todo        | float  | loc       | Position point `n`, where `n` is at most 8, meaning that polygons with up to `8` vertices can be drawn. |
|          | description | string |           | description, utf8 encoding, max 32 bytes                     |

Note:

1. The `radius` is only effective when the fence shape is set to circular.
2. `points` is forced to 1 when setting the fence area as a circle; when the fence area is a polygon, the minimum is 3 and the maximum is 8.
3. Location points are used to provide the latitude and longitude coordinates required to draw shapes on a map.
4. Position point 0 When selecting a circular fence shape, this represents the center point of the circular area.
5. When GEO is configured to detect both entry and exit, the corresponding event will be output upon detecting entry or exit. The geofence configured triggered upon detecting entry or exit.

---

### (0x51) Fall down detection config :id=0x51

**Function description**

Fall detection refers to the configuration of working parameters for the fall detection algorithm, enabling it to be better aligned with practical application scenarios. 

**Write request/read response data format**

| Byte No. | Parameter   | Type | Converter | Description                                                  |
| -------- | ----------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length      | byte |           | length                                                       |
| 2        | key         | byte |           | key                                                          |
| 3        | enable      | byte | bool      | enable<br />0- disable fall detection<br />1- enable fall detection |
| 4        | sensitivity | byte |           | sensitivity, range:[1-10]                                    |

Note:

1. The higher the sensitivity value, the less strict the model matching requirements are, making fall detection easier to trigger, but also increasing the likelihood of false positives.



---

### (0x52)motion detection config :id=0x52

**Function description**

Motion detection is generally used to detect whether a device meets the activity threshold within the configured time period.

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br />0- disable activity detection <br />1- enable activity detection |
| 4        | interval  | byte |           | interval, unit: seconds                                      |
| 5        | threshold | byte |           | threshold, unit: seconds                                     |

Note:

1. The detection logic is: when the device is not in a static state for a certain duration, it is considered that the device is active.


---

### (0x53)static detection config :id=0x53

**Function description**

Stationary detection refers to determining that the device is in a stationary state if it does not move for a continuous period of time.

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br />0- disable static detection <br />1- enable static detection |
| 4        | threshold | byte |           | Inactivity time threshold, unit: s                           |

Note:

1. During charging, the device should stop detecting stationary or ignore detected stationary events.

---

### (0x54) tilt detection config :id=0x54

**Function description**

Tilt detection refers to determining that device is in a tilted state if its positional angle exceeds the set threshold for a continuous duration.

**Write request/read response data format**

| Byte No. | Parameter      | Type   | Converter | Description                                                  |
| -------- | -------------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length         | byte   |           | length                                                       |
| 2        | key            | byte   |           | key                                                          |
| 3        | enable         | byte   | bool      | enable<br />0- disable tilt detection <br />1- enable tilt detection |
| 4        | angleThreshold | byte   |           | angle threshold                                              |
| 5-6      | timeThreshold  | ushort |           | time threshold                                               |

---

### (0x55) overspeed detection config :id=0x55

**Function description**

Overspeed detection refers to the function where the device monitors its current speed via GPS, and determines an overspeed condition if the speed surpasses the configured threshold and is sustained for the defined period of time. the current device's speed viaconfiguredfor a sdurationin an overspeed state

**Write request/read response data format**

| Byte No. | Parameter      | Type | Converter | Description                                                  |
| -------- | -------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length         | byte |           | length                                                       |
| 2        | key            | byte |           | key                                                          |
| 3        | enable         | byte | bool      | enable<br />0- disable overspeed detection <br />1- enable overspeed detection |
| 4-7      | speedThreshold | uint |           | speed threshold, unit: km/h                                  |
| 8-11     | timeThreshold  | uint |           | time threshold, unit: seconds                                |

---

### (0x56)low power detection config :id=0x56

**Function description**

Low battery detection — triggered when the battery level is below the preset threshold and the device is not charging. determining that the device is in a low battery state when the battery level falls below the configured threshold and the device is not charging.

**Write request/read response data format**

| Byte No. | Parameter        | Type | Converter | Description                                                  |
| -------- | ---------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length           | byte |           | length                                                       |
| 2        | key              | byte |           | key                                                          |
| 3        | enable           | byte | bool      | enable<br />0- disable low power detection <br />1- enable low power detection |
| 4        | levelThreshold   | byte |           | low battery percentage threshold                             |
| 5        | triggerThreshold | byte |           | percentage of battery drop to trigger again                  |

---

### (0x57) leave/at home detection config :id=0x57

**Function description**

Home/Away detection: identifies whether the device is at home or away by comparing BLE-scanned devices or Wi-Fi-scanned APs against predefined references.
It determines whether the device is within a designated area by scanning surrounding devices via BLE or scanning nearby APs via Wi-Fi and performing a comparison.

**Write request/read response data format**

| Byte No. | Parameter      | Type | Converter | Description                                                  |
| -------- | -------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length         | byte |           | length                                                       |
| 2        | key            | byte |           | key                                                          |
| 3        | enable         | byte | bool      | enable<br />0- disable away/at home detection <br />1- enable away/at home detection |
| 4-7      | scanInterval   | uint |           | interval of scan,unit: second                                |
| 8        | leaveThreshold | byte |           | away device number threshold                                 |
| 9        | homeThreshold  | byte |           | home device number threshold                                 |

---

### (0x58)fatigue detection config :id=0x58

**Function description**

Fatigue reminder — the device notifies the user to move when the exercise target is not met within the configured time period.

**Write request/read response data format**

| Byte No. | Parameter  | Type | Converter | Description                                                  |
| -------- | ---------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length     | byte |           | length                                                       |
| 2        | key        | byte |           | key                                                          |
| 3        | enable     | byte | bool      | enable<br />0- disable sedentary detection <br />1- enable sedentary detection |
| 4-7      | period     | uint |           | detect period, unit: s, range: [30 minutes, 4 hours]         |
| 8-11     | stepTarget | uint |           | target step, range: [30, 1000]                               |

Note:

1. This feature is disabled during Focus Mode.

---

### (0x59)wear detection config :id=0x59

**Function description**

Wearing detection: the device evaluates combined sensor data to determine whether the conditions indicate that the device is currently being worn.The device determines whether the current sensor data meets the criteria for being worn by analyzing comprehensive data.

**Write request/read response data format**

| Byte No. | Parameter      | Type | Converter | Description                                                  |
| -------- | -------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length         | byte |           | length                                                       |
| 2        | key            | byte |           | key                                                          |
| 3        | enable         | byte | bool      | enable<br />0- disable wear detection <br />1- enable wear detection |
| 4-7      | period         | uint |           | Testing cycle, unit: s, range: [1 minute, 4 hours]           |
| 8        | retryCondition | byte |           | When the following conditions are met, re-detect<br />* When the device transitions from a stationary state to an active state |

Note:

1. The Wearing Detection switch here does not affect the logic and judgment of wearing detection used to initiate the heart rate and blood oxygen algorithms.
2. The Wearing Detection here will, in the future, determine the wearing status by integrating data from multiple sensors based on technical capabilities. Currently, the determination is solely based on PPG wearing detection.
3. The Wearing Detection logic: When the device is active and detects that it is not being worn, should perform multiple periodic checks to confirm the status.

---

### (0x5A)welfare detection config :id=0x5A

**Function description**

Set/Obtain the functional detection parameters for welfare.

**Write request/read response data format**

| Byte No. | Parameter   | Type | Converter | Description                                                  |
| -------- | ----------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length      | byte |           | length                                                       |
| 2        | key         | byte |           | key                                                          |
| 3        | enable      | byte | bool      | enable<br />0- disable welfare function <br />1- enable welfare function |
| 4-7      | duration    | uint |           | countdown duration, unit: s, range: [600, 3600000]           |
| 8-11     | warningTime | uint |           | warning duration, unit: s, range: [120, 600]                 |



Note:

1. After the warefare function is activated, the countdown time is longer than the warning time.
2. The following events may be generated by this function: preparation time expiration, check-in, stop, and alarm.

---

### (0x60)GEO fence alert settings :id=0x60

**Function description**

When a preset event is detected (entering or leaving the defined area), the device will:

+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br/>0- GEO Fence Alert is disabled<br/>1- GEO Fence Alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |


Note：

1. Enable: Refers to the buit-in alert process, which is the alert logic code embedded in the standard firmware version. In the current version, when GEO detection is enabled, enable is forced to be enabled. The corresponding UI interface does not require check two switches.
2. Allow sending SMS/calling to specified emergency contacts, which requires preset valid phone numbers in advance.



---

### (0x61)SOS alarmt settings :id=0x61

**Function description**

When a SOS alarm is activated, the device will:

+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br/>0- SOS Alert is disabled<br/>1- SOS Alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x62)Fall down alert settings :id=0x62

**Function description**

When a fall down is detected, the device will:

+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br/>0- Fall Down Alert is disabled<br/>1- Fall Down Alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |


---

### (0x63) Motion alert settings :id=0x63

**Function description**

When a motion alert is detected,the device will:

+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br />0- Motion Alert is disabled<br/>1- Motion Alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x64)No motion alert settings :id=0x64

**Function description**

When a No motion alert is detected, the device will:

+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br/>0- No Motion Alert is disabled<br/>1-No Motion Alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x65)Tilt alert settings :id=0x65

**Function description**

When a tilt alert is detected, the device will:


+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br/>0- Tilt Alert is disabled<br/>1-Tilt Alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x66)Over speed alert settings :id=0x66

**Function description**

When an over speed event is detected, the device will:


+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br />0- Over speed alert is disabled<br/>1- Over speed alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x67)Low battery alert settings :id=0x67

**Function description**

When a low battery alert is detected, the device will:

+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br/>0- Low battery alert is disabled<br/>1- Low battery alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |
---

### (0x68) Power-on alert settings:id=0x68

**Function description**

When a power-on event is detected, the device will:

+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br/>0- Power-on alert is disabled<br/>1- Power-on alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x69)Shutdown alert settings:id=0x69

**Function description**

When a shutdown event is detected, the device will:

+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br />0- do not execute the built-in reminder process <br />1- execute the built-in reminder process |
| 4        | vibrate   | byte |           | 0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x6A)At Home/ Out of Home alert settings:id=0x6A

**Function description**

When a “return home/out of home” event is detected, the device will:

+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br/>0- At home/ Out of home alert is disabled<br/>1- At home/ Out of home alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x6B)Sedentary/Fatigue alert setting:id=0x6B

**Function description**

When detecting a sedentary/fatigue event, the device will:


+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br/>0- Sedentary/ fatigue alert is disabled<br/>1- Sedentary/ fatigue alert is enabled |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

---

### (0x6C)FindMe Reminder Execute settings :id=0x6C

**Function description**

When device search request is received, the device will:

+ Activate sound, light, and electrical alerts

**Write request/read response data format**

| Byte No. | Parameter     | Type   | Converter | Description                                                  |
| -------- | ------------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length        | byte   |           | length                                                       |
| 2        | key           | byte   |           | key                                                          |
| 3-4      | cancelSource  | byte   |           | alert can be canceled by:<br />bit0: SOS button， bit1: button1， bit2: button2, bit3: button3 <br /> bit8: touchpanel |
| 5        | screenLedMode | byte   |           | Light indication method, <br />0-No light indication<br />1-Light indication (flashing) |
| 6        | vibrate       | byte   |           | Vibration alert, <br />0-No vibration<br />others-Built-in vibration mode |
| 7        | voicePrompt   | byte   |           | Audio prompt, <br />0-no prompt<br />others-specify audio sequence number |
| 8        | voiceVolume   | byte   |           | Audio prompt volume, range:[0, 100]                          |
| 9-10     | duration      | ushort |           | alert duration, unit: s                                      |

---

### (0x6D)Welfare Alert Setting :id=0x6D

**Function description**

When a welfare check-in or check-out is detected, the device will:

+ Activate sound, light, and electrical alerts
+ Upload events to the server
+ Call emergency contacts
+ Send text message to contact

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable<br/>0- do not enable welfare alert<br/>1- enable welfare alert |
| 4        | vibrate   | byte |           | vibrate<br />0- do not vibrate <br />1- built-in vibration reminder method |
| 5        | sound     | byte |           | sound<br />0- do not remind <br />others- specify the sound index <br />255- default sound index |
| 6        | notify    | byte | bits      | notify<br />bit0- 0: do not notify the server; 1: notify the server<br />bit1- 0: do not send SMS; 1: send SMS to emergency contacts<br />bit2- 0: do not call; 1: call emergency contacts |
| 7-10     | smsAllow  | uint | bits      | smsAllow<br />bit0: 0- do not send; 1- allow sending to emergency contact 1<br />bit1: 0- do not send; 1- allow sending to emergency contact 2<br />...<br />bit9: 0- do not send; 1- allow sending to emergency contact 10 |
| 11-14    | callAllow | uint | bits      | callAllow<br />bit0: 0- do not call; 1- allow calling emergency contact 1<br />bit1: 0- do not call; 1- allow calling emergency contact 2<br />...<br />bit9: 0- do not call; 1- allow calling emergency contact 10 |

?> 1. Since there are multiple events for welfare, additional configuration may be required for other events .

---

### (0x90) Emergency contact list :id=0x90

**Function description**

The emergency contact list is only for phone calls or text messages actively initiated by the device in case of an emergency. On devices with a display screen, it is not allowed to directly select and dial numbers from this list.

**Write request/read response data format**

| Byte No. | Parameter     | Type   | Converter | Description                                                  |
| -------- | ------------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length        | byte   |           | length                                                       |
| 2        | key           | byte   |           | key                                                          |
| 3        | index         | byte   |           | no 序号                                                      |
| 4-7      | talkTimeLimit | byte   |           | Call duration limit, unit: s, 0 indicates that call duration is unlimit. |
| 8        | micGain       | byte   |           | MIC gain, range : [0, 100]                                   |
| 9        | speakerVolume | byte   |           | Speaker volume, range : [0, 100]                             |
| #string1 | name          | String | utf8      | Name string length:[0, 32]                                   |
| #string2 | number        | String | ascii     | number string, Encode: ASCII, length: [0, 32]                |
| #string3 | imageUrl      | String | utf8      | image url/fullname, [0, 64]                                  |

---

### (0x91)Regular contact list :id=0x91

**Function description**

Regular contact list, where users are allowed to select and dial contacts on devices with a display.

**Write request/read response data format**

| Byte No. | Parameter     | Type   | Converter | Description                                                  |
| -------- | ------------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length        | byte   |           | length                                                       |
| 2        | key           | byte   |           | key                                                          |
| 3        | index         | byte   |           | no 序号                                                      |
| 4-7      | talkTimeLimit | byte   |           | Call duration limit, unit: s, 0 indicates that call duration is unlimit. |
| 8        | micGain       | byte   |           | MIC gain, range : [0, 100]                                   |
| 9        | speakerVolume | byte   |           | Speaker volume, range : [0, 100]                             |
| #string1 | name          | String | utf8      | Name string length:[0, 32]                                   |
| #string2 | number        | String | ascii     | number string, Encode: ASCII, length: [0, 32]                |
| #string3 | imageUrl      | String | utf8      | image url/fullname, [0, 64]                                  |

---

### (0x92)incoming call setting :id=0x92

**Function description**

When a call comes in, the device can be set with:

+ Answer automatically
+ Answer mode
+ Hang up mode
+ Enable whitelist
+ Alert mode

**Write request/read response data format**

| Byte No. | Parameter        | Type | Converter        | Description                                                                                                                                               |
| -------- | ---------------- | ---- | ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1        | length           | byte |                  | length                                                                                                                                                    |
| 2        | key              | byte |                  | key                                                                                                                                                       |
| 3        | autoAnswer       | byte |                  | enable auto answer<br/>0-Disable auto answer<br/>1-Enable auto answer                         |
| 4        | autoAnswerDelay  | byte |                  | auto answer after rings, unit: s                                                                        |
| 5        | answerMethod     | byte |                  | answer method:<br />bit0: button1<br />bit1: button12<br />bit2:button13<br />bit3:button3 <br />bit4:touchpanel |
| 6        | hangupMethod     | byte |                  | hangup/reject method<br />bit0: button1<br />bit1: button2<br />bit2:button3<br />bit3:button4 <br />bit4:touchpanel                                               |
| 7        | whitelistMode    | byte |                  | Enable call whitelist<br />0- all incoming calls will be notified<br />1- Only numbers in the emergency contact list are allowed. <br />2- Only numbers in the contact list are allowed.<br />3- Only emergency contacts & contact list allowed |
| 8        | vibration        | byte |                  | vibration:<br />0: no vibration<br />others: select vibration mode |
| 9        | ringtone         | byte |                  | ringtone:<br />0: default ringtone<br />others: select ringtone |
| 10-13    | emegencyContacts | uint |                  | List of emergency contacts allowed to receive incoming call alerts<br />bit0: emergency contact 1 <br />bit1: emergency contact 2 ... <br />bit9: emergency contact 10 |
| 14-17    | normalContacts   | uint |                   | List of contacts allowed to receive incoming call alerts<br />bit0: contact 1 <br />bit1: contact 2 ... <br />bit9: contact 10 |

                                                                                                                                                     |

Note


1. Answering or hanging up is determined based on the reserved codes that are bound with click/ short press/ slide to
   answer etc. .

---

### (0x95)DTMF enable :id=0x95

**Function description**

During a call, the remote end can send key commands to the device via virtual dial keys using the DTMF function.

**Write request/read response data format**

| Byte No. | Parameter            | Type | Converter | Description                                                  |
| -------- | -------------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length               | byte |           | length                                                       |
| 2        | key                  | byte |           | key                                                          |
| 3        | enable               | byte |           | enable:<br />0: close<br />1: only emergency contacts are allowed<br />2: only normal contacts are allowed<br />3: only contacts are allowed<br />255: all calls are allowed |
| 4-7      | emergencyContactMask | uint |           | Emergency contacts who can use this feature<br />bit0: emergency contact 1<br />bit1: emergency contact 2 <br />... <br />bit9: emergency contact 10 |
| 8-11     | contactMask          | uint |           | Normal contacts who can use this feature<br />bit0: contact 1<br />bit1: contact 2 <br />... <br />bit9: contact 10 |


---

### (0x96)DTMF key's function :id=0x96

**Function description**

Customize the function of the `DTMF` virtual keyboard.


**Write request/read response data format**

| Byte No. | Parameter   | Type  | Converter | Description                                                  |
| -------- | ----------- | ----- | --------- | ------------------------------------------------------------ |
| 1        | length      | byte  |           | length                                                       |
| 2        | key         | byte  |           | key                                                          |
| 3        | index       | byte  |           | index, range:[0,9]                                           |
| 4        | action      | byte  |           | task/action<br />0:no action<br />1.speaker's volume up<br />2:speaker's volume down<br />3:mute on<br />4: mute off<br />5:mic volume up<br />6:mic volume down<br />7:end call<br />8:cancel SOS alert |
| 5-n      | keySequence | array |           | key/combine, 0/1/2/3/4/5/6/7/8/9/*/#                         |

---

### (0x97)Permission of SMS command :id=0x97

**Function description**

SMS commands allow you to set or query device configurations by sending text messages to the device.

The current command is used to set SMS command permissions and filter SMS commands sent from untrusted numbers.

**Write request/read response data format**

| Byte No. | Parameter            | Type | Converter | Description                                                  |
| -------- | -------------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length               | byte |           | length                                                       |
| 2        | key                  | byte |           | key                                                          |
| 3        | permissionLevel      | byte |           | permission level:<br />0: close<br />1: only emergency contacts are allowed<br />2: only normal contacts are allowed<br />3: only emergency contacts and normal contacts are allowed |
| 4-7      | emergencyContactMask | int  |           | specific emergency contacts:<br />bit0: emergency contact 1<br />bit1: emergency contact 2 <br />... <br />bit9: emergency contact 10 |
| 8-11     | contactMask          | int  |           | specific normal contacts:<br />bit0: contact 1<br />bit1: contact 2 <br />... <br />bit9: contact 10 |


---

### (0x98)Password for SMS command :id=0x98

**Function description**

To prevent others from sending text message commands using permitted contact numbers, add text message password.

**Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                      |
| -------- | --------- | ----- | --------- | -------------------------------- |
| 1        | length    | byte  |           | length                           |
| 2        | key       | byte  |           | key                              |
| 3        | enable    | byte  | bool      | enable                           |
| 4-n      | password  | array |           | numeric password / text password |


+  numeric password

| Byte No. | Parameter       | Type | Converter | Description |
| -------- | --------------- | ---- | --------- | ----------- |
| 1        | length          | byte |           | length      |
| 2        | key             | byte |           | key         |
| 3        | enable          | byte | bool      | enable = 1  |
| 4-7      | numericPassword | int  |           | numeric password |


+ text password

| Byte No. | Parameter    | Type   | Converter | Description |
| -------- | ------------ | ------ | --------- | ----------- |
| 1        | length       | byte   |           | length      |
| 2        | key          | byte   |           | key         |
| 3        | enable       | byte   | bool      | enable = 2  |
| 4-9      | textPassword | String | utf8      | text password |


---

### (0x9B)GPS location URL for format :id=0x9B

**Function description**

GPS positioning link — a string used to generate the formatted link, enabling users to click and open the map to view the location.

**Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description |
| -------- | --------- | ------ | --------- | ----------- |
| 1        | length    | byte   |           | length      |
| 2        | key       | byte   |           | key         |
| #string1 | urlFormat | String | utf8      | URL format  |

---

### (0x9C)WiFi&LBS location URL for format :id=0x9C

**Function description **

Generates a location link based on WiFi or LBS positioning data.

**Write request/read response data format**

| Byte No. | Parameter | Type   | Converter | Description              |
| -------- | --------- | ------ | --------- | ------------------------ |
| 1        | length    | byte   |           | length                   |
| 2        | key       | byte   |           | key                      |
| #string  | urlFormat | String | utf8      | URL format, Type: String |

---

### (0xB0)AGPS 的使用 AGPS's usage :id=0xB0

**Function description**

During use, GPS modules can speed up positioning time and improve positioning accuracy by updating positioning assistance information.

**Write request/read response data format**

| Byte No. | Parameter | Type  | Converter | Description                                                                                            |
| -------- | --------- | ----- | --------- | ------------------------------------------------------------------------------------------------------ |
| 1        | length    | byte  |           | length                                                                                                 |
| 2        | key       | byte  |           | key                                                                                                    |
| 3        | enable    | byte  | bool      | enable:<br />0: close<br />1: open<br />2: update reference position when GPS position is updated |
| 4-7     | lat       | float | loc       | Reference latitude                                                                                             |
| 8-11      | lng       | float | loc       | Reference longitude                                                                                        |



Note：

1. Reference latitude and longitude can provide more accurate auxiliary data more quickly when requesting auxiliary data from the server.

---

### (0xB1) Continuous Positioning Configuration :id=0xB1

**Function description**

Continuous positioning refers to the device performing positioning functions over a continuous period of time until the preset working duration is reached. Configures the device's continuous positioning mode. In this mode, the device repeatedly attempts to acquire location fixes for a predefined total duration.

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3-6      | duration  | int  |           | duration, unit: second                                       |
| 7-10     | interval  | int  |           | interval, unit: second                                       |
| 11-13    | timeout   | int  |           | timeout,unit: second <br />Location timeout refers to a situation where location information has not been updated beyond a specified time limit, which is considered a location timeout, resulting in the premature termination of location tracking. |


---

### (0xB3) BLE Scan schedule :id=0xB3

**Function description**

BLE periodic scanning, commonly used to detect when someone is away from home or at home. You can set a schedule for BLE scanning.

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description              |
| -------- | --------- | ---- | --------- | ------------------------ |
| 1        | length    | byte |           | length                   |
| 2        | key       | byte |           | key                      |
| 3-6      | scanTime  | int  |           | scan time, unit: second  |
| 7-10     | sleepTime | int  |           | sleep time, unit: second |

---

### (0xB8)BLE device list with location :id=0xB8

**Function description**

BLE periodic scanning, commonly used to detect when someone is leaving from home or back home. You can set a schedule for BLE scanning.

**Write request/read response data format**

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

### (0xB9)WiFi AP list with location :id=0xB9

**Function description**

A list of BLE devices with location information. By scanning BLE to determine the number of corresponding devices and comparing other location information, the approximate location of the device can be determined.

**Write request/read response data format**

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

### (0xD0)Watchface :id=0xD0

**Function description**

Specify the watch face used by the current device.

**Write request/read response data format**

| Byte No. | Parameter     | Type   | Converter | Description                 |
| -------- | ------------- | ------ | --------- | --------------------------- |
| 1        | length        | byte   |           | length                      |
| 2        | key           | byte   |           | key                         |
| #string1 | watchFaceName | String | utf8      | name, length: [1, 32] |



---

### (0xD3) Screen setting :id=0xD3

**Function description**

Set the operating parameters for the display.

**Write request/read response data format**

| Byte No. | Parameter      | Type | Converter | Description                                                  |
| -------- | -------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length         | byte |           | length                                                       |
| 2        | key            | byte |           | key                                                          |
| 3        | autoOffTimeout | byte |           | Auto-screen-off duration, unit: seconds, range:[3, 240]      |
| 4        | brightness     | byte |           | Screen brightness, range: [0, 100]                           |
| 5        | touchWakeUp    | byte |           | Touch to wake up the screen<br />0:disable <br />1：touch <br />2:long press(1s) |
| 6        | buttonWakeUp   | byte |           | Press button to turn on screen<br />0:disable <b r/>1:click <br />2:long press(1s) |
| 7        | wristWakeUp    | byte |           | Flip wrist to wake screen<br />0:disable<br />1:enable       |

---

### (0xD4)Navigation Page Settings:id=0xD4

**Function description**

Devices with display screens can set the visibility of the primary interface(navigation page).


**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3-6      | hideMask  | int  |           | bit=0 is visible ，bit=1 is hide<br />bit0：step; <br />bit1:heartrate; <br />bit2: bloodoxygen; <br />bit3:emergency contacts; <br />bit4: contacts; <br />bit5: weather; <br />bit6: alarmclock; <br />bit7: reminder; <br />bit8: settings |

---

### (0xD5)Home page settings :id=0xD5

**Function description**

The watch face interface, also known as the Home Page, automatically returns to the Home Page when the screen is turned on again after exceeding the preset inactivity duration. Auto-return to Home Page: This function configures the device to automatically return to the Home Page when the screen wakes, provided the screen has been inactive for longer than a predefined duration.

Inactivity refers to the length of time after the screen has been turned off.

**Write request/read response data format**

| Byte No. | Parameter       | Type | Converter | Description                    |
| -------- | --------------- | ---- | --------- | ------------------------------ |
| 1        | length          | byte |           | length                         |
| 2        | key             | byte |           | key                            |
| 3        | enableHomePage  | byte |           | enable                         |
| 4-7      | homePageTimeout | int  |           | inactive time screen , unit: s, range: [5, 240] |


---

### (0xD6)Watchface Element Settings:id=0xD6

**Function description**

The standard Watchface displays a variety of data elements, and you can set the Watchface to display the data types you want.

**Write request/read response data format**

| Byte No. | Parameter   | Type | Converter | Description                                                  |
| -------- | ----------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length      | byte |           | length                                                       |
| 2        | key         | byte |           | key                                                          |
| 3-6      | elementMask | int  |           | data types, bit=0 is hide, bit=1 is show <br />bit0: step;<br />bit1: heartrate;<br />bit2: bloodoxygen;<br />bit3: weather; |


Note:

1. The watch face elements are effective only in the system watch face. Such configuration changes will not take effect in custom watch face elements.

---

### (0xE0)Alarm clock list :id=0xE0

**Function description**

Set a specific time to remind users.

**Write request/read response data format**

| Byte No. | Parameter   | Type   | Converter | Description                                                  |
| -------- | ----------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length      | byte   |           | length                                                       |
| 2        | key         | byte   |           | key                                                          |
| 3        | alarmIndex  | byte   |           | index                                                        |
| 4        | enable      | byte   | bool      | enable<br />0- disable <br />1- enable                       |
| 5        | alarmType   | byte   |           | type<br />0- alarmclock<br />1- medicine<br />2- exercise    |
| 6        | hour        | byte   |           | hour  range:[0, 23]                                          |
| 7        | minute      | byte   |           | minute ， range:[0, 59]                                      |
| 8        | repeatMask  | byte   | bits      | repeat week, bit=0 is disable, bit=1 is enable <br />bit0: monday; <br />bit1: tuesday; <br />bit2: wednesday; <br />bit3: thursday; <br />bit4: friday; <br />bit5: saturday; <br />bit6: sunday |
| 9        | vibrateMode | byte   |           | vibrate<br />0- disable <br />others- enable                 |
| 10       | soundMode   | byte   |           | audio prompt<br />0- disable <br />others: select audio file |
| 11       | duration    | byte   |           | duration, unit: seconds, range:[5, 120]                      |
| 12-n     | description | String | utf8      | description: length:[0, 32]                                  |

Note

1. After executing a One-time alarm, disable the enable bit, save the parameters, and synchronously update the relevant working parameters (such as the alarm timestamp).
2. The reminder duration does not need to be strictly enforced during execution. Instead, it can be determined whether the reminder duration is met after one reminder action cycle. That is, set the actual reminder duration longer than the alarm duration, to avoid interruptions such as audio playback interruptions that may affect the user's experience.


---

### (0xE1)Periodic heart rate measurement :id=0xE1

**Function description**

Periodic heart rate measurement: device automatically measures heart rate at periodic intervals and records each
measurement.

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable periodic measurement <br />0- disable<br />1- enabled |
| 4-7      | interval  | int  |           | interval, unit: second, range:[180=3mins, 60x60x8=8 hours]   |


Note

1. The timing of periodic measurements is not exact. Environmental differences can delay a reading, so a result is not guaranteed within the scheduled minute/second.2. Periodic measurement: Since the measurement environment may vary across different time periods, the results cannot be guaranteed to be obtained within the exact same minute.
2. On charging: The periodic measurement function will not execute when the device is charging.
3. Unwear status:The device will not perform a scheduled measurement if it is in an un-worn state.

---

### (0xE2)Periodic blood oxygen measurement :id=0xE2

**Function description**

Periodically measure blood oxygen and record the measurement results. Periodic blood oxygen measurement: device automatically measures blood oxygen at periodic intervals and records each measurement.


**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable periodic measurement <br />0- disable<br />1- enabled |
| 4-7      | interval  | int  |           | timed measurement interval, unit: s, range: [180, 28800] (3mins,8<br/>hours) |

Note

1. Periodic measurement: Since the measurement environment may vary across different time periods, the results cannot be guaranteed to be obtained within the exact same minute.
2. The timing of periodic measurements is not exact. Environmental differences can delay a reading, so a result is not guaranteed within the scheduled minute/seconds.
3. On charging: The periodic measurement function will not execute when the device is charging.
4. Un-worn status: The device will not perform a scheduled measurement if it is in an un-worn state.

---

### (0xE3)Periodic temperature measurement :id=0xE3

**Function description**

Periodically measure the temperature and record the measurement results. Periodic temperature measurement: device automatically measures temperature at periodic intervals and records each measurement.


**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description                                                  |
| -------- | --------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length    | byte |           | length                                                       |
| 2        | key       | byte |           | key                                                          |
| 3        | enable    | byte | bool      | enable periodic measurement <br />0- disable<br />1- enabled |
| 4-7      | interval  | int  |           | interval, unit: second, range:[180=3mins, 60x60x8=8 hours]   |

---

### (0xE4) Weather function configuration :id=0xE4


**Function description**

Set the working parameters of the weather app.

**Write request/read response data format**

| Byte No. | Parameter            | Type | Converter | Description                                    |
| -------- | -------------------- | ---- | --------- | ---------------------------------------------- |
| 1        | length               | byte |           | length                                         |
| 2        | key                  | byte |           | key                                            |
| 3        | enable               | byte | bool      | enable weather function <br />0- disable<br />1- enabled   |
| 4        | tempUnit             | byte |           | temperature unit <br />0: Celsius <br />1: Fahrenheit          |
| 5-8      | autoUpdateInterval   | int  |           | weather auto update interval, unit: s                      |
| 9-12     | manualUpdateInterval | int  |           | weather manual update interval, unit: s                       |




---

### (0xE5)Third-party accessory integration configuration :id=0xE5

**Function description**

The Third-party accessory could be configured to be open or closed after it's integrated into Eview devices.

The third-party accessory must be already adapted to the current device.


**Write request/read response data format**

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

### (0xE6)Periodic data reporting :id=0xE6

**Function description**

Set the device to regularly upload the measured health data to the server.

**Write request/read response data format**

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

1. When `motionUploadInterval` is not 0, it means that the activity data reporting function is enabled, and the data reporting interval will override uploadInterval.
2. When `staticUploadInterval` is not 0, it means that the static data reporting function is enabled, and the data reporting interval will override `uploadInterval`.
3. When both `motionUploadInterval` and `staticUploadInterval` are 0, the data reporting interval will use `uploadInterval`.
4. When the enable type is set to `2`, `uploadInterval` indicates fixed hourly and minute reporting. For example, when set to 5 minutes, data is uploaded at xx:00/xx:05/xx:10/xx:15/xx:20/xx:25/xx:30/xx:35/xx:40/xx:45/xx:50/xx:55 (where xx represents the hour). The same applies to other cases.

---

### (0xE8)Initialize mileage :id=0xE8

**Function description**

Used to record the total distance traveled by the device during normal use.

**Write request/read response data format**

| Byte No. | Parameter | Type | Converter | Description      |
| -------- | --------- | ---- | --------- | ---------------- |
| 1        | length    | byte |           | length           |
| 2        | key       | byte |           | key              |
| 3-6      | mileage   | int  |           | mileage, unit: m |




---

### (0xE9)Step counting function configuration:id=0xE9

**Function description**

Used to set parameters related to the step counting function.

**Write request/read response data format**

| Byte No. | Parameter          | Type | Converter | Description                                                  |
| -------- | ------------------ | ---- | --------- | ------------------------------------------------------------ |
| 1        | length             | byte |           | length                                                       |
| 2        | key                | byte |           | key                                                          |
| 3        | enable             | byte | bool      | step function enable<br />0:disable<br />1:enabled           |
| 4-7      | targetStepDaily    | byte |           | target step daily<br />0:disable<br />target steps:[1_000, 100_000] |
| 8-11     | targetCalorieDaily | int  |           | target calorie burned daily<br />0:disable<br />target:[10, 999] |

Note

1. Disabling the step counting feature only prevents the data from being uploaded; the device continues to recordsteps internally, while the UI displays the count as 0.
2. Step count data will only be uploaded when the step count function is enabled.

---

### (0xF0)Read key :id=0xF0

**Function description**

Used to read other key information or configuration parameters.

**Write request data format**

| Byte No. | Parameter | Type  | Converter | Description       |
| -------- | --------- | ----- | --------- | ----------------- |
| 1        | length    | byte  |           | length            |
| 2        | readkey   | byte  |           | readkey           |
| 3-n      | keys      | array |           | keys list to read |

Note

1. If `readkey` is not followed by any parameters, it means to read all parameters. However, due to memory limitations, only the first part of the data will be uploaded.

### (0xF1)read key range :id=0xF1

**Function description**

Given two `keys`, read the list of `keys` or configuration information that exists between the two `keys`.

**Write request data format**

| Byte No. | Parameter | Type | Converter | Description |
| -------- | --------- | ---- | --------- | ----------- |
| 1        | length    | byte |           | length      |
| 2        | readkey   | byte |           | readkey     |
| 3        | keystart  | byte |           | key start   |
| 4        | keyend    | byte |           | key end     |

Note

1. Specify parameters to read between `keystart` and `keyend`.
2. Adjust the parameters of `keystart` and `keyend` within the program.
3. If `keystart` > `keyend`, the program automatically swaps the values of `keystart` and `keyend`.
4. If the parameters between `keystart` and `keyend` exceed the maximum length of `packet`, only the preceding parameters will be replied.
5. Used to read device configuration information in segments.


