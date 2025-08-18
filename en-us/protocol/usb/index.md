
# USB 协议

USB协议当前使用自定义`USB HID`设备来进行数据交互。由于每个`packet`固定63个字节，为简化使用，定义一个简单的协议快速处理数据。


## 数据结构

| byte0 | byte1-2 | byte3 | byte4-62 |
| --- | --- | --- | --- |
| command | crc16 | len | payload |

* command: 用于识别当前`packet`来内容类别
* crc16: 有效payload的校验
* len: 有效payload的字节数据
* payload: 内容


## 命令

| type | value(hex) | description |
| --- | --- | --- |
| 通用协议 | 0x01 | 用于封装通用协议完整`packet` |
| shell脚本 | 0x03 | |
| log配置 | 0x05 | 用于约束`log`的行为 |
| log输出 | 0x06 | `log`内容输出 |
| GSM AT发送 | 0x10 | 向GSM模块发送AT指令-调试使用 |
| GSM AT接收 | 0x11 | 模拟GSM模拟接收AT指令-调试使用 |


## 功能说明

---
### 通用协议

在通用协议定义了完整的协议功能，如果需要在`USB`协议中使用通用协议，则需要进行封包处理。

比如我们有一个`general-protocol-packet`, 称为`gppkt`, 它的总长度为`[8,65543]`, 而`usb`协议中每个`packet`的`payload`只有`63`字节，因此，我们需要对`gppkt`进行分包处理。

设备发送时：
> 1st: `0x01 crc 60 gppkt[0, 62]`<br>
> 2nd: `0x01 crc 60 gppkt[63, 126]`<br>
> 3rd: `0x01 crc 60 gppkt[127, 189]`<br>
> 4th: `0x01 crc 60 gppkt[190, 253]`<br>
> 5th: `0x01 crc 60 gppkt[254, 317]`<br>
> 6th: `0x01 crc 60 gppkt[318, 381]`<br>
> ....

上位机接收时:

> 1st: `0x01 crc 60 gppkt[0, 62]`<br>
> 2nd: `0x01 crc 60 gppkt[63, 126]`<br>
> xxx: `0x06 crc 60 [log content]`<br>
> 3rd: `0x01 crc 60 gppkt[127, 189]`<br>
> 4th: `0x01 crc 60 gppkt[190, 253]`<br>
> 5th: `0x01 crc 60 gppkt[254, 317]`<br>
> 6th: `0x01 crc 60 gppkt[318, 381]`<br>
理论上，同一个类型数据。最理想的情况下，每个类型的数据按顺序发送，中间没有其他数据。但实际使用过程不可避免地夹带其他类型数据，对上位机而言，需要将同一类型数据缓存。在接收到足够的数据后再进行完整校验及处理。

---
### 日志配置

用于修改日志输出打印，`关闭/打开`日志类型。

---
### 日志输出

用于日志输出。日志数据格式，查看`通用协议`中的日志数据格式描述。

---
### GSM AT发送

在调试阶段，用于手动向GSM模块手动发送AT命令。

---
### GSM AT接收

在调试阶段，用于手动向`GSMLib`模拟数据接收，用于测试`GSMlib`库的解析处理。

---
### shell脚本

用于向设备端发送`shell`脚本命令，回复`shell`脚本处理结果。
`shell`脚本的数据格式查看`项目规范`。

