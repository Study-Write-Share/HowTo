# 嘉立创PCB设计教程

本教程将介绍嘉立创PCB画板的基本操作，想深入学习的可以参考b站在线教程或者嘉立创官方视频。

## 📋 目录

- PCB与嘉立创
- 嘉立创eda下载
- 嘉立创入门
- 嘉立创进阶
- 更多学习资料

## PCB与嘉立创

### 什么是PCB板？

​	PCB 板全称为**印刷电路板（Printed Circuit Board）**，又常被简称为印制板，是电子工业的重要部件之一。

![image-20251102152638778](C:\Users\22236\AppData\Roaming\Typora\typora-user-images\image-20251102152638778.png)

- **基本结构**：由<u>基板、铜箔层、阻焊层、丝印层等</u>组成。

  ​	<u>基板</u>一般由玻璃纤维增强的环氧树脂（FR-4）制成，起到机械支撑和电气绝缘的作用；

  ​	<u>铜箔层</u>是导电层，通过蚀刻工艺形成电路走线；

  ​	<u>阻焊层</u>覆盖在铜箔层上方，防止焊料回流到不需要的区域，同时保护电路板；

  ​	<u>丝印层</u>通常为白色字体，用于标记元器件的位置和编号，方便安装和维修。

- **主要功能**：PCB 板的主要功能是为电子元器件提供机械支撑，让电阻、电容、芯片等元器件有一个稳固的安装位置。同时，它承载着连接各个电子元器件的<u>电路走线</u>，通过这些走线引导电流在不同元器件之间传输，实现复杂的电气功能，使整个电子系统得以正常运转。

- **分类**：按照层数可分为单面板、双面板和多层板。

  ​	<u>单面板</u>只有一面有导电图形，适用于简单电路，成本较低；

  ​	<u>双面板</u>两面都有导电图形，通过过孔实现两面电路的连接，应用较为广泛；

  ​	<u>多层板</u>包含三层及以上的导电层，具备更高的电路密度，能满足高端电子产品对空间和性能的要求。

- **应用领域**：PCB 板应用十分广泛，**几乎涵盖了所有电子设备领域**。在消费电子领域，如智能手机、平板电脑、智能电视等；在医疗保健领域，如血糖仪、心电监护仪等；在工业制造领域，如自动化生产线中的控制器、传感器等；在交通运输领域，如汽车的电子控制系统、飞机的航空电子设备等，都离不开 PCB 板的支持。

  ### 什么是嘉立创？



​	嘉立创EDA是国产PCB设计工具，提供网页在线编辑和软件下载两种设计模式，在线与软件下载均提供**标准版**和**专业版**两个版本（**均免费下载使用**，可根据需要选择，本次教学采用专业版）。登录官网可查询更多：[国产嘉立创EDA - 一个用心为中国人定制的电路板开发平台](https://lceda.cn/)

其优点有：

​	国产软件，界面全中文；

​	上手快，简单易学，官方提供教学视频/训练营/书籍等；

​	嘉立创公司提供**电子产业链一站式服务**：包括 <u>EDA 软件、PCB CAM 软件</u>等设计软件；<u>PCB 打样 / 小批量 / 大批量生产制造</u>；立创商城的<u>电子元器件采购</u>；激光钢网、治具生产；以及 SMT 贴片等服务。

### 嘉立创EDA下载

在线版：

​	[嘉立创EDA(专业版) - V2.2.43](https://pro.lceda.cn/editor)

[	嘉立创EDA(标准版) - 免费、易用、强大的在线电路设计软件](https://lceda.cn/editor)

官网也可查询以下下载路径（2025.12更新）：

#### 嘉立创专业版

**Windows下载：**

​	Windows10及以上(x64)：[lceda-pro-windows-x64-2.2.43.4.exe](https://image.lceda.cn/files/lceda-pro-windows-x64-2.2.43.4.exe)

​	Windows10及以上(arm64)：[lceda-pro-windows-arm64-2.2.43.4.exe](https://image.lceda.cn/files/lceda-pro-windows-arm64-2.2.43.4.exe)

​	Windows7(x64)：[lceda-pro-windows7-x64-2.2.43.4.exe](https://image.lceda.cn/files/lceda-pro-windows7-x64-2.2.43.4.exe)

**Linux下载（4.9以上）：**

​	Intel/AMD(x64)：[lceda-pro-linux-x64-2.2.43.4.zip](https://image.lceda.cn/files/lceda-pro-linux-x64-2.2.43.4.zip)

​	龙芯(loong64)：[lceda-pro-linux-loong64-2.2.43.4.zip](https://image.lceda.cn/files/lceda-pro-linux-loong64-2.2.43.4.zip)

​	申威(sw64)：[lceda-pro-linux-sw_64-2.2.43.4.zip](https://image.lceda.cn/files/lceda-pro-linux-sw_64-2.2.43.4.zip)

​	飞腾/鲲鹏(arm64)：[lceda-pro-linux-arm64-2.2.43.4.zip](https://image.lceda.cn/files/lceda-pro-linux-arm64-2.2.43.4.zip)

**Mac下载（10.15及以上）：**

​	Intel(x64)：[lceda-pro-mac-x64-2.2.43.4.zip](https://image.lceda.cn/files/lceda-pro-mac-x64-2.2.43.4.zip)

​	Apple(arm64)：[lceda-pro-mac-arm64-2.2.43.4.zip](https://image.lceda.cn/files/lceda-pro-mac-arm64-2.2.43.4.zip)

#### 嘉立创标准版

**Windows下载：**

​	Intel/AMD(x64)：[lceda-windows-x64-6.5.51.exe](https://image.lceda.cn/files/lceda-windows-x64-6.5.51.exe)

​	Intel/AMD(ia32)：[lceda-windows-ia32-6.5.51.exe](https://image.lceda.cn/files/lceda-windows-ia32-6.5.51.exe?d=1123)

**Linux下载（4.9以上）：**

​	Intel/AMD(x64)：[lceda-linux-x64-6.5.51.zip](https://image.lceda.cn/files/lceda-linux-x64-6.5.51.zip?d=1123)

**Mac下载（10.15及以上）：**

​	Intel(x64)：[lceda-mac-x64-6.5.51.zip](https://image.lceda.cn/files/lceda-mac-x64-6.5.51.zip?d=1123)

## 嘉立创入门
