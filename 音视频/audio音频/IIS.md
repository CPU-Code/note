<!--
 * @Author: cpu_code
 * @Date: 2020-06-26 13:48:58
 * @LastEditTime: 2020-06-27 23:01:46
 * @FilePath: \md\音视频\audio音频\IIS.md
 * @Gitee: https://gitee.com/cpu_code
 * @CSDN: https://blog.csdn.net/qq_44226094
--> 
 * @Author: cpu_code
 * @Date: 2020-06-26 13:48:58
 * @LastEditTime: 2020-06-27 23:01:39
 * @FilePath: \md\音视频\audio音频\IIS.md
 * @Gitee: https://gitee.com/cpu_code
 * @CSDN: https://blog.csdn.net/qq_44226094

## IIS总线 

　　IIS ( Integrate InteRFace of Sound ,  集成音频接口 )，在上个世纪80年代首先被Philips公司用于消费产品的音频设备，并在一个称为 LRCLK( Left／Right CLOCK )的信号机制中经过多路转换，将两路音频信号合成单一的数据队列。

当 `LRCLK` 为高时，左声道数据被传输；

`LRCLK` 为低时，右声道数据被传输 (也可以反过来，高低 与 左右声音的对应可以自定义)。

对于多通道系统，在同样的 BCLK 和 LRCLK 条件下，并行执行几个数据队列也是可能的。由于IIS、PCM 和 类似的音频接口不能提供寄存器入口，因此需要独立的控制接口。

　　IIS总线一般具有4根信号线，如图1所示，包括 **串行数据输入**( IISDI )、串行数据**输出**( IISDO )、**左／右声道选择**( IISLRCK ) 和 串行数据**时钟**( IISCLK )；产生 IISLRCK 和 IISCLK 的是**主设备**。

