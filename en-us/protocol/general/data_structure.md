# Protocol Package Format

The data structure consists of the defined communication data packets.

![packet structure](assets/packet-structure.svg)

A complete command packet consists of `a header (8 bytes fixed)` + `payload (1 to n bytes)`:

+ The header indicates the start of a data packet reception, data length, and verification method, among other information.
+ The payload carries detailed command data.

---

## 1. Header

| Field    | Size   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| -------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| preamble | 1byte  | Preamble is used to indicate the start of a communication packet, and the default value is `0xAB`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| flag     | 1byte  | Flag is used to indicate the brief information of the current packet.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| length   | 2bytes | The payload size range is 1–65535 bytes. In practical, due to limited device memory, we will restrict its length in different communication methods. When the length exceeds the limit, the data packet will be discarded and not processed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| crc      | 2bytes | Perform CRC16 calculation on the Payload. The calculation method is described in the appendix.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| tid      | 2bytes | Tid is used to identify the sequence number of the current packet, and the response packet must use the same tid as the corresponding request packet. <br />For packets proactively sent by the device, the tid range is 0xD000–0xFFFF.<br />For request packets initiated by the host or server, the tid range is 0x0001–0xCFFF. <br />Tid is used cyclically. For different command operations, the tid either increments sequentially or is calculated as the previous tid plus the payload length. |

+ When the device proactively uploads data (such as logs or sensor data), a large number of packets may be generated, causing the tid to increase rapidly within a short period of time. To avoid conflicts with tids of request packets initiated by the host or server, the usage range is therefore restricted.

### 1.1 Flag data format

The `flag` is a one-byte filed used to provide a simple description and representation of protocol information.

| bit No. | name | description                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| bit3-0  | version        | it3-0 indicates the current protocol version number, which is used to distinguish between protocol versions. Note: The protocol version is not the same as the protocol API version number.                                                                                                                                                                                                   |
| bit4    | indicate       | bit4 indicates that the current packet is proactively reported by the device, and the host must acknowledge receipt. After the data transmission, the device must wait for the host to send an acknowledgment (ACK). When replying, the host shall set this bit (indicate=1). |
| bit5    | error          | bit5 indicates that the current packet is an error response, which can be a negative response or an error response defined internally by a specific commands.                                                                                                                                                                                                                 |
| bit6-7  | RFU            | Reserved for future use.                                                                                                                                                                                                                                                                                                                                                                                                                               |

---

## 2. Payload

The payload is the actual carrier of useful information, indicating the scope of the current processing and specific execution items.

The payload is composed of `command` + multiple `item(s)`. The number of `item(s)` can be 0 or more.

> Note that the total byte length of `command` + `item` * n cannot exceed the maximum supported byte length in `Header`.

An item is composed of three parts: `len-key-value` , which is used in many protocols, such as BLE broadcast .

* len


