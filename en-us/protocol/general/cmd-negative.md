# 负响应 Negative Response(0x7F)

## 命令说明

`负响应`-negative response，否定回答。即设备出于一系列原因，无法正确执行命令时需要返回的一种回复，通知 `主机发送的命令`未成功执行的原因。

在[📄响应篇](response.md)中已经说明，正响应有固定的回复方式，请务必注意。

## key列表

| Value(HEX) | Parameter               | 功能描述               | 可写 | 可读 | 通知 |
| ---------- | ----------------------- | ---------------------- | ---- | ---- | ---- |
| 0x00       | success                 | Success                | ❌   | ❌   | ❌   |
| 0x01       | lengthError             | 无效的长度             | ❌   | ❌   | ✅   |
| 0x02       | dataFormatInvalid       | 无效的格式             | ❌   | ❌   | ✅   |
| 0x03       | invalidDataSize         | 错误的数据大小         | ❌   | ❌   | ✅   |
| 0x04       | insufficientResource    | 资源不足以执行当前命令 | ❌   | ❌   | ✅   |
| 0x05       | subFunctionNotSupported | 子功能不支持           | ❌   | ❌   | ✅   |
| 0x06       | noMemory                | 内存不足               | ❌   | ❌   | ✅   |
| 0x07       | invalidAddressResponse  | 错误的地址回复         | ❌   | ❌   | ✅   |
| 0x08       | keyInvalid              | 无效的密钥             | ❌   | ❌   | ✅   |
| 0x09       | delayNotMet             | 延时未达到预期时长     | ❌   | ❌   | ✅   |
| 0x0A       | invalidState            | 无效的状态             | ❌   | ❌   | ✅   |
| 0x0B       | invalidParameter        | 无效的参数             | ❌   | ❌   | ✅   |
| 0x0C       | busy                    | 忙碌                   | ❌   | ❌   | ✅   |
| 0x0D       | peripheralNotSupported  | 不支持的外设操作       | ❌   | ❌   | ✅   |
| 0x0E       | programmingError        | 编程错误               | ❌   | ❌   | ✅   |
| 0x0F       | sensorNotReady          | 传感器未就绪           | ❌   | ❌   | ✅   |
| 0x10       | invalidState            | 无效的状态             | ❌   | ❌   | ✅   |
| 0x20       | dfu                     | DFU命令负响应          | ❌   | ❌   | ✅   |
| 0x21       | file                    | 文件命令负响应         | ❌   | ❌   | ✅   |
| 0xF8-0xFE  | RFU                     | 内部使用(RFU)          | ❌   | ❌   | ❌   |
| 0xFF       | unknownError            | 未知错误               | ❌   | ❌   | ✅   |

Note:

1. 当前的 `key`列表不代表功能，而代表负响应的 `类型`。
2. `0x00- Success` 在本协议中不再出现，正响应有对应的回复方式。
3. 本章中的 `len-key-value`, `value`作为对 `key`的扩展补充说明。
4. 常用功能尽量使用单个 `key`来直接回复失败原因。
5. 不常用的功能，则尽可能使用扩展 `key`来回复失败原因。

---

## 类型说明

---

### DFU命令负响应

**负响应描述**

当前负响应代码，专用于 `DFU`命令下发操作时，回复的命令失败原因。

**通知数据格式**

| Byte No. | Parameter | Type | Converter | Description          |
| -------- | --------- | ---- | --------- | -------------------- |
| 1        | length    | byte |           | length               |
| 2        | key       | byte |           | key                  |
| 3        | code      | byte |           | 扩展代码，见下面列表 |

* 扩展代码列表
  *未说明的代码未使用*

| code | Parameter            | 描述               | 处理方式                                                                                                                             |
| ---- | -------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| 0x01 | invalidLength        | 无效的数据长度     | 在写入固件内容时, 必须以 `4字节`对齐，即内容的字节数必须是 `4`的整数倍                                                           |
| 0x02 |                      |                    |                                                                                                                                      |
| 0x03 |                      |                    |                                                                                                                                      |
| 0x04 | insufficientResource | 资源不足           | 设备使用动态内存来存储部分数据，当数据发送过快，之前的工作未执行完毕导致没有更多的内存使用。主机需要延时一段时间后再重新发送当前命令 |
| 0x05 |                      |                    |                                                                                                                                      |
| 0x06 | noMemory             | 内存不足           | 同 `0x04`原因与操作                                                                                                                |
| 0x0A | invalidState         | 无效的状态         | 文件 `key`的操作时序不对，按照 `DFU`命令顺序要求进行操作                                                                         |
| 0x0B | invalidParameter     | 无效的参数         | 初始化包内容参数不对，无法识别到有效的参数数据                                                                                       |
| 0x0C |                      |                    |                                                                                                                                      |
| 0x0D | invalidAddress       | 错误的地址         | 在写固件内容时，偏移地址不一致，需要按照文件顺序写入内容                                                                             |
| 0x0E | internalError        | 内部错误           | 函数入口参数为 `NULL`                                                                                                              |
| 0x0F |                      |                    |                                                                                                                                      |
| 0x11 | versionLow           | 固件版本低         | 不允许更新 `低版本的固件`。需要重新提供固件                                                                                        |
| 0x12 | signatureInvalid     | 签名校验失败       | 初始化数据包签名未通过，检查 `crypto-lib`是否一致                                                                                  |
| 0x13 | fileSizeInvalid      | 文件大小不符合要求 | 当前待升级的文件过小或过大, 异常的文件升级包                                                                                         |
| 0x14 | fileCrcError         | 文件CRC错误        | 文件内容与初始化包中的文件 `CRC`描述不一致                                                                                         |
| 0x15 | timeout              | 超时               | 程序执行当前命令超时，前置工作未完成，请重发此命令                                                                                   |
| 0x16 | fileTypeUnsupported  | 文件类型不支持     | 当前固件未支持此类型的文件更新，请检查生成包是否有误                                                                                 |
| 0x17 | modelError           | 模型错误           | 当前升级包非当前设备程序所支持的, 无法兼容                                                                                           |
| 0x18 | customFlagInvalid    | 自定义标志无效     | 当前固件包自定义标志 与 固件所要求的不一致，无法兼容, 检查固件包                                                                     |