![img](https://gitee.com/cpu_code/picture_bed/raw/master//20200626143259.jpg)

I2S（Inter-IC Sound Bus）是飞利浦公司为数字音频设备之间的音频数据传输而制定的一种总线标准。在飞利浦公司的 I2S 标准
中，既规定了硬件接口规范，也规定了数字音频数据的格式。

I2S 有 3 个主要信号

**串行时钟** SCLK，也叫位时钟（BCLK），即对应数字音频的每一位数据，SCLK 都有 1 个脉冲。SCLK 的频率=2×采样频率×
采样位数

 **帧时钟** LRCK，用于切换左右声道的数据。LRCK 为“1”表示正在传输的是左声道的数据，为“0”则表示正在传输的是右声道的
数据。LRCK 的频率等于采样频率。
LRCK 必须与 SCLK 保持同步，通常 LRCK 的下降沿同步于 SCLK 下降沿。

**串行数据** SDATA，就是用二进制补码表示的音频数据。

I2S 发送端在 SCLK **下降沿发送**数据、I2S 接收端在 **SCLK 上沿接收**数据。

 有时为了使系统间能够更好地同步，还需要另外传输一个信号 MCLK，称为主时钟，也叫系统时钟（Sys Clock），是采样频
率的 256 倍或 384 倍。通常 MCLK、SCLK、LRCK 都是同步的，LRCK 和 SCLK 的下降沿同步于 MCLK 的下降沿。但内置有
PLL 的音频 CODEC 可能不需要 MCLK 与 SCLK、LRCK 同步，但 SCLK 无论如何应该是与 LRCK 同步的。
一个典型的 I2S 信号见图 3。（图 3 I2S 信号）图 3  

<img src="https://gitee.com/cpu_code/picture_bed/raw/master//20200626163155.png" alt="image-20200626163155459" style="zoom: 150%;" />

I2S 格式的信号无论有多少位有效数据，数据的最高位总是出现在 LRCK 变化（也就是一帧开始）后的第 2 个 SCLK 脉冲处。
这就使得接收端与发送端的有效位数可以不同。如果接收端能处理的有效位数少于发送端，可以放弃数据帧中多余的低位数据；
如果接收端能处理的有效位数多于发送端，可以自行补足剩余的位。这种同步机制使得数字音频设备的互连更加方便，而且不会
造成数据错位  

## Left Justified(左对齐) 与 Right Justified(右对齐)

随着技术的发展，在统一的 I2S 接口下，出现了多种不同的数据格式。

根据 SDATA 数据相对于 LRCK 和 SCLK 的位置不同，分为左对齐（较少使用）、I2S 格式（即飞利浦规定的格式）和右对齐（也叫日本格式、普通格式）。这些不同的格式见图 4 和 图 5。（图 4 几种非 I2S 格式）图 4（图 5 几种 I2S 格式）图 5
为了保证数字音频信号的正确传输，发送端和接收端应该采用相同的数据格式和长度。当然，对 I2S 格式来说数据长度可以不同  

<img src="https://gitee.com/cpu_code/picture_bed/raw/master//20200626163255.png" alt="image-20200626163255785" style="zoom:150%;" />

<img src="https://gitee.com/cpu_code/picture_bed/raw/master//20200626163307.png" alt="image-20200626163307497" style="zoom:150%;" />



I2S、Left Justified(左对齐)与 Right Justified(右对齐)在时序上的区别  

这三种数据格式的区别在于 SDATA 数据相对于 LRCK 和 SCLK 的位置不同

I2S 格式的信号数据的最高位总是出现在 LRCK 变化（也就是一帧开始）后的第 2 个 SCLK 脉冲处。

Left Justified 格式的信号数据的最高位总是出现在 LRCK 变化（也就是一帧开始）后的第 1 个 SCLK 脉冲处。

Right Justified 格式的信号数据的最低位总是出现在 LRCK 变化（也就是一帧结束）前的第 1 个 SCLK 脉冲处。  

常见的 MCLK、SCLK、LRCK 时钟频率  

常见的 LRCK 频率（音频采样时钟频率）有以下几种，

48kHz

44.1kHz，它和 48kHz 是最常用的音频采样频率。在 DVD、机顶盒、多媒体播放器、平板电脑的音乐播放器
中，这两个采样频率最经常被使用

 32kHz。32kHz 是最经常在无线音频中应用的。 24kHz，16kHz , 12kHz  ,  11.025kHz，它和 24kHz、16kHz、12kHz 经常应用在要求音质较好的录音过程中

8kHz，这是广泛用于语音录音、语音回放的一种采样频率。常用在视频语音通话、DVR 监控等应用中  

| 数据长度 | SCLK (kHz)      | MCLK (MHz) | LRCK (kHz) |
| ---------- | ---------------- | ---------- | ---------- |
| 32 bits    | 48 * 2 * 32      | 4 * SCLK   | 48kHz      |
| 24 bits    | 48 * 2 * 24      | 4 * SCLK   |            |
| 20 bits    | 48 * 2 * 20      | 4 * SCLK   |            |
| 18 bits    | 48 * 2 * 18      | 4 * SCLK   |            |
| 16 bits    | 48 * 2 * 16      | 4 * SCLK   |            |
| 32 bits    | 44.1 * 2 * 32    | 4 * SCLK   | 44.1kHz    |
| 24 bits    | 44.1 * 2 * 24    | 4 * SCLK   |            |
| 20 bits    | 44.1 * 2 * 20    | 4 * SCLK   |            |
| 18 bits    | 44.1 * 2 * 18    | 4 * SCLK   |            |
| 16 bits    | 44.1 * 2 * 16    | 4 * SCLK   |            |
| 32 bits    | 32 * 2 * 32      | 4 * SCLK   | 32kHz      |
| 24 bits    | 32 * 2 * 24      | 4 * SCLK   |            |
| 20 bits    | 32 * 2 * 20      | 4 * SCLK   |            |
| 18 bits    | 32 * 2 * 18      | 4 * SCLK   |            |
| 16 bits    | 32 * 2 * 16      | 4 * SCLK   |            |
| 32 bits    | 24 * 2 * 32      | 4 * SCLK   | 24kHz      |
| 24 bits    | 24 * 2 * 24      | 4 * SCLK   |            |
| 20 bits    | 24 * 2 * 20      | 4 * SCLK   |            |
| 18 bits    | 24 * 2 * 18      | 4 * SCLK   |            |
| 16 bits    | 24 * 2 * 16      | 4 * SCLK   |            |
| 32 bits    | 16 * 2 * 32      | 4 * SCLK   | 16kHz      |
| 24 bits    | 16 * 2 * 24      | 4 * SCLK   |            |
| 20 bits    | 16 * 2 * 20      | 4 * SCLK   |            |
| 18 bits | 16 * 2 * 18     | 4 * SCLK |           |
| 16 bits | 16 * 2 * 16     | 4 * SCLK |           |
| 32 bits | 12 * 2 * 32     | 4 * SCLK | 12kHz     |
| 24 bits | 12 * 2 * 24     | 4 * SCLK |           |
| 20 bits | 12 * 2 * 20     | 4 * SCLK |           |
| 18 bits | 12 * 2 * 18     | 4 * SCLK |           |
| 16 bits | 12 * 2 * 16     | 4 * SCLK |           |
| 32 bits | 11.025 * 2 * 32 | 4 * SCLK | 11.025kHz |
| 24 bits | 11.025 * 2 * 24 | 4 * SCLK |           |
| 20 bits | 11.025 * 2 * 20 | 4 * SCLK |           |
| 18 bits | 11.025 * 2 * 18 | 4 * SCLK |           |
| 16 bits | 11.025 * 2 * 16 | 4 * SCLK |           |
| 32 bits | 8 * 2 * 32      | 4 * SCLK | 8kHz      |
| 24 bits | 8 * 2 * 24      | 4 * SCLK |           |
| 20 bits | 8 * 2 * 20      | 4 * SCLK |           |
| 18 bits | 8 * 2 * 18      | 4 * SCLK |           |
| 16 bits | 8 * 2 * 16      | 4 * SCLK |           |



##  IIS音频驱动实现

音频驱动有3种模式：**MDD／PDD模式**、**Wavedev2**模式、**UAM**模式。

它们相同的地方很明显：接口相同，都是**流驱动**，透过 流接口 与 上层的 `waveapi.dll` 交互。

　　**MDD／PDD**模式 是最早的模式，也是其他驱动常见的分层模式。如果使用CE提供的MDD( wavem—dd.1ib )，会受到一些限制：**仅支持一个设备**；**一个设置仅支持一个流**；对循环的支持不大可靠；对流的支持较弱。当然，由于提供了源码，可以自己修改MDD，突破以上这些限制。

　　**Wavedev2**模式，是因为2000年的 Smartphone 项目产生了新的要求，这些需求需要大改MDD／PDD。

比如上面的限制2，根据CE的开发历史，此时 `waveapi.dll` 也不支持 software mixer，这就是说只能同时允许一个应用在播放。所以根据当时情况，CE的多媒体开发团队设计了 `Wavedev2` 模式。

这是一个**单体** ( 不分层 ) 的驱动模式，平台相关的模块都在 `hwctxt.h` 和 `hwetxt.cpp` 中，此外还加入了 midi 支持、software mixer支持、S／PDlF接口、gain class接口、forcespeaker接口，等等。因此，开发Smartphone或者PPC，这个模式是挺适合的。

　　**UAM**模式( 统一音频模式 , Unified AudioModel )，在开发 WinCE4.2 时，要增加对 DirectSound 的支持，而且有一些音频设备是支持硬件 mixer 的，对此使用 UAM 是很好的选择。

　　本测试采用MDD／PDD的驱动结构，下面讲述本驱动的关键点。

### DMA控制及驱动

　　通俗地讲，DMA( 直接内存存取 )**不需要CPU干扰**也 **不消耗CPU资源**，可以把 音频数据自动地从 **系统**总线 搬到 **IIS** 总线上；

如 音频平均按采样频率 44.1 kHz、16位字长、左右2声道计算，码流为1.411 Mbps，通常在1～3Mbps，所以采用 DMA传输 十分必要。

### 时钟配置

　　只要 位时钟 和 采样时钟 能匹配好，IIS 数据格式 主从一致 ，DMA配置好，音频就可以工作了。

```c
v_pIOPregs->IGPEUP |= DISABLE_IIS_PULLUPS;
V_PIOPregs->IGPECON |= (ENABLE_IISSDO | ENABLE_IISSDI | ENABLE_IISCDCLK | ENABLE_IISSCLK | ENABLE_IISLRCLK);
v_pIISregs->rIISCON = (TRANSMIT_DMA_REQUEST_ENABLE | IIS_PRESCALER_ENABLE);
v_pIISregs->rIISMOD = (MASTER_CLOCL_MPLLIN | IIS_SLAVE_MODE | 
                       IIS_TRANSMIT_RECEIVE_MODE | ACTIVE_CHANNEL_LEFT | 
                       SERIAL_INTERFACE_IIS_COMPAT | DATA_16_BITS_PER_CHANNEL |
                       MASTERCLOCK_FREQ_256fs | SERIAL_BIT_CLOCK_FREQ_32fs);
v_pIISregs->rIISFCON = (TRANSMIT_FIFO_ACCESS_DMA | TRANSMIT_FIFO_ENABLE);
SetIISClockRate(IS2LRCLK_48000);
```



IIS数据格式主要分3种：**左对齐**、**右对齐**、**IIS格式**。如果 声音听起来“怪怪地”，就是数据格式不对。

频率计算方法如表1所列 : 

![img](https://gitee.com/cpu_code/picture_bed/raw/master//20200626152241.jpg)



IIS主设备时钟频率可以通过 **采样频率** 来选择。

IIS主设备时钟频率是由 IIS**预分频器**产生的( IIS主设备时钟频率 = MCLK／预分频器值)，因此必须选择合适的 **预分频器**的值 和 CODECLK的**采样频率**类型( 256 或 384fs )，才能获得合适的 IISLRC **采样频率** ( IISLRCK频率 = IIS主设备时钟频率／CODECLK的采样频率类型 )；

串行位采样 频率类型(16／32／48fs) 可以通过配置 每个通道的**串行位数** 和 CODECLK采样**频率类型** 来完成，它们之间的关系如表2所列。

![img](https://gitee.com/cpu_code/picture_bed/raw/master//20200626152524.jpg)



如晶振频率为16.9344 MHz，通过384分频为44.1kHz(采样频率就是这么来的)。

　　位时钟频率 = 采样频率 × 数据位 × 2 = 44.1 kHz × 16 × 2 = 1.411 MHz

　　对于其他频率的**晶振** 或 是来自于**总线**的时钟频率，就要计算出 IISC0N 中的分频系数了，以最大限度拟合 CODECLK。

### CODEC控制

　　目前有 **SPI** 、**I2C** 和 **L3** 三种总线控制CODEC 。L3总线 ( L3MODE、L3CLOCK、L3DATA ) 都是由通用的 I／O端口 来控制的。其中L3接口实际上是一种串行接口，它由3根信号线组成，完成处理器和C0DEC之间的数据和控制信号交换。UDAl341TS就是采用L3接口的。

　　L3DATA：处理器接口数据线。

　　L3MODE：处理器接口模式信号线。

　　L3CLOCK：处理器接口时钟信号线。

　　三种控制方式中以I2C最为常见。其中I2C又分为 **寄存器**方式 和 **I／O模拟**方式 两种，I／O模拟方式的可移植性好，仅I／0模拟方式的 I2C驱动 又可分为 8位、9位、16位，以及是否带子地址、是否可以连读连写、是否要兼容SCCB总线。 

 

### 音量控制节点

　　使用音量调节的地方较多。图2是音量控制节点的一般模型。

　　①处的 增益 由 播放器的**音量控制**功能 决定，最小是0dB。也就是说，最多只能还原出原信号强度。

　　②和③处的增益由 Coded IC 自身控制，WM8731 没有产生增益功能，②处容易引入信号失真，一般置为0 dB，codec加大音量时主要在③处提高增益。

　　④、⑤处由**功放**决定，最大也是O dB，便携式功放通常是电流型，靠放大电流去推动扬声器。

　　① + ② + ③ 三处的增益和超过O dB时，1 kHz的信号就会产生失真，但是大部分音乐的强度都小于1 kHz测试方波时的强度，所以这三项的和可以比O dB略大，但不能太大，否则会引起信号失真。

　　a．应用程序通过调用waveOutSetVolume，与手工在控制面板中调节音量等效。

　　b．调节MediaPlay播放器音量时，通过消息跟踪可以判断是否改变了①处的增益，即ARM的DSP数字输出增益。

　　c．调节控制面板里的音量时，会发现 CODEC 的功放寄存器值也会改变。猜想是通过IIS总线实现控制相关寄存器，因为在IoControl 消息中没有发现通过 I2C 改写任何寄存器。

![img](https://gitee.com/cpu_code/picture_bed/raw/master//20200626152709.jpg)



通过分析调整音量的方法，有图2所示的5个节点可控制，目的是音量最大失真最小：让①处输出增益最大的情况下，②处PCM Volume置为0 dB(此处放大最容易引入失真)，功放置最大时便能获得不失真最大音量了；如果想再增大音量只能牺牲失真度了，人耳最多接收10％ THD ( Total Hamonic Distortion，总谐波失真)，此种情况下主要靠调节③处的增益。



## 提高音量的有效方法

　　①在C0DEC与功放不可更改的前提下，选择合适的喇叭至关重要(不同的喇叭效果大不一样)。口径大小不等，纸盆有深有浅。在选择喇叭时一般要求功放的额定功率是喇叭额定功率的2倍以上，喇叭的实际最大承受功率是其额定输出功率的2～3倍。喇叭的灵敏度参数很重要，一般是 0.1 W时 85 dB左右，还要看额定功率时的灵敏度。灵敏度用来衡量将电能转换为声音的效率，只讲额定功率不讲额定功率时的灵敏度是没有意义的，额定功率下的低灵敏度无益于电阻丝“发热不出声”。

　　②提高功放电压，根据P=U * U／R，很小的提升电压，就能获得平方级的功率提升。如由4 V → 6V，功率可提高2．25倍。

　　③改善音腔设计。

　　④原则上不建议以牺牲保真度来换取音量。如不得已而为之，使用时也要严格控制在THD < 10％。

### 功放与扬声器的匹配和选择

　　功放的输出功率一定要大于喇叭的输出功率，否则不但会影响声音效果，而且会加速功放的损坏。如选择的喇叭阻抗比功放的输出阻抗高时，将影响放大器的输出功率；而当喇叭的阻抗过低时(如低于4Ω)，使用的功率放大器与额定的输出功率又不相匹配，这种情况下失真将增大。如果喇叭的阻抗符合要求，额定功率又比功放的额定功率稍小，失真就相对小，喇叭的声音质量就好。

　　扬声器的选择：

　　①口径大，纸盆深，转换效率就高，承受功率也越大；口径小，纸盆过浅，高频响应就不好。

　　②用手轻按同样口径的纸盆时，比较费力的扬声器谐振频率高，动态范围较大。

　　③坚硬、密实纸盆的扬声器，高频性能一般较好；粗疏、柔软纸盆的扬声器，音质一般较柔和。

　　④放大器应该有足够的功率输出，尤其是晶体管放大器。扬声器的最大输出功率应该是其额定功率的3倍以上，并且扬声器的最大输入功率应该等于放大器的输出功率，以保护扬声器的安全。

　　⑤阻抗匹配是最基本的要求：对于Class D类功效，由于 PWM 易引起高频干扰，因此还要考虑合适的感抗，以起到滤波作用

如图3所示，线圈的阻抗和感抗组成了一个低通滤波器，理想情况下将阻隔PWM产生的高频谐波干扰。

这里选择增益为一3 dB时的频率作为高频的截止点fc=RL／2πL。当阻抗为8Ω时，令截止频率为20kHz，则有L=RL／2πfc=8Ω／(2π×20 kHz)=64μH。8 Ω的便携式扬声器感抗为20～100μH。如果实际感抗>64μH，将限制带通特性；

如果实际感抗<64μH，截止频率会>20 Hz，此时又会引入噪声。所以，选择扬声器时感抗要尽量接近64μH；对于AB类功放，则不作严格要求。

![img](https://gitee.com/cpu_code/picture_bed/raw/master//20200626152720.jpg)



### 音腔设计

　　好的音腔，同样的功率下，音量会更大。

　　①音腔内要平，不要有高低不平的落差感。

　　②出音孔是音腔面积的15％～20％(手机中常用的)。

　　③音腔要尽量深，形成“V”型出音，效果较好。 

　　④前后音腔要隔开，以免前后声音互相干扰。这个原理和喇叭放出的声音比起喇叭装在箱子里面的声音要小很多的原因一致。

　　⑤前音腔：扬声器前面音腔的大小主要由扬声器上面的泡棉高度所决定，一般来说至少要留O．2 mm的泡棉。前音腔主要对高频声音有所影响，对于SPL(SoundPressure Level，声压级)影响不是太大。

　　⑥后音腔：要足够大，如果能够达到手机喇叭的等效声容积的2倍的水平最好；更大的后音腔使得扬声器在低频可以得到更好的效果。

　　⑦前音腔和出音孔要设计合理、恰当：前音腔和出声孔形成一个Helmholtz共鸣器，会在某个频率点出现谐振峰。若不是特殊设计，可以把该谐振峰调整到高频端(>10 kHz)，相应地就要求前腔浅，出音孔面积大；若有特殊设计要求，譬如为了提高响度，可以把谐振峰调整到3．4～6 kHz，不过带来的结果将是声音偏单调，而且对音源的要求会苛刻。

　　⑧密封性：最基本的是要让扬声器的前音腔和后音腔分开，保证良好的密封性(尽可能地保证手机音腔的密封性)。良好的密封性使得扬声器在低频段可以得到更好的效果(可以得到更大、更柔美的声音)。

## 音效测试

　　由于人耳对音频发声的感官不尽相同，且主观差异较大，曾想写一篇文章，专门介绍音效的评测及控制方法，需控要什么样的仪器，实验方案如何。但由于实验条件和本人能力有限，加上专业性很强，不敢写也怕写不好，只好作罢。以下是Wolfson Microelectronics plc Jason Fan所列(仅供参考)，同时期待这类文章早日出现。

　　①基本仪器：稳压电源、内置滤波器的毫伏表(可以测量输出的噪声和输出的功率)、失真仪、声压仪、信号发生器。

　　②高级仪器：AP音频分析仪、音频全频扫描仪(用来测试扬声器功率)。

　　③音频系统的评估指标有基本指标和升级指标。

　　基本指标有：输出功率、信噪比、频率响应、失真度、左右通道分离度、左右声道平衡度。

　　升级指标(需使用音频分析仪测量)有：THD+N、动态范围、FFT。

　　作音频测试时，一般会使用一些标准的测试信号，如左右声道1 kHz O dB；左右声道30 Hz O dB；左右声道100Hz 0 dB；左右声道10 kHz 0 dB；左右声道16 kHz O dB；左声道l kHz O dB；右声道1 kHz 0 dB。

　　上述仪器都会附带使用方法和实验方案。

## 总结

　　一般的IIS音频CODEC均能驱动。WM XXX系列(WM9712／WM8978／WM8960／WM8731)、UDAl314、PCMl770、UCBl440、CS4344等芯片

　　在应用CS42L52时，发现背景噪声明显，但耳机音质很好，说明噪声来自于功放；一上电不做任何初始化照样有，进一步说明来自功放，而且不随音量改变而改变。不能正面降噪，后来采取的规避措施是：没有DMA传输时关掉声音通道，此问题后来通过新老电路板对比，查出是扬声器的输出端所接LC回路中电感参数不当产生了自激。把电感换成O Ω电阻后，噪声基本消除。

　　在ARM中，晶振以12 MHz和16．934 4 MHz最为常见(视频系统中也有27 MHz或28．XXX MHz)，系统外围总线是50 MHz，能不能配成精准的44．1 kHz或48kHz，要视各芯片自身的PLL了，这一点要格外重视。如果频率相差太多，也会引入噪声且有语速不正常现象。

　　选型建议：

　　①选型时，一定要贴在自己的电路板上实测，不能仅凭供应商的DEMO板演示。

　　②背景噪声有没有，在正版歌曲的前5 s空白时间基本上可以听出来。

　　③要看看芯片是否已量产且是不是已被人采用，口碑是参考的重要因素。

　　WM9712l带有四线电阻Touch接口，在MP4视频播放器方案中应用较为普遍。WM897x在手机中或智能手机采用得比较多；在MP3或是低成本方案中，PCMl770占不少份额；欧胜(Wolfson)推出的WM897x系统IIS音频协议不但提高了系统的集成度，也提高了系统的音频质量；WM897x以DSP微处理器为内核，可将风声等过滤来提高音频系统的录音功能，新产品还采用了5波段与3D音频系统的均衡来提高音频输出以及可编程阻态滤波器消除噪声。这些系统通常也支持时钟频率在12～19MHz的麦克风及手机喇叭的驱动部分，可进一步减少产品中元器件的数量。为使高质量音频喇叭以及压电型喇叭的功耗可以达到900 mW，使用了数字式录音回放限制器，以防止喇叭的过量输出。