> `len` occupies one byte and can represent a range of [1,255].
>  <br />When `len = 0`, all remaining data after `key` is used as `value`.
>  <br />`Generally, longer data packets are split into multiple `key`s and combined. However, the protocol still recommends adding compatibility for this issue in the code.
* key

> The value of the key varies depending on the command and represents the purpose of the current item. 
> The command defines the processing scope,while the key is used to indicate specific operations,e,g.config-command->personal info.

* value

> `value` corresponds to the description of the current `key`, if `value` is not `NULL`.

Code example for splitting the length-key-value of the payload using C#

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

# 3. Data types

Data type refers to the number of bytes occupied by the `value` in the protocol and the parsing method.

Data type's length `> 1 byte` is parsed in `little endian` by default.

The data types used in the current protocol include:

| **bit width**           | **Type** | **description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------------------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bitfield                      | bit/bits       | Range: 0-1 Commonly used to indicate switch/enable                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| byte                          | byte/sbyte     | 1. Unsigned representation range: [0, 255] <br />2. Signed representation range: [-128, 127]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 2bytes                        | short/ushort   | 1. Unsigned representation range: [0, 65535] <br />2. Signed representation range: [-32768, 32767]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 4bytes                        | int/uint/float | 1. Unsigned representation range: [0, 4294967295]<br /> 2. Signed representation range: [-2147483648, 2147483647] <br />3. Single-precision floating-point representation range: [-3.4E38, 3.4E38]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| nbytes                        | array          | Byte array                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| fixed-length string 1-nbytes | fixstring      | Fixed-length string<br />1. A string with a length indicator must be obtained according to the specified length value<br />2. A string with a fixed length allocated in the protocol is parsed and truncated according to the fixed length, and if it is insufficient, it is padded with “\0” null characters.                                                                                                                                                                                                                                                                                                                                                   |
| variable-String 1-nbytes      | String         | 1. Strings ending with ‘\0’. If the value is empty (when it cannot be successfully obtained internally by the device), only ‘\0’ is used to indicate this. <br />2. For example, when reading the `IMEI` normally, it returns `“014475002391632\0”`.<br />3. If the module has not been fully initialized, reading `IMEI` will return “\0”.<br />4. The description of the string length does not include the number of bytes occupied by “\0”, but only indicates the effective content length of the string. |
| 6bytes                        | MAC            | The hexadecimal reception sequence for MAC is: 0x12 0x34 0x56 0x79 0xab 0xcd. When converted to a string, it is displayed as: `CD:AB:79:56:34:12`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

Note:

1. The representable range of a data type may be further restricted by specific parameters. For example, weight uses the `ushort` type, but 0 is not allowed.
2. When there are two consecutive `String` types, the starting address of the second String type cannot be annotated in the protocol document. It needs to be separated by an end marker during protocol parsing to obtain it.
3. For parsing String type, in C# the method `BitConvert. ToFloat` can be used to convert 4 bytes into a `float` . The pricinpe is the same, and other languages have similar approaches.

---

# 4. Data converters

A data converter is used to specify how raw byte data is converted into a specific data type format. During protocol parsing, the converter defines how data is parsed and displayed.

Data converters supported in the current protocol include:

| **Converter Name**                                         | **Key** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ---------------------------------------------------------------- | ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ASCII String                                    | ascii         | ASCII string                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Fixed-length ASCII string               | asciiFixed    | When the length is insufficient, use ‘\0’ to fill it up.                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| UTF-8 encoded string                           | utf8          | UTF-8 encoded string                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Fixed-length UTF-8 string               | utf8Fixed     | Fixed-length UTF-8 string                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Unix timestamp                                   | unixTime      | Similar to Epoch & Unix Timestamp. 4 bytes, but uses unsigned numbers and does not support times before `1970.1.1 00:00:00`. <br />The current `timestamp` data is generated during device use and does not involve historical data. <br />The server side should pay attention to the conversion rules. |
| Birthday                                               | birthday      | Birthday format                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| little-endian hexadecimal                      | leHex         | Little end in front, hex parsing, little end in back                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Fixed-length little-endian hexadecimal | leHexFixed    | Little end in front, hex parsing, fixed length, and if it is insufficient, it is padded with 0.                                                                                                                                                                                                                                                                                                                                                                            |
| Location                                           | loc           | Latitude, longitude                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Little-endian MAC address                       | leMac         | Little end in front, big end in back                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Big-endian MAC address                          | beMac         | Big end in front, little end in back                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| IPv4 address                                       | ipv4          | IPv4 address format                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Time zone                                              | timeZone      | Time zone information                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Number to hexadecimal                        | numHex        | Number to hexadecimal format                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Temperature                                            | temperature   | Temperature format                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Bits                                                     | bits          | Different bits represent different meanings                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Version                                                | version       | uint->Version                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Custom converter                               | custom        | Custom converter will not be checked, and you need to add a validation mechanism yourself                                                                                                                                                                                                                                                                                                                                                                                        |
| IEEE754 floating point number                 | IEEE754Float  | Private protocol parsing, not recommended to use                                                                                                                                                                                                                                                                                                                                                                                                                                               |

Note:

1. The choice of converter should be determined based on the specific data type and function requirements.
2. Custom converters require developers to implement their own validation mechanisms.
3. Some converters (such as MAC addresses) have endianness differences, so byte order must be considered when using them.
4. Fixed-length converters perform padding when the data length is insufficient.
5. `Converters` are tools developed for the host/server side to automate data conversion. Customers may review this section but are not required to implement the current converters.