---

### 文件命令负响应

**负响应描述**

当前负响应代码，专用于 `File`命令下发操作时，回复的命令失败原因。

**通知数据格式**

| Byte No. | Parameter | Type | Converter | Description          |
| -------- | --------- | ---- | --------- | -------------------- |
| 1        | length    | byte |           | length               |
| 2        | key       | byte |           | key                  |
| 3        | code      | byte |           | 扩展代码，见下面列表 |

* 扩展代码列表

| code | Parameter             | 描述                       | 处理方式                                                         |
| ---- | --------------------- | -------------------------- | ---------------------------------------------------------------- |
| 0x01 | fileNotOpen           | 文件未打开                 | 操作文件时，需要先打开文件                                       |
| 0x02 | threadOccupied        | 线程被占用                 | 已有其他文件打开，需要等待上一个文件关闭后才能打开新的文件操作   |
| 0x03 | fileTypeUnsupported   | 文件类型不支持             | 当前的文件类型在固件中不支持                                     |
| 0x04 | fileTypeNotFound      | 未发现文件类型             | 检查命令中文件类型是否正确                                       |
| 0x05 | ioNotImplemented      | 当前文件类型未实现IO操作   | 协议规定的文件类型，但是当前程序上未找到它的实现接口             |
| 0x06 | ioBusy                | IO忙碌                     | 已打开同一类型的文件                                             |
| 0x07 | processing            | 正在处理                   | 正在处理，延时重试                                               |
| 0x08 | noWritePermission     | 无写入操作权限             | 当前文件类型未申请写入权限                                       |
| 0x09 | writeOffsetError      | 写入偏移地址错误           | 文件内容未按顺序写入                                             |
| 0x0A | insufficientResource  | 资源不足                   | 程序没有足够的内存来保存当前写入内容，主机延时再次操作           |
| 0x0B | fileSizeExceeded      | 文件超出大小               | 写入的文件超过了参数中的大小，检查是参数传错误，还是写入过程出错 |
| 0x0C | operationNotSupported | 操作不支持                 | 当前文件类型不支持此操作                                         |
| 0x0D | moduleNotOpen         | 模块未打开，无法执行操作   | 部分文件写入外部模块中，操作前需要先让模块正常工作               |
| 0x10 | modeLengthError       | 文件参数- 模式长度错误     | 检查命令格式是否正确                                             |
| 0x11 | typeLengthError       | 文件参数- 类型参数长度错误 | 检查命令格式是否正确                                             |
| 0x12 | fileSizeLengthError   | 文件参数- 文件大小长度错误 | 检查命令格式是否正确                                             |
| 0x13 | fileNameLengthError   | 文件参数- 名称长度错误     | 检查命令格式是否正确                                             |
| 0x14 | filePathLengthError   | 文件参数- 路径长度错误     | 检查命令格式是否正确                                             |
| 0x15 | crc32LengthError      | 文件参数- CRC32长度错误    | 检查命令格式是否正确                                             |
| 0x16 | sum32LengthError      | 文件参数- SUM32长度错误    | 检查命令格式是否正确                                             |
| 0x17 | md5LengthError        | 文件参数- MD5长度错误      | 检查命令格式是否正确                                             |
| 0x18 | undefinedParameter    | 有未定义的参数传入         | 检查命令格式是否正确                                             |
| 0x19 | pathNameTooLong       | 路径和名称长度超过指定范围 | 检查命令中路径文件是否符合要求                                   |
| 0x1A | checkTypeUnsupported  | 不支持的校验方式           | 检查命令格式是否正确                                             |
| 0x1B | invalidFilePath       | 无效文件完整路径           | 检查命令格式是否正确, 要求填写路径+文件名参数                    |
| 0x1C | fileCheckFailed       | 文件校验失败               | 文件写入校验失败，重新写入                                       |
