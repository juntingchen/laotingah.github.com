---
layout: post
title: "Kindle 4: Change fonts on Mac OS"
date: 2013-01-21 19:35
comments: true
categories: 
---

## 更换Kindle字体指南（基于Kindle 4和Mac OS）
这是一篇更换Kindle字体的快速指南，不要求读者有任何技术背景。对于没有任何经验的用户，完成以下步骤大概需要20分钟。
本文基于[@miaoo](http://miaoo.in)的《[Kindle 4更换中文字体](http://miaoo.in/kindle4-modify-font.html)》，但针对Mac OS用户提供了更为简捷的ssh方法（下文介绍）。更换字体的原因是Kindle1-4上默认的中文字体Het显示较为粗糙，读者会希望更换更舒服的字体（比如宋体）。
###1、准备想要更换的字体
准备两种字体文件，普通字体用于显示正文以及粗体用于显示标题，分别命名为CJK.ttf和CJK_Bold.ttf。
一般认为比较舒服的是方正雅宋和方正特雅宋，可以在[这里](http://ishare.iask.sina.com.cn/f/22080046.html)下载。
###2、获取Kindle序列号和root密码
序列号可在Menu->Settings->Device Info找到（Serial Number）。root密码可以通过[这个帖子](http://miaoo.in/kindle4-modify-font.html)破解。但如果Kindle版本为4.0，则root密码为"mario"（不含引号）。
###3、将字体文件传输到Kindle
连接Kindle到电脑，将两个字体文件CJK.ttf和CJK_Bold.ttf拷贝到 documents/目录下。
###4、进入调试模式
在Kindle根目录下创建一个名为“ENABLE_DIAGS”的空文件夹。重启设备，Menu->Settings->menu->restart.(注：Menu是指菜单按钮。)
###5、开启USBnet模式
设备进入调试模式后，根据导航菜单依次进入Misc individual diagnostics -> Utilities -> Enable USBnet。看到"PASS“以后，按右方向键(FW Right)确认，屏幕将返回上层菜单。
###6、在Mac OS中为Kindle配置IP地址
在Mac OS中打开系统偏好（System preferences），打开”网络(Network)"，在左栏将看到一个叫”RNDIS/Ethernet Gadget"的设备。将IP地址配置方式设为“手动（Manually）”，设置IP地址（IP Address）为192.168.15.1，子网掩码（Subnet Mask）为255.255.255.0.

{% img /images/2013-01-21-kindle-4-change-fonts/networking1.jpg %}
###7、ssh登录Kindle
打开终端(Terminal)。这东西一般在应用程序(Applications)->实用工具(Utilities)里。输入以下命令：

{% codeblock %}
ssh root@192.168.15.244
{% endcodeblock %}

终端将会提示输入密码，即是第2步获取的root密码。注意，输入密码过程屏幕不会有任何显示，输入完毕后直接按回车即可。
连接成功后，终端提示符将变为
{% codeblock %}
root>
{% endcodeblock %}
###8、拷贝字体文件到系统目录
依次输入以下命令
{% codeblock %}
mntroot rw
mount /dev/mmcblk0p1 /mnt/base-mmc
cp /mnt/base-us/documents/CJK.ttf /mnt/base-mmc/usr/java/lib/fonts/
cp /mnt/base-us/documents/CJK_Bold.ttf /mnt/base-mmc/usr/java/lib/fonts/
{% endcodeblock %}
第一条命令是将系统目录权限改为可读写。第二条命令是挂在系统目录。最后两条命令是将两个字体文件从用户目录复制到系统的字体目录。读者可以看到，在系统字体目录usr/java/lib/fonts/下还有其他字体文件。
###9、修改系统字体设置
{% codeblock %}
cd /mnt/base-mmc/usr/java/lib/
cp font.properties font.properties.bak
vi font.properties
{% endcodeblock %}
上面第一条命令是进入系统库目录。第二条命令是备份原有的字体设置文件。第三条命令是打开vi编辑器修改字体设置文件。

在vi中找到如下行，
{% codeblock %}
hans.0=MHeiM18030_E.ttf
hans.plain=MHeiM18030_E.ttf
hans.1=MHeiM18030_E_Bold.ttf
hans.bold=MHeiM18030_E_Bold.ttf
{% endcodeblock %}
按 i 键进入编辑模式，将其修改为
{% codeblock %}
hans.0=CJK.ttf
hans.plain=CJK.ttf
hans.1=CJK_Bold.ttf
hans.bold=CJK_Bold.ttf
{% endcodeblock %}
保存退出(按ESC，输入 :wq)。注意，要输入冒号":"。
最后重新设定系统目录的只读权限：
{% codeblock %}
mntroot ro
{% endcodeblock %}
###10、退出调试模式
关闭终端(Terminal）。在Kindle上返回主菜单，可一直按方向键右键(FW Right)。然后依次进入Exit, Reboot or Disable Diags -> Disable Diagnostics。屏幕询问"Are you sure"时，按方向键左键(FW Left)退出。
设备将重启并回到正常模式。
字体修改设置完成！
###FAQ
- 为何有些书籍中文显示仍粗糙？

Ans：通常的原因是书籍未标明语言。解决方法是用[Calibre](http://calibre-ebook.com/download_osx)修改书籍的元数据，将其标明为中文。

{% img /images/2013-01-21-kindle-4-change-fonts/Calibre1.jpg %}

另一种方法是直接指定显示语言，如下。
在Kindle上按Home键回到主界面，然后按键盘键调出键盘。输入
{% codeblock %}
;debugOn
~changeLocale zh-CN.utf8
;debugOff
{% endcodeblock %}
注意要输入分号“;”和波浪号“~"，注意大小写，以及每一行输入完毕后输入”回车符”。每行输入完后，屏幕不会有特别显示的。