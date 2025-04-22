# pcileech-guide
In order to prevent being deceived, this tutorial has been specially written.
为了防止上当受骗，特地编写的此教程

Please check the scam list: https://github.com/fif5o/pcileech-ScamList

本教程会以 https://github.com/fif5o/ASM1042-full-msix-interrupt 为基础，更新到实现基于XHCI控制器的MSI-X中断教程，而不是某些为了骗取信任而随意做出一些"谁都会的"垃圾教程(特指Beaters这个discord服务器)

我相信很多人都已经看完了配置空间的配置，如果你还不会，请使用telscan to coe.py去转换生成自动配置空间。在这些scammer的教程里，他们喜欢对如何下载Vscode/Arbor/telescan这些软件长篇大论，显得他们的教程非常专业。
而在writemask这里，大家似乎都变得默不作声了，随即一笔草草带过。你知道我什么意思吗？因为这些人根本不想让你学会，或者是他们根本不懂(不是指Writemask)，而让你带着疑问去找他付费解答/购买他们的固件。

# writemask

拿最简单的方式来说，你可以在telescan中对所有的灰色/黑色寄存器改为0，对所有的白色寄存器改为1，然后照着抄就好了(尽管有些地方应该是0，但并无大碍)。例如第094H行为：BF FF 20 00。但你需要记住一个特例，如不修改，在drvscan中你会得到错误。

将64 bit address capable 设置为0，看起来像00 00 71 04

![image](https://github.com/user-attachments/assets/7adfd375-1411-42b9-bd44-24610f08f9ce)

