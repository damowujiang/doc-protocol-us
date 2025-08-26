# Negative Response(0x7F)

## Command Instruction

Negative response – a denial reply. This is a type of response that the device must return when, for various reasons,
it cannot correctly execute a command, in order to inform the host of the reason why the command was not
successfully executed.

As explained [📄Response](response.md), positive responses have a fixed reply format, so please pay close attention.

---

## key list

| Value(HEX) | Parameter               | Function Description | Write | Read | Notification |
| ---------- | ----------------------- | ---------------------- | ---- | ---- | ---- |
| 0x00       | success                 | Success                | ❌   | ❌   | ❌   |
| 0x01       | lengthError             | Invalid length | ❌   | ❌   | ✅   |
| 0x02       | dataFormatInvalid       | Invalid format | ❌   | ❌   | ✅   |
| 0x03       | invalidDataSize         | Incorrect data size | ❌   | ❌   | ✅   |
| 0x04       | insufficientResource    | Insufficient resources to execute the current command | ❌   | ❌   | ✅   |
| 0x05       | subFunctionNotSupported | Sub-function not supported | ❌   | ❌   | ✅   |
| 0x06       | noMemory                | Insufficient memory | ❌   | ❌   | ✅   |
| 0x07       | invalidAddressResponse  | Invalid address response | ❌   | ❌   | ✅   |
| 0x08       | keyInvalid              | Invalid key  | ❌   | ❌   | ✅   |
| 0x09       | delayNotMet             | The delay did not reach the expected time | ❌   | ❌   | ✅   |
| 0x0A       | invalidState            | Invalid state | ❌   | ❌   | ✅   |
| 0x0B       | invalidParameter        | Invalid parameter | ❌   | ❌   | ✅   |
| 0x0C       | busy                    | Busy               | ❌   | ❌   | ✅   |
| 0x0D       | peripheralNotSupported  | Unsupported peripheral operation | ❌   | ❌   | ✅   |
| 0x0E       | programmingError        | Programming error | ❌   | ❌   | ✅   |
| 0x0F       | sensorNotReady          | Sensor not ready | ❌   | ❌   | ✅   |
| 0x10       | invalidState            | Invalid state | ❌   | ❌   | ✅   |
| 0x20       | dfu                     | DFU command response | ❌   | ❌   | ✅   |
| 0x21       | file                    | File command response | ❌   | ❌   | ✅   |
| 0xF8-0xFE  | RFU                     | Reserved for internal use (RFU) | ❌   | ❌   | ❌   |
| 0xFF       | unknownError            | Unknown error  | ❌   | ❌   | ✅   |

Note:

1. The current key list does not represent functions; instead, it represents types of negative responses.
2. `0x00 – Success` will no longer appear in this protocol, as positive responses have their own corresponding reply format.
3. In this section, for `len-key-value`, the `value` serves as an extended supplementary explanation for the
   key
4. For commonly used functions, try to use a single key to directly indicate the reason for failure.
5. For less frequently used functions, use extended keys whenever possible to indicate the reason for failure.

---

## Catagory Description

---

### Negative Response for DFU Command

**Description**

The current negative response codes are specifically used to indicate the reasons for command failure when
executing DFU command operations.

**Notification data format**

| Byte No. | Parameter | Type | Converter | Description                                      |
| -------- | --------- | ---- | --------- | ------------------------------------------------ |
| 1        | length    | byte |           | length                                           |
| 2        | key       | byte |           | key                                              |
| 3        | code      | byte |           | For extended codes, refer to the list<br/>below. |

* For extended codes, refer to the list below.
  *Codes not specified are not in use.*

| code | Parameter            | Description                          | Handling Method                                              |
| ---- | -------------------- | ------------------------------------ | ------------------------------------------------------------ |
| 0x01 | invalidLength        | Invalid data length                  | During firmware content writing, data must be aligned on a 4-byte boundary, meaning the total number of bytes must be an integer multiple of 4. |
| 0x02 |                      |                                      |                                                              |
| 0x03 |                      |                                      |                                                              |
| 0x04 | insufficientResource | Insufficient resources               | The device utilizes dynamic memory to store part of the data. If data is transmitted too rapidly, incomplete execution of previous operations may lead to insufficient available memory. In such cases, the host must delay for a specified period before resending the current command. |
| 0x05 |                      |                                      |                                                              |
| 0x06 | noMemory             | Insufficient memory                  | Same reason and operation as 0x04                            |
| 0x0A | invalidState         | Invalid state                        | The operation sequence of the file key is incorrect. Please perform the operation according to the DFU command sequence requirements. |
| 0x0B | invalidParameter     | Invalid parameter                    | The parameters in the initialization packet are incorrect, and no valid<br/>parameter data can be identified. |
| 0x0C |                      |                                      |                                                              |
| 0x0D | invalidAddress       | Invalid address                      | The offset address is inconsistent when writing firmware content. The<br/>content must be written in the order of the file. |
| 0x0E | internalError        | Internal error                       | The function entry parameter is NULL.                        |
| 0x0F |                      |                                      |                                                              |
| 0x11 | versionLow           | Firmware version is too low          | Updating to a lower firmware version is not allowed. A new firmware must be provided |
| 0x12 | signatureInvalid     | Signature verification failed        | The initialization packet signature did not pass verification. Check whether the crypto library is consistent. |
| 0x13 | fileSizeInvalid      | File size does not meet requirements | The file to be upgraded is either too small or too large, indicating an abnormal upgrade package. |
| 0x14 | fileCrcError         | File CRC error                       | File content does not match the file CRC described in the initialization packet. |
| 0x15 | timeout              | Timeout                              | Execution of the current command has timed out. The prerequisite tasks were not completed; please resend this command. |
| 0x16 | fileTypeUnsupported  | File type not supported              | The current firmware does not support updating this type of file. Please check whether the generated package is correct. |
| 0x17 | modelError           | Model error                          | The current upgrade package is not supported by the device’s program and is incompatible. |
| 0x18 | customFlagInvalid    | Invalid custom flag                  | The custom flag in the current firmware package does not match the requirement of the firmware, making it incompatible. Please check the firmware package. |

