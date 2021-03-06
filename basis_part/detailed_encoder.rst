.. vim: syntax=rst

编码器详解
===============
编码器介绍
~~~~~~~~~~~~~~~
编码器，是一种用来测量机械旋转或位移的传感器。这种传感器能够测量机械部件在旋转或直线运动时的位移位置或速度等信息，
并将其转换成一系列电信号。编码器是工业中常用的传感器之一，广泛应用于工业生产当中需要对机械系统进行监视或控制的场景，
包括工业控制、机器人、照相机镜头、雷达平台以及部分计算机输入设备例如轨迹球和鼠标滚轮等等。

编码器可以根据不同的方式分出很多种类型。例如根据检测原理，可分为光学式、磁式、感应式和电容式。
根据内部机械结构的运动方式，可分为线性编码器和旋转编码器。根据其刻度实现方法及信号输出形式，
又可分为增量式、绝对式以及混合式三种。编码器种类繁多，本章主要讲解旋转编码器，如下图所示，外形很像一个电机。

.. image:: ../media/旋转编码器.png
   :align: center
   :alt: 旋转编码器

图7-1 旋转编码器

增量式编码器
------------
增量式旋转编码器是将设备运动时的位移信息变成连续的脉冲信号，脉冲个数表示位移量的大小。只有当设备运动的时候增量式编码器才会输出信号。
编码器一般会把这些信号分为通道A和通道B两组输出，并且这两组信号间有90°的相位差。同时采集这两组信号就可以知道设备的运动和方向。
除了通道A、通道B以外，很多增量式编码器还会设置一个额外的通道Z输出信号，用来表示编码器特定的参考位置，传感器转一圈Z轴信号才会输出一个脉冲。
增量式编码器只输出设备的位置变化和运动方向，不会输出设备的绝对位置。

绝对式编码器
------------
绝对式旋转编码器是将设备运动时的位移信息通过二进制编码的方式变成数字量直接输出。
这种编码器与增量式编码器的区别主要在内部的码盘。绝对式编码器的码盘利用若干透光和不透光的线槽组成一套二进制编码，
这些二进制码与编码器转轴的每一个不同角度是唯一对应的，读取这些二进制码就能知道设备的绝对位置，所以叫它绝对式编码器。
绝对式编码器一般常用自然二进制、格雷码或者BCD码等编码方式。

混合式绝对式编码器
------------------
混合式绝对式编码器，它输出两组信息：一组信息用于检测磁极位置，带有绝对信息功能；另一组则和增量式编码器的输出信息完全相同。

旋转编码器原理
~~~~~~~~~~~~~~~
旋转编码器的原理示意图如下图所示。旋转编码器内部大都由码盘、光电检测装置和信号处理电路等部分构成。码盘上刻了若干圈线槽，
线槽等距并且可透光，当码盘旋转时就会周期性的透过和遮挡来自光电检测装置的光线，这样检测装置就会周期性的生成若干电信号。
但是这些电信号通常比较微弱，需要加入一套处理电路对信号进行放大和整形，最后把信号整形为脉冲信号并向外输出。

.. image:: ../media/编码器原理示意图.png
   :align: center
   :alt: 编码器原理示意图

图7-2 编码器原理示意图

虽然旋转编码器的原理在总体上差不多，但是对于这些原理的具体实现方法却有很大不同。

增量式编码器原理
----------------
首先来看增量式编码器。上节提到过，增量式编码器都有A、B两通道信号输出，这是因为增量式编码器的码盘上有两圈线槽，
两圈线槽的之间会错开一定的角度，这个角度会使得光电检测装置输出的两相信号相差 1/4 周期(90°)。码盘的具体工作方式如下图所示。
图中黑色代表透光，白色代表遮光。当码盘转动时，内圈和外圈的线槽会依次透过光线，光电检测装置检测到光线通断的变化，
就会相应的输出脉冲信号，因为内外圈遮光和透光时候存在时间差，所以也就有了A、B两通道信号的相位差。

.. image:: ../media/增量式编码器码盘运作方式1.png
   :align: center
   :alt: 增量式编码器码盘运作方式1

图7-3 增量式编码器码盘运作方式1

.. image:: ../media/增量式编码器码盘运作方式2.png
   :align: center
   :alt: 增量式编码器码盘运作方式2

图7-4 增量式编码器码盘运作方式2

.. image:: ../media/增量式编码器码盘运作方式3.png
   :align: center
   :alt: 增量式编码器码盘运作方式3
   
图7-5 增量式编码器码盘运作方式3

根据两相信号变化的先后顺序就可以判断运动方向，记录输出的脉冲个数可以知道位移量的大小，同时通过输出信号的频率就能得到速度。

一些增量式编码器上会有4圈线槽，分别对应A、B、-A、-B四相信号，相邻两相信号间也是差1/4周期，只不过这种编码器会把-A和-B两相信号反相，
然后叠加到A、通道B，用来增强信号。除了通道A、通道B以外，很多增量式编码器还会设置一个额外的通道Z输出信号。通道Z信号也在码盘上有对应的线槽，
不过只有一条，码盘转一圈才会经过一次。通道Z信号一般用做参考零位，指示设备位置或者清除积累量。

