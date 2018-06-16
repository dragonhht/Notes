今天又重装一次deepin，电脑里是Windows10+deepin的配置，但是在deepin装好后，重启发现引导出现了问题，`error:file '/boot/grub/i386-pc/normal.mod' not found`。当时就感觉完了，又要重装了，但是在查找了众多资料后发现，还是有办法补救的。<br>
-   首先得知道引导在哪个地方，输入`ls`命令，可查看所有分区

-   然后依次输入尝试`ls 分区/boot/grub`，查看，如`ls (hd0,gpt3)/boot/grub`,当出现的内容不是`error:unknown filesystem`,类似于`./ ../ grxblacklist.txt ...`的目录信息时，则说明应该就是这个分区了。

-   输入如下命令,该命令以引导分区为`(hd0,gpt3)`为例

    -   set root=(hd0,gpt3)/boot/grub

    -   set prefix=(hd0,gpt3)/boot/grub

    -   insmod normal

    -   normal

-   按照如上步骤输入`normal`后将正确进入引导界面。然后进入系统

你以为这样就结束了吗，你还是太年轻了
在你再一次重启后还是会发现，没有变化，还是那个错误

你需要在正确进入Linux系统后，输入如下命令修复引导

-   sudo update-grub

-   sudo grub-install /dev/sda

不过在输入第二条命令后，我的电脑竟然提示`grub-install: warning: this GPT partition label contains no BIOS Boot Partition; embedding won’t be possible`，遇到这种情况后，可以输入一下命令

-   parted /dev/sda set 1 bios_grub on

-   parted /dev/sda print

-   最后再输入一次`sudo grub-install /dev/sda`，之后应该就没有问题了