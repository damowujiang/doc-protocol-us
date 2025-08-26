# Overview


The protocol document explains how to communicate with devices and exchange data.

If there is any discrepancy between this protocol document and the device behavior, the protocol shall prevail.

The protocol document is managed using version numbers in the format  `vx.y.z`, where `x` represents the major version number, `y` represents the minor version number, and `z` represents the revision version number.

## Conventions

+ Byte order: The default is `little-endian (LE)`. If a different byte order is used, it will be noted in the documentation. For example, the received order `0x12 0x34 0x56 0x78` is converted to `32bits` as `0x78563412`.
+ UTF-8 is used as the default character encoding. If any other character encoding is used, it will be explicitly noted in this document.
+ Timestamp: The default starting time is `1970.1.1 00:00:00`, similar to the `Unix epoch/Unix time`. However, unlike Unix time, which uses signed representation, the timestamp of this protocol uses an unsigned 32-bit. This allows dates to be represented up to the year 2106,extending the usable time range by 68 years compared to the signed 32-bit limit of the year 2038.

## Definitions & Abbreviations

| Abbreviation  | Definition of Term                                               |
| ---- | ------------------------------------------------------- |
| LKV  | length-key-value                    |
| BLE  | Bluetooth low energy                          |
| DFU  | Device's File update  |
| RFU  | Reserved for future use                 |
| NFC  | Near field communication                        |
| NDEF | NFC data exchange format                |
| ACK  | acknowleage                                 |
