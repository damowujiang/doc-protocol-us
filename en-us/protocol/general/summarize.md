# 概述 <br />Overview


协议文档是用于解释如何与设备进行通讯及交换数据。 <br />Protocol documents are used to explain how to communicate with devices and exchange data.

协议文档与设备中表现不一致的，以协议为准。<br />If there is any inconsistency between the protocol document and the device, the protocol shall prevail.

协议文档使用版本号进行管理。版本号格式为 `vx.y.z`，分别表示主版本号、次版本号、修订版本号。<br />Protocol documents are managed using version numbers. The version number format is `vx.y.z`, where `x` represents the major version number, `y` represents the minor version number, and `z` represents the revision version number.

## 约定 Conventions

+ 字节顺序，默认使用小端模式(little endian， 简称 LE)，未使用 `LE`的字节顺序文档中会标注。比如接收到的顺序为 `0x12 0x34 0x56 0x78`转为 `32bits`为 `0x78563412`。<br />Byte order: The default is little-endian (LE). If a different byte order is used, it will be noted in the documentation. For example, the received order `0x12 0x34 0x56 0x78` is converted to `32bits` as `0x78563412`.
+ 字符编码，默认使用 `utf8`进行字符编码，使用其他特殊编码时，文档中会标注。 <br />Character encoding: The default character encoding is `utf8`. If other special encodings are used, they will be noted in the documentation.
+ 时间戳，默认为 `1970.1.1 00:00:00`开始时间，类似于unix epoch/unix time。但区别于unix time使用有符号表示，`timestamp`使用的是 `无符号32bits`,可表示的年份最大为2106年。相对于有符号32bits可表示到2038年延长68年的使用时间。<br />Timestamp: The default starting time is `1970.1.1 00:00:00`, similar to the Unix epoch/Unix time. However, unlike Unix time, which uses signed representation, `timestamp` uses `unsigned 32-bit`, with the maximum year it can represent being 2106. This extends the usable time by 68 years compared to signed 32-bit, which can only represent up to 2038.

## 定义&缩写 <br />Definitions & Abbreviations

| 缩写 <br />Abbreviation  | 名词定义 <br />Definition of Term                                               |
| ---- | ------------------------------------------------------- |
| LKV  | length-key-value 协议中子命令的构成                    |
| BLE  | Bluetooth low energy 低功耗蓝牙                         |
| DFU  | Device's File update 设备文件升级 |
| RFU  | Reserved for future use 预留用于未来使用                |
| NFC  | Near field communication 近场通讯                       |
| NDEF | NFC data exchange format  NFC数据交换格式              |
| ACK  | acknowleage 响应/确认包                                 |
