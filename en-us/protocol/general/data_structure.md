# 协议包格式 Protocol Package Format


数据结构是约定的通讯数据包组成。<br />Data structure is the composition of communication data packets.


![数据包结构](assets/packet-structure.svg)

完整的一个命令包(packet) 由 `header(8bytes fixed)`+`payload(1~n bytes)`组成:<br />A complete command packet consists of `a header (8 bytes fixed)` + `payload (1 to n bytes)`:

+ header 用于提示 `packet`的接收开始，数据长度，以及校验方式等。<br />The header is used to indicate the start of a data packet reception, data length, and verification method.
+ payload 携带详细的命令数据。<br />The payload carries detailed command data.

---

## 1. Header

| 字段<br />Field          | 大小<br />Size | 说明<br />Description |
| -------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| preamble | 1byte  | 前导码，用于指示一个通讯包的开始，默认使用 `0xAB`<br />Preamble is used to indicate the start of a communication packet, and the default value is `0xAB`. |
| flag     | 1byte  | 用于表示当前包的简要信息。<br />Flag is used to indicate the brief information of the current packet. |
| length   | 2bytes | Payload 的字节数，使用范围:`1-65535`。在实际使用过程中，由于设备的内存有限，在不同的通讯方式中，我们会限制它的长度，当长度超过限制时，这个数据包将被丢弃不处理。<br />The number of bytes in the payload, with a range of `1-65535`. In actual use, due to limited device memory, we will restrict its length in different communication methods. When the length exceeds the limit, the data packet will be discarded and not processed. |
| crc      | 2bytes | 对 Payload 进行 CRC16 计算.<br />计算方式参考附录说明。<br />Perform CRC16 calculation on the Payload.<br />Refer to the appendix for the calculation method.|
| tid      | 2bytes | 描述当前包的一个流水号，回复包必须与请求包的 `tid`一致。<br />设备主动发出的数据包'tid'使用范围：`0xD000-0xFFFF`.<br />上位机/服务器 主动发起的请求包 `tid` 使用范围： `0x0001-0xCFFF`。`tid`为循环使用，不同的命令操作按顺序递增 `tid`， 或者每次将上个`tid`+`payload`长度为作为新的`tid`。<br />Describes a sequence number for the `current packet`. `The reply packet` must match the `tid` of the request packet. <br />The range of `tid` used in data packets actively sent by the device is `0xD000-0xFFFF`. <br />The range of `tid` used in request packets actively initiated by the host (server, app, etc.) is `0x0001-0xCFFF`. `tid` is reused in a cycle, Different command operations increment `tid` in sequence, or each time the `previous tid` + `payload length` is used as the `new tid`.|

+ 在设备主动上传数据时，比如 `log`,`sensor data`, 它的 `packet`数量较多，短时间内 `tid`变化递增，为避免与上位机/服务器 请求冲突，因此限制使用范围。<br />When the device actively uploads data, such as `log` and `sensor data`, the number of `packets` is relatively high, and the `tid` changes incrementally within a short period of time. To avoid conflicts with requests from the host computer/server, the scope of use is therefore restricted.

### 1.1 Flag 的数据格式 Flag data format

`Flag`是一个字节数据，用于简单描述及表示协议的信息。 <br />`Flag` is a byte of data used to simply describe and represent protocol information.

| bit No. | 名称<br />name     | 描述<br />description                                                                                                                                                     |
| ------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bit3-0  | version  | 表示当前协议的版本号，用于区分协议版本。注意：协议版本不等于协议 API 版本号。<br />Indicates the version number of the current protocol, used to distinguish between protocol versions. Note: The protocol version is not the same as the protocol API version number.                                                                            |
| bit4    | indicate | 表示当前数据包为设备主动上报的, 主机需要承认或确认收到。数据传递后，设备端需要等待主机发送确认(ACK)。<br />主机回复时，此 `bit`要同时置位(indicate=1).<br />Indicates that the current data packet is actively notified by the device, and the host needs to acknowledge or confirm receipt. After the data is transmitted, the device needs to wait for the host to send an acknowledgment (ACK). When the host responds, this `bit` must also be set (indicate=1). |
| bit5    | error    | 表示当前数据包是一个错误回复包。可以是 `negative response`，也可以是某些命令内部定义的错误回复。<br />Indicates that the current packet is an error response packet. It can be a `negative response` or an error response defined in  some commands.                                                             |
| bit6-7  | RFU      | 预留后续使用<br />Reserved for future use.                                                                                                                                             |


