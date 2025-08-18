

# 统一数据结构

本文档描述不同协议中，实现相同功能使用的数据结构。
有以下数据类型：
* shell脚本
* log数据
* log配置


## shell脚本数据结构

shell脚本命令，通过下发`字符串命令`或`控制符`，设备处理接收数据，解析到对应命令处理。

| 内容 | 处理 |
| --- | --- |
| '\t` | 查询当前shell命令列表 |
| '\n' | 执行shell命令 |
| 0x5b 0x41 | 查询上一个历史命令 |
| 0x5b 0x42 | 查询下一个历史命令 |
| 其他 | 根据命令列表处理 |

---
## log内容结构

| 段号 | 字节 | 描述 |
| --- | --- | --- |
| sync | 2字节 | 用来识别日志内容开头，固定`0xFEA5` |
| crc | 2字节 | timestamp & id & lvl & size & tag 校验和 |
| timestamp | 4字节 | 设备的utc时间 |
| millsecond | 2字节 | 设备的utc时间后的毫秒数 |
| lvl | 4bits | 日志的分级 |
| size | 12bits | 日志内容的长度 |
| tag | 2bytes| 日志标志，用于日志分类 |
| data | 0-4096 bytes| 日志内容 |

* lvl的分级

| 级别 | 值 | 描述 |
| --- | --- | --- |
| error | 1 | 错误级别日志内容 |
| warning | 2 | 警告级别日志内容 |
| info | 3 | 一般信息级别日志内容 |
| debug | 4 | 调试信息级别日志内容 |
| string | 12 | 字符串日志内容 |
| time | 13 | 打印时间内容 |
| raw | 14 | 十六进制原始数据内容 |
| frame error | 15 | 此值是提供上位机标志接收错误帧的，设备不使用 |

`log`内容当前输出时，并非一个`log`单独输出，它是同时多个`log`组合输出，尽可能地利用通讯带宽，提升输出效率。

上位机在拉收数据时，先把数据缓存到一个`buffer`，再根据`sync`来识别每条`log`的开始，根据`crc`计算内容是否完整，再将对应`size`的字节保存为日志内容。

由于传输过程的不确认因素影响，数据可能存在丢失，因此：
* 超时处理
* 标志错误帧 

