# Question & Answer

本文档用于解释 `v1` 版本协议问题。不定期整理。

---

## 问题列表

[Q1: **AudioPrompt**: `v0-configuration`中key=0x19的功能，如何关闭/打开指定音频提示?](#_0x01)

[Q2: **UploadServer**: 在 `v0-configuration`中，`server`有 `GPRS`开关作用，在`v1`版本如何关闭GSM的GPRS移动数据功能，禁止设备使用GPRS连接服务器呢?](#_0x02)

[Q3: **Positive Response**: 当设备使用`indicate=1`通知数据时，主机或者服务器如何回复?](#_0x03)

[Q4: **API Version**: 由于协议版本不断迭代，如何在旧版本上追加功能?](#_0x04)

---

## :id=0x01 Q1:`v0-configuration`中key=0x19的功能，如何关闭/打开指定音频提示?

> 在当前协议版本中，音频提示是否打开由提醒功能执行配置决定，即每个提醒功能都有单独配置来决定执行过程中的提醒方式。
>
> 对应key:[0x60, 0x6C]，这些提醒执行过程包含了所有提醒逻辑设置。

---

## :id=0x02 Q2:在 `v0-configuration`中，`server`有 `GPRS`开关。`v1`版本如何关闭GSM的GPRS移动数据功能，禁止设备使用GPRS连接服务器呢? :id=0x02

> 当前为 `GSM` 移动数据单独提供开关，`server`配置允许多个，以应对后续不同需求，比如允许配置 ` main server`, `log server`, `ota server`等。

## :id=0x03 Q3: 当设备使用`indicate=1`通知数据时，主机或者服务器如何回复?

> 当设备使用`indicate=1`通知数据时,表示当前数据包要求主机必须使用正响应来进行回复。需要满足:
> 1. Header->Flag.indicate=1 
> 2. Payload->Data长度为1
> Example: 
> <br />Device-> AB 10 xx xx xx xx xx xx CMD LKV(1) LKV(2) LKV(3)
> <br />Host-> AB 10 01 xx xx xx xx xx xx CMD


## :id=0x04 Q4: 由于协议版本不断迭代，如何在旧版本上追加功能?

> 由于协议版本不断迭代，为了不影响旧版本的设备正常使用，新增功能需要在旧版本上追加。
> 1. 新增功能的`key`值，需要在旧版本中不存在。
> 2. 需要在旧协议版本上增加功能，在对应的`release`版本文命令档上增加功能，同`git`版本分支管理策略。 


