# pcileech-guide
In order to prevent being deceived, this tutorial has been specially written.
为了防止上当受骗，特地编写的此教程

任何疑问 https://discord.gg/dg3RNcVNra

Please check the scam list 请查看此链接，避免在Github上上当受骗: https://github.com/fif5o/pcileech-ScamList


我相信很多人都已经看完了配置空间的配置，如果你还不会，请使用telscan to coe.py去转换生成自动配置空间。在这些scammer的教程里，他们喜欢对如何下载Vscode/Arbor/telescan这些软件长篇大论，显得他们的教程非常专业。
而在writemask这里，大家似乎都变得默不作声了，随即一笔草草带过。你知道我什么意思吗？因为这些人根本不想让你学会，或者是他们根本不懂(不是指Writemask)，而让你带着疑问去找他付费解答/购买他们的固件。

# writemask

拿最简单的方式来说，你可以在telescan中对所有的灰色/黑色寄存器改为0，对所有的白色寄存器改为1，然后照着抄就好了(尽管有些地方应该是0，但并无大碍)。例如第094H行为：BF FF 20 00。但你需要记住一个特例，如不修改，在drvscan中你会得到错误。

将64 bit address capable 设置为0，看起来像00 00 71 04

![image](https://github.com/user-attachments/assets/7adfd375-1411-42b9-bd44-24610f08f9ce)

# 关于rw 20位和21位

将`rw[20]<= 0`和`rw[19]<= 0;`更改为1的目的是将Command寄存器配置为 ``rw[143:128] <= 16'h0000``(你可以理解为强制设置)
而正确的配置空间和驱动加载后，修改与否都是不受影响的，因此你大可不必修改此处。

# 自动配置空间

使用.telscan转换为coe配置空间的应用，将  `parameter [5:0]   EXT_CFG_CAP_PTR = 6'h0a,`
`parameter [9:0]   EXT_CFG_XP_CAP_PTR = 10'h0a,` 改为**0A**

## > 然而，很多人都不知道为何是0A?**更有甚者**宣传将0A改为其他的可以使其不被检测?

此两行含义为扩展能力链表的起始指针为配置空间偏移0x0A

在Telescan扫描后，我们可以看到每一行有32个字节，对于配置空间来说是[**16进制的**](https://https://zh.wikipedia.org/zh-cn/%E5%8D%81%E5%85%AD%E8%BF%9B%E5%88%B6)
代表着
也就是从第一行开始，依次往下数00 01 02 03 04 05... 08 09 **0A** 则为从**Header Type**寄存器开始接管配置空间，因此你需要在`pcie_7x.sv`文件中手动修改Header Type寄存器(包括此寄存器)之前的所有寄存器。
![image](https://github.com/user-attachments/assets/0e3d79c0-915d-42e6-a7ea-783bde4e785e)




# 关于MSI/MSI-X中断的实现

> 前言：有很多人问我为什么他用gpt这种AI实现了MSI的中断后，然而他们依旧在DRVSCAN是"获取中断计数失败"或者是"中断未启用"。这是因为一些教程里教你们在core_top.sv中更改MSI/MSI-X能力的开关，以为这样就可以实现中断，实际上此只声明了模块的存在与否，而开关与此并无太大关联。所以，实现需要运用[**使能信号(Enable Signal)**]([https://](https://aijishu.com/a/1060000000310121))
