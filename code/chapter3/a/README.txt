1.����FreeDos��װ������ѹ��õ�freedos-img�������ļ��н�����a.img���ļ�������Ϊfreedos.img��
2.ʹ��bximage����һ���µ�����ӳ������Ϊpm.img��
3.�޸ĵ�ǰ����Ŀ¼�µ�bochsrc�����ļ�
�������¼��У�
floppya: 1_44="freedos.img", status=inserted
floppyb: 1_44="pm.img", status=inserted
boot: a
4.����bochs��bochs  -f  bochsrc������ִ��"c"
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

5.ִ��c����֮������freedos��Ȼ��ִ�����������ʽ��B�̲��л���B�̣�
format.ext b:
b:

6.��pmtest1.asm�е�0x7c00��Ϊ0100h,�����±��룺
nasm pmtest1.asm  -o pmtest1.com

7.��pmtest1.com���Ƶ���������pm.img�У���������������£�
1)sudo mkdir /mnt/floppy
2)sudo mount -o loop pm.img /mnt/floppy
3)sudo cp pmtest1.com   /mnt/floppy/
4)sudo umount/mnt/floppy 

8.��freedos B�̷�������:\pmtest1.com

9.����ܿ�����ʾ��ɫP.
