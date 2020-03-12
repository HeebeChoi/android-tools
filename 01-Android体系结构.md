**1.Applications应用程序：**<br>
> 应用层是一个核心应用程序的集合，所有安装在手机上的应用程序都属于这一层，例如短信，浏览器，通讯录等，或者下载的微信、QQ、支付宝等<br>

**2.Application Framework应用程序框架：**<br>
> Android为开发者提供的开放平台，位于应用程序的下一层，主要提供了构建应用程序时用到的各种API。Android提供的是一组服务和系统，在开发Applications层应用是会直接使用到。具体有：
```
1）视图系统（View System）：构建应用程序的界面。
2）内容提供者（Content Provider）：允许应用程序访问其他应用程序的数据或者共享数据。
3）通知管理器（Notification Manager）：允许应用程序在状态栏上显示定制的提示信息。
4）活动管理器（Activity Manager）：管理应用程序的生命周期，提供一个通用的导航回退功能。
5）资源管理器（Resource Manager）：提供对非代码资源的管理。
```
**3.Libraries 库：**<br>
> 核心类库包含了系统库和Android运行环境。系统库主要包括一组C/C++库，用于Android系统中不同的组件，这些功能通过Android应用程序框架对开发者开放。
> 一些相关的核心类库：<br>
```
1）C语言系统（libc）：派生于标准C语言系统，并根据嵌入式Linux设备进行调优。
2）多媒体库（Media Framework）：基于OpenCore多媒体开源框架。支持多种视频、音频文件
3）外观管理器（Surface Manager）：管理访问子系统的显示，将2D绘图与3D绘图进行显示上的合成。
4）SGL：底层的2D图形引擎。
5）OpenGL|ES：基于OpenGL ES API的实现。该库使用了硬件3D加速或高度优化的3D软件光栅。
6）FreeType：用于位图和矢量字体的渲染。
7）SQLite：一个强大得瑟关系型数据库。
```
**4.Android Runtime:**<br>
> Android的一些核心库，提供大部分Java编程语言核心库的功能，还包括Dalvik虚拟机，Android应用程序是在Dalvik虚拟机的实例下以进程形式运行。
> Dalvik虚拟机：
> Dalvik是Google公司自己设计的用于Android平台的虚拟机，它可以简单地完成进程隔离和线程管理，并且提高内存的使用效率。每一个Android应用程序在底层都会对应一个独立的Dalvik虚拟机实例。
> jvm.png
> Java 虚拟机：Java虚拟机是基于栈的结构（栈是一个连续的内存空间，取出和存入的速度比较慢）。Java虚拟机运行的是.class字节码文件，Java程序中的Java类会被编译成一个或多个字节码文件（.class）然后打包到.jar文件，之后Java虚拟机会从相应的.class和.jar文件中获取相应的字节码。
> Dalvik 虚拟机：Dalvik 虚拟机是基于寄存器结构（寄存器是CPU上的一块缓存，寄存器的存取速度比内存中存取的速度快很多，这样就可以根据硬件最大限度地优化设备）。Dalvik 虚拟机运行的其专有的.dex文件，Android程序会在编译成.class字节码文件后，通过工具将所有的.class文件转换成一个.dex文件，然后Dalvik虚拟机会从其中读取指令和数据，最后的.odex文件是为了在运行过程中进一步提高性能而对.dex文件进行的进一步优化，加快软件的加载速度和开启速度。

**5.Linux Kernel:**<br>
> Android依赖于Linux相应版本的核心系统服务，例如安全、内存管理、进程管理、网络堆栈、驱动程序模型
