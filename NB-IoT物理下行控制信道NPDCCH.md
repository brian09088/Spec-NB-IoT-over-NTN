# NB-IoT物理下行控制信道NPDCCH

在NB-IoT中，NPDCCH有三種類型：第一種是Type1-NPDCCH，UE透過檢測該信道獲取上行調度資訊；第二種是Type2-NPDCCH，UE透過檢測該信道獲取下行調度資訊及隨機接入回應消息（RAR, Random Access Response）；第三種是Type3-NPDCCH，UE透過檢測該信道獲取專屬控制資訊（如尋呼消息）。表 6-6總結了三種類型的NPDCCH資訊，對應於三種格式的DCI。

下面分別介紹三種DCI消息的具體內容。DCI N0中包含的字段如表6-1所示。

<div align=center>

表6-1 DCI Format N0字段
</div>

| **Fields**              | **Size (bit)**          | 
| --------------------- | ------------------- |
| 區別格式N0和格式N1的標識 (0-N0, 1-N1)       | 1 |  
| 子載波指示Isc                    | 6      | 
| 資源分配                 | 3      |  
| 調度時延                   | 2            | 
| 調制編碼方式             | 4            |  
| 冗餘版本          | 1     | 
| 重複次數               | 3      |  
| 新數據指示 | 1        |  
| DCI子幀重複次數 | 2        |

對於DCI Format N0，其功能為指示NPUSCH傳輸時使用的頻域位置、時域位置和調制編碼方式等。其中，調度時延指的是UE在收到N0消息（即NPDCCH傳輸結束）與開始執行NPUSCH調度之間的時間間隔，NB-IoT規定該間隔要不小於8個子幀的時間長度。DCI子幀重複次數表示承載DCI Format N0的NPDCCH重複次數。對於DCI Format N0中6 bits的子載波指示資訊Isc，可以有4種不同的形式，分別對應於1個、3個、6個和12個子載波的使用情況（如圖6-1所示）。具體地，當Isc=0~11時，只有被指示的子載波被使用；當Isc=12-15時，有3個子載波可被使用；當Isc=16,17時，有6個子載波可被使用；當Isc=18時，所有子載波都可被使用。

<div align=center>
<img src=".\pics\pic2.png" width="50%">

圖6-1 DCI Format N0中的子載波指示Isc
</div>

表6-2所示為DCI Format N1中各字段含義。

<div align=center>

表6-2 DCI Format N1字段
</div>

| **Fields**              | **Size (bit)**          | 
| --------------------- | ------------------- |
| 區別格式N0和格式N1的標識 (0-N0, 1-N1)       | 1 |  
| NPDCCH order indicator分配專用前導序列觸發隨機接入指示          | 1      | 
| NPRACH初始重複次數 (NPDCCH order indicator=1)     | 2      |  
| 特定的子載波訊號指示 (NPDCCH order indicator=1)       | 6         | 
| 保留資訊 (NPDCCH order indicator=1)              | 剩餘比特置1            |  
| 調度時延 (NPDCCH order indicator=0)          | 3     | 
| 資源分配 (NPDCCH order indicator=0)               | 3      |
| 調制編碼方式 (NPDCCH order indicator=0)               | 4      |  
| 重複次數 (NPDCCH order indicator=0)               | 4      |
| 新數據指示 (NPDCCH order indicator=0) | 1        |
| HARQ-ACK資源 (NPDCCH order indicator=0) | 4        |  
| DCI子幀重複次數 (NPDCCH order indicator=0) | 2        |

DCI Format N1增加了NPDCCH order以指示隨機接入信道的傳輸。在指示為NPDCCH order時，及PDCCH order觸發的隨機接入時，需要指示NPRACH初始重複次數以及指示特定的子載波訊號，以確定NPRACH的傳輸資源。同時相對於調度NPDSCH（NPDCCH order indicator = 0）時的比特數目而言，需要將剩餘的比特置1。另外，需要注意的是，調度隨機接入回應RAR消息並不使用額外的DCI Format，也使用Format N1。對於終端UE來說，下行NPDSCH的調度時延不少於4個子幀的時間長度。

DCI Format N2各字段的資訊如表表6-3所示。

<div align=center>

表6-3 DCI Format N2字段
</div>

| **Fields**              | **Size (bit)**          | 
| --------------------- | ------------------- |
| 區別尋呼和系統消息更新直接指示的標識Flag       | 1 |  
| 直接指示資訊 (Flag=0)          | 8      | 
| 保留資訊 (Flag=0)     | 5      |  
| 資源分配 (Flag=1)               | 3      |
| 調制編碼方式 (Flag=1)               | 4      |  
| 重複次數 (Flag=1)               | 4      | 
| DCI子幀重複次數 (Flag=1) | 3       |

對於DCI Format N2，透過1bit標識位Flag來區分是用於承載paging消息的NPDSCH的DL grant，還是僅攜帶系統消息更新的直接指示。

在Flag = 0指示為系統消息更新的直接指示，即後續沒有NPDSCH時，需要指示系統消息更新指示等共計8bits資訊。同時相對於調度NPDSCH時的比特數目而言，將剩餘的比特位保留，保證與Format N2在Flag=1時的比特數目一致，其中預留比特數為6bits。

在Flag = 1指示有承載尋呼消息的NPDSCH時，資源分配、調制編碼方式與Format N1中字段大小和含義是一樣的。重複次數（4bits）可以影響NPDSCH傳輸的時間長度，其數值可以為集合{1,2,4,8,16,32,64,128,192,256,384,512,768,1024,1536,2048}中的一個。
