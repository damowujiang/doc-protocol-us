# 概述

在嵌入式系统、物联网（IoT）及各类网络通信协议中，鉴权（Authentication） 是确保通信安全的核心机制，其重要性主要体现在以下几个方面：

+ 身份真实性验证 identity verification鉴权机制能够确认通信双方（如设备与服务器、主从节点）的合法身份，防止非法设备或恶意节点接入系统，确保数据来源可信。
+ 防止未授权访问 Prevents unauthorized access通过挑战-响应（Challenge-Response）、Token验证或数字签名等方式，鉴权可有效拦截伪造请求，避免未授权的设备或用户获取敏感数据或控制系统。
+ 抵御重放攻击 against replay attacks
  动态生成的挑战值（Challenge）、时间戳或随机数（Nonce）能够确保每次通信的请求唯一性，防止攻击者截获并重复发送合法数据包进行欺骗。

# 功能说明

首先，将权限等级一共划为 0/1/2/3 四级，每个权限等级允许使用的命令不相同。

## 权限等级：

| **等级** | **允许使用的命令**     | **读写权限(R/W)** |
| -------------- | ---------------------------- | ----------------------- |
| 0              | 鉴权协议命令                 | RW                      |
| 1              | 配置参数命令                 | RW                      |
| 2              | 配置参数、历史数据、系统命令 | RW                      |
| 3              | 开发者权限                   | RW                      |

不同的接口默认使用权限不一样，这取决于设备的信任边界。

## 接口默认权限

| **接口类型** | **默认权限等级** | **备注说明** |
| ------------------ | ---------------------- | ------------------ |
| USB                | 0                      |                    |
| BLE                | 0                      |                    |
| TCP/IP             | 2                      |                    |

## 子命令列表

| **key(hex)** | **名称**   | **备注**           |
| ------------------ | ---------------- | ------------------------ |
| 01-0F              | 设备的基础参数   | 与配置设备前面使用的一致 |
| 10-7F              | 预留             | 预留后续新增参数         |
|                    |                  |                          |
| 80                 | challenge        |                          |
| 81                 | verify signature |                          |
|                    |                  |                          |
| 90                 | token            | extension for user       |

# keys' description

_**note**__: general information of device is inherit from __**configuration**__ command._

_for example:   _`_key=0x03 device IMEI_`_ is available for this command._

_设备的基本信息读取继承于配置命令，因此可以通过 _`_0x03_`_来读设备的 IMEI。_

## challenge

### description

由主机或者服务器发起，设备收到后也同时发起质询。因此两边使用的 `数据`为随机数，提升抗攻击能力。

The `challenge`is initiated by the host, send with random seed , device response  with random numbers.

### 发送数据结构

由 `host`发起，一般为上位机、APP、服务器等。

| byte No. | Description           | Remark |
| -------- | --------------------- | ------ |
| 01       | length                |        |
| 02       | chanllege key         |        |
| 03-06    | privilge level        |        |
| 07-10    | random seed from host |        |

### 回复数据结构

由设备回复。

| byte No. | Description               | Remark |
| -------- | ------------------------- | ------ |
| 01       | length                    |        |
| 02       | chanllege key             |        |
| 03-06    | random number from device |        |

## response

在 `challenge`轮中，双方已经收到对方的随机数值，综合两边的随机数，按照规则生成签名或密钥。

### 发送数据结构

由 `host`发起，一般为上位机、APP、服务器等。

| byte No. | Description       | Remark |
| -------- | ----------------- | ------ |
| 01       | length            |        |
| 02       | response key      |        |
| 03-n     | keys or signature |        |

### 回复数据结构

由设备回复。

| byte No. | Description                                                                         | Remark                               |
| -------- | ----------------------------------------------------------------------------------- | ------------------------------------ |
| 01       | length                                                                              |                                      |
| 02       | response key                                                                        |                                      |
| 03       | 0 - success``1 - key or signature invalid``2 - delay time not expired | 回复或许使用 `7F`来回复更方便一些? |

![](https://cdn.nlark.com/yuque/__puml/1da848920ccbbd7276a75fc983be56ae.svg)

**注意**：

1. 在进行签名验证时，如果失败，则需要发送 `challenge`, 重启认证流程。
2. 为防止快速鉴权攻击 `校验签名或密钥`，连续失败 `n`次后，延长下次认证流程时间。 比如第 `n`次失败，必须等待 10 秒后，才能发起认证流程；第 `n+1`次失败，延时 30 秒；最大延时 `1` 个小时。认证通过后，清除延时，重置失败次数。 暂定 `n`为 10 次。
3. 在接收到认证时，记录失败次数；在重启时按规则延时计算。
4. 断连超时时间内，重新连接不需要重建鉴权机制。
5. 鉴权失败，则其他命令应该返回 `权限不足的错误代码` 提示。

## token

token 方式提供用户扩展生成临时使用 token, `token`所能开启的权限与客户协商定制，`token`的生成方式也同时需要确定一致。

### 发送数据结构

由 `host`发起，一般为上位机、APP、服务器等。

| byte No. | Description | Remark |
| -------- | ----------- | ------ |
| 01       | length      |        |
| 02       | token key   |        |
| 03-n     | tokens      |        |