以下是`C#`的日志接收处理代码示例:
```csharp
public List<DevLogItem> Receive(byte[] data)
{
    List<DevLogItem> logs = new List<DevLogItem>();

    recvDataBuff.AddRange(data);

    bool Exit = false;
    while (Exit == false)
    {
        switch (status)
        {
            case RecvStatus.Head:
                {
                    if (recvDataBuff.Count <= DevLogItem.HeadSize)
                    {
                        Exit = true;
                        break;
                    }

                    // 只检索 日志的前部分，否则容易
                    byte[] arr = recvDataBuff.ToArray();

                    for (int i = 0; i < arr.Length - DevLogItem.HeadSize; i++)
                    {
                        // 逐字节来查询 日志的同步头
                        ushort sync = arr.ToUshort(i);
                        if (sync == DevLogItem.SYNC)
                        {
                            // 获取到同步头后，检查校验是否有效
                            ushort crc = arr.ToUshort(i + 2);
                            // 计算头部分的校验是否有效
                            if (crc == Digest.Crc16Compute(arr.Slice(i + 4, DevLogItem.HeadSize - 4), 0xffff))
                            {
                                lastTime = DateTime.Now;

                                // 先解析出日志的头部信息
                                logRecvItem = DevLogItem.GetHeader(arr.Slice(i, DevLogItem.HeadSize));
                                logRecvBuff.Clear();
                                logRecvBuff.AddRange(arr.Slice(i, DevLogItem.HeadSize));

                                // 移除头数据 
                                recvDataBuff.RemoveRange(0, i + DevLogItem.HeadSize); 
                                // 计算剩余的缓存数据 
                                remainBytes = recvDataBuff.Count;

                                // 将logRecvError添加到log上
                                if (errDataTempBuff.Count > 0)
                                {
                                    logRecvError.RawData = errDataTempBuff.ToArray();//.Add(errDataTempBuff.ToArray());
                                    logs.Add(logRecvError); // 将错误帧添加到解析结果列表中
                                    //debugLogItems.Add(logRecvItem);
                                    errDataTempBuff.Clear();
                                }
                                status = RecvStatus.Content;
                                break;
                            }
                            else
                            {
                                errDataTempBuff.Add(arr[i]); // 记录错误内容数据 
                                if (logRecvError.Error == false)
                                {
                                    logRecvError.Error = true;
                                    logRecvError.ErrInfo = DevLogItem.EnumErrorInfo.CRC_IS_INVALID.ToString();
                                }
                            }
                        }
                        else
                        {
                            errDataTempBuff.Add(arr[i]); // 记录错误内容数据 
                            if( logRecvError.Error == false)
                            {
                                logRecvError.Error = true;
                                logRecvError.ErrInfo = DevLogItem.EnumErrorInfo.SYNC_IS_NOT_EXPECTED.ToString();
                            }
                        }
                    }

                    // 循环走完，则表示没有找到有效头
                    if (status != RecvStatus.Content)
                    {
                        Exit = true;
                    }
                }
                break;

            case RecvStatus.Content:
                if (_is_req_check_timestamp == true)
                {
                    if (DateTime.Now - lastTime > new TimeSpan(0, 0, 0, 0, 1000))
                    {
                        status = RecvStatus.Head;
                        // 超时了，也应该添加异常, 移除原来接收的字节个数
                        logRecvBuff.AddRange(recvDataBuff.GetRange(0, remainBytes).ToArray());
                        logRecvItem.RawData = logRecvBuff.ToArray();// recvDataBuff.GetRange(0, remainBytes).ToArray();
                        // 添加到异常记录中
                        logRecvItem.Error = true;
                        logRecvItem.ErrInfo = DevLogItem.EnumErrorInfo.TIMEOUT.ToString();

                        logs.Add(logRecvItem);

                        // 移除上次的剩余字节
                        recvDataBuff.RemoveRange(0, remainBytes);                                
                        break;
                    }
                }

                // 添加超时，防止截断后无法接收log 
                if (recvDataBuff.Count >= logRecvItem.Size)
                {
                    logRecvBuff.AddRange(recvDataBuff.GetRange(0, logRecvItem.Size).ToArray());
                    logRecvItem.RawData = logRecvBuff.ToArray();

                    logRecvItem.SetBaseDatetTime();

                    // 日志项解码数据，增加时间戳 
                    logs.Add(logRecvItem);

                    recvDataBuff.RemoveRange(0, logRecvItem.Size);
                    status = RecvStatus.Head;
                }
                else
                {
                    Exit = true;
                }
                break;
        }
    }

    return logs;
}
```


## log配置

log配置分为`主开关 type`和`子开关sub_type`。

* **主开关**

使用`32/64bits`值标志。除`bit0`为全局`log`开关，其他位表示不同的`log`类型开关。

| bit | 功能 |
| --- | --- |
| bit0 | global enabled |
| bit1-n| 由项目为每个bit分配内容 |

主开关分配完毕，则主开关的偏移位置:[1-63]。在类型分配较多时，可以扩展到`64bits`来使用。

允许分别为每个主开关设置不同的`log level`，`level`向低值等级覆盖，即选择了`warning level`，同`error`&`warning`同时打开.

```clike
// mask 指主开关掩码
// level: 0-disable, 1-error level, 2-warning, 3-info, 4-debug
void log_set_level( uint32_t mask, uint32_t level );
```


* **子开关**

使用`24bits`值标志。每个`bit`位表示一个`log`子功能开关。

| bit | 功能 |
| --- | --- |
| bit0-25 | 根据功能分配一个开关 |
| bit26-31 | 主开关偏移值-对应序号 |

设置子开关，要指定类别。

```clike
void log_set_sublevel( uint32_t type, uint32_t subtype, bool enabled );
```
并非所有的`主开关`定义了`子开关`，具体查看项目代码。


