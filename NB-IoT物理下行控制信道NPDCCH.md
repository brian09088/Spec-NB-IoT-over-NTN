# NB-IoT物理下行控制信道NPDCCH

在NB-IoT中，NPDCCH有三种类型：第一种是Type1-NPDCCH，UE通过检测该信道获取上行调度信息；第二种是Type2-NPDCCH，UE通过检测该信道获取下行调度信息及随机接入响应消息（RAR, Random Access Response）；第三种是Type3-NPDCCH，UE通过检测该信道获取专属控制信息（如寻呼消息）。表 6-6总结了三种类型的NPDCCH信息，对应于三种格式的DCI。

下面分别介绍三种DCI消息的具体内容。DCI N0中包含的字段如表6-1所示。

<div align=center>

表6-1 DCI Format N0字段
</div>

| **Fields**              | **Size (bit)**          | 
| --------------------- | ------------------- |
| 区别格式N0和格式N1的标识 (0-N0, 1-N1)       | 1 |  
| 子载波指示Isc                    | 6      | 
| 资源分配                 | 3      |  
| 调度时延                   | 2            | 
| 调制编码方式             | 4            |  
| 冗余版本          | 1     | 
| 重复次数               | 3      |  
| 新数据指示 | 1        |  
| DCI子帧重复次数 | 2        |

对于DCI Format N0，其功能为指示NPUSCH传输时使用的频域位置、时域位置和调制编码方式等。其中，调度时延指的是UE在收到N0消息（即NPDCCH传输结束）与开始执行NPUSCH调度之间的时间间隔，NB-IoT规定该间隔要不小于8个子帧的时间长度。DCI子帧重复次数表示承载DCI Format N0的NPDCCH重复次数。对于DCI Format N0中6 bits的子载波指示信息Isc，可以有4种不同的形式，分别对应于1个、3个、6个和12个子载波的使用情况（如图6-1所示）。具体地，当Isc=0~11时，只有被指示的子载波被使用；当Isc=12-15时，有3个子载波可被使用；当Isc=16,17时，有6个子载波可被使用；当Isc=18时，所有子载波都可被使用。

<div align=center>
<img src=".\pics\pic2.png" width="50%">

图6-1 DCI Format N0中的子载波指示Isc
</div>

表6-2所示为DCI Format N1中各字段含义。

<div align=center>

表6-2 DCI Format N1字段
</div>

| **Fields**              | **Size (bit)**          | 
| --------------------- | ------------------- |
| 区别格式N0和格式N1的标识 (0-N0, 1-N1)       | 1 |  
| NPDCCH order indicator分配专用前导序列触发随机接入指示          | 1      | 
| NPRACH初始重复次数 (NPDCCH order indicator=1)     | 2      |  
| 特定的子载波讯号指示 (NPDCCH order indicator=1)       | 6         | 
| 保留信息 (NPDCCH order indicator=1)              | 剩余比特置1            |  
| 调度时延 (NPDCCH order indicator=0)          | 3     | 
| 资源分配 (NPDCCH order indicator=0)               | 3      |
| 调制编码方式 (NPDCCH order indicator=0)               | 4      |  
| 重复次数 (NPDCCH order indicator=0)               | 4      |
| 新数据指示 (NPDCCH order indicator=0) | 1        |
| HARQ-ACK资源 (NPDCCH order indicator=0) | 4        |  
| DCI子帧重复次数 (NPDCCH order indicator=0) | 2        |

DCI Format N1增加了NPDCCH order以指示随机接入信道的传输。在指示为NPDCCH order时，及PDCCH order触发的随机接入时，需要指示NPRACH初始重复次数以及指示特定的子载波讯号，以确定NPRACH的传输资源。同时相对于调度NPDSCH（NPDCCH order indicator = 0）时的比特数目而言，需要将剩余的比特置1。另外，需要注意的是，调度随机接入响应RAR消息并不使用额外的DCI Format，也使用Format N1。对于终端UE来说，下行NPDSCH的调度时延不少于4个子帧的时间长度。

DCI Format N2各字段的信息如表表6-3所示。

<div align=center>

表6-3 DCI Format N2字段
</div>

| **Fields**              | **Size (bit)**          | 
| --------------------- | ------------------- |
| 区别寻呼和系统消息更新直接指示的标识Flag       | 1 |  
| 直接指示信息 (Flag=0)          | 8      | 
| 保留信息 (Flag=0)     | 5      |  
| 资源分配 (Flag=1)               | 3      |
| 调制编码方式 (Flag=1)               | 4      |  
| 重复次数 (Flag=1)               | 4      | 
| DCI子帧重复次数 (Flag=1) | 3       |

对于DCI Format N2，通过1bit标识位Flag来区分是用于承载paging消息的NPDSCH的DL grant，还是仅携带系统消息更新的直接指示。

在Flag = 0指示为系统消息更新的直接指示，即后续没有NPDSCH时，需要指示系统消息更新指示等共计8bits信息。同时相对于调度NPDSCH时的比特数目而言，将剩余的比特位保留，保证与Format N2在Flag=1时的比特数目一致，其中预留比特数为6bits。

在Flag = 1指示有承载寻呼消息的NPDSCH时，资源分配、调制编码方式与Format N1中字段大小和含义是一样的。重复次数（4bits）可以影响NPDSCH传输的时间长度，其数值可以为集合{1,2,4,8,16,32,64,128,192,256,384,512,768,1024,1536,2048}中的一个。
