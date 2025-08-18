设备数据


待完成列表：
- [ ] 补充数据交换时序流程图
- [ ] 补充之前的数据格式，以及对数据类型说明。

---


待完成列表：
- [ ] 补充数据交换时序流程图
- [ ] 补充之前的数据格式，以及对数据类型说明。

---

数据记录包含了以下内容：
* 设备使用过程中的健康数据
* 设备的状态信息
* 设备的使用记录

数据记录按类整理：
* 设备的身份信息，用于识别设备的唯一性
* 设备的状态变化，设备的网络变化
* 定位信息，不同模块获取到的定位记录
* 健康记录，如计步、心率、睡眠等，用于分析查看使用者的健康情况。
* 健康数据第三方来源，从第三方配件上传过来的健康数据。
* 设备的使用记录，记录设备使用状态、关键记录。

## Key列表
| Value<br>(HEX) | Parameter           | 功能描述               | 可写 | 可读 | 通知 |
| -------------- | ------------------- | ---------------------- | ---- | ---- | ---- |
| [0x01](#_0x01) | DeviceId            | 设备ID                 | ❌    | ❌    | ✅    |
| [0x02](#_0x02) | CustomizedId        | 用户自定义ID           | ❌    | ❌    | ✅    |
| [0x03](#_0x03) | Iccid               | ICCID                  | ❌    | ❌    | ✅    |
| [0x10](#_0x10) | AlarmCode           | 报警代码               | ❌    | ❌    | ✅    |
| [0x11](#_0x11) | DeviceStatus        | 设备状态               | ❌    | ❌    | ✅    |
| [0x20](#_0x20) | Gps                 | GPS定位记录            | ❌    | ❌    | ✅    |
| [0x21](#_0x21) | Gsm                 | 基站定位记录           | ❌    | ❌    | ✅    |
| [0x22](#_0x22) | Ble                 | BLE定位记录            | ❌    | ❌    | ✅    |
| [0x23](#_0x23) | Homebeacon          | homebeacon定位记录     | ❌    | ❌    | ✅    |
| [0x24](#_0x24) | Beacon              | Beacon 定位记录        | ❌    | ❌    | ✅    |
| [0x25](#_0x25) | Homewifi            | homewifi定位记录       | ❌    | ❌    | ✅    |
| [0x26](#_0x26) | Wifi                | WiFi 定位记录          | ❌    | ❌    | ✅    |
| [0x40](#_0x40) | CallRecord          | 通话记录               | ❌    | ❌    | ✅    |
| [0x41](#_0x41) | BleConnection       | BLE连接记录            | ❌    | ❌    | ✅    |
| [0x42](#_0x42) | AccessoryStatus     | 配件状态               | ❌    | ❌    | ✅    |
| [0x50](#_0x50) | DailyStep           | 天步数                 | ❌    | ❌    | ✅    |
| [0x51](#_0x51) | HourlyStep          | 每小时步数记录         | ❌    | ❌    | ✅    |
| [0x52](#_0x52) | HourlyActivity      | 每小时活动记录         | ❌    | ❌    | ✅    |
| [0x53](#_0x53) | Activity            | 活跃度记录             | ❌    | ❌    | ✅    |
| [0x54](#_0x54) | StaticData          | 静止记录               | ❌    | ❌    | ✅    |
| [0x60](#_0x60) | HeartRate           | 心率测量记录           | ❌    | ❌    | ✅    |
| [0x61](#_0x61) | BloodOxygen         | 血氧测量记录           | ❌    | ❌    | ✅    |
| [0x62](#_0x62) | HRV                 | HRV测量记录            | ❌    | ❌    | ✅    |
| [0xA0](#_0xA0) | ThirdPartyAccessory | 第三方配件健康测量记录 | ❌    | ❌    | ✅    |
| [0xC0](#_0xC0) | DeviceResetReason   | 设备复位原因           | ❌    | ✅    | ❌    |



---

## 命令描述

---

### (0x01)设备ID :id=0x01


**功能描述**

设备ID用于向上层提供设备唯一标识。当使用设备的`IMEI`, 参考[配置命令中-IMEI](cmd-configuration.md)。



**写请求数据格式**

length+key+value

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-n | IMEI | String | ascii | IMEI, Type: String, length:[1, 32] |

Note:
1. 上报数据时,一帧数据包含 IMEI号必须第一个item是length-key-value, 以方便服务器快速解析判断当前设备是否合法。

---


### (0x02)用户自定义码 :id=0x02


**功能描述**

设备自定义识别码，由服务器定义的另一个设备识别标识，在[配置命令中-设备自定义识别码](cmd-configuration.md)中配置后自动启用。



**写请求数据格式**

length+key+value

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-n | customDeviceID | String | ascii | 自定义设备ID, length:[0, 32] |

Note:
1. 当设备自定义识别码 被设置为非空时，上报数据时，必须包含自定义识别码，它跟在`设备ID`后面，作为第二个`length-key-value`
2. 当设备自定义识别码 被设置为空时，上报数据时，不能包含自定义识别码。


---


### (0x03)ICCID :id=0x03


**功能描述**
ICCID是集成电路卡识别码，用于唯一标识SIM卡。为可选上报数据类型。



**写请求数据格式**

length+key+value

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-n | ICCID | String | ascii | ICCID, 类型:字符串 |

---

### (0x10) 警报数据 :id=0x10


**功能描述**

当设备发生 紧急/重要/订阅的事件 变化，需要及时向服务器上传相关事件的状态信息描述。



**写请求数据格式**

length+key+value

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-6 | dateTime | uint | unixTime | 报警时间戳 |
| 7-n | statusEvent | array | bits | 状态/事件<br />bit0:1为低电报警<br />bit1:1为超速报警<br />bit2:1为跌倒报警<br />bit3:1为倾斜报警<br />bit4:1为geo1-out报警<br />bit5:1为geo1-in报警<br />bit6:1为geo2-out报警<br />bit7:1为geo2-in报警<br />bit8:1为关机报警<br />bit9:1为开机报警<br />bit10:1为移动报警<br />bit11:1为不移动报警<br />bit12:1为sos报警<br />bit13:1为sos结束报警<br />bit14:1跌倒结束报警<br />bit15:1倾斜结束报警<br />bit24:1为离家报警<br />bit25:1为在家报警<br />bit26:1为geo3-out报警<br />bit27:1为geo3-in报警<br />bit28:1为geo4-out报警<br />bit29:1为geo4-in报警<br />bit30:1为weflare报警<br /> |

---

Note

1. 在同一个时间段可能有多个报警触发，时间会被覆盖，设备端自行处理多个报警item拼接，
2. 报警时间必须真实有效，如果时间没同步之前，真实时间=同步后的utc-同步前设备RTC的s
3. 这些报警都可配置是否上传
4. 长度根据拓展数据变化

### (0x11) 设备状态 :id=0x11


**功能描述**

设备状态描述设备当前的状态。



**写请求数据格式**

length+key+value

| byte No. | Parameter | Type | Converter | Description                                                                                                                                                                             |
| --- | --- | --- | --- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1 | length |  byte |  | key+value                                                                                                                                                                               |
| 2 | key | byte |  | key                                                                                                                                                                                     |
| 3-6 | dateTime | uint | unixTime | 打包时间，定位后需要打包的UTC时间戳                                                                                                                                                                     |
| 7 | mode | byte |  | 设备应用模式，目前1-6 |
| 8 | batteryLevel | byte |  | 电池电量，范围：0-100                                                                                                                                                                           |
| 9 | networkType | byte | bits | 网络类型，bit0-6<br />0-无服务<br />2-2G网络<br />3-3G网络<br />4-4G网络<br />10-cat-m网络<br />11-nb<br />30-wifi网络<br />bit7-置1有VOLTE，0为无VOLTE                                                        |
| 10 | networkSignal | byte |  | 网络信号，范围：0-31,无服务需要配置为0                                                                                                                                                                  |
| 11-12 | networkBand | ushort | bits | 网络频段，网络类型4G：1,2,3....表示B1、B2、B3....<br />网络类型3G：频点原始值925-2170<br />网络类型2G：频点原始值925-970<br />网络类型0：无服务<br />网络类型cat-M:1,2,3....表示B1、B2、B3....<br />网络类型nb:1,2,3,....表示B1、B2、B3....<br />网络类型wifi：0为2.4G，1为5G<br /> |
| 12-n | status | array | bits | 状态<br />bit0:1为gps定位<br />bit1:1为gsm定位<br />bit2:1为蓝牙信标定位<br />bit3:1为wifi信标定位<br />bit4:1为homewifi定位<br />bit5:1为homebeacon定位<br />bit6:1为beacon定位<br />bit7:1为充电状态<br />bit8:1为设备动，0为设备静止<br />bit9:1为历史数据，0为实时数据<br />bit10:1为佩戴，否则无 |

---

Note:

1. 网络类型：VOLTE的是值设备查询对应的apn对应的服务id，查询到服务id有获取到注册ip地址
2. simcom除了2G模块的网络信号是RSRP并不是CSQ，其它模块需要问原厂
3. ​在家和离家状态由服务前端判断homebeacon和homewifi、蓝牙定位在家状态






### (0x20) GPS定位记录 :id=0x20


**功能描述**

记录GPS工作过程中所获取到的位置信息。



**写请求数据格式**

length+key+value

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-6 | lat | float | loc | latitude, type: float，GPS经度 |
| 7-10 | lng | float | loc | longitude, type float，GPS纬度 |
| 11-12 | speed | ushort |  | GPS速度，单位: km/h |
| 13-14 | direction | ushort |  | 设备移动方向, 单位:度, 范围：[0, 359] <br>以`方向北`为起点0 |
| 15-16 | altitude | short |  | 海拔高度, 单位: 米,有负海拔 |
| 17-18 | hdop | ushort |  | 定位精度因子，放大了十倍，服务器缩小十倍显示小数点 |
| 19-22 | mileage | uint | | GPS更新上个定位点和本次点距离超过100m进行累加，单位m |
| 23 | number of satellites | byte | | GPS可见星数 |

Note:
1. hdop水平精度因子，从`GPGGA`中获取
2. mileage里程，初始化里程在`配置命令`中，每次更新计算新的移动距离进行累加
3. ​

---

### (0x21) 基站定位记录 :id=0x21


**功能描述**

GSM模组通过获取当前注册所在基站的位置信息，提供给服务器来获取设备所在的区域位置范围。

一般服务器要去访问高德、百度、谷歌第三方服务器才能获取到定位位置

**写请求数据格式**

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-4 | mcc | ushort |  | Mobile Country Code,移动国家代码 |
| 5-6 | mnc | ushort |  | 移动网络号码 |
| 7 | rxl | sbyte |  | RX level value for base station selection |
| 8-9 | lac | ushort |  | LAC（Location Area Code）是一个位置区域码 |
| 10-13 | cellId | uint |  | Service-cell Identify |

---

Note:

1. 对于 **3G基站和4G基站**，其LAC取值有所不同。对于3G基站，其LAC值应大于等于40960，而对于4G基站，其LAC值则小于40960且其CID值大于65535
2. 设备可以携带其他定位key+基站定位key,由服务器做第三方纠偏动作
3. ​

### (0x22)蓝牙定位记录 :id=0x22

**功能描述**

蓝牙定位，指设备走连接蓝牙后询问带定位信息且走ab协议的设备，该定位信息由客户通过手机app或者设备通过协议配置好定位信息，定位信息准确度由配置者确定，通过连接配件获取到的位置信息和描述符。

**写请求数据格式**

length+key+value

| byte No. | Parameter   | Type | Converter | Description                        |
| -------- | ----------- | ---- | --------- | ---------------------------------- |
| 1        | length      | byte |           | key+value                          |
| 2        | key         | byte |           | key                                |
| 3-8      | mac         | MAC  | leMac     | 信标的mac地址                      |
| 9-12     | lat         | float | loc       | 设备里信标的经度float，占用4个字节 |
| 13-16    | lng         | float | loc       | 设备里信标的纬度float，占用4个字节 |
| 17-n     | description | String | utf8      | 最大字节为32字符,结束符'\0'        |

Note:

1. 设备做从机，目前只能一对一连接
2. 可以作为在家定位基准


### (0x23)homebeacon定位记录 :id=0x23

**功能描述**

homebeacon信标定位指蓝牙信标把对应的mac信息，位置，描述等信息写到设备数据库。通过定位扫描周变广播信标解析私有协议匹配到mac地址认为是homebeacon

**写请求数据格式**

length+key+value

| byte No. | Parameter    | Type   | Converter   | Description                                                  |
| -------- | ------------ | ------ | ----------- | ------------------------------------------------------------ |
| 1        | length       | byte   |             | key+value                                                    |
| 2        | key          | byte   |             |                                                              |
| 3-8      | mac          | MAC    | leMac       | 信标的mac地址                                                |
| 9        | rssi         | sbyte  |             | 蓝牙信标的RSSI                                               |
| 10       | index        | byte   |             | 信标的偏移量，范围1-20                                       |
| 11-12    | type         | ushort | bits        | bit0: 置1有温度，否则无<br />bit1:置1有电量，否则无<br />Bit2:置1有经纬度，否则无<br />bit3:置1有描述符，否则无<br />bit4-15预留 |
| 13       | batteryLevel | byte   |             | 范围0-100，占一个字节                                        |
| 14-15    | temperature  | short  | temperature | 摄氏度*10，设备上传放大十倍，服务器显示需要缩小，占两个字节  |
| 16-19    | lat          | float  | loc         | 设备里信标的经度float，占用4个字节                           |
| 20-23    | lng          | float  | loc         | 设备里信标的纬度float，占用4个字节                           |
| 24-n     | description  | String | utf8        | 最大字节为32字符,结束符'\0'                                  |
|          |              |        |             |                                                              |
|          |              |        |             |                                                              |

Note:

1. type的类型是一个可组合且会改变后面的顺序
2. 多个beacon每个都带有key+len+value
3. 可以作为在家定位基准

### (0x24)beacon定位记录 :id=0x24

**功能描述**

beacon信标定位指设备需要定位扫描周边广播蓝牙信标，设备集成了私有广播协议进行对广播数据解析对应电量，温度，经纬度，描述符等数据。没有私有数据不进行解析。可拓展其他标准协议或者其他私有协议

**写请求数据格式**

length+key+value

| byte No. | Parameter    | Type   | Converter   | Description                                                  |
| -------- | ------------ | ------ | ----------- | ------------------------------------------------------------ |
| 1        | length       | byte   |             | key+value                                                    |
| 2        | key          | byte   |             |                                                              |
| 3-8      | mac          | MAC    | leMac       | 信标的mac地址                                                |
| 9        | rssi         | sbyte  |             | 蓝牙信标的RSSI                                               |
| 10       | index        | byte   |             | 信标的偏移量，范围1-20                                       |
| 11-12    | type         | ushort | bits        | bit0: 置1有温度，否则无<br />bit1:置1有电量，否则无<br />Bit2:置1有经纬度，否则无<br />bit3:置1有描述符，否则无<br />bit4-15预留 |
| 13       | batteryLevel | byte   |             | 范围0-100，占一个字节                                        |
| 14-15    | temperature  | short  | temperature | 摄氏度*10，设备上传放大十倍，服务器显示需要缩小，占两个字节  |
| 16-19    | lat          | float  | loc         | 设备里信标的经度float，占用4个字节                           |
| 20-23    | lng          | float  | loc         | 设备里信标的纬度float，占用4个字节                           |
| 24-n     | description  | String | utf8        | 最大字节为32字符,结束符'\0'                                  |
|          |              |        |             |                                                              |
|          |              |        |             |                                                              |

Note:

1. 该定位key可定制标准beacon或者eview私有数据beacon
2. 多个beacon每个都带有key+len+value
3. 如有定制beacon需要其他定位拼接上传，由服务器做其他数据处理

### (0x25)homewifi定位记录 :id=0x25

**功能描述**

homewifi信标定位是指将周围信标信息保存在设备。在需要定位过程中，设备扫描附近的信标, 如果扫描附近的信标后适配上本地数据库视为homewifi。

对于没有经纬度和描述符的信标，一般服务器要去访问高德、百度、谷歌第三方服务器才能获取到定位位置

**写请求数据格式**

| byte No. | Parameter   | Type   | Converter | Description                                                  |
| -------- | ----------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length      | byte   |           | key+value                                                    |
| 2        | key         | byte   |           |                                                              |
| 3-8      | mac         | MAC    | leMac     | wifi的mac地址                                                |
| 9        | rssi        | sbyte  |           | wifi的信号值，signed char 有符号数据类型-127-128             |
| 10       | index       | byte   |           | index 1～15                                                  |
| 11       | type        | byte   | bits      | bit0:1有经纬度，否则无<br />bit1:1有描述符，否则无<br />Bit2:1有wifi的SSID，否则无<br />Bit3-7:预留 |
| 12-15    | lat         | float  | loc       | 设备里信标的经度float                                        |
| 16-19    | lng         | float  | loc       | 设备里信标的纬度float                                        |
| 20-n     | description | String | utf8      | m最大字节为64字符,结束填'\0'                                 |
| n+21+m   | ssid        | String | utf8      | n最大字节为64字符,结束填'\0'                                 |

------

Note:

1. 如无经纬度，需要由服务器获取第三方服务获取
2. 需要上传多个wifi拼包多组len+key+value
3. 后面有协议拓展加入需要按照顺序，拓展需要在type标明拓展协议类型
4. 设备匹配原则有mac优先级，其次是ssid
5. 可以作为在家定位基准

### (0x26)wifi定位记录 :id=0x26

**功能描述**

wifi信标定位记录是在设备需要定位，设备扫描附近的wifi信标信息上传到服务器。

由服务器要去访问高德、百度、谷歌第三方服务器才能获取到定位位置


**写请求数据格式**

| byte No. | Parameter   | Type   | Converter | Description                                                  |
| -------- | ----------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length      | byte   |  | key+value                                                    |
| 2        | key         | byte   |  |                                                              |
| 3-8      | mac         | MAC    | leMac     | wifi的mac地址                                                |
| 9        | rssi        | sbyte  |  | wifi的信号值，signed char 有符号数据类型-127-128             |
| 10       | index       | byte   |  | index 1～15                                                  |
| 11 | type | byte | bits | bit0:1有ssid,否则无<br />bit1-7：预留 |
| 12+n | ssid        | String | utf8      | n最大字节为64字符,结束填'\0'                                 |

---

Note:

1. 需要上传多个wifi拼包多组len+key+value

2. 后面有协议拓展加入需要按照顺序，拓展需要在type标明拓展协议类型









### (0x40) 通话记录 :id=0x40


**功能描述**

通讯记录包含GSM通讯模组使用过程中的：
* 未接来电
* 拨号未接
* 通话



**写请求数据格式**

| byte No. | Parameter   | Type | Converter | Description                                                                                                                              |
|----------|-------------|------| --------- |------------------------------------------------------------------------------------------------------------------------------------------|
| 1        | length      | byte |  |                                                                                                                                          |
| 2        | key         | byte |  |                                                                                                                                          |
| 3-6      | dateTime    | uint | unixTime  | 拔打时间/来电时间                                                                                                                                |
| 7        | callSource  | byte |  | 0- 来电, <br />1- side key拨号，<br />2-跌倒拨号，<br />3-倾斜拨号，<br />4-动报警拨号，<br />5-不动报警拨号，<br />6-wefare报警拨号，<br />7-sos报警拨号，<br />8-短信拨号，<br />9-shell测试拨号，<br />10-服务器拨号，11-蓝牙拨号（短信发起拨打/或者复位器） |
| 8        | callStatus  | byte |  | 状态: 0- 接听(拨打);1-自动接听(来电)；<br />2-手动接听(来电) 3-未接听; 4-播不通;<br />255-未知错误                                                                    |
| 13-16   | duration    | uint |  | 时长, 单位:秒;<br /> 未接听时为响铃时长; <br />已接听表示通话时长                                                                                               |
| 17-n    | phoneNumber | String | utf8      | 对方号码，结束为\n                                                                                                                               |




### (0x50)每天步数记录 :id=0x50


**功能描述**

记录设备当天使用过程中产生的步数统计。



**写请求数据格式**

| byte No. | Parameter | Type | Converter | Description         |
| -------- | --------- | ---- | --------- | ------------------- |
| 1        | length    | byte |  | key+value           |
| 2        | key       | byte |  |                     |
| 3-6      | dateTime  | uint | unixTime  | 步数对应的日期, UTC |
| 7-10     | steps     | uint |  | 当天的步数累加和    |

Note:

1. 跨天需要清零

### (0x51)每小时步数记录 :id=0x51


**功能描述**

记录设备使用过程中，每小时产生的步数累加。



**写请求数据格式**

| byte No. | Parameter | Type | Converter | Description     |
|----------|-----------|------| --------- |-----------------|
| 1        | length    | byte |  | key+value |
| 2        | key       | byte |  |                 |
| 3-6      | dateTime | uint | unixTime  | 步数对应的日期、小时, UTC |
| 7-10     | steps     | uint |  | 步数              |

---


### (0x52)每分钟步数记录 :id=0x52


**功能描述**

记录设备使用过程中，每分钟产生的步数累加。



**写请求数据格式**

| byte No. | Parameter    | Type | Converter | Description        |
|----------|--------------|------| --------- |--------------------|
| 1        | length       | byte |  | key+value |
| 2        | key          | byte |  |                    |
| 3-6      | dateTime    | uint | unixTime  | 步数对应的日期、小时、分钟, UTC |
| 7        | minute0Steps | byte |  | 开始时间第0分钟内产生的步数     |
| 9        | minute1Steps | byte |  | 开始时间第1分钟内产生的步数     |
| 9        | minute2Steps | byte |  | 开始时间第2分钟内产生的步数     |
| ...      | ...          | ...  | ...      | ....               |
| n        | minuteNSteps | byte |  | 开始时间第n-1分钟内 产生的步数  |

Note:
1. 每分钟记录步数，使用的是连续时间记录。
2. 当连接5分钟没有步数产生时，则自动存储为1条记录。
3. 最小连续记录1分钟。开始时间内无数据产生的自动移除。
4. 最大连续记录30分钟步数，当满30分钟后，自动存储为1条记录。
5. 人的步频一般为2-4，单字节应该能满足1分钟内步数记录。

---

### (0x53) 活跃度记录 :id=0x53

**功能描述**

指设备在一定时间内累计设备的加速度累计波动，由应用获取sensor的三轴加速度计算加速度 - 0.98(重力加速度)

**写请求数据格式**

| byte No. | Parameter | Type | Converter | Description |
| -------- | --------- | ---- | --------- | ----------- |
| 1        | length    | byte |           | key+value   |
| 2        | key       | byte |           |             |
| 3-6      | dateTime  | uint | unixTime  | utc的时间戳 |
| 7-10     | activity  | uint |           |             |

Note:

1. 不同的满量程还是不一样
2. 设备五分钟记录一次

### (0x54)静止记录 :id=0x54


**功能描述**

记录设备使用过程中静止的时长。



**写请求数据格式**

| byte No. | Parameter   | Type | Converter | Description         |
| -------- | ----------- | ---- | --------- | ------------------- |
| 1        | length      | byte |  | key+value           |
| 2        | key         | byte |  |                     |
| 3-6      | dateTime    | uint | unixTime  | 静止记录时间戳, UTC |
| 7-10     | idleMinutes | uint |  | 连续静止的分钟数    |

---


### (0x60)心率测量记录 :id=0x60


**功能描述**

记录设备使用过程中，心率测量工作期间测量到的数据。



**写请求数据格式**

| byte No. | Parameter       | Type | Converter | Description                                                  |
| -------- | --------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length          | byte |  | key+value                                                    |
| 2        | key             | byte |  |                                                              |
| 3-6      | dateTime        | uint | unixTime   | 时间戳                                                       |
| 7        | measurementType | byte |  | 类型 <br>0-定时测量 <br>1-手动测量(屏幕)<br>2-远端控制测量, USB/BLE/短信/电话DTMF/服务器 |
| 8        | average         | byte |  | 心率值平均值                                                 |
| 9        | min             | byte |  | 心率最小值                                                   |
| 10       | max             | byte |  | 心率最大值                                                   |
| 11       | signalQuality   | byte |  | 本次测量的信号质量                                           |

---


### (0x61) 血氧测量记录 :id=0x61


**功能描述**

记录设备使用过程中，血氧测量工作期间测量到的数据。



**写请求数据格式**

| byte No. | Parameter       | Type | Converter | Description                                                  |
| -------- | --------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length          | byte |  | key+value                                                    |
| 2        | key             | byte |  |                                                              |
| 3-6      | dateTime        | uint | unixTime   | 时间戳                                                       |
| 7        | measurementType | byte |  | 类型 <br>0-定时测量 <br>1-手动测量(屏幕)<br>2-远端控制测量, USB/BLE/短信/电话DTMF/服务器 |
| 8        | average         | byte |  | 血氧平均值                                                   |
| 9        | min             | byte |  | 血氧最小值                                                   |
| 10       | max             | byte |  | 血氧最大值                                                   |
| 11       | signalQuality   | byte |  | 本次测量的信号质量                                           |

---

### (0x62)温度测量记录 :id=0x62


**功能描述**

记录设备使用过程中，温度测量工作期间测量到的数据。



**写请求数据格式**

| byte No. | Parameter       | Type | Converter | Description                                                  |
| -------- | --------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length          | byte |  | key+value                                                    |
| 2        | key             | byte |  |                                                              |
| 3-6      | dateTime        | uint | unixTime   | 时间戳                                                       |
| 7        | measurementType | byte |  | 类型 <br>0-测量失败<br>1-定时测量 <br>2-手动测量(屏幕)<br>3-远端控制测量, USB/BLE/短信/电话DTMF/服务器 |
| 8-11     | average         | float | temperature | 温度平均值, float                                            |
| 12-15    | min             | float | temperature | 温度最小值, float                                            |
| 16-19    | max             | float | temperature | 温度最大值, float                                            |

---

### (0xC0)设备复位原因 :id=0xC0

**功能描述**

由设备异常导致hardfault记录的程序栈回溯信息保存在flash里。开机后由开发人员主动读取。由设备拿到信息定位复位原因





**写请求数据格式**

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-n | string | array | utf-8 | 以结束符'\0' |

Note:

1. 复位原因记录信息可能超过255个字节，目前只能在头里计算内容
2. 设备不主动发送复位原因

### ()


**功能描述**





**写请求数据格式**

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | length |
| 2 | key | byte |  | key |
| 3-n |  |  |  |  |

### ()


**功能描述**





**写请求数据格式**

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | length |
| 2 | key | byte |  | key |
| 3-n |  |  |  |  |



