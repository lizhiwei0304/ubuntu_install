Ubuntu16 安装 MATLAB2019b
前期准备
软件下载
断网
挂载镜像
启动安装
破解
配置
添加桌面快捷方式
添加命令行启动
参考链接
ubuntu18.04下安装MATLAB2019b 下载+安装
Ubuntu16.04/18.04安装MATLAB2018b

前期准备
软件下载
链接：https://pan.baidu.com/s/1nWRG1i3CqxPU_evkfRnotQ
提取码: 6666

断网
切记断网！
切记断网！
切记断网！

挂载镜像
命令:

# 格式
sudo mount -o loop [镜像文件的路径] [指定的挂载路径]
# 例如我的:
sudo mount -o loop /home/lzm/Download/R2019b_Linux.iso /home/lzm/matlab

启动安装
以root权限安装

sudo su
sudo /home/lzm/matlab/install  
此时将打开图形化安装界面，切记勿在挂载的目录中执行上述命令。

选择第二个 Use a File Installation Key

下一步

选择 Yes
下一步

输入秘钥

解压 Crack.rar，打开里面的 readme.txt 文件，找到秘钥并复制粘贴到此处。

下一步

安装路径配置

默认安装路径或根据自己喜好自行修改，记住此路径或者复制出来，后面会用到。
下一步

默认全选

下一步

点击 Install

进入安装状态……

下一步

安装完成

点击 Finish

## 破解

在这里插入图片描述

手动创建 licenses 文件夹
在 R2019b 的安装目录下面没有 licenses 这个文件夹，需要自己手动创建，并将 license_standalone.lic 文件放到里面去，R2018 以前的的版本在安装目录里就存在 licenses ，而 2019 版的需要自己手动创建。

cd /usr/local/matlab/R2019b
sudo mkdir licenses

添加破解文件

复制破解文件 Crack 中 license_standalone.lic ，添加到安装目录中的 licenses 目录下

sudo cp -f ~/Downloads/matlab/Crack/license_standalone.lic /usr/local/matlab/R2019b/licenses

替换 bin 文件

复制 Crack 中 R2019b 中的 bin 文件夹到安装目录中，替换 bin 文件

sudo cp -f ~/Downloads/matlab/Crack/R2019b/* /usr/local/matlab/R2019b/

修改权限

修改目录的权限，否则会出现很多warning。

sudo chmod -R 777 /usr/local/MATLAB/

配置
添加桌面快捷方式
创建快捷方式文件

sudo gedit /usr/share/applications/Matlab.desktop

写入以下内容

[Desktop Entry]
Type=Application
Name=Matlab 2018b
GenericName=Matlab 2018b
Exec=sh /usr/local/matlab/R2019b/bin/matlab -desktop
Icon=/usr/local/matlab/R2019b/toolbox/shared/dastudio/resources/MatlabIcon.png
Terminal=false
Categories=Development;Matlab;Application;

主要修改以下三个：

Exec: 执行路径，改成自己安装所对应的位置
Icon: MATLAB 图标位置
Terminal: false时，启动 MATLAB 将不启动终端
添加命令行启动
打开~/.bashrc，在后面添加 MATLAB 执行程序简写

alias matlab=`/usr/local/matlab/R2019b/bin/matlab`

此时在终端输入 matlab 即可启动 MATLAB
