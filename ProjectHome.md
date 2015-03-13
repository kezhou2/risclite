本项目为一本关于FPGA设计的书——《兼容ARM9的软核处理器设计：基于FPGA》提供配套教程下载。该书由机械工业出版社华章分社出版。读者可以在这里找到书中对应的设计文件。

![http://risclite.googlecode.com/files/cover.jpg](http://risclite.googlecode.com/files/cover.jpg)

# **第一章: 数字电路设计模型** #

_本章主要讲述数字电路设计的基本模型。在进行数字电路设计之前，必须建立一个数字电路模型。这个建模的过程，就是在心中建立一个电路的基本轮廓。在了解了数字电路的基本单元后，运用它们构建大致模型。_

  1. _最初的模型——带有输入输出的模块_
  1. _组合逻辑_
  1. _时序逻辑_
  1. _同步电路_
  1. _同步电路时序路径_
  1. _RTL描述_
  1. _综合生成电路_


这一章建立数字电路设计的基本概念，是非常基础性的内容，无下载文件。

# **第二章：Verilog RTL编程** #

_本章讲述如何使用Verilog进行RTL编程，讲述如何使用Verilog精炼的进行RTL描述。在对Verilog的描述方法进行了基本归类后，总结了进行RTL设计的基本流程，并在最后，使用一个简单的UART串口设计实例来启发读者完成RTL设计。_

  1. _Verilog语言与RTL描述_
  1. _Verilog描述语句对应电路_
  1. _如何进行RTL设计_
  1. _RTL设计要点_
  1. _UART串口通讯设计实例_

本章使用Verilog设计了一个简单高效的UART控制器。作者已经对它进行了参数化。读者只需要在例化时，指出串口的波特率（波特/秒），以及开发板的工作频率（兆赫兹），即可用在自己的设计当中。下面就是它的例化方式，从这段参数化可以理解设计的参数：它支持的波特率是9600波特/秒，rxtx.v的clk端口的时钟频率是25 MHz。

```
rxtx 
 # ( .baud ( 9600 ),
     .mhz  ( 25   )  
)
u_rxtx ( ... ); 
```

它的下载方式：[rxtx.v](http://risclite.googlecode.com/files/rxtx.v)

这段串口控制器的Verilog RTL代码只有简单的100来行，非常简单实用，适于在开发板上调试使用。推荐各位读者在理解它的功能的基础上进行修改，一定会让你用开发板进行串口调试时更加得心应手。

这个rxtx.v现在支持的配置方式是：8 bit数据位，1 bit奇偶校验位，它的奇偶校验方式偶校验，以及1 bit的停止位。读者在配置PC端的控制终端时，可以按照上面的描述去配置。也可以修改代码以满足你特定的需求。

# **第三章：Modelsim仿真** #

_本章讲述如何使用Modelsim对Verilog RTL设计进行仿真验证。验证是设计中重要的一步，Modelsim是最流行的仿真工具，使用Modelsim建立一个测试环境可以对RTL设计进行各种级别的验证测试。_

  1. _仿真的意义_
  1. _testbench文件_
  1. _Modelsim仿真工具使用_
  1. _UART串口仿真实例_

本章讲述如何使用Modelsim进行设计仿真。但也可以使用开源的iVerilog仿真工具完成。它的下载方式是：[iVerilog下载页](http://bleyer.org/icarus/)。读者在安装完毕后，下载本章设计的tb.v和rxtx.v，链接是：[uart.zip](http://risclite.googlecode.com/files/uart.zip)。在解压缩某个目录，例如D:\sim下后。在windows的“开始”下面，点击“运行”，然后输入cmd，点击确定。在cmd页面内，输入命令："D:"，以及"cd sim"进入解压缩目录。

如果安装正确的话，只需要输入：iverilog tb.v rxtx.v -o run，就会生成一个可执行的仿真文件：run。然后，输入vvp run，即可让刚刚生成的run进行执行，那么tb.v里面的打印语句就会在cmd窗口中输出。

如果你想生成波形，很简单，打开tb.v，在endmodule语句前面加上：
```
initial
begin
        $dumpfile("tb.lxt2");
        $dumpvars(0);
end
```
然后保存tb.v。重新运行iverilog tb.v rxtx.v -o run，重新得到run文件。然后运行vvp run -lxt2，那么就得到lxt2格式的波形文件：tb.lxt2。最后在cmd窗口中，运行gtkwave tb.lxt2，就可以进入波形观察窗口，拉入自己想要的信号，仔细分析吧。具体也可以参照博文：[使用iverilog+gtkwave学习Verilog HDL](http://hi.baidu.com/yuhuntero/blog/item/90a199a4088d56f09152eec6.html)

# **第四章：FPGA开发板原型验证** #

_本章主要讲述如何使用FPGA开发板对设计进行原型验证。FPGA开发板作为数字设计运行的实体，具有实践出真理的价值。掌握FPGA开发板对设计进行验证，对于设计者无比重要。_

  1. _FPGA内部结构_
  1. _FPGA开发板_
  1. _FPGA设计开发流程_
  1. _FPGA设计内部单元_
  1. _UART设计在Altera FPGA的下载执行_
  1. _UART设计在Xilinx FPGA的下载执行_

这一章有两个FPGA开发板运行的工程。一个针对Altera的DE2-115开发板，一个是Digilent的Starter kit开发板。这里提供的运行例程，只针对作者手头的[Digilent公司的Nexys3开发板](http://www.digilentchina.com/product-more.asp?ClassId=1&Unid=166)。这两个工程都会用到串口终端。读者当然可以使用xp系统自带的“超级终端”，但很多win7用户或觉得“超级终端”不给力的，可以下载作者提供的一个简单易用的串口终端软件：[迷你终端](http://risclite.googlecode.com/files/MiniComm.exe)。(注：在进行串口设置时，建议将“流量控制”设为“无”)

第一个FPGA开发板工程，使用前面两章的串口控制器，把接收到的数据+1后，发送回串口中。在书中使用的是Altera公司的DE2-115开发板。这里提供的是：[Nexys3开发板的工程数据包](http://risclite.googlecode.com/files/chapter41.zip)。

第二个FPGA开发板工程，会把“迷你终端”发送的数据，保存入FPGA内部的Block RAM内，在开发者按动某个按钮后，顺序把保存的数据通过串口回送入“迷你终端”。在书中使用的是Digilent的starter kit开发板。这里提供的是：[Nexys3开发板的工程数据包](http://risclite.googlecode.com/files/chapter42.zip)。

# **第五章：ARM9微处理器编程模型** #

_本章主要对ARM9处理器架构进行介绍，使读者对ARMv4这一套运行在众多智能手机上的流行架构有个切实的了解。本章从建立微处理器的基本模型开始，从实现的角度对ARMv4架构的方方面面进行了探讨。在总结出了7种中断和20条指令后，对于下一章执行做了全面的总结和铺垫。_

  1. _ARM公司历史_
  1. _ARM处理器架构_
  1. _微处理器基本模型_
  1. _ARMv4架构模式_
  1. _ARMv4架构内部寄存器_
  1. _ARMv4架构的异常中断_
  1. _ARMv4的架构支持的ARM指令集_
  1. _ARM指令与中断分析_

这里提供ARMv4的指令集对照表(excel格式）。读者可以对指令集的区分一目了然，下载地址：[arm9.xls](http://risclite.googlecode.com/files/arm9.xls)。

# **第六章：兼容ARM9微处理器Verilog RTL设计** #

_本章是本书的核心。讲述了如何在不到1800行的verilog程序里，去实现上一章总结的ARMv4的架构。从现在经典的三级流水线和五级流水线开，对如何有效的实现处理器描述做了全面展开。以此为基础，逐步对兼容ARM9微处理器进行剖析，让读者从处理器内核的实现过程中，学习到Verilog RTL设计的各种技巧。_

  1. _确定RTL设计的输入输出端口_
  1. _经典的三级流水线架构_
  1. _经典的五级流水线架构_
  1. _三级流水线改进架构_
  1. _适于兼容ARM9微处理器的三级架构_
  1. _影响流水线架构执行的四种状况_
  1. _第一级：取指阶段的Verilog RTL实现_
  1. _第二级：乘法运算阶段的Verilog RTL实现_
  1. _第三级：加法运算阶段的Verilog RTL实现_
  1. _寄存器组的写入_
  1. _CPSR/SPSR的写入_
  1. _数据池的读写_
  1. _第四级：读操作数据的回写_

这里提供这一章设计的Verilog RTL代码下载：[兼容ARM9软核处理器Verilog RTL代码](http://risclite.googlecode.com/files/arm9_compatiable_code.v)

# **第七章：Hello world--兼容ARM9处理器内核运行的第一个程序** #

_本章介绍简单的ROM code生成流程，并让它在兼容ARM9处理器内核上运行。KEIL是嵌入式开发中流行的工具，它的后续RealView MDK也因为它良好的特性受到嵌入式设计工程师的欢迎。本章帮助读者编写简单的printf("Hello world")打印程序，以此为契机，建立简单的SoC设计工程。_

  1. _基于FPGA的SoC设计流程_
  1. _使用`RealView` MDK编译Hello World程序_
  1. _Modelsim仿真输出Hello World_
  1. _建立Hello World的FPGA设计工程_

这一章使用ARM公司的[Keil RealView MDK](http://www.keil.com/arm/mdk.asp)作为嵌入式软件开发工具。读者只需在[评估版下载页面](https://www.keil.com/arm/demo/eval/arm.htm)上填写个人信息，即可下载该软件的评估版，可以产生小于32 KB大小的ROM code。

在安装完毕Keil `RealView` MDK后，在安装目录下，会找到examples目录。在这个目录里面，会有Hello子目录，里面放着一个简单的hello world打印项目。读者可以从[这里](http://risclite.googlecode.com/files/Hello_pre.zip)下载。读者可以按照书中的指导，对hello项目进行修改，也可以在这里直接下载修改后的[Hello项目](http://risclite.googlecode.com/files/Hello_pro.zip)。

在准备了ROM code后，我们进入仿真阶段。书中采用的是Modelsim作为仿真工具，这里却使用前面提到的iVerilog开源仿真工具。读者下载[仿真包](http://risclite.googlecode.com/files/sim_hello.zip)，里面包含了两个简单的文件：tb.v和arm9\_compatiable\_code.v。读者需要打开tb.v，对里面的`parameter BINFILE = "D:/keil/Hello/Obj/hello.bin";`语句进行修改，使得仿真获取的bin文件指向读者在hello目录下生成的hello.bin。

修改tb.v结束后，打开cmd界面，进入sim\_hello.zip的解压缩目录。然后运行：`iverilog tb.v arm9_compatiable_code.v -o run`；最后运行`vvp run`，即可见到打印出hello world字样。由于tb.v里没有自动结束语句，读者需要输入ctrl+C键，然后输入finish，强制结束。读者接下来要做的是，对hello工程的源文件hello.c进行修改，打印出你想要的字符串。在Keil `RealView` MDK进行重编译后，只需运行`vvp run`，即可打印出你想要的字符串。接下来，读者可以按照书中的指导，使得仿真支持中断，这里，不再提供参考。

如果你能够进行仿真，那么在FPGA开发板上执行将是顺理成章、水到渠成的事情了。这里，提供在Nexys3开发板上的例子，它是上面仿真的翻版。在这个例子里，采用了Block RAM作为ROM和RAM，使用前面开发的串口程序。读者可以下载这个FPGA工程，作为你在FPGA开发板上执行打印hello world的一个参考。下载地址：[hello world在Nexys3开发板上的例程](http://risclite.googlecode.com/files/chapter7.zip)。建议读者在这个例子的基础上逐步增加一些自己的东西，例如中断的执行，打印一些其他异常符号等等。

# **第八章：Dhrystone Benchmark--兼容ARM9处理器内核性能测试** #

_Dhrystone Benchmark是为各种嵌入式内核测试“体质”的代码。本章结合ARM公司给出的优化方法，使用`RealView` MDK对Dhrystone 2.1代码进行编译。然后使用Modelsim进行仿真，并用FPGA开发板结合串口，打印出真实的测试结果。_

  1. _Dhrystone 2.1介绍_
  1. _移植Dhrystone 2.1进行编译_
  1. _Modelsim仿真运行Dhrystone Benchmark_
  1. _在线可编程的FPGA SoC设计工程_
  1. _Dhrystone Benchmark在开发板中运行_

同上一章一样，本章也将使用Keil `RealView` MDK自带的Dhrystone 2.1测试项目，对兼容ARM9软核处理器进行测试。读者可以再这里下载[DHRY的源项目](http://risclite.googlecode.com/files/DHRY_pre.zip)。然后，按照书中的指导，对它进行修改，让它更加适于兼容ARM9软核处理器，也可直接在这里下载[修改后的DHRY项目](http://risclite.googlecode.com/files/DHRY_pro.zip)。

在编译了软件后，读者可以按照上一章的方法对它进行仿真。仿真文件包里面包含了一个tb.v，以及arm9\_compatiable\_code.v，它的下载方式：[DHRY仿真包](http://risclite.googlecode.com/files/sim_dhry.zip)。使用iverilog仿真软件对该仿真包进行仿真，可能需要十来分钟。最后的DMIPS/MHz结果，也会随着Keil的版本不同而不同。例如，作者采用最新的Keil 4.23版，将得到Dhrystone 2.1的DMIPS/MHz结果为：1.16 DMIPS/MHz。在写作此书时，作者采用的是4.10版本，那时的DMIPS/MHz结果为：1.21 DMIPS/MHz。

同上一章一样，经过仿真以后，在FPGA开发板上重现这一测试过程。作者给出一个参考例程：[Nexys3开发板上的Dhrystone 2.1测试项目](http://risclite.googlecode.com/files/chapter8.zip)。

# **第九章：ucLinux仿真--结合Skyeye，启动不带MMU的操作系统** #

_Skyeye是ARM9处理器的软件模拟器，通过它解析ucLinux内核，可以在软件平台上运行嵌入式软件。本章建立了Modelsim的仿真环境，加载同样的ucLinux内核，可以打印出同Skyeye一样的启动信息。在这个过程中，用户可以通过查看波形，从RTL设计工程师的角度解析嵌入式操作系统。_

  1. _ARM7TDMI-S处理器内核_
  1. _以ARM7TDMI为核心的单片机_
  1. _uClinux嵌入式操作系统_
  1. _`SkyEye`硬件模拟平台_
  1. _Modelsim下仿真uClinux启动过程_

本章将在仿真器中启动uClinux操作系统。这个启动过程以软件模拟器[SkyEye](http://www.skyeye.org/)为参考，整个仿真包已经发布在网上了，下面是链接地址：[uClinux仿真包](http://arm-cpu-core.googlecode.com/files/uclinux.rar)。

# **第十章：Linux OS--结合mini2440开发板，启动带MMU的嵌入式操作系统** #

_Mini2440 ARM9开发板是一种流行的嵌入式开发工具。本章从开发板中得到含有操作系统及文件系统的NAND flash的镜像，然后在Modelsim下建立testbench环境，从该镜像中读出bootloader的第一条指令开始，一步步的启动Linux操作系统。这个过程涉及到带MMU功能模块的处理器的工作机理，读者在本章中可以了解到Linux操作系统的硬件工作环境。_

  1. _ARM920T处理器内核_
  1. _S3C2440A 32-bit微控制器_
  1. _Mini2440 ARM9开发板_
  1. _NAND flash仿真模型_
  1. _为兼容ARM9处理器内核增加协处理器指令_
  1. _建立仿真Linux操作系统的testbench_

Mini2440是由广州友善之臂计算机科技有限公司推出的一块[基于ARM9的开发板](http://www.arm9.net/mini2440.asp)。本章完全参照它的启动过程。仿真包也已经发布在网上，下载地址是：[Linux操作系统仿真包](http://arm-cpu-core.googlecode.com/files/linux.rar)。

谢谢大家购买我的书！希望大家学习愉快！