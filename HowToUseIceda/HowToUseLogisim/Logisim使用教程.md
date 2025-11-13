# Logisim使用教程

## 一、Logisim介绍

​	Logisim是由Carl Burch开发，不过原始Logisim项目已经停止维护，现在多采用社区维护的Evolution分支。Logisim包含基本门电路（与或非门等）、组合逻辑元件、时序逻辑原件、输入输出设备以及存储单元等多种电路元件。

- 支持层次化设计，可以重复将电路部分定义为子电路以及**封装**，提高设计效率和电路可维护性
- 实时仿真与调试

​	学习基础：**基础电路分析**、模电（比较少）、**数电**（比较多）、**计算机硬件基础**、Multisim仿真基础（学过这个上手Logisim会很快）

​	在此教程中，我们介绍设计一款8bit计算机为例子来简单介绍一下该软件。

## 二、8bit计算机的简单硬件系统架构设计

​	系统采用总线结构连接各主要部件，主要包括：中央处理器（CPU）作为核心控制和运算单元、程序存储器（ROM）用来存放汇编代码、数据存储器（RAM）用来存放软件运行时的状态数据（如坐标等）、输出/显示模块（LED矩阵）用来显示画面。

### **2.1RG寄存器组设计**

​	寄存器组是计算机体系结构中的核心组件，用于快速存储和读取CPU运行所需的少量数据。在此次项目设计中，设计有两个独立的读取端口，用于存储坐标X和Y，中间部分由8个D触发器组成。此寄存器组设计了两个读取端口，可以在一个时钟周期内同时读取两个不同寄存器的值。左侧的解码器和与门，用来控制向具体哪个寄存器存入数据。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps1.jpg)

​											图1.寄存器组RG设计图

### **2.2运算器ALU设计原理**

​	ALU的核心是一个两级结构，通过一个多路选择器（MUX）来选择算术单元（AU）或者逻辑单元（LU）的结果作为最终输出。如图2：

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps2.jpg)

​											图2.运算器ALU设计图

​	AU是由一个全加器和多路选择器构成，其中减法是通过一个非门取反，即B非，送入加法器与1相加，即按位取反加1。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps3.jpg)

​											图3.AU设计示意图

​	LU主要是由多个并行的逻辑门和一个多路选择器组成，用来执行按位与/按位或的计算。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps4.jpg)

​											  图4.LU设计示意图

### **2.3存储器设计**

​	ROM（程序存储器）： 存储汇编指令机器码。大小应足够存储整个游戏程序，例如256x8位（256条指令）。本项目设计Address Bit Width采用8位，Data Bit With采用32位。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps5.jpg)

​											    图5.ROM示意图

​	ROM左右接线分别指的是Address、Data，将鼠标放置在sel引脚上，显示注释“Chip select: 0 disables component”，即片选。在本项目中并不使用。

  				![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps6.jpg)![	](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps7.jpg)	
  	
  									 (a)                			    (b)

​									图6.ROM分线器分配示意图

​	其中因为ROM存储的输入指令是32位，但是读、写、运算器功能选择等需要不同宽度的指令，于是增加一个分线器给这32位指令进行一个分配，如上图。但是对于ALU功能选择func其实只需要4位，而B端输入需要8位，此时就需要两个位扩展器来进行调整，分别是将10位扩展成8位、6位扩展成4位，遵循高位补0、多余高位舍弃的扩展原则。根据之前寄存器RG的设计，读写地址都需要5位，于是将第16-20、21-25位分给RG的读写地址。根据布线，将RG的写允许放到最高位，将第30位接到多路选择器的选择信号，将第29位接到RAM的读写功能线上。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps8.jpg)

​												图7.RAM示意图

​	RAM（数据存储器）： 用于存储游戏变量。RAM左侧A、D接线分别是指读地址、写地址，下方str、ld分别是读、写的使能端，为1有效，clr是将RAM里面数据全部清空。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps9.jpg)

​											图8.RAM引脚接线示意图

​	如图8，常将str、ld用一个非门连接控制，表示对“读/写”的控制。其中使用一段连续的RAM区域，每两个字节存储一个段的(X, Y)坐标，存储蛇身的X和Y坐标对。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps10.jpg)

​											图9.功能验证逻辑示意图

​	完成初步的连接之后，进行一个功能验证，即把上图展示的代码运行出来。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps11.jpg)

​										图10.RG使能端功能验证示意图

​	在分线器输入端给一个常数，换成16进制即0x80010046，点击左上角“手”的标志，查看各条线路的工作是否正常。首先是写使能端，如上图所示为“1”，没问题。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps12.jpg)

​										   图11.RAM功能验证示意图

​	如上图，检查分线器第三条线的输出为0，即RAM不可以写。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps13.jpg)

​										图12.RG读地址端功能验证示意图

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps14.jpg)

​										图13.RG写地址端功能验证示意图

​	读地址为0，相应的写地址为1。虽然在RG里面封装了两个读地址口和输出，但是检验时，仅使用第一次设计的读地址口和输出口。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps15.jpg)

​										图14.ALU B端功能验证示意图

​	然后给B值1，即从RAM A端输入地址处读数与1相加，写到寄存器里面

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps16.jpg)

​										图15.ALU B端功能验证示意图

​	最后运算器的功能码是6，进入加法模式。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps17.jpg)

​									图16.RG内部D触发器输出功能验证示意图

​	给一次时钟之后，能看到寄存器一输出为1。则RG、ALU运算正常。

### **2.4 输出模块设计**

​	输出模块采用Logisim的LED矩阵（LED Matrix）作为显示器（8x8），显示蛇头的实时坐标。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps18.jpg)

​											图17.显示器连接示意图

​	对此，设计一个输出逻辑来将蛇身和食物的坐标写入特定的显示输出端口。显示逻辑负责将这些坐标转换为LED矩阵的点亮信号。对于8x8矩阵，可以将8个8位寄存器映射到8行，通过地址译码器和RAM来存储每个像素的状态。

![img](file:///C:\Users\22236\AppData\Local\Temp\ksohtml14828\wps19.jpg)

​											图18.显示器驱动示意图

### 2.5完整项目文件试运行

​	Logisim打开： [demo_1.circ](项目1\demo_1.circ) 或者 [progect2.circ](项目2\progect2.circ) 

​	对应代码文件： [code](项目1\code) 或者 [code](项目2\code) 

​	对应<项目1><项目2>

## 三、Logisim下载

​	网站下载：[Download Logisim](https://sourceforge.net/projects/circuit/files/latest/download)

​	现有.exe：  [logisim-win-2.7.1.exe](logisim-win-2.7.1.exe) 

​	Mac可能下载会出现问题

## 四、界面简介