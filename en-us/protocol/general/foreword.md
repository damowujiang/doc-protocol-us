# 前言 Foreword

本次重新整理的通讯协议，基于原来 EV07B 协议重新优化。
<br />The newly revised communication protocol is based on the original EV07B protocol and has been re-optimized.


## 本次协议的优化和修改内容有: The optimizations and modifications to this protocol include:


- [x] 优化协议命令功能描述 Optimizing the protocol command function description
- [x] 考虑功能模块化设计 Considering functional modular design
- [x] 预留功能参数扩展 Reserving function parameter expansion
- [x] 增强参数安全管理 Enhancing parameter security management



## 未变化的命令 Commands unchanged

- [X] DFU，修改为Device File Update

---

## 修改项 Modifications

### 1. 协议头的变化 Protocol header 

- [X] 协议头 版本号为 `1` Protocol header version number is `1`

---

### 2. 正响应回复 Positive response

- [X] 回复使用 `command`, 不携带参数。比如[配置命令](cmd-configuration.md)写参数的`正响应回复` 由之前的 `7F 01 00` 修改为 `02`.<br />Reply using `command` without parameters. For example, in [configuration command](cmd-configuration.md), the `positive response reply` for writing parameters is changed from the previous `7F 01 00` to `02`.
- [X] 原上位机使用 `id`来判断响应包对应的请求包仍然可用。 <br />The original host uses `tid` to determine whether the response packet corresponding to the request packet is still available.

---

### 3. 负响应代码扩展 Negative response code extension

- [X] 原负响应代码由 `Length-Key`回复，`len-key`即为错误代码。<br />The original negative response code is replied by `Length-Key`, and `len-key` is the error code.

- [X] 部分 `key`添加更多错误原因说明  <br />Some `key`s have more detailed explanations of error causes.

---

### 4. 增加定制功能命令使用 Add custom function commands

- [X] 增加定制功能来 配置/修改 定制功能业务逻辑。<br />Add custom functions to configure/modify custom function business logic.
- [X] 当定制功能经评审后，可并入标准分支时，配置参数移到[配置命令](cmd-configuration.md),原定制版本分支的配置参数保持不变。<br />When the custom function is reviewed and can be merged into the standard branch, the configuration parameters are moved to [configuration command](cmd-configuration.md), and the configuration parameters of the original custom version branch remain unchanged.


---

### 5. 配置keys Configure keys

- [X] 重新优化 `key`的分布位置，及预留空间扩展。<br />Reoptimize the distribution position of `key` and reserve space expansion.
- [X] 删除部分参数的组合控制。<br />Delete the combination control of some parameters.
- [X] 删除之前定制功能参数，后续有需求在`定制功能命令`中增加。<br />Remove previously customized function parameters; add them to the `customized function command` if needed in the future.

---

### 6. 历史数据 添加 更多数据类型 Add more data types to historical data

- [X] 优化历史数据类型。<br />Optimize the historical data type.
- [X] 添加历史数据更多记录类型。<br />Add more record types to historical data.

---

### 7. 添加更多的命令 Add more commands


- [X] 新增 `鉴权`命令 <br />Add `Authentication` command
- [X] 新增 `实验室`命令 <br />Add `Laboratory` command
- [X] 新增 `开发者`命令 <br />Add `Developer` command
- [X] 新增 `File`命令 <br />Add `File` command
- [X] 新增 `Log`命令 <br />Add `Log` command
- [X] 新增 `Memory`命令 <br />Add `Memory` command
- [X] 新增 `Application`命令 <br />Add `Application` command
- [x] 新增 `AdvanceConfiguation`命令 <br />Add `AdvanceConfiguation` command
- [x] 新增 `CustomConfiguration`命令 <br />Add `CustomConfiguration` command

具体命令作用，查看各命令文件中描述。<br />For specific command descriptions, please refer to the corresponding command files.


新增命令的影响，在工厂测试过程中，需要对比的数据不单止 `configuration`参数，还需要对比：<br />The impact of the new command is that during factory testing, the data to be compared is not limited to the `configuration` parameter, but also includes:

* `实验室`中的配置参数 <br />Configuration parameters in `Laboratory`
* `Application`中的定制功能参数 <br />Customized function parameters in `Application`
* `AdvanceConfiguation`中的配置参数 <br />Configuration parameters in `AdvanceConfiguation`
* `CustomConfiguration`中的配置参数 <br />Configuration parameters in `CustomConfiguration`

---

### 8. 删除加密部分 Remove encryption part


- [X] 之前的 `A5` 封装属性新的协议，其执行的是一个封包动作，在[加密协议](protocol/crypto/index.md)文档来描述。<br />The previous A5 encapsulation attribute is a new protocol that performs a packet action, as described in the [Encryption Protocol](protocol/crypto/index.md) documentation.

---

### 9.  命令协议的使用优化 Optimization of command protocol usage

- [X] 上位机端 仅发送 `command`，用于向设备查询当前 `command`所实现/已支持的 `keys`，不携带`indicate`属性, 否则视为上位机回复。 <br />The host only sends the `command` to query the device for the `keys` currently implemented/supported by the `command`, without carrying the `indicate` attribute, otherwise it is regarded as a response from the host.
- [X] 定制命令使用的分类：写/读/通知 <br />Classification  of custom commands: write/read/notify


---

### 10.  数据类型优化 Data type optimization


- [X] 常用经纬度数据修改为 `float`类型 <br />Common latitude and longitude data is modified to `float` type.
- [X] 删掉之前字符串需要长度字节指定的方式 <br />Delete the previous string using a specified length in bytes.
- [X] 添加字符串类型，要求 以'\0'结尾。字符串按顺序填入，某个字符串为空时使用'\0'占位。 <br />Add a string type, requiring that the string ends with '\0'. Strings are filled in order, and '\0' is used to placeholders when a string is empty.

