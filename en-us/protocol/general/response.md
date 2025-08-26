# Response/ACK

In the communication process, the entity receiving a packet must generate a reply, referred to as a `response` or `acknowledgement (ACK)`

The following response types are supported：

+ `Read response` - indicates that the current packet has returned data.
+ `Notification` - indicates that the device has proactively reported data.
+ `Postive response` - indicates that the current packet has been processed successfully.
+ `Negative response`  - indicates that the current packet cannot be processed, and the `reason/error code` is returned.


---
## Read response / Notification

Read response data/notification data, return commands and the content of the read parameter key.

---
## Postive Response

Responds with the original command without returning any `LKV`. This is commonly used to indicate that the command was received and processed correctly, with no additional information returned.

Responds with `(0x)command`.


!> `Protocol V0` uses `(0x)7F 01 00` as the positive response reply, which is deprecated in `Version V1`.



---
## Negative Response

A unified command 0x7F is used for the reply, indicating a negative response packet. Please check the negative response command in the function commands section.

?> Allow some commands to define their own error response methods, such as assigning a separate `key` to return as an error code, but only for the current command.

---
## Reason/Error Code

Reason/error codes are generally used in conjunction with `negative response`.

Error codes are upgraded and extended within the original protocol. They indicate the reason for errors in the request packet or why execution was unsuccessful.

?> When developing protocol communication, **some error codes may be unavoidable**, such as when request packets are sent too quickly, causing **the device to lack sufficient resources to process them**. <br />Therefore, upon receiving a negative response, it is necessary to check the specific meaning of the error code and
determine the appropriate handling method.


### Extended Error Code

Expand some error codes to more clearly indicate or track the source of errors..

Extended error codes, indicating which error codes are supported by the current command in different commands.







