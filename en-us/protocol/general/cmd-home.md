
# Command Description


As mentioned in the previous section [📄Data Structures](data_structure.md), `payload` consists of `command` and multiple `item`, while `item` consists of `length-key-value`.

This document focuses on defining and standardizing the behavior/attributes of the protocol:
- [x] write request
- [x] read request
- [x] notify
  

In the subsequent command sections, we annotate the `key` in the `key-table` with the operations it supports.

---
## 1.1 Only command

When the host sends only `command` to the device to query the key table implemented by the device for that command.

!> Requires the `indicate` attribute to be 0.



In the same command, due to differences in device capabilities, we can use “send command only” to query the “key-table” implemented by the device code.
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

The device command supports `key-table` using `key = 0` as the outgoing key.
* The use of `key = 0` is not allowed in the `key-table` of all commands.
* When the host replies only with `command`, the `indicate` attribute is required to be 0, otherwise it will conflict with the positive response we defined previously.



---
## 1.2 write request

In the `packet` sent to the device, the `key` is followed by `parameters`. We refer to this behavior as a `write request`.
- [x]  Sets the parameter corresponding to the  `key`
- [x]  Read the content of the `memory` at the specified position.


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

In the `packet` sent to the device,
- [x]  There are no parameters after `key`.
- [x]  The `key` is followed by the `index` parameter (e.g., alarm clock number).
- [x]  `readkey` is followed by the `key list`.

All of these can be regarded as queries to the device for the current parameter configuration list for the `key`. We refer to this behavior as a `read request`.

### 1.3.1 length-key query 


```plain
TX:
AB 00 03 00 70 4D 02 00 
02 01 01

RX:
AB 00 07 00 40 0D 02 00 
02 05 01 01  07 23 20                                

```
In TX, `02 01 01` refers to the `configuration command (0x02)` reading the content of `length (01)-key (01)`, i.e., `key=01`.


RX returns `02 05 01 01 07 23 20`, which is parsed as `configuration command(0x02)`, returning `05 01 01 07 23 20` with the result `length(05)-key(01)-value(0x20230701)`. 0x20230701 is the little-endian `int` parsing result of the array `01 07 23 20`.

!> 1. To read multiple keys at the same time using this method, the format is: `command + [01 + key1] + [01 + key2] + [01 + key3]`.
<br />2. If the current parameter has an index value, this method will read all parameters corresponding to the index and return them. For example, `command + [01 + alarmclock key]` will read and return multiple alarm clock parameters: `command + [01 + alarmclock key + 00 + param] + ..[01 + alarmclock key + 03 + param]`.



### 1.3.2 length-key-value(index) Query


When there are index parameters under a certain `key`, you can read the content of the specified index.

```plain
TX:
AB 00 04 00 F2 5F 03 00 02 02 0B 00

RX:
AB 00 09 00 50 4A 03 00 
02 07 0B 00  00 00 00 00  00
```

Use this method to read multiple indexes simultaneously: `command + [02 + key1 + idx0] + [02 + key1 + idx1] ...`


### 1.3.3 readkey + key Query

- [x] `readkey + key`, Portion of the command of `readkey+key` allows appending the keys to be read, in order to query the parameters or
  status of one or more specified items.
- [x] `readkey + key` does not support reading the specified index (index) of `key`.
- [x] Do not follow a `readkey` with a `write-request` operation, as this may result in a timing issue where the return result does not match the set parameters.
- [x] `readkey` is currently only defined in `configuration commands` and is not supported by other commands. It is recommended to use the `readkey` method to read parameters in configuration commands.

```plain
TX:
AB 00 04 00 E8 80 03 00 02 02 F0 01

RX:
AB 00 07 00 40 0D 03 00 
02 05 01 01  07 23 20
```

As seen in the example, the two query results are consistent.



## 1.4 notify

The `key` is used to store configuration parameters, and some `keys` are also used by devices to provide data to external sources:
* Historical data
* Raw sensor data
* Logs

The host cannot directly read the data of the specified `key` using commands. This data is actively sent to the outside when the `status` is updated, so the behavior of this type of `key` is called `notify`.




