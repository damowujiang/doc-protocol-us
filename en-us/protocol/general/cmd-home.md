
# 命令说明 Command Description


在前文中[📄数据结构](data_structure.md)提到，`payload`由`command`和多个`item`组成，而`item`由`length-key-value`组成。<br />As mentioned in the previous section [📄Data Structures](data_structure.md), `payload` consists of `command` and multiple `item`, while `item` consists of `length-key-value`.

本文档重点定义和规范协议的行为/属性：<br />This document focuses on defining and standardizing the behavior/attributes of the protocol:
- [x] write request
- [x] read request
- [x] notify
  
以及在后续命令章节中，我们对`key-table`中的`key`标注它所支持的操作行为。<br />In the subsequent command sections, we annotate the `key` in the `key-table` with the operations it supports.

---
## 1.1 仅命令 only command

当上位机向设备端仅发送`command`时，用于查询设备`commmand`已实现的`key-table`。<br />When the host sends only `command` to the device end, the `key-table` used to query the device `commmand` has been implemented.

!> 要求`indicate`属性为0.<br />Requires the `indicate` attribute to be 0.


在同一个命令中，由于设备能力有差异，我们可以通过`仅发送command`来查询设备代码已实现的`key-table`。
此行为我们后续称为`command-query`
<br />In the same command, due to differences in device capabilities, we can use “send command only” to query the “key-table” implemented by the device code.
We will refer to this behavior as “command-query” in the following.

```plain
TX:
AB 00 01 00 88 91 01 00 80 

RX:
AB 00 2A 00 D7 60 01 00 
80 00 00 10  11 12 20 21  22 28 29 2A  30 31 32 38   
39 3A 50 51  52 54 55 58  59 60 61 80  98 99 9A A0   
A1 A8 A9 B0  B1 B2 B3 B4  B5 F0
```

设备命令所支持`key-table` 使用 `key = 0`由传出。<br />The device command supports `key-table` using `key = 0` as the outgoing key.
* 所有命令的`key-table`中不允许定义`key = 0`的用途。<br />The use of `key = 0` is not allowed in the `key-table` of all commands.
* 上位机回复中仅携带`command`进行回复时，`indicate`属性要求不能为1, 否则与我们之前定义的正响应冲突。<br />When the host replies only with `command`, the `indicate` attribute is required to be 0, otherwise it will conflict with the positive response we defined previously.



---
## 1.2 write request

在向设备发送的`packet`中，`key`后面跟随`参数`，此行为我们称为 `写请求`。In the `packet` sent to the device, the `key` is followed by `parameters`. We refer to this behavior as a `write request`.
- [x]  设置`key`对应的参数变化。<br />Set the parameter change of the `key`.
- [x]  读取`memory`指定位置的内容。<br />Read the content of the `memory` at the specified position.


```plain
TX:
AB 00 07 00 2B E0 06 00 
02 05 0D BB  33 00 80                                

02 - configuration command
05 - length of key(0D)
0D - key of SMS Password
BB 33 00 80 - digital password parameter

```

---
## 1.3 read request

在向设备发送的`packet`中，<br />In the `packet` sent to the device,
- [x]  `key`后面没有携带参数 <br />There are no parameters after `key`.
- [x]  `key`后面跟随`index`参数（比如闹钟序号)<br />The `key` is followed by the `index` parameter (e.g., alarm clock number).
- [x]  `readkey`加上`key list`<br />`readkey` is followed by the `key list`.

都可以视作向设备查询当前`key`的参数配置列表。此行为我们称为 `读请求(read-request)`。<br />All of these can be regarded as queries to the device for the current parameter configuration list for the `key`. We refer to this behavior as a `read request`.

### 1.3.1 length-key query 


```plain
TX:
AB 00 03 00 70 4D 02 00 
02 01 01

RX:
AB 00 07 00 40 0D 02 00 
02 05 01 01  07 23 20                                

```
TX中的 02 01 01 指`configuration command(0x02)`读`length(01)-key(01)`即`key=01`的内容。<br />In TX, 02 01 01 refers to the `configuration command (0x02)` reading the content of `length (01)-key (01)`, i.e., `key=01`.