---

## 2. Payload

payload 是真正有效信息载体，表示当前处理的范围和具体执行事项。<br />The payload is the actual carrier of useful information, indicating the scope of the current processing and specific execution items.

payload 由 `command`+ 多个 `item`  组成, `item`的个数可以是0个,也可以是多个。<br />The payload is composed of `command` + multiple `item(s)`. The number of `item(s)` can be 0 or multiple.

> **注意 `command`+`item` * n 的总字节长度不能超过 `Header`中的 `length`最大支持字节长度。**<br />Note that the total byte length of `command` + `item` * n cannot exceed the maximum supported byte length in `Header`.


`item`由 `len-key-value`三个组成，这种描述方式在很多协议中用到，例如 `BLE`的广播数据包组成。<br />An `item` is composed of `len-key-value` three parts, which is used in many protocols, such as `BLE` broadcast packet composition.


* len

> `len` 占用一个字节，可表示范围为 [1,255]。
> 当 `len = 0`时，则 `key`后面所有剩余的数据都作为 `value`来使用。
> 一般来说，较长的数据包我们会拆分成多个 `key` 来组合, 不过协议仍建议在代码中针对此问题进行一个兼容。
> <br />`len` occupies one byte and can represent a range of [1,255].
> <br />When `len = 0`, all remaining data after `key` is used as `value`.
> <br />Generally, longer data packets are split into multiple `key`s and combined. However, the protocol still recommends adding compatibility for this issue in the code.



* key

> `key`的值在不同的 `command`有着不相同的作用，它代表当前 `item`的用途。命令用来限制处理范围，`key`用来指示具体操作，比如`config-command`->`personal info`。
> <br />The value of `key` has different functions in different `commands`. It represents the current use of the `item`. Commands are used to limit the scope of processing, while `key` is used to indicate specific operations, such as `config-command`->`personal info`.


* value

> `value`对应当前 `key`的描述，如果 `value`不为 `NULL`的话。 <br />`value` corresponds to the description of the current `key`, if `value` is not `NULL`.

使用 `C#`分割 `payload`的 `length-key-value` <br />Code example for splitting the length-key-value of the payload using C#

```csharp

// payload to length-key-value
    List<PBKeyValue > lkv_list = new List<PBKeyValue>();
    byte[] arrays = payload.ToArray();

    try
    {
        for (int i = 1; i < arrays.Length;)
        {
            byte len = arrays[i];
            byte key = arrays[i + 1];
            if (len == 0)
            {  
                // if len = 0, then bytes remain all as value
                lkv_list.Add(new PBKeyValue(arrays.Length - 2 - i, key, arrays.Slice(i + 2, arrays.Length - 3)));
                break;
            }
            else
            {
                // if len != 0, then value's bytes nums is len - 1
                lkv_list.Add(new PBKeyValue(len - 1, key, arrays.Slice(i + 2, len - 1)));
                i += len + 1;
            }
        }

        return lkv_list;
    }
    catch
    {
        return lkv_list;
    }
```

---

# 3. 数据类型 Data types


数据类型，指协议中 `value`的占用字节数据，以及解析方式。<br />Data type refers to the number of bytes occupied by the `value` in the protocol and the parsing method.

数据类型长度 `大于1byte`时, 默认数据顺序为小端模式 `little endian`。<br />Data type length `> 1 byte` is parsed in `little endian` by default.


当前协议中所使用的数据类型有:<br />The data types used in the current protocol include:

