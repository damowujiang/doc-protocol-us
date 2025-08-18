# Docs Online

## Guide

This document library uses `docsify` to dynamically render `markdown` files. [See how to view it in your local browser](guide.md).

---

## Protocol Document

The protocol document describes the protocol used when communicating with Eview Tracker.

* [EV-W101 Class Protocol](protocol/general/index.md)

> The general protocol is suitable for data exchange of subsequent new products.

* [USB HID Protocol](protocol/usb/index.md)

> The USB HID protocol is used when the device needs to use the `USB HID` method to access the device and exchange data with the device.

* [Encryption Protocol](protocol/crypto/index.md)

> For encryption data requirements, this protocol needs to be implemented. It itself encrypts the `general protocol` symmetrically and then encapsulates it.

* [Pipe Communication](protocol/pipe/pipe.md)

> This protocol provides communication between different upper computers, and provides data sharing between  software tools.

* Supplemental Documents
  - [Command Data Structure](protocol/datastructure.md)

  > In different protocols, the same functional parameters use the same data format. Here is a unified description.
  >
