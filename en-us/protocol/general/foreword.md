# Foreword

This document presents the revised communication protocol, which has been optimized and updated from the
original protocol to enhance performance, compatibility, and maintainability.


## This revision incorporates the following optimizations and modifications in the following aspects:


- [x] Enhancement of protocol command functionality description
- [x] Consideration of functional modularization in design
- [x] Reserved for future functional expansionedparamterfields for future
- [x] Improved security management for parameters


---

## Modifications

### 1. Protocol header 

- [X]  Protocol header version number is `1`

---

### 2. Positive response

- [X] The command now responds without carrying any parameters.
- [X] The original Eview Parameter Editer uses the ID to determine whether the response packet corresponds to a still-valid request packet.

---

### 3. Negative response code extension

- [X] Originally, negative responses were returned in the Length-Key format, with the len-key field indicating the error code. 

- [X] Additional error reason descriptions have been added for certain keys.

---

### 4. Add custom function commands

- [X] Add custom functionality to configure and modify the logic of custom features.
- [X] When the custom function has been reviewed and approved for merging into the standard branch, its configuration parameters are moved to [configuration command](cmd-configuration.md), while the configuration parameters of the original custom version branch remain unchanged..


---

### 5. Configuration keys

- [X] Re-optimize the distribution position of `keys` and reserved space for future expansion.
- [X] Remove the combination control of certain parameters.
- [X] Remove the previous customized function parameters; future requirements will be implemented by adding them to the custom function command.

---

### 6. Add more types to historical data

- [X] Optimize the historical data types.
- [X] Add more record types to historical data.

---

### 7. Add more commands


- [X] Add `Authentication` command
- [X] Add `Laboratory` command
- [X] Add `Developer` command
- [X] Add `File` command
- [X] Add `Log` command
- [X] Add `Memory` command
- [X] Add `Application` command
- [x] Add `AdvanceConfiguation` command
- [x] Add `CustomConfiguration` command

For the specific functions of each command, please refer to the respective command description files.


The impact of the newly added commands during factory testing requires comparing not only the configuration parameter, but also the following data items:

* Configuration parameters in `Laboratory`
* Customized function parameters in `Application`
* Configuration parameters in `AdvanceConfiguation`
* Configuration parameters in `CustomConfiguration`

---

### 8. Remove encryption part


- [X] Under the previous A5 package attribute protocol, the operation executed was a packet encapsulation, which is now described in the [encryption protocol document](protocol/crypto/index.md).

---

### 9.  Improved the usage of the command protocol

- [X] The Eview Parameter Editer sends command only to query the device for the keys implemented/supported by the current command. The indicate attribute is not included; otherwise, it will be treated as a reply from Eview ParameterEditer. (In query commands, the indicate flag MUST NOT be set to 1.)
- [X] Classification  of custom commands: write/read/notify


---

### 10.  Data type optimization


- [X] The data type of the commonly used latitude and longitude fields has been revised to `float` type
- [X] Removed the previous method of requiring a length byte to specify string size.
- [X] Added a string type that must be terminated with '\0'. Strings are filled in sequential order, and an empty string is represented by '\0' as a placeholder.
