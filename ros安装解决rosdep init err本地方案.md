    在ros机器操作系统的安装上相信不少同学都遇到的rosdep init无法解决的问题，很大的原因其实是网络方面的原因，所以我们细心研究其实既然是文件传输上的问题，我们便可以将其本地化解决，本文章试解决如何本地化快速解决该问题，从源头出发解决ros安装的最头疼的问题，成功跑起小乌龟。

0x1 rosdep init:

获取本地的rosdep 文件

网盘地址链接：https://pan.baidu.com/s/11mdep1ukCyhYRPB9AdkRCw 提取码：nv4o 

将下载的文件放到ubuntu用户目录下并执行

sudo mv /home/"你的用户名"/ros /etc
注:执行上面命令先要保证/etc目录下没有ros目录。

0x2 rosdep update:

进入gbpdistro_support.py、rep3.py、init.py
三个文件将raw.githubusercontent.com地址改为本地地址
sudo gedit /usr/lib/python2.7/dist-packages/rosdep2/gbpdistro_support.py
FUERTE_GBPDISTRO_URL =
'https://raw.githubusercontent.com/ros/rosdistro/master/releases/fuerte.yaml’更换---->
FUERTE_GBPDISTRO_URL =
‘file:///etc/ros/rosdistro-master/releases/fuerte.yaml’

sudo gedit /usr/lib/python2.7/dist-packages/rosdep2/rep3.py
REP3_TARGETS_URL =
'https://raw.githubusercontent.com/ros/rosdistro/master/releases/targets.yaml’更换---->
REP3_TARGETS_URL =
‘file:///etc/ros/rosdistro-master/releases/targets.yaml’

sudo gedit /usr/lib/python2.7/dist-packages/rosdistro/__init__.py
DEFAULT_INDEX_URL =
'https://raw.githubusercontent.com/ros/rosdistro/master/index-v4.yaml’更换---->
DEFAULT_INDEX_URL =‘file:///etc/ros/rosdistro-master/index-v4.yaml’

0x3全部更换成本地地址后新开一个终端：

rosdep update
0x3跑起小乌龟：

roscore

rosrun turtlesim turtlesim_node

rosrun turtlesim turtle_teleop_key


                                                          

rosdep 完美解决方案