RX中返回 02 05 01 01  07 23 20，解析为`configuration command(0x02)` 返回`05 01 01  07 23 20`结果`length(05)-key(01)-value(0x20230701)`。 0x20230701是数组`01 07 23 20`的小端模式下的`int`解析结果。<br />RX returns 02 05 01 01 07 23 20, which is parsed as `configuration command(0x02)`, returning `05 01 01 07 23 20` with the result `length(05)-key(01)-value(0x20230701)`. 0x20230701 is the little-endian `int` parsing result of the array `01 07 23 20`.

!> 1. 使用此方式如果要同时读取多个`key`,格式为：`command + [01 + key1] + [01 + key2] + [01 + key3]`<br />To read multiple keys at the same time using this method, the format is: `command + [01 + key1] + [01 + key2] + [01 + key3]`.
<br />2. 当前参数如果有索引值，使用此方式读取时将读取所有索引对应参数进行回复。比如 `command + [01 + alarmclock key]`, 则读取返回多个闹钟参数:`command + [01 + alarmclock key + 00 + param] + ..[01 + alarmclock key + 03 + param]`。 <br />If the current parameter has an index value, this method will read all parameters corresponding to the index and return them. For example, `command + [01 + alarmclock key]` will read and return multiple alarm clock parameters: `command + [01 + alarmclock key + 00 + param] + ..[01 + alarmclock key + 03 + param]`.



### 1.3.2 length-key-value(index) Query


当某个`key`下还有`index`索引参数时，可以读指定`index`的内容。<br />When there are index parameters under a certain `key`, you can read the content of the specified index.

```plain
TX:
AB 00 04 00 F2 5F 03 00 02 02 0B 00

RX:
AB 00 09 00 50 4A 03 00 
02 07 0B 00  00 00 00 00  00
```

使用此方式同时读取多个索引:`command + [02 + key1 + idx0] + [02 + key1 + idx1] ...`。<br />Use this method to read multiple indexes simultaneously: `command + [02 + key1 + idx0] + [02 + key1 + idx1] ...`


### 1.3.3 readkey + key Query

- [x] `readkey - key`部分命令提供 `readkey`追加需要读取的 `keys`来查询 1 个或多个指定项的参数或状态。<br />The `readkey - key` command provides `readkey` to append the `keys` to be read to query the parameters or status of one or more specified items.
- [x] `readkey - key`不支持读`key`的指定索引(index)。<br />`readkey - key` does not support reading the specified index (index) of `key`.
- [x] `readkey`后面不要跟`write-request`操作，容易得到`返回结果与设置参数不一致`的时序操作问题。<br />Do not follow `readkey` with a `write-request` operation, as this may result in a timing issue where the return result does not match the set parameters.
- [x] `readkey`当前仅在`配置命令`中定义，其他命令暂时不支持。建议配置命令中使用 `readkey`方式来读参数。<br />`readkey` is currently only defined in `configuration commands` and is not supported by other commands. It is recommended to use the `readkey` method to read parameters in configuration commands.

```plain
TX:
AB 00 04 00 E8 80 03 00 02 02 F0 01

RX:
AB 00 07 00 40 0D 03 00 
02 05 01 01  07 23 20
```

可以看到，这两个查询结果是一致的。<br />As seen in the example, the two query results are consistent.



## 1.4 notify

`key`本身是用于保存配置参数的，也同时存在部分`key`是设备用于提供向外部提供数据使用的：<br />The `key` is used to store configuration parameters, and some `keys` are also used by devices to provide data to external sources:
* 历史数据 <br />Historical data
* 传感器原始数据 <br />Raw sensor data
* 日志 <br />Logs

这几类数据，上位机无法直接使用命令读取指定`key`的数据。这些数据是在`状态`更新时主动向外部发送的，因此将这一类`key`的行为称为`notify`。<br />The host cannot directly read the data of the specified `key` using commands. This data is actively sent to the outside when the `status` is updated, so the behavior of this type of `key` is called `notify`.


TODO: 协议修改，示例协议后面更新 <br />TODO: Update protocol examples later.

