# nanoDAP-wireless 用户手册
* [产品介绍](#产品介绍) 
* [产品特点](#产品特点)
* [使用场景](#使用场景)
* [使用步骤](#使用步骤)
    * [无线配置](#无线配置)
    * [仿真器选择](#仿真器选择)
    * [目标检测](#目标检测)
    * [烧写算法](#烧写算法)
    * [复位设置](#复位设置)
* [LED状态](#led状态)
* [产品链接](#产品链接)
* [FAQ](#faq)
    * [无线通信会断开，导致调试失败如何解决？](#q-无线通信会断开导致调试失败如何解决)
    * [可以支持多长距离的无线通信？](#q-可以支持多长距离的无线通信)
    * [无线通信连接成功之后，应该如何进行后续的设置？](#q-无线通信连接成功之后应该如何进行后续的设置)
    * [可支持多少个无线仿真器同时使用，互相之间是否会产生干扰？](#q-可支持多少个无线仿真器同时使用互相之间是否会产生干扰)
    * [可支持一个发射机和多个接收机配对使用吗?](#q-可支持一个发射机和多个接收机配对使用吗)
    * [目前支持哪些芯片的调试烧录？](#q-目前支持哪些芯片的调试烧录)
    * [在linux下可以进行调试吗？](#q-在linux下可以进行调试吗)

# 产品介绍
nanoDAP-wl 是实验室推出的基于cmsis-dap的无线调试器，即插即用，速度快，支持虚拟串口。无线调试器包括发射机/接收机，基于2.4G无线通信，可对10m范围内的目标进行程序烧录和调试，在某些有线仿真器不便调试的场景，如目标始终处于移动状态（飞行器、小车、机器人等），目标已经组装成产品形态，并且已安装在墙上或者高处等。此时使用无线调试器能较好的解决这些场景下调试问题，有效提高研发效率。
![nanoDAP-wl-v2.0-a](https://github.com/wuxx/nanoDAP-wireless/blob/master/doc/nanoDAP-wl-v2.0-a-0.1.jpg)
![nanoDAP-wl-v2.0-b](https://github.com/wuxx/nanoDAP-wireless/blob/master/doc/nanoDAP-wl-v2.0-b-0.1.jpg)
# 产品特点
- 使用极简，PC端无需安装额外软件，只需将发射机和接收器分别上电，等待连接成功，即可开始调试
- 支持SWD协议，典型的基于ARM Cortex-M系列芯片均支持SWD调试，常见的芯片如STM32系列，GD32系列，ATMEL-SAM系列，NORDIC-NRF51/52系列，NXP-LPC系列等芯片均支持SWD调试下载
- 支持JTAG协议，配合开源调试器OpenOCD可支持全球范围内几乎所有SoC芯片的调试，如ARM Cortex-A系列、DSP、FPGA、MIPS等，因为SWD协议只是ARM自己定义的私有协议，而JTAG则是国际IEEE 1149标准
- 支持虚拟串口，而且支持同时进行仿真调试和串口输出
- 接收机支持向目标板供电（5V、3.3V），以及从目标板取电（5V、3.3V）两种方式进行工作
- 支持MDK/IAR/OpenOCD，支持Windows/Linux/Mac 下进行调试开发
- 软件基于CMSIS-DAP实现，使用USB HID协议，无需安装驱动即可下载调试


# 使用场景
1. 用于调试飞行器，小车，机器人，由于调试目标为通常处于移动状态，若使用传统调试器不仅下载比较麻烦，而且无法进行单步调试。
2. 目标板已经组装好外壳，成为产品形态，此时传统的有线方式不便调试。  
3. 产品安装在高处，如路灯、高塔等位置，此时使用有线方式不便调试。

# 使用步骤
## 无线配置
无线的连接配置不需要安装额外的软件配置，您只需将发射机和接收机上电，观察无线模块上的蓝灯从闪烁变为常亮即说明无线连接已经建立，无线连接建立之后即可按照有线的方式操作使用，以下是详细的说明。
1. 将接收器和目标单板相接，并上电，此时可以观察到接收器开始蓝灯慢速闪烁，等待和发射器建立连接
2. 将发射器和PC相接，上电后发射器会开始搜索接收器并蓝灯闪烁，当成功搜索到接收器后并建立连接后，则发射器和接收器均为处于蓝灯常亮的状态
3. 亦可先连接发射器，再连接接收器，只需最终观察到双方蓝灯都从闪烁到蓝灯常亮的状态变化即说明连接正常建立
4. 建立连接后，即可开始进行调试，此时只需想象发射器和接收器之间连接了一根虚拟的信号线即可，使用方式和nanoDAP仿真器完全一样，具体用法请见下文或者参考[nanoDAP用户手册](https://github.com/wuxx/nanoDAP/blob/master/doc/README.md)
5. 若由于目标超出通信距离（一般电磁环境下10m左右），或者供电不稳定，则可能会造成连接断开，发射机/接收机处于蓝灯慢速闪烁的状态，此时若需要恢复通信，只需重新调通信距离或者供电电源，无需重新插拔发射机或者接收机，然后注意观察蓝灯连接状态即可

## MDK配置说明
将DAP仿真器 插入到PC的USB口中，若一切正常，则在设备管理器中会出现一个虚拟串口和USB-HID设备，如图所示  
![usb_device](https://github.com/wuxx/nanoDAP-wireless/blob/master/doc/usb_device.png)
## 仿真器选择
在 Option -> Debug 一栏中选择CMSIS-DAP Debugger  
![debug_select](https://github.com/wuxx/nanoDAP-wireless/blob/master/doc/debug_select.jpg)
## 目标检测
在 Option -> Debug 菜单中点击Settings 进入配置菜单，如图所示，假若仿真器已经正常连接，则在左侧窗口会识别出仿真器的相关信息，假若使用SWD接口进行调试烧录，则请将接口配置成和左侧红框处一致。假若此时目标单板已经正常连接，则在右侧红框出会识别出目标单板的相关信息。  
![target_id1](https://github.com/wuxx/nanoDAP-wireless/blob/master/doc/target_id1.png)

## 烧写算法
对于特定的目标芯片，您需要为其配置特定的烧写算法，以stm32f10x系列为例，如图所示：  
![flash_algorithm](https://github.com/wuxx/nanoDAP-wireless/blob/master/doc/flash_algorithm.jpg)


## 复位设置
一般情况下，您或许希望烧写完芯片之后立即开始运行，我们的DAP仿真器经过软件定制，支持复位后立即开始运行，您需要在Debug 选项中进行配置，如图所示：  
![reset_select](https://github.com/wuxx/nanoDAP-wireless/blob/master/doc/reset_select.jpg)


# LED状态
为方便说明，称发射机称之为dap-host，因其与host PC连接。接收机则称为dap-target，因其与目标单板相接。

LED | 说明
---|---
dap-host 蓝灯慢速闪烁 | 等待和dap-target连接
dap-host 蓝灯常亮,偶尔快速闪烁| 已经和dap-target成功建立连接
dap-host 蓝灯快速闪烁 | 正在和dap-target交换数据
dap-target 蓝灯慢速闪烁 | 等待和dap-host连接
dap-target 蓝灯常亮,偶尔快速闪烁 | 已经和dap-host成功建立连接
dap-target 蓝灯快速闪烁 | 正在和dap-host交换数据

# 产品链接
[nanoDAP-wl无线仿真器](https://item.taobao.com/item.htm?spm=a1z10.1-c-s.w4004-21349689053.3.4f8d20f8MryK8Q&id=596673065140)

# FAQ
### Q: 无线通信会断开，导致调试失败如何解决？  
由于无线调试器工作在ISM 2.4G公共频段内，而蓝牙、wifi、及部分遥控器均工作在此频段内，此频段内的电磁干扰较大，有一定可能会造成通信失败。且假若您在室内进行调试，室内的物体遮挡、天线的位置，通信的多径效应均有可能导致连接断开。当检测到连接断开后，发射器和接收机间会自动重新建立连接，请观察连接状态指示灯，即可重新进行调试。假若通信频繁断开，请检查接收机的供电是否稳定，适当调整位置、距离，其均有可能影响通信的稳定性。
### Q: 可以支持多长距离的无线通信？
在空旷场地上，可以实现10m内的无线调试。
### Q: 无线通信连接成功之后，应该如何进行后续的设置？
由于系统基于cmsis-dap实现，连接成功后，所有的配置和cmsis-dap仿真器一样操作，具体操作说明请参考 请见[nanoDAP用户手册](https://github.com/wuxx/nanoDAP/blob/master/doc/README.md)
### Q: 可支持多少个无线仿真器同时使用，互相之间是否会产生干扰？
每对无线仿真器出厂时已经一一配对，而且互相之间不会干扰、交叉联接。理论上, 可支持256对无线仿真器同时进行工作。
### Q: 可支持一个发射机和多个接收机配对使用吗？
无线仿真器的固件经过精心设计，支持一个发射机和多个接收机进行配对，（当然，同一瞬间只能有一对发射机接收机处于工作状态）。在某些使用场景，你可只需采购一个发射机以及多个接收机。配对仍然无需安装额外的软件，您只需将发射机上电，将接收机的nRST引脚和GND短路之后再上电，此时接收机启动后会重新搜索当前无线频段中的发射机并与之配对，等待接收机上的红灯变为绿灯，即说明配对成功。
### Q: 目前支持哪些芯片的调试烧录？
典型的使用场景为对单片机进行编程调试，理论上Cortex-M系列的内核均可以使用DAP进行烧录调试，典型的芯片如STM32全系列的芯片，GD32全系列，nRF51/52系列等，由于也支持JTAG协议，理论上可支持更多的芯片调试，如ARM Cortex-A系列，MIPS、DSP、FPGA等。
### Q: 在linux下可以进行调试吗？
linux下可以使用openocd配合DAP仿真器进行调试（windows下亦可使用openocd），openocd是目前全世界最流行，最强大的开源调试器上位机，由于openocd是跨平台的，你也可以在windows下使用openocd，通过编写适当的配置脚本，可以实现对芯片的调试、烧录等操作。由于涉及内容较多，更多说明请读者自行搜索，或者留言咨询。  


有任何问题或者建议，请在本仓库的[Issues](https://github.com/wuxx/nanoDAP-wireless/issues)页面中提出，我们会持续跟进解决。