| **bit width**           | **Type** | **description**                                                                                                                                                                                                                                                                    |
| ----------------------------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bitfield                      | bit/bits       | 范围：0-1 常用于表示开关/使能<br />Range: 0-1 Commonly used to indicate switch/enable|
| byte                          | byte/sbyte     | 1. 无符号表示范围：[0, 255] <br />2. 有符号表示范围：[-128, 127]<br />1. Unsigned representation range: [0, 255] <br />2. Signed representation range: [-128, 127]|
| 2bytes                        | short/ushort   | 1. 无符号表示范围：[0, 65535] <br />2. 有符号表示范围：[-32768, 32767]<br />1. Unsigned representation range: [0, 65535] <br />2. Signed representation range: [-32768, 32767]|
| 4bytes                        | int/uint/float | 1. 无符号表示范围：[0, 4294967295]<br /> 2. 有符号表示范围：[-2147483648, 2147483647]3. 单精度浮点数表示范围:[-3.4E38, 3.4E38] <br />1. Unsigned representation range: [0, 4294967295]<br /> 2. Signed representation range: [-2147483648, 2147483647] 3. Single-precision floating-point representation range: [-3.4E38, 3.4E38]|
| nbytes                        | array          | 字节数组 <br />Byte array|
| fixed-length string 1-nbytes | fixstring      | 定长串<br />1. 带长度指示的字符串，必须根据指定的长度值来获取字符串<br />2. 在协议中固定分配长度的字符串，按固定长度解析截取, 不足时补"\0"空字符串。<br />Fixed-length string<br />1. A string with a length indicator must be obtained according to the specified length value<br />2. A string with a fixed length allocated in the protocol is parsed and truncated according to the fixed length, and if it is insufficient, it is padded with “\0” null characters.|
| variable-String 1-nbytes      | String         | 1. 带 '\0'结尾的字符串，如果值为空（设备内部未成功获取时），则仅使用 '\0'表示。<br />2.比如 正常读取到的 `IMEI`，返回 `"014475002391632\0"` <br />3. 在模块未初始化完成时，读取 `IMEI`则返回 "\0"<br />4. 描述中对于字符串长度不包含"\0"占用的字节数，仅表示字符串的有效内容长度。<br />1. Strings ending with ‘\0’. If the value is empty (when it cannot be successfully obtained internally by the device), only ‘\0’ is used to indicate this. <br />2. For example, when reading the `IMEI` normally, it returns `“014475002391632\0”`.<br />3. If the module has not been fully initialized, reading `IMEI` will return “\0”.<br />4. The description of the string length does not include the number of bytes occupied by “\0”, but only indicates the effective content length of the string.|
| 6bytes                        | MAC            | MAC 的十六进制接收顺序为: 0x12 0x34 0x56 0x79 0xab 0xcd``转为字符串显示为：`CD:AB:79:56:34:12`<br />The hexadecimal reception sequence for MAC is: 0x12 0x34 0x56 0x79 0xab 0xcd. When converted to a string, it is displayed as: `CD:AB:79:56:34:12`.|

Note:

1. 不同的数据类型可表示的范围，会在部分参数中被进一步限制其允许值的范围。比如 `weight`使用了 `ushort`类型表示，但其范围不允许为 0。<br />The range that different data types can represent may be further restricted in some parameters. For example, `weight` uses the `ushort` type, but its range cannot be 0.
2. 当有两个连续的 `String`类型时，此时第二个 String 类型起始地址无法在协议文档标注开始地址，需要在协议解析中以结束符分隔开获取。<br />When there are two consecutive `String` types, the starting address of the second String type cannot be annotated in the protocol document. It needs to be separated by an end marker during protocol parsing to obtain it.
3. `String`类型的解析，`C#`解析时可使用 `BitConvert.ToFloat`来完成 `4字节到float`类型转换。了解原理，其他语言方式类似。<br />When parsing `String` type in `C#`, you can use `BitConvert.ToFloat` to convert `4 bytes` to `float` type. Understand the principle, and other languages have similar methods.


---

# 4. 数据转换器 Data converters


数据转换器用于指定如何将原始字节数据转换为特定格式的数据类型。在协议解析过程中，转换器定义了数据的解析方式和显示格式。<br />
A data converter is used to specify how raw byte data is converted into a specific data type format. During protocol parsing, the converter defines how data is parsed and displayed.