---

### File Command Negative Response

**Description**

The current negative response codes are specifically used to indicate the reasons for command failure when executing File command operations.

**Notification Data Format**

| Byte No. | Parameter | Type | Converter | Description                                  |
| -------- | --------- | ---- | --------- | -------------------------------------------- |
| 1        | length    | byte |           | length                                       |
| 2        | key       | byte |           | key                                          |
| 3        | code      | byte |           | For extended codes, refer to the list below. |

* Extended Code List

| code | Parameter             | Description                                         | Handling Method                                              |
| ---- | --------------------- | --------------------------------------------------- | ------------------------------------------------------------ |
| 0x01 | fileNotOpen           | File not opened                                     | the file must be opened before performing operations on it.  |
| 0x02 | threadOccupied        | Thread occupied                                     | another file is already open. You must wait for the<br/>previous file to be closed before opening a new one. |
| 0x03 | fileTypeUnsupported   | File type not supported                             | The current file type is not supported in the firmware.      |
| 0x04 | fileTypeNotFound      | File type not found                                 | please check whether the file type in the command is<br/>correct. |
| 0x05 | ioNotImplemented      | I/O operation not implemented for current file type | The file type is defined in the protocol, but its<br/>implementation interface was not found in the current<br/>program. |
| 0x06 | ioBusy                | IO occupied                                         | a file of the same type is already open.                     |
| 0x07 | processing            | Under processing                                    | Processing – retry after a delay.                            |
| 0x08 | noWritePermission     | No write permission                                 | The current file type has not been granted write access.     |
| 0x09 | writeOffsetError      | Write offset address error                          | The file content was not written in sequence.                |
| 0x0A | insufficientResource  | Insufficient resources                              | he program does not have enough memory to store the<br/>current write content. The host should delay before<br/>retrying the operation. |
| 0x0B | fileSizeExceeded      | File size exceeded                                  | the written file exceeds the size specified in the<br/>parameters. Check whether the parameter was passed<br/>incorrectly or if an error occurred during the writing<br/>process. |
| 0x0C | operationNotSupported | Operation not supported                             | the current file type does not support this operation.       |
| 0x0D | moduleNotOpen         | Module not opened,cannot execute operation          | Some files are written to an external module, which must be operating normally before performing the operation. |
| 0x10 | modeLengthError       | File parameter –mode length error                   | Check whether the command format is correct                  |
| 0x11 | typeLengthError       | File parameter – type parameter length error        | Check whether the command format is correct                  |
| 0x12 | fileSizeLengthError   | File parameter – file size length error             | Check whether the command format is correct                  |
| 0x13 | fileNameLengthError   | File parameter –name length error                   | Check whether the command format is correct                  |
| 0x14 | filePathLengthError   | File parameter – path length error                  | Check whether the command format is correct                  |
| 0x15 | crc32LengthError      | File parameter –CRC32 length error                  | Check whether the command format is correct                  |
| 0x16 | sum32LengthError      | File parameter –SUM32 length error                  | Check whether the command format is correct                  |
| 0x17 | md5LengthError        | File parameter – MD5 length error                   | Check whether the command format is correct                  |
| 0x18 | undefinedParameter    | Undefined parameter passed in                       | Check whether the command format is correct                  |
| 0x19 | pathNameTooLong       | Path and name length exceed specified limits        | Check whether the file path in the command meets the<br/>requirements. |
| 0x1A | checkTypeUnsupported  | Unsupported verification method                     | Check whether the command format is correct                  |
| 0x1B | invalidFilePath       | Invalid full file path                              | Check whether the command format is correct – the<br/>path and file name parameters are required. |
| 0x1C | fileCheckFailed       | File verification failed                            | File write verification failed – rewrite the file.           |