另一种较为常用的增量式编码器是霍尔编码器。霍尔增量式编码器在结构上和光电式几乎相同，只不过检测原理变成了霍尔效应。
内部元件也稍有不同，霍尔编码器的码盘上不是线槽，而是不同的磁极，或者有些直接把电机的旋转磁场当作码盘，
然后检测装置换成了霍尔传感器。输出和光电式相同，仍然是相位差1/4周期的A、B两通道信号。

增量式编码器计数起点任意设定，可实现多圈无限累加和测量。需要提高分辨率时，可触发A、B两通道信号的上升沿和下降沿对原脉冲数进行倍频。
但是当接收设备停机重启后，增量式编码器需要重新寻找参考零点。

绝对式编码器原理
----------------
接着是绝对式编码器。绝对式编码器在总体结构上与增量式比较类似，都是由码盘、检测装置和放大整形电路构成，但是具体的码盘结构和输出信号含义不同。
绝对式编码器的码盘上有很多圈线槽，被称为码道，每一条码道内部线槽数量和长度都不同。它们共同组成一套二进制编码，
一条码道对应二进制数的其中一个位，通常是码盘最外侧的码道表示最低位，最内侧的码道表示最高位。码道的数量决定了二进制编码的位数，
一个绝对式编码器有 N 条码道，它就能输出 N 位二进制数，且输出二进制数的总个数是 2\ :sup:`N`\  个。
这些二进制数与转轴的机械位置是固定的，和编码器外部因素无关，所以叫做绝对式编码器。在接收设备断电重启后绝对式编码器无需寻找参考零点。

下图是一个简化版的绝对式编码器码盘，其中白色块透光表示0，黑色块不透光表示1。码盘上的二进制数逆时针依次增大。

.. image:: ../media/绝对式编码器码盘（自然二进制）.png
   :align: center
   :alt: 绝对式编码器码盘（自然二进制）

图7-6 绝对式编码器码盘（自然二进制）

图中码盘有3条码道，一共可表示2\ :sup:`3`\ =8个二进制数，所以整个码盘被分成了8个扇区，每个扇区表示一个3位二进制数，
每个二进制数对应一个转轴的位置信息。码盘采用自然二进制编码，自然二进制编码的优点是很方便直观，但是受编码器制造和安装精度的影响，
实际应用中二进制数的每一位不可能同时改变，或者出现码盘停在两个扇区中间，这些情况都很容易造成读数错误。

为了避免出现读数错误，可以使用格雷码来解决。下图是一个使用格雷码的码盘，同样的，白色块透光表示0，黑色块不透光表示1。码盘上的二进制数逆时针依次增大。

.. image:: ../media/绝对式编码器码盘（格雷码）.png
   :align: center
   :alt: 绝对式编码器码盘（格雷码）

图7-7 绝对式编码器码盘（格雷码）

图中码盘的码道数与上面的自然二进制码盘完全一致，也能表示8个3位二进制数，只不过将编码方式换成了格雷码。
利用任意相邻的二进制格雷码数都只有一位不同的特性，采用这种编码的码盘在一定程度上克服了自然二进制码盘容易产生读数错误的问题。

绝对式编码器还分为单圈绝对式编码器和多圈绝对式编码器，上面举的两个例子都是针对单圈也就是360°以内的情况，当码盘转动超过360°，
输出的编码会重复，这样不符合绝对式编码器数据唯一的要求，所以就出现了多圈绝对式编码器。多圈绝对式编码器的量程可以超过360°，并且通常超出很多，
其内部结构也比单圈的复杂，但是基本原理都是一样的。

编码器基本参数
~~~~~~~~~~~~~~~
- 分辨率：指编码器能够分辨的最小单位。对于增量式编码器，其分辨率表示为编码器转轴旋转一圈所产生的脉冲数，
  即脉冲数/转(Pulse Per Revolution或PPR)。码盘上透光线槽的数目其实就等于分辨率，也叫多少线，较为常见的有5-6000线。
  对于绝对式编码器，内部码盘所用的位数就是它的分辨率，单位是位(bit)，具体还分单圈分辨率和多圈分辨率。

- 精度：首先明确一点，精度与分辨率是两个不同的概念。精度是指编码器每个读数与转轴实际位置间的最大误差，通常用角度、角分或角秒来表示。
  例如有些绝对式编码器参数表里会写±20′′，这个就表示编码器输出的读数与转轴实际位置之间存在正负20角秒的误差，精度由码盘刻线加工精度、
  转轴同心度、材料的温度特性、电路的响应时间等各方面因素共同决定。

- 最大响应频率：指编码器每秒输出的脉冲数，单位是Hz。计算公式：最大响应频率 = 分辨率 * 轴转速/60。

- 信号输出形式：对于增量式编码器，每个通道的信号独立输出，输出电路形式通常有集电极开路输出、推挽输出、差分输出等。
  对于绝对式编码器，由于是直接输出几十位的二进制数，为了确保传输速率和信号质量，一般采用串行输出或总线型输出，
  例如同步串行接口(SSI)、RS485、CANopen或EtherCAT等，也有一部分是并行输出，输出电路形式与增量式编码器相同。

