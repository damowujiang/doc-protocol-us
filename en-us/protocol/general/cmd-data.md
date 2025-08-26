Data 

---

The data records include the following:
* Health data generated during device usage
* Device status information
* Device usage logs

The data records are organized by category:
* Device identity information – used to identify the device’s uniqueness

* Device status changes – such as network status changes

* Location information – positioning records obtained from different modules

* Health records – such as step count, heart rate, and sleep data, used to analyze and review the user’s health
  condition

* Third-party health data sources – health data uploaded from third-party accessories

* Device usage logs – records of device usage status and key events

  

---

## Key List
| Value<br>(HEX) | Parameter           | Function Description                            | 可写 | 可读 | 通知 |
| -------------- | ------------------- | ----------------------------------------------- | ---- | ---- | ---- |
| [0x01](#_0x01) | DeviceId            | Device ID                                       | ❌    | ❌    | ✅    |
| [0x02](#_0x02) | CustomizedId        | User-defined ID                                 | ❌    | ❌    | ✅    |
| [0x03](#_0x03) | Iccid               | Device ICCID                                    | ❌    | ❌    | ✅    |
| [0x10](#_0x10) | AlarmCode           | Alarm code record                               | ❌    | ❌    | ✅    |
| [0x11](#_0x11) | DeviceStatus        | Device status record                            | ❌    | ❌    | ✅    |
| [0x20](#_0x20) | Gps                 | GPS positioning record                          | ❌    | ❌    | ✅    |
| [0x21](#_0x21) | Gsm                 | Cell tower positioning record                   | ❌    | ❌    | ✅    |
| [0x22](#_0x22) | Ble                 | BLE positioning record                          | ❌    | ❌    | ✅    |
| [0x23](#_0x23) | Homebeacon          | Home Beacon positioning record                  | ❌    | ❌    | ✅    |
| [0x24](#_0x24) | Beacon              | Beacon positioning record                       | ❌    | ❌    | ✅    |
| [0x25](#_0x25) | Homewifi            | home wifi positioning record                    | ❌    | ❌    | ✅    |
| [0x26](#_0x26) | Wifi                | Wi-Fi positioning record                        | ❌    | ❌    | ✅    |
| [0x40](#_0x40) | CallRecord          | Call record                                     | ❌    | ❌    | ✅    |
| [0x41](#_0x41) | BleConnection       | BLE connection record                           | ❌    | ❌    | ✅    |
| [0x42](#_0x42) | AccessoryStatus     | Accessory status                                | ❌    | ❌    | ✅    |
| [0x50](#_0x50) | DailyStep           | Daily step count                                | ❌    | ❌    | ✅    |
| [0x51](#_0x51) | HourlyStep          | Hourly step count record                        | ❌    | ❌    | ✅    |
| [0x52](#_0x52) | HourlyActivity      | Hourly activity record                          | ❌    | ❌    | ✅    |
| [0x53](#_0x53) | Activity            | Activity record                                 | ❌    | ❌    | ✅    |
| [0x54](#_0x54) | StaticData          | Inactivity record                               | ❌    | ❌    | ✅    |
| [0x60](#_0x60) | HeartRate           | Heart rate measurement record                   | ❌    | ❌    | ✅    |
| [0x61](#_0x61) | BloodOxygen         | Blood oxygen measurement record                 | ❌    | ❌    | ✅    |
| [0x62](#_0x62) | HRV                 | HRV (Heart Rate Variability) measurement record | ❌    | ❌    | ✅    |
| [0xA0](#_0xA0) | ThirdPartyAccessory | Third-party accessory health measurement record | ❌    | ❌    | ✅    |
| [0xC0](#_0xC0) | DeviceResetReason   | Device reset reason                             | ❌    | ✅    | ❌    |



---

## Command Description

---

### (0x01)DeviceID :id=0x01


**Function Description**

The device ID is a unique identifier to the upper-level system/ backend server / platform.

All device data must be concatenated with the device ID

**Notification Data Format**

length+key+value

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-n | IMEI | String | ascii | IMEI, Type: String, length:[1, 32] |

Note:
1. The uploaded data packet consists of: `Standard Protocol Header` + `Device ID` + `Device Status Item` + `User-Defined Code Item (optional)` + `Device ICCID Item (optional)` + `Other Record Data Items`
2. The data response packet consists of: `Standard Protocol Header` + `Data Command`. If the response is normal, the current historical data will be cleared.
3. When reporting data, a single data frame must contain the IMEI number, and the first item must follow the
   `length–key–value` format to allow the server to quickly parse and verify whether the device is valid.

---


### (0x02) User-Defined ID :id=0x02

**Function Description**

The device has a Custom Device ID , which is an additional identification code defined by the server.

Once configured in the server settings, this ID will be automatically activated.
Whenever the device communicates with the server, it will include this code in the data transmission.

**Notification Data Format**

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-n | customDeviceID | String | ascii | length:[0, 32] |




---


### (0x03)ICCID :id=0x03

**Function Description**
The ICCID (Integrated Circuit Card Identifier) is used to uniquely identify the SIM card.

**Notification Data Format**

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-n | ICCID | String | ascii | ICCID string |

---

### (0x10) Alarm Data Record :id=0x10

**Function Description**

Whenever there is a change in an emergency, critical, or subscription event, the device should promptly report the corresponding status information to the server.

**Notification Data Format**

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-6 | dateTime | uint | unixTime | Alarm Timestamp — the time when the alarm was triggered, in Unix time format (seconds) |
| 7-n | statusEvent | byte | bits | Alarm Code (1 – 255) — Indicates the type of alarm<br/>event<br/>1: SOS alarm<br/>2: Fall detection alarm<br/>3: Tilt alarm<br/>4: Movement alarm<br/>5: Stationary alarm<br/>6: Leaving home alarm<br/>7: Returning home alarm<br />8: Power-on alarm<br/>9: Power-off alarm<br/>10: Low battery alarm<br/>11: Overspeed alarm<br/>12: Welfare alarm<br/>13: Check-in alarm<br/>14: Check-out alarm<br/>30: Geo-fence 1 exit alarm<br/>31: Geo-fence 1 entry alarm<br/>32: Geo-fence 2 exit alarm<br/>33: Geo-fence 2 entry alarm<br/>34: Geo-fence 3 exit alarm<br />35: Geo-fence 3 entry alarm<br/>36: Geo-fence 4 exit alarm<br/>37: Geo-fence 4 entry alarm<br/>50: SOS end alarm<br/>51: Fall detection end alarm<br/>52: Tilt end alarm<br/>100-199 Custom alarms<br/>200: Simulated alarm |

---

Note

1. If multiple alarms occur within the same time period, multiple items must be concatenated in the report.
2. The reporting of these alarms can be configured as enabled or disabled.
3. “Other Record Data Items” may include alarms, location records, call logs, step counts, heart rate, blood oxygen
   level, body temperature, and similar data.

---

### (0x11) Device Status Records::id=0x11

**Function Description**

Describes the current operating state of the device.  Each location data packet or alarm packet must include the following status information:Network status, operating mode, positioning type, battery level, device action, behavior information



**Notification Data Format**

| byte No. | Parameter | Type | Converter | Description                                                                                                                                                                             |
| --- | --- | --- | --- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1 | length |  byte |  | key+value                                                                                                                                                                               |
| 2 | key | byte |  | key                                                                                                                                                                                     |
| 3-6 | dateTime | uint | unixTime | Packaging Time — UTC timestamp (in Unix time) generated after positioning                                                                                       |
| 7 | mode | byte |  | Device Application Mode — Current mode value: 1–6; other values are reserved |
| 8 | batteryLevel | byte |  | Battery Level — Range: 0–100                                                                                                                                               |
| 9 | networkType | byte | bits | Network Type — Bits 0 – 6 indicate the<br/>network status:<br/>0 – No service<br />2 – 2G<br/>3 – 3G<br/>4 – 4G<br/>10 – Cat-M<br/>11 – NB-IoT<br/>30 – Wi-Fi<br/>Bit 7: 1 = VoLTE enabled, 0 = No VoLTE |
| 10 | networkSignal | byte |  | Network Signal Strength — Range: 0–31;<br/>Must be set to 0 when there is no service                                                                              |
| 11-12 | networkBand | ushort | bits | Network Band / Frequency — Status value<br/>depends on network type:<br/>4G: Band index (1, 2, 3… for B1, B2, B3…)<br />3G: Raw frequency value 925–2170<br/>2G: Raw frequency value 925–970<br/>0: No service<br/>Cat-M: Band index (1, 2, 3… for B1, B2, B3…)<br/>NB-IoT: Band index (1, 2, 3… for B1, B2, B3…)<br/>Wi-Fi: 0 = 2.4 GHz, 1 = 5 GHz |
| 12-n | status | array | bits | Status Flags — Bit-mapped status values:<br/>Bit 1: 1 = Charging<br/>Bit 2: 1 = Device in motion, 0 = Stationary<br/>Bit 3: 1 = Historical data, 0 = Real-time data<br/>Bit 4: 1 = Worn, 0 = Not worn |

---

Note:

1. `Network Type`: For VoLTE, the device queries the service ID corresponding to the APN. If a service ID is found, the device then obtains the registered IP address.
2. `Network Signal`: For SIMCom modules, except for 2G modules, the network signal is measured in RSRP rather
   than CSQ. For other modules, please consult the original manufacturer.
3. `Other Record Data Items`: May include alarms, location records, call logs, step counts, heart rate, blood oxygen
   level, body temperature, and similar data.

---


### (0x20) GPS Positioning Record :id=0x20

**Function Description**

Contains location data acquired during GPS operation.

**Notification Data Format**

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-6 | lat | float | loc | latitude, type: float |
| 7-10 | lng | float | loc | longitude, type float |
| 11-12 | speed | ushort |  | GPS Speed，单位: km/h |
| 13-14 | direction | ushort |  | Movement Direction, in degrees, range: [0–359], where<br/>0° = due north |
| 15-16 | altitude | short |  | Altitude, in meters, may be negative |
| 17-18 | hdop | ushort |  | HDOP (Horizontal Dilution of Precision) — value is multiplied by 10 in transmission; server divides by 10 for display |
| 19-22 | mileage | uint | | Accumulated when the distance between the last GPS point and the current point exceeds 100 m, unit: meters |
| 23 | number of satellites | byte | | Number of Visible Satellites |

Note:
1. `HDOP (Horizontal Dilution of Precision)`: Obtained from the GPGGA sentence.
2. `Mileage`: The initial mileage is defined in the configuration command. Each time an update occurs, the newly
   calculated movement distance is accumulated.

---

### (0x21) Base Station Positioning Record: id=0x21

**Function Description**

The cellular module obtains information from the currently registered cell tower(s) and provides it to the server to determine the approximate location range of the device. Multiple cell tower records are supported.

In most cases, the server needs to query third-party services such as Amap (Gaode), Baidu, or Google in order to
resolve the positioning information.

**Notification Data Format**

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-4 | mcc | ushort |  | Mobile Country Code |
| 5-6 | mnc | ushort |  | Mobile Network Code (MNC)                                    |
| 7 | rxl | sbyte |  | RX level value for base station selection |
| 8-9 | lac | ushort |  | LAC（Location Area Code） LAC is a location<br/>area code used to identify a specific location<br/>area within the mobile network. |
| 10-13 | cellId | uint |  | Service-cell Identify |

---

Note:

1. LAC Values for 3G and 4G Base Stations: For 3G base stations, the LAC value must be greater than or equal to 40960. For 4G base stations, the LAC value must be less than 40960, and the CID value must be greater than 65535.
2. Hybrid Positioning Keys: The device can carry both other positioning keys and the base station positioning key, allowing the server to perform
   third-party correction (offset adjustment).
3. Multiple Base Station Scanning:  The device should scan multiple nearby base stations to avoid risks caused by an incomplete or inaccurate single
   base station database, or cases where parsing fails.

---

### (0x22)Bluetooth Positioning Record :id=0x22

**Function Description**

Bluetooth Positioning, Refers to the scenario where the device connects via Bluetooth to another device that provides location information using the AB protocol. The location information is configured either by the customer through the mobile app or by the device through the protocol. The accuracy of the location information is determined by the party who performs the configuration. The device obtains both the location information and its descriptors through the connected accessory.

**Notification Data Format**

| byte No. | Parameter   | Type | Converter | Description                        |
| -------- | ----------- | ---- | --------- | ---------------------------------- |
| 1        | length      | byte |           | key+value                          |
| 2        | key         | byte |           | key                                |
| 3-8      | mac         | MAC  | leMac     | Ble beacon MAC Address                                       |
| 9-12     | lat         | float | loc       | Beacon Longitude (float) — 4 bytes |
| 13-16    | lng         | float | loc       | Beacon Latitude (float) — 4 bytes |
| 17-n     | description | String | utf8      | Maximum length: 32 characters, ending with '\0' (null terminator) |

Note:

1. Device as Slave Mode: The device operates in slave mode and currently supports only one-to-one connections.
2. The BLE positioning accuracy is fixed by the server at 5 meters. Since the device functions as a slave, RSSI is not required to be included. The connection does not necessarily need to be continuously broadcast.

---


### (0x23)homebeacon positioning record :id=0x23

**Function Description**

Home Beacon Positioning, Refers to the process where a Bluetooth beacon writes its corresponding MAC address, location, description, and related information into the device database. Through positioning, the device scans surrounding broadcast beacons and parses them using a private protocol. The device then packages key information and uploads it to the server.

**Notification Data Format**

| byte No. | Parameter    | Type   | Converter   | Description                                                  |
| -------- | ------------ | ------ | ----------- | ------------------------------------------------------------ |
| 1        | length       | byte   |             | key+value                                                    |
| 2        | key          | byte   |             |                                                              |
| 3-8      | mac          | MAC    | leMac       | Beacon MAC Address                                           |
| 9        | rssi         | sbyte  |             | Bluetooth Beacon RSSI                                        |
| 10       | index        | byte   |             | Beacon Offset — Range: 1–20                                  |
| 11-12    | type         | ushort | bits        | Beacon Data Flags (bit-mapped):<br />Bit 0: 1 = Battery available, 0 = No battery info<br/>Bit 1: 1 = Temperature available, 0 = No temperature info<br/>Bit 2: 1 = Latitude/Longitude available, 0 = No location info<br/>Bit 3: 1 = Descriptor available, 0 = No descriptor<br/>Bit 4: 1 = Beacon name available, 0 = No beacon name<br/>Bits 5–15: Reserved |
| 13       | batteryLevel | byte   |             | range: 0-100                                                 |
| 14-15    | temperature  | short  | temperature | unit: ℃/10                                                   |
| 16-19    | lat          | float  | loc         | Beacon Latitude                                              |
| 20-23    | lng          | float  | loc         | Beacon longitude                                             |
| #string  | description  | String | utf8        |                                                              |

---

### (0x24)beacon positioning record :id=0x24

**Function Description**

Beacon positioning refers to the device scanning and locating nearby Bluetooth beacons. The device integrates a proprietary broadcasting protocol to parse broadcast data corresponding to power, temperature, latitude and longitude, descriptors, and other data. Data that is not proprietary will not be parsed. Other standard protocols or proprietary protocols can be added.

**Notification Data Format**

length+key+value

| byte No. | Parameter    | Type   | Converter   | Description                                                  |
| -------- | ------------ | ------ | ----------- | ------------------------------------------------------------ |
| 1        | length       | byte   |             | key+value                                                    |
| 2        | key          | byte   |             |                                                              |
| 3-8      | mac          | MAC    | leMac       | beacon MAC address                                           |
| 9        | rssi         | sbyte  |             | RSSI of beacon                                               |
| 10       | index        | byte   |             | index, range: 1-20                                           |
| 11-12    | type         | ushort | bits        | bit0: =1 with temperature info<br />bit1: =1 with battery info of  beacon<br />Bit2: =1, with location info<br />bit3: =1, with description<br />bit4-15, reserved |
| 13       | batteryLevel | byte   |             | range: 0-100                                                 |
| 14-15    | temperature  | short  | temperature | unit: ℃/10                                                   |
| 16-19    | lat          | float  | loc         | latitude                                                     |
| 20-23    | lng          | float  | loc         | longitude                                                    |
| 24-n     | description  | String | utf8        |                                                              |

Note:

1. This positioning key can be customized for standard beacons or Eview private data beacons.

---

### (0x25)Home Wi-Fi Location Record :id=0x25

**Function Description**

Home Wi-Fi beacon-based positioning refers to storing nearby beacon (Wi-Fi) information on the device. During the
positioning process, the device scans for surrounding beacons. If the scanned beacons match the locally stored database, the location is recognized as a "home Wi-Fi" environment.
If a beacon lacks coordinate data (latitude and longitude) and textual location descriptors, the server must access external location services (such as Amap, Baidu, or Google Maps) to resolve the beacon’s actual location.

**Notification Data Format**

| byte No. | Parameter   | Type   | Converter | Description                                                  |
| -------- | ----------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length      | byte   |           | key+value                                                    |
| 2        | key         | byte   |           |                                                              |
| 3-8      | mac         | MAC    | leMac     | MAC address of theWiFi                                       |
| 9        | rssi        | sbyte  |           | WiFi signal strength                                         |
| 10       | index       | byte   |           | index:: 1～15                                                |
| 11       | type        | byte   | bits      | Bit 0: 1 = latitude/longitude available, 0 =<br/>no latitude/ longitude available<br/>Bit 1: 1 = description available, 0 = No<br/>description<br/>Bit 2: 1 = SSID available, 0 = No SSID<br /> Bit 3-7: Reserved |
| 12-15    | lat         | float  | loc       | Beacon latitude in device, controlled by bit0                |
| 16-19    | lng         | float  | loc       | Beacon longitude in device, controlled by bit0               |
| 20-n     | description | String | utf8      | size:[0, 64]                                                 |
| n+21+m   | ssid        | String | utf8      | size:[0, 64]                                                 |

------

Note:

1. All fields, except latitude/longitude and description (which are device configuration parameters), are reported via broadcasts from the slave device. Latitude and longitude, if present, are assumed to have maximum precision (~5meters). If latitude and longitude are not included, the server may optionally access third-party location services to resolve the coordinates.
2. Multiple Home WiFi items may be uploaded in one or more concatenated packets, each following the `len + key + value` format.
3. Bits in the type field are optional flags; when all optional bits are set, the items should follow the protocol-defined order, and for composite items, the packet order of sub-items must be matched manually.
4. The device matching principle is mac priority, followed by `ssid`.
5. Can be used as a positioning reference at home

---

### (0x26)WiFi location records :id=0x26

**Function Description**

WiFi beacon location records are generated when the device requires positioning. The device scans nearby WiFi  beacons and uploads the data to the server. To resolve the actual location, the server may access third-party location services such as Amap, Baidu, or Google Maps.

**Notification Data Format**

| byte No. | Parameter   | Type   | Converter | Description                                                  |
| -------- | ----------- | ------ | --------- | ------------------------------------------------------------ |
| 1        | length      | byte   |  | key+value                                                    |
| 2        | key         | byte   |  |                                                              |
| 3-8      | mac         | MAC    | leMac     | MAC address of theWiFi                          |
| 9        | rssi        | sbyte  |  | WiFi signal strength |
| 10       | index       | byte   |  | index 1～15                                                  |
| 11 | type | byte | bits | bit0:1有ssid,否则无<br />bit1-7：预留 |
| 12+n | ssid        | String | utf8      | n最大字节为64字符,结束填'\0'                                 |

---

### (0x40)  Call Record :id=0x40


**Function Description**

Communication records include call-related information from the cellular module, such as:
* Missed incoming calls
* Outgoing calls not answered
* Completed calls

**Notification Data Format**

| byte No. | Parameter   | Type | Converter | Description                                                                                                                              |
|----------|-------------|------| --------- |------------------------------------------------------------------------------------------------------------------------------------------|
| 1        | length      | byte |  |                                                                                                                                          |
| 2        | key         | byte |  |                                                                                                                                          |
| 3-6      | dateTime    | uint | unixTime  | Call or incoming time                                                                                                           |
| 7        | callSource  | byte |  | 0 - Incoming call<br/>1 - Side button dial<br />2 - Fall dial<br/>3 - Tilt dial<br/>4 - Motion alarm dial<br/>5 - No-motion alarm dial<br/>6 - Welfare alarm dial<br/>7 - SOS alarm dial<br/>8 - SMS dial<br/>9 - Shell test dial<br/>10 - Server dial<br/>11 - Bluetooth dial (initiated by SMS or resetter) |
| 8        | callStatus  | byte |  | Status:<br/>0 - Answered (outgoing)<br />1 - Auto answered (incoming)<br/>2 - Manual answered (incoming)<br/>3 - Missed<br/>4 - Cannot connect<br/>255 - Unknown error |
| 9 | hangupSource | byte | | Status:<br/>0 - Physical button hangup (manual)<br/>1 - Screen trigger hangup (manual)<br/>2 - Auto hangup (device)<br/>3 - Passive hangup (remote or network)<br/>255 - Unknown hangup (call immediately disconnected) |
| 10-13 | duration    | uint |  | Duration in seconds; <br />for missed calls, indicates ringing duration;<br/>for answered calls, indicates talk duration |
| 14-n    | phoneNumber | String | utf8      | Counterparty number                                                                                           |




### (0x50)Hourly Step Count Record :id=0x50


**Function Description**

The accumulated step count generated per hour during device usage.

**Notification Data Format**

| byte No. | Parameter | Type | Converter | Description         |
| -------- | --------- | ---- | --------- | ------------------- |
| 1        | length    | byte |  | key+value           |
| 2        | key       | byte |  |                     |
| 3-6      | dateTime  | uint | unixTime  | Date and hour for step count, UTC |
| 7-10     | steps     | uint |  | Step count |

---

### (0x51)Hourly Step Count Record: :id=0x51


**Function Description**

The accumulated step count generated per hour during device usage.

**Notification Data Format**

| byte No. | Parameter | Type | Converter | Description     |
|----------|-----------|------| --------- |-----------------|
| 1        | length    | byte |  | key+value |
| 2        | key       | byte |  |                 |
| 3-6      | dateTime | uint | unixTime  | Date and hour for step count, UTC |
| 7-10     | steps     | uint |  | Step count    |

---


### (0x52)Per-Minute Step Count Record: :id=0x52


**Function Description**

The accumulated step count generated per minute during device usage.

**Notification Data Format**

| byte No. | Parameter    | Type | Converter | Description        |
|----------|--------------|------| --------- |--------------------|
| 1        | length       | byte |  | key+value |
| 2        | key          | byte |  |                    |
| 3-6      | dateTime    | uint | unixTime  | Date, hour, and minute for the step data, UTC |
| 7        | minute0Steps | byte |  | Step count during the 0th minute from start time |
| 9        | minute1Steps | byte |  | Step count during the 1st minute from start time |
| 9        | minute2Steps | byte |  | Step count during the 2nd minute from start time |
| ...      | ...          | ...  | ...      | ....               |
| n        | minuteNSteps | byte |  | Step count during the (n-1)th minute from start time |

Note:
1. Step counts are recorded per minute using continuous time logging.
2. If no steps are detected for 5 consecutive minutes, a record is automatically stored as one item.
3. The minimum continuous record duration is 1 minute. Any intervals with no steps during the start time are
   automatically discarded.
4. The maximum continuous record duration is 30 minutes; once 30 minutes are reached, the record is automatically stored as one item.
5. Typical human step frequency is 2–4 steps per second; a single byte is sufficient to store steps per minute.

---

### (0x53) Activity Record :id=0x53

**Function Description**

Refers to the accumulated acceleration fluctuation of the device over a certain period, calculated by the application
using the sensor’s three-axis acceleration values: acceleration minus 0.98 m/s² (gravity).

**Notification Data Format**

| byte No. | Parameter | Type | Converter | Description   |
| -------- | --------- | ---- | --------- | ------------- |
| 1        | length    | byte |           | key+value     |
| 2        | key       | byte |           |               |
| 3-6      | dateTime  | uint | unixTime  | UTC timestamp |
| 7-10     | activity  | uint |           |               |

---

### (0x54)Inactivity Record :id=0x54

**Function Description**

Record the length of time the device remains inactive while in use.



**Notification Data Format** 

| byte No. | Parameter   | Type | Converter | Description         |
| -------- | ----------- | ---- | --------- | ------------------- |
| 1        | length      | byte |  | key+value           |
| 2        | key         | byte |  |                     |
| 3-6      | dateTime    | uint | unixTime  | Timestamp of the idle record, UTC |
| 7-10     | idleMinutes | uint |  | Number of consecutive idle minutes |

---


### (0x60) Heart Rate Measurement Record :id=0x60

**Function Description**

Record the data obtained during heart rate measurement with the device.



**Notification Data Format**

| byte No. | Parameter       | Type | Converter | Description                                                  |
| -------- | --------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length          | byte |  | key+value                                                    |
| 2        | key             | byte |  |                                                              |
| 3-6      | dateTime        | uint | unixTime   | timestamp                                                    |
| 7        | measurementType | byte |  | Measurement type<br/>0 – Scheduled measurement<br/>1 – Manual measurement (via device screen)<br/>2 – Remote-controlled measurement (triggered<br/>via USB, BLE, SMS, DTMF phone input, or server) |
| 8        | average         | byte |  | Average heart rate                               |
| 9        | min             | byte |  | Minimum heart rate                                 |
| 10       | max             | byte |  | Maximum heart rate                                 |
| 11       | signalQuality   | byte |  | Signal quality                             |

---


### (0x61) Blood Oxygen Measurement Record :id=0x61


**Function Description**

Record the data obtained during blood oxygen measurement with the device.

**Notification Data Format**

| byte No. | Parameter       | Type | Converter | Description                                                  |
| -------- | --------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length          | byte |  | key+value                                                    |
| 2        | key             | byte |  |                                                              |
| 3-6      | dateTime        | uint | unixTime   | Timestamp                                              |
| 7        | measurementType | byte |  | Measurement type<br/>0 – Scheduled measurement<br/>1 – Manual (via screen)<br/>2 – Remote-controlled (via USB, BLE, SMS,<br/>DTMF, or server) |
| 8        | average         | byte |  | Average blood oxygen (SpO₂ )                       |
| 9        | min             | byte |  | Minimum blood oxygen (SpO₂ )                       |
| 10       | max             | byte |  | Maximum blood oxygen (SpO₂ )                       |
| 11       | signalQuality   | byte |  | Signal quality of this measurement        |

---

### (0x62)Temperature Measurement Record::id=0x62


**Function Description**

Record the data obtained during temperature measurement with the device.

**Notification Data Format**

| byte No. | Parameter       | Type | Converter | Description                                                  |
| -------- | --------------- | ---- | --------- | ------------------------------------------------------------ |
| 1        | length          | byte |  | key+value                                                    |
| 2        | key             | byte |  |                                                              |
| 3-6      | dateTime        | uint | unixTime   | Timestamp                                              |
| 7        | measurementType | byte |  | Measurement type:<br/>0 – Measurement failed<br/>1 – Scheduled measurement<br/>2 – Manual (via screen)<br/>3 – Remote-controlled (via USB, BLE, SMS, DTMF, or server) |
| 8-11     | average         | float | temperature | float Average temperature value (°C), float format |
| 12-15    | min             | float | temperature | float Minimum temperature value (°C), float format |
| 16-19    | max             | float | temperature | float Maximum temperature value (°C), float format |

---

### (0xC0) Device Reset Reason :id=0xC0

**Function Description**

When a device experiences an exception causing a `hardfault`, the program stack trace information is stored in flash memory. After the device is powered on, the information can be actively read by developers. This data is used to identify the cause of the reset or fault.

**Notification Data Format**

| byte No. | Parameter | Type | Converter | Description |
| --- | --- | --- | --- | --- |
| 1 | length |  byte |  | key+value |
| 2 | key | byte |  | key |
| 3-n | string | array | utf-8 | 以结束符'\0' |

