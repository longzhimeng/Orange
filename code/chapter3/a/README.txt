1.下载FreeDos安装包，解压后得到freedos-img，进入文件夹将其中a.img的文件重命名为freedos.img。
2.使用bximage生成一个新的软盘映像起名为pm.img。
3.修改当前工作目录下的bochsrc配置文件
增加如下几行：
floppya: 1_44="freedos.img", status=inserted
floppyb: 1_44="pm.img", status=inserted
boot: a
4.启动bochs：bochs  -f  bochsrc，并且执行"c"
You can also start bochs with the -q option to skip these menus.

1. Restore factory default configuration
2. Read options from...
3. Edit options
4. Save options to...
5. Restore the Bochs state from...
6. Begin simulation
7. Quit now

Please choose one: [6] 
00000000000i[      ] installing x module as the Bochs GUI
00000000000i[      ] using log file bochsout.txt
Next at t=0
(0) [0x0000fffffff0] f000:fff0 (unk. ctxt): jmpf 0xf000:e05b          ; ea5be000f0
<bochs:1> c

5.执行c命令之后，启动freedos，然后执行如下命令，格式化B盘并切换到B盘：
format.ext b:
b:

6.将pmtest1.asm中的0x7c00改为0100h,并重新编译：
nasm pmtest1.asm  -o pmtest1.com

7.将pmtest1.com复制到虚拟软驱pm.img中，具体操作步骤如下：
1)sudo mkdir /mnt/floppy
2)sudo mount -o loop pm.img /mnt/floppy
3)sudo cp pmtest1.com   /mnt/floppy/
4)sudo umount/mnt/floppy 

8.在freedos B盘符下输入:\pmtest1.com
9.最后能看到显示红色P.

------------------------------------------------------------------------------------------------------
A20 地址线问题
	1981 年8 月，IBM 公司最初推出的个人计算机IBM PC 使用的CPU 是Intel 8088。在该微机中地址线只有20 根(A0 – A19)。在当时内存RAM 只有几百KB 或不到1MB 时，20根地址线
已足够用来寻址这些内存。其所能寻址的最高地址是0xffff:0xffff，也即0×10ffef。对于超出 0×100000(1MB)的寻址地址将默认地环绕到0×0ffef。当IBM公司于1985 年引入AT 机时，
使用的是Intel 80286 CPU，具有24 根地址线,最高可寻址16MB，并且有一个与8088完全兼容的实模式运行方式。然而，在寻址值超过1MB 时它却不能象8088那样实现地址寻址的环绕。
但是当时已经有一些程序是利用这种地址环绕机制进行工作的。为了实现完全的兼容性，IBM公司发明了使用一 个开关来开启或禁止0×100000地址比特位。由于在当时的8042键盘控制
器上恰好有空闲的端口引脚（输出端口P2，引脚P21），于是便使用了该引 脚来作为与门控制这个地址比特位。该信号即被称为A20。如果它为零，则比特20及以上地址都被清除。从而
实现了兼容性。
	由于在机器启动时，默认条件下，A20地址线是禁止的，所以操作系统必须使用适当的方法来开启它。但是由于各种兼容机所使用的芯片集不同，要做到这一点却是非常的麻烦。因此通
常要在几种控制方法中选择。对A20 信号线进行控制的常用方法是通过设置键盘控制器的端口值。这里的setup.s程序（138-144行）即使用了这种典型的控制方式。对于其它一些兼容微 
机还可以使用其它方式来做到对A20线的控制。有些操作系统将A20的开启和禁止作为实模式与保护运行模式之间进行转换的标准过程中的一部分。由于键盘的 控制器速度很慢，因此就不
能使用键盘控制器对A20线来进行操作。为此引进了一个A20 快速门选项(Fast Gate A20)，它使用I/O 端口0×92 来处理A20信号线，避免了使用慢速的键盘控制器操作方式。对于不含键
盘控制器的系统就只能使用0×92端口来控制，但是该端口也有可能被其它兼容微机上 的设备（如显示芯片）所使用，从而造成系统错误的操作。还有一种方式是通过读0xee端口来开启A20 
信号线，写该端口则会禁止A20 信号线。

—————————————————————-
enableA20    macro 
                     in       al,92h 
                     or      al,2 
                     out        92h,al 
                     endm 
disableA20   macro 
                     in        al,92h 
                     and       al,0fdh 
                     out        92h,al 
                     endm  

92h,即“1001‘0010“的第二位决定是否打开A20(1=yes   0=no) , 在看过上面转载的文章后 我想这里的92h是怎么来的 也就不言而喻了。

