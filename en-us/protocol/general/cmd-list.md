# 命令列表 Command list


设备的常用命令列表如下： <br />The list of commonly used commands for the device is as follows:

| 命令  Command                            | 要求 Requirement <sup>1</sup> | 参数 Parameter                 | 功能 Function           | 说明  Description                                                     |
| :-------------------------------- | -------------------------- | :------------------- | :------------- | :--------------------------------------------------------- |
| [0x01](cmd-data.md)                  | M                          | Data                 | 设备数据 Device's Data      | 包含历史数据，定位、通话记录、计步等; 以及特定的数据类型。<br />Includes historical data, location, call logs, step counts, etc.; as well as specific data types. |
| [0x02](cmd-configuration.md)         | M                          | Configuration        | 设备配置 Device's Configuration       | 查询设备信息, 设备设备应用配置参数<br />Query device information, device application configuration parameters                         |
| [0x03](cmd-server-service.md)        | M                          | ServerService        | 远程服务器服务 Services of Server | 与远程服务器沟通获取服务内容, 心跳包、定位解码等<br />Communicate with remote servers to obtain service content, heartbeat packets, location decoding, etc.           |
| [0x04](cmd-system-function.md)       | M                          | SystemFunction       | 系统控制 System Control       | 设备系统级别的功能控制，重启、恢复出厂设置等<br />Device system-level function control, restart, restore factory settings, etc.               |
| [0x10](cmd-io-control.md)            | O                          | IOControl            | IO控制 IO Control         | 用于测试设置输入/输出功能。<br />Used to test input/output functions.                                |
| [0x11](cmd-advance-configuration.md) | O                          | AdvanceConfiguration | 高级配置 Advanced Configuration      | 提供更深度的设置参数配置 <br />Provide advanced configuration configuration.                                  |
| [0x12](cmd-custom-configuration.md)  | C                          | CustomConfiguration  | 定制功能配置 Custom Function Configuration      | 定制功能所使用到的配置参数<br />Configuration parameters used for custom functions                                 |
| [0x7A](cmd-file-operation.md)        | O                          | FileOperation        | 文件操作 File Operation       | 提供文件写入、查询功能<br />Provides file writing and query functions                                     |
| [0x7B](cmd-memory-operation.md)      | O                          | MemoryOperation      | 内存操作 Memory Operation       | 提供查询操作设备上的内存，仅调试使用<br />Provides query operations on device memory, for debugging use only.                       |
| [0x7E](cmd-device-file-update.md)    | M                          | DeviceFileUpdate     | 设备文件更新 Device File Update      | 提供带签名的文件更新功能<br />Provide a signed file update function                                   |
| [0x7F](cmd-negative.md)              | M                          | Negative             | 负响应 Negative Response        | 用于回复命令未执行原因<br />Used to respond to the reason why the command was not successfully executed.                                     |
| [0x80](cmd-factory-test.md)          | O                          | FactoryTest          | 工厂测试  Factory Test          | 用于工厂测试时的功能执行<br />Function execution for factory testing                                   |
| [0x81](cmd-log.md)                   | O                          | LogPrint             | LOG            | 日志输出<br /> log print                                                   |
| [0x82](cmd-log-config.md)            | O                          | LogConfig            | LOG configuration     | 日志配置<br /> log configuration                                                   |
| [0x83](cmd-developer.md)             | O                          | Developer            | 开发者模式 Developer Mode     | 用于开发人员调试测试模块使用<br />Used for developers to debug test modules. |
| [0x84](cmd-experimental.md)          | O                          | Experimental         | 实验室功能 Experimental Function     | 用于设置实验性质的功能配置<br />Function configuration for setting experimental properties |
| [0xA0](cmd-authentication.md)        | O                          | Authentication       | 鉴权 Authentication         | 用于提升命令使用权限 <br />Used to elevate command privileges                                       |


Note:

1. 字段 `要求`中，指主机端必须实现的命令支持。`M`=Mandatory强制要求实现的命令，`C`=Conditional条件性要求实现的命令，`O`=Optional可选支持的命令。<br />In the `Requirements` field, this refers to the commands that the host end must support. `M` = Mandatory: Commands that must be supported. `C` = Conditional: Commands that may be supported depending on conditions. `O` = Optional: Commands that are optional to support.
2. 不同设备除强制要求实现的命令外，其他命令支持情况视设备所使用平台的能力存在差异。(todo: 后续根据不同设备添加说明)<br />Except for commands that are mandatory to implement, support for other commands varies depending on the capabilities of the platform used by the device. (todo: Add notes for different devices later)
