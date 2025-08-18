# 响应 <br />Response


处于通讯过程中的角色，在接收到 `packet`时，需要对此进行回复，我们称之为响应回复 `response/acknowledge`.<br />A role in the communication process that needs to respond when it receives a `packet` is called a `response/acknowledgment`.

响应有以下方式：<br />There are several ways to response:

+ 读返回 read response, 表示当前`packet`读返回了数据。<br />Read response indicates that the current packet has returned data.
+ 通知 notification , 表示设备主动上报了数据。<br />Notification indicates that the device has reported data actively.
+ 正响应 postive response, 表示当前 `packet`已被正确处理。<br />Postive response indicates that the current packet has been processed correctly.
+ 负响应 negative response，表示当前 `packet`无法处理，同时返回 `未处理原因/错误代码（fail reason/error code)`。<br />Negative response indicates that the current packet cannot be processed, and the `reason/error code` is returned.


---
## 读返回 read response / notification

读回复数据/通知数据，返回命令及所读取到的参数`key`内容。<br />Read reply data/notification data, return commands and the content of the read parameter `key`.

---
## 正响应 postive response

正响应使用原命令回复，不携带任何 `LKV`返回，常用于表示命令接收并正确处理，没有额外的信息返回。<br />Responds with the original command without returning any `LKV`. This is commonly used to indicate that the command was received and processed correctly, with no additional information returned.

使用`(0x)command`来回复。<br />Responds with `(0x)command`.


!> `协议V0` 使用 `(0x)7F 01 00`作为正响应回复，当前`版本V1`弃用。<br />`Protocol V0` uses `(0x)7F 01 00` as the positive response reply, which is deprecated in `Version V1`.



---
## 负响应 negative response

使用统一的命令 `0x7F`进行回复，表示负响应包 `negative packet`。查看功能命令中 ` 负响应命令 `。<br />Use the unified command `0x7F` to respond, indicating a negative response packet. See the `negative response command` in the function commands.

?> 允许一些命令中定义为其本身服务的错误回复方式, 比如单独分配一个`key`来作为错误代码的返回, 但其本身只服务于当前命令。<br />Allow some commands to define their own error response methods, such as assigning a separate `key` to return as an error code, but only for the current command.

---
## 原因/错误 代码 reason/error code

原因/错误 代码一般用于配合`negative response`来使用。<br />Reason/error codes are generally used in conjunction with `negative response`.

错误代码在原本的协议中进行升级扩展。错误代码用于指示请求包的错误原因，或未执行成功的原因。<br />Error codes are upgraded and expanded in the original protocol. Error codes are used to indicate the cause of an error in a request packet or the reason for failure to execute successfully.

?> 在开发协议通讯时，**有些错误代码可能无法避免**，如请求包发送过快导致**设备没有足够的资源处理**。<br />
因此，在开发过程中，如果接收到负响应信息时，我们需要查看 错误代码具体含义是什么，同时查看当前错误代码该如何处理。<br />When developing protocol communication, **some error codes may be unavoidable**, such as when request packets are sent too quickly, causing **the device to lack sufficient resources to process them**. <br />Therefore, during development, if a negative response message is received, we need to check the specific meaning of the error code and how to handle the current error code.


### 扩展错误代码 extended error code

为更加清晰指示或跟踪错误来源，对一些错误代码进行扩展。<br />Expand some error codes to more clearly indicate or track the source of errors..

扩展的错误代码，在不同的命令中标明 当前命令支持哪些错误代码。<br />Extended error codes, indicating which error codes are supported by the current command in different commands.







