# 射频识别技术RFID

![image-20200904164907814](https://gitee.com/cpu_code/picture_bed/raw/master//20200904164907.png)

## RFID介绍

射频识别： 英文名称是\(Radio Frequency Identification\)， 简称是“ RFID” 又称 无线射频识别， RFID是物联网的其中一种终端技术。

RFID是一种通信技术， 可通过无线电讯号耦合识别特定目标并读写相关数据， 而无需识别系统与特定目标之间建立机械或光学接触。

正被广泛用于采购分配、 商业贸易、 生产制造、 物流、 防伪以及军事用途上。

RFID主要位于典型物联网架构中的感知层，是整个物联网的最底层， 也是与‘ 万物’链接的媒介之一。

正因其具有非接触式特性， 其运用很广泛，且随着不同应用场景的出现， RFID协议也随着增多， 难以统一。

如在消费电子行业中比较典型的应用NFC（ RFID的子集） ， 以统一的标准， 安全，低功耗， 近距离等特性， 在支付领域应用广泛。

![image-20200904165104650](https://gitee.com/cpu_code/picture_bed/raw/master//20200904165104.png)

## RFID应用

智慧交通

![image-20200904165139123](https://gitee.com/cpu_code/picture_bed/raw/master//20200904165139.png)

智慧仓库

![image-20200904165149599](https://gitee.com/cpu_code/picture_bed/raw/master//20200904165149.png)

## RFID 原理

读写器向M1卡发一组固定频率的电磁波， 卡片内有一个LC串联谐振电路， 其频率与读写器发射的频率相同， 在电磁波的激励下， LC谐振电路产生共振， 从而使电容内有了电荷， 在这个电容的另一端， 接有一个单向导通的电子泵， 将电容内的电荷送到另一个电容内储存， 当所积累的电荷达到2V时， 此电容可做为电源为其它电路提供工作电压， 将卡内数据发射出去或接取读写器的数据。

![image-20200904165216145](https://gitee.com/cpu_code/picture_bed/raw/master//20200904165216.png)

## RFID 组成

* 应答器

由**天线**， **耦合元件**及**芯片**组成， 一般来说都是用标签作为应答器， 一般分为**被动式**、 **主动式**、 **半主动式**， 每个标签具有唯一的电子编码， 附着在物体上标识目标对象。

* 阅读器

由天线， 耦合元件， 芯片组成， 读写标签信息的设备， 可设计为手持式rfid读写器或固定式读写器。

* 应用软件系统

应用层软件， 主要是把收集的数据进一步处理，并为人们所使用。

nfc可以既是应答器， 也可以是阅读器存在。

## RFID协议

ISO/IEC 14443:国际标准ISO 14443定义了两种信号接口， 分别是 `TypeA` 和 `TypeB` 且互不兼容。

* TypeA类卡:

MIFARE Std 1k\(MF1 IC S50\)： 国内常称MF1 S50

MIFARE Std 4k\(MF1 IC S70\)： 国内常称为MF1 S70

广泛应用

* TypeB类卡:

  我国第二代居民身份证

  AT88RF020: 美国爱特梅尔\(ATMIL\)生产， 典型应用, 如地铁卡。

卡的状态 :

POWER OFF： 缺少载波能量

IDLE： 等待读写器发来的请求

READY： 收到读写器发来的请求

ACTIVE： 收到读写器发来的选择

HALT： 读写器发来的停止命令

卡片请求命令：

REQA： 请求未被HALT的TypeA卡--0x26

WAKE-UP： 请求所有的TypeA卡--0x52

![image-20200904165316967](https://gitee.com/cpu_code/picture_bed/raw/master//20200904165317.png)

* 防冲突

多卡操作， 判断卡号是否完整

* 选择

  根据完整的UID， 选择相应的卡片

* HALT

  停止该卡， 除非读写发送WAKE-UP请求

* CRC

  检验值为6363（ A类卡）

  校验值为FFFF（ B类卡）

* 流程 :

  REQB : 寻卡

  ATTRIB : 匹配属性

GetUID : 获得卡号

* 验证密码

指定加密类型， 指定块号（ 0~63） ， 指定密码， 指定卡号

* 读块内容

验证密码后， 再次指定块号（ 0~63）

* 写块内容

验证密码后， 再次指定块号（ 0~63）

## RFID应答器— — 卡片

ID卡（ Identification Card， 身份识别卡）

* 一种不可写入的感应卡， 含固定的编号。
* 主要有台湾SYRIS的EM格式、 美国HIDMOTOROLA等各类ID卡。
* ID卡与磁卡一样， 都仅仅使用了“ 卡的号码” 而已，卡内除了卡号外， 无任何保密功能， 其“ 卡号” 是公开、 裸露的。
* ID卡就是“ 感应式磁卡” 。

IC卡 \(Integrated Circuit Card， 集成电路卡\)

* 将一个微电子芯片嵌入符合ISO 7816标准的卡基中， 做成卡片的形式。
* IC卡与读写器之间的通讯方式可以是接触式， 也可以是非接触式。
* 由于其固有的信息安全、 便于携带、 比较完善的标准化等优点， 在身份认证、 银行、 电信、 公共交通、车场管理等领域正得到越来越多的应用。
* 常见的有二代身份证， 银行的电子钱包， 电信的手机SIM卡， 公共交通的公交卡、 地铁卡， 用于收取停车费的停车卡等  

ID卡 :

![image-20200904171114992](https://gitee.com/cpu_code/picture_bed/raw/master//20200904171115.png)

IC卡 :

![image-20200904171129026](https://gitee.com/cpu_code/picture_bed/raw/master//20200904171129.png)

S50非接触式IC卡性能简介（ M1）

* 容量为8K位EEPROM
* 分为16个扇区， 每个扇区为4块， 每块16个字节,以块为存取单位
* 每个扇区有独立的一组密码及访问控制
* 每张卡有唯一序列号， 为32位
* 具有防冲突机制， 支持多卡操作
* 无电源， 自带天线， 内含加密控制逻辑和通讯逻辑电路  

数据保存期为10年， 可改写10万次， 读无限次

* 工作温度： -20℃ ~50℃ \(湿度为90%\)
* 工作频率： 13.56MHZ
* 通信速率： 106 KBPS
* 读写距离： 10 cm以内（ 与读写器有关）  

## RFID阅读器— — FM17550\(MFRC522\)

读写器：

* 通信频率
* 支持协议
* 读写距离
* 通信速率
* 寄存器配置

天线发送， 接收

系统复位

调制方式

缓存区操作

* 常见的有FM17550， FM17522， MFRC523， MFRC522  

软件通信接口

* SPI, I2C, UART

数据流

* 上位机&lt;--&gt;读写器&lt;--&gt;射频卡

软件架构

* 软件通信接口初始化
* 读写器初始化
* 根据相应射频协议组包， 通过读写器与射频卡通信

对于FM17550串口配置

FM17550等读写器默认复位后串口配置为：

* 波特率为9600
* 无奇偶校验位
* 无硬/软流控
* 数据位为8bit
* 1位停止位

在linux下配置串口应按上述配置， 尤其注意我们需要将串口配置成原始输出模式， 以及关闭软流控

硬件复位

* 外部IO， 保持低电平一定时间

确认复位成功

* 读地址为0x37h的版本寄存器的值
* mfrc522— — 0x92
* fm17550— — 0x88

接下来步骤： 防冲突， 选卡， 操作卡

软件复位

* 向CommIEnReg寄存器写复位命令

发送部分

* TxModeReg， 根据A类， B类设置bit  

接收部分

* RxModeReg， 根据A类， B类设置bit

天线

* TxControlReg， 复位

特征参数

* Status2Reg， 关闭加密传输
* ModeReg， 根据A类， B类设置相应的CRC校验值
* TReloadRegL， TReloadRegH， TModeReg，TPrescalerReg设置定时器
* TxASKReg， A类100%ASK， B类无需100%ASK

![image-20200904230818987](https://gitee.com/cpu_code/picture_bed/raw/master//20200904230826.png)

上位机主动借助读写器与卡通信

读写器内部有64字节的FIFO缓存区

* FIFOLEVELREG： 该寄存器表示缓存区内的待读取数据字节数
* FIFODATAREG： 64字节的FIFO缓存区， 上位机将要发送给射频卡的数据写入该寄存器， 或上位机根据 FIFOLEVELREG得到可用数据字节数N， 连续读取N字节该寄存器得到读写器接收到射频卡的回应  

由于在通信过程中， 数据接收可能存在非全字节，即可能为5bit， 而非完整的8bit， 出现非全字节的情况发生在接收数据的最后一个字节， 所以我们需要对最后一字节内的比特数进行判断

CONTROLREG： 寄存器内的rxlastbits位， 该标志位表明最后一字节内的比特数

实际接收到的数据比特位总数为：假设FIFOLEVELREG读的值N， N字节实际接收的数据比特位总数 = \(N-1\)\*8 + rxlastbits

COMMANDREG， 该命令寄存器里的 command 位， 可以写入相应的值代表相应的命令:

* （ RESETPHASE） 1111代表读写器立即软复位命令
* （ TRANSCEIVE） 1100代表读写器准备发送并接收射频卡的数据。
* 读写器执行TRANSCEIVE命令收发数据前， 需要将 BITFRAMINGREG寄存器内的startsend位置1。  

根据14443协议规定读卡器发送数据后， 在25ms内读写器必须接收到射频卡的响应， 否则该次通信失效。

实现25ms定时：

* 上位机可以启动定时器或通过软件延时
* 也可以利用读写器内部的定时器：
* TMODEREG : 内部的Tauto位可以在读写器无线数据通信时， 自动开启定时器
* TPRESCLALERREG : 与TMODEREG的低两位组成一个10位的分频器
* TRELOADHIREG,TRELOADLOREG : 共2字节， 定时器装初值  

在相应的时间结束后， 我们不能直接去读FIFO

而是在读写器中有相应的接收完成状态标志位

状态位相关寄存器：

* COMMIENREG ： 状态中断使能， 我们可以使能TX,RX,ERR,TIMER等状态标志位。
* COMMIRQREG ： 状态标志位， 使用前需软件复位清零， 我们使能了RX， 就可在COMMIRQREG寄存器中判断RX的相应为是否被置1， 即接收完毕。
* 同理， 如何知道25ms的定时是否到了， 我们可以while的读取COMMIRQREG寄存器中TIMER的状态，被置1， 说明时间到了。  