当前协议中所支持的数据转换器有：<br />
Data converters supported in the current protocol include:


| **Converter Name** | **Key** | **Description**                                                                                                                                                                                             |
| -------------------- | ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ASCII字符串 <br />ASCII String         | ascii         | ASCII字符 <br />ASCII string串|
| 固定长度ASCII字符串 <br />Fixed-length ASCII string  | asciiFixed    | 长度不足时，使用'\0'填充 <br />When the length is insufficient, use ‘\0’ to fill it up. |
| UTF-8字符串<br /> UTF-8 encoded string          | utf8          | UTF-8编码字符串<br />UTF-8 encoded string|
| 固定长度UTF-8字符串<br />Fixed-length UTF-8 string  | utf8Fixed     | 固定长度的UTF-8字符串<br />Fixed-length UTF-8 string |
| Unix时间戳<br />Unix timestamp           | unixTime      | 类似Epoch & Unix Timestamp. 4字节，但是使用无符号数，不支持 `1970.1.1 00:00:00 `之前的时间。<br />当前使用 `timestamp`数据为设备使用过程中产生的数据，不涉及过往数据。<br />服务器端应当注意转换规则。<br />Similar to Epoch & Unix Timestamp. 4 bytes, but uses unsigned numbers and does not support times before `1970.1.1 00:00:00`. <br />The current `timestamp` data is generated during device use and does not involve historical data. <br />The server side should pay attention to the conversion rules. |
| 生日<br />Birthday                 | birthday      | 生日格式 <br /> Birthday format|
| 小端十六进制<br />little-endian hexadecimal         | leHex         | 小端在前，hex解析，小端在后<br />Little end in front, hex parsing, little end in back|
| 固定长度小端十六进制<br />Fixed-length little-endian hexadecimal | leHexFixed    | 小端在前，hex解析，固定长度，长度不足时，补0<br />Little end in front, hex parsing, fixed length, and if it is insufficient, it is padded with 0.|
| 位置信息<br />Location             | loc           | 纬度，经度 <br />Latitude, longitude|
| 小端MAC地址<br />Little-endian MAC address          | leMac         | 小端在前，大端在后 (Little end in front, big end in back)|
| 大端MAC地址<br />Big-endian MAC address          | beMac         | 大端在前，小端在后 (Big end in front, little end in back)|
| IPv4地址<br />IPv4 address             | ipv4          | IPv4地址格式<br />IPv4 address format|
| 时区<br />Time zone                 | timeZone      | 时区信息<br />Time zone information|
| 数值转十六进制<br />Number to hexadecimal       | numHex        | 数值转HEX格式<br />Number to hexadecimal format|
| 温度<br />Temperature                 | temperature   | 温度格式<br />Temperature format|
| 位<br />Bits                   | bits          | 不同的位代表不同的意<br />Different bits represent different meanings|
| 版本<br />Version                 | version       | uint->版本<br />uint->Version|
| 自定义转换器<br />Custom converter         | custom        | 自定义不会被校验，需要自己添加校验机制<br />Custom converter will not be checked, and you need to add a validation mechanism yourself|
| IEEE754浮点数<br />IEEE754 floating point number        | IEEE754Float  | 私有协议解析，不建议使用<br />Private protocol parsing, not recommended to use|


Note:

1. 转换器的选择应根据具体的数据类型和业务需求来确定。<br />The choice of converter should be determined based on the specific data type and function requirements.  
2. 自定义转换器需要开发者自行实现校验机制。<br />Custom converters require developers to implement their own validation mechanisms.  
3. 某些转换器（如MAC地址）有大小端之分，使用时需要注意字节序。<br />Some converters (such as MAC addresses) have endianness differences, so byte order must be considered when using them.  
4. 固定长度转换器在数据长度不足时会进行填充处理。<br />Fixed-length converters perform padding when the data length is insufficient.
5. 转换器是编写上位机/服务器部分制作用于自动化处理转换数据的工具，客户可以阅读当前部分但不是强制要求实现当前转换器。<br />Converters are tools developed for the host/server side to automate data conversion. Customers may review this section but are not required to implement the current converters.


