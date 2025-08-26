# Command list


The list of commonly used commands for the device is as follows:

|   Command                            | Requirement <sup>1</sup> | Parameter                 | Function           | Description                                                     |
| :-------------------------------- | -------------------------- | :------------------- | :------------- | :--------------------------------------------------------- |
| [0x01](cmd-data.md)                  | M                          | Data                 | Device's Data      | Includes historical data, location, call logs, step counts, etc.; as well as specific data types. |
| [0x02](cmd-configuration.md)         | M                          | Configuration        | Device's Configuration       | Query device information, device application configuration parameters                         |
| [0x03](cmd-server-service.md)        | M                          | ServerService        | Services of Server | Communicate with remote servers to obtain service content, heartbeat packets, location decoding, etc.           |
| [0x04](cmd-system-function.md)       | M                          | SystemFunction       | System Control       | Device system-level function control, restart, restore factory settings, etc.               |
| [0x10](cmd-io-control.md)            | O                          | IOControl            | IO Control         | Used to test input/output functions.                                |
| [0x11](cmd-advance-configuration.md) | O                          | AdvanceConfiguration | Advanced Configuration      | Provide advanced configuration configuration.                                  |
| [0x12](cmd-custom-configuration.md)  | C                          | CustomConfiguration  | Custom Function Configuration      | Configuration parameters used for custom functions                                 |
| [0x7A](cmd-file-operation.md)        | O                          | FileOperation        | File Operation       | Provides file writing and query functions                                     |
| [0x7B](cmd-memory-operation.md)      | O                          | MemoryOperation      | Memory Operation       | Provides query operations on device memory, for debugging use only.                       |
| [0x7E](cmd-device-file-update.md)    | M                          | DeviceFileUpdate     | Device File Update      | Provide a signed file update function                                   |
| [0x7F](cmd-negative.md)              | M                          | Negative             | Negative Response        | Used to respond to the reason why the command was not successfully executed.                                     |
| [0x80](cmd-factory-test.md)          | O                          | FactoryTest          | Factory Test          | Function execution for factory testing                                   |
| [0x81](cmd-log.md)                   | O                          | LogPrint             | LOG            | log print                                                   |
| [0x82](cmd-log-config.md)            | O                          | LogConfig            | LOG configuration     | log configuration                                                   |
| [0x83](cmd-developer.md)             | O                          | Developer            | Developer Mode     | Used for developers to debug test modules. |
| [0x84](cmd-experimental.md)          | O                          | Experimental         | Experimental Function     | Function configuration for setting experimental properties |
| [0xA0](cmd-authentication.md)        | O                          | Authentication       | Authentication         | Used to elevate command privileges                                       |


Note:

1. In the field `Requirements` , this refers to commands that must be implemented by the host side. <br />`M` = Mandatory: Commands that must be supported. <br />`C` = Conditional: Commands that may be supported depending on conditions.<br /> `O` = Optional: Commands that are optional to support.
2. Except for commands that are mandatory to implement, support for other commands varies depending on the capabilities of the platform used by the device. (todo: Add notes for different devices later)
