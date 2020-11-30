# ubuntu装机软件教程

## 一、ubuntu自身配置

0.设置启动项的 ```nomodeset```，以保证系统可以正常运行

1.设置源为清华源

2.调整系统语言，首先进入语言，调整汉语最前，应用系统语言，登出，进入后选择不再提示，保持旧的语言格式，使得文件目录是英文的，再次打开语言，选择fcitx，登出重新进入，配置切换语言的格式

3.安装Nvidia的显卡

1）将Nvidia的驱动移动到home下面

```
	sudo mv NVIDIA-Linux-x86_64-450.57.run /home/
```

2)  将卸载旧版本的英伟达显卡驱动，打开terminal

```
	sudo apt-get purge nvidia*
```

3）禁用系统自带的 nouveau 驱动：

（1）打开编辑配置文件：

```
	sudo gedit /etc/modprobe.d/blacklist.conf
```

（2）在最后一行添加：

```
	blacklist 
```

​	这一条的含义是禁用**nouveau**第三方驱动，之后也不需要改回来。由于nouveau是构建在内核中的，所以要执行下面命令生效:

```
	sudo update-initramfs -u	
```

（6）重启

```
	reboot
```

​			重启之后，可以查看nouveau有没有运行

```
	lsmod | grep nouveau  # 没输出代表禁用生效
```

 （7） 停止可视化桌面

​		为了安装新的**Nvidia**驱动程序，我们需要停止当前的显示服务器。最简单的方法是使用**telinit**命令更改为运行级别**3**。执行以下**linux**命令后，显示服务器将停止，因此请确保在继续之前保存所有当前工作（如果有）：

```
	sudo telinit 3
```

​		之后会进入一个新的命令行会话，使用当前的用户名密码登录

（8）安装驱动

​		给驱动文件增加可执行权限：

```
	sudo chmod a+x NVIDIA-Linux-x86_64-450.57.run
```

​		然后执行安装：

```
	sudo ./NVIDIA-Linux-x86_64-390.48.run -no-opengl-files –no-x-check –no-nouveau-check
```

（9）常见问题解决

1. 安装完驱动后，HDMI扩展屏幕不能使用，现象表现为能识别扩展屏幕但是黑屏。
   这种情况需要确定以下内容是否已经设置：

   - bios内是否已经禁止安全启动、快速启动。
   - linux系统是否设置了禁止nouveau

   如果上面的都已经做了，但还是有问题，可以尝试下面的配置：

   ```shell
   sudo gedit /usr/share/X11/xorg.conf.d/10-amdgpu.conf
   ```

   > 有可能不是这个文件，但是类似。

   修改为下面这样

   ```
   Section "OutputClass"
      Identifier "AMDgpu"
      MatchDriver "amdgpu"
      Driver "amdgpu"
      Option "PrimaryGPU" "no"
   EndSection
   ```

   下面修改nvidia的配置

   ```shell
   sudo gedit /usr/share/X11/xorg.conf.d/10-nvidia.conf
   ```

   修改为下面这样：

   ```
   Section "OutputClass"
      Identifier "nvidia"
      MatchDriver "nvidia-drm"
      Driver "nvidia"
      Option "AllowEmptyInitialConfiguration"
      Option "PrimaryGPU" "yes"
      ModulePath "/usr/lib/x86_64-linux-gnu/nvidia/xorg"
   EndSection
   ```
   

然后重新启动。

到此NVIDIA的安装方式讲解完了。

4.安装 wifi驱动

1）移动firmware

```
	sudo mv firmware /lib/
```

2) 安装wifi的驱动后，重启

```
	sudo apt-get install linux-generic-lts-wily
	sudo add-apt-repository ppa:canonical-hwe-team/backport-iwlwifi
	sudo apt-get update
	sudo apt-get install linux-generic-lts-wily
```

5.卸载系统软件
   1）卸载火狐浏览器

```
	dpkg --get-selections |grep firefox
	sudo apt-get purge firefox firefox-locale-en firefox-locale-zh-hans unity-	scope-firefoxbookmarks 
```

  2） 卸载亚马逊

```
	sudo apt-get remove unity-webapps-common
```

  3） 卸载自带的Word

```
	sudo apt-get remove --purge libreoffice
```

6.安装福昕PDF阅读器

```
	tar -zxvf FoxitReader.enu.setup.2.4.4.0911.x64.run.tar.gz 
	sudo ./FoxitReader.enu.setup.2.4.4.0911\(r057d814\).x64.run 
```

7.安装谷歌浏览器

```
	sudo dpkg -i google-chrome-stable_current_amd64.deb
```

8.安装Visual Studio Code

```
	sudo dpkg -i code_1.41.0-1576089540_amd64.deb
```

9.安装WPS和字体 

```
	sudo dpkg -i wps-office_11.1.0.8392_amd64.deb
```

1）安装WPS的字体

```
	unzip wps_symbol_fonts.zip
	sudo cp mtextra.ttf  symbol.ttf  WEBDINGS.TTF  wingding.ttf  WINGDNG2.ttf  WINGDNG3.ttf  /usr/share/fonts
```

8.安装MarkDown-typora
       

```
	sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
	sudo add-apt-repository 'deb http://typora.io linux/'
	sudo apt-get update
	sudo apt-get install typora
```

10.生成ssh，并添加到github中

```
	ssh-keygen -t rsa -C "lizw_0304@163.com"
	cat ~/.ssh/id_rsa.pub
```

11.安装微信

###### 1) 解压缩 linux-x64.tar.gz

```
	tar -zxvf linux-x64.tar.gz
```

###### 2) 将微信添加到启动器

```
	sudo mv electronic-wechat-linux-x64/ /home/opt/
```

3) 将 electronnic-wechat 打开，锁定在任务栏

12.安装网易云音乐

```
	sudo dpkg -i netease-cloud-music_1.0.0_amd64_ubuntu16.04.deb
```

如果发生错误，你可以执行下面的指令：

```
	sudo apt-get -f install
```

执行之后，你可以继续执行上面的指令

```
	sudo dpkg -i netease-cloud-music_1.0.0_amd64_ubuntu16.04.deb
```

13.修改ubuntu的主题

1）安装MacBuntu OS Y Theme,Icons and cursors

```
	sudo add-apt-repository ppa:noobslab/macbuntu
	sudo apt-get update
	sudo apt-get install macbuntu-os-icons-lts-v7
	sudo apt-get install macbuntu-os-ithemes-lts-v7
```

2）将Saved Pictures 移动到PIctures，修改桌面壁纸

```
	sudo mv Saved Pictures/ /Pictures/
```

3) 安装Tweak tool软件启用主题、图标等，设置ubuntu的主题，图标

```
	sudo apt-get install unity-tweak-tool
```

14.修改蓝牙设备的key，实现蓝牙设备在双系统的应用。

tips：楼主在实践中，发现华为的蓝牙鼠标居然有三个MAC地址，所以请大家注意在操作的过程中注意电脑MAC地址和蓝牙设备MAC地址的匹配，从而才能实现蓝牙设备的双系统实现。

具体操作轻参考链接：https://blog.csdn.net/10km/article/details/61201268

15.修改ubuntu启动项：

```
	sudo gedit /etc/default/grub
	打开后，修改启动的值 GRUB_DEFAULT=0
	执行 sudo update-grub
```

16.安装matlab_linux:

参考链接：https://www.cnblogs.com/taoyuyeit/p/8823311.html

17.时间同步

```
	sudo apt-get install ntpdate
	sudo ntpdate time.windows.com
	sudo hwclock --localtime --systohc
	
	sudo timedatectl set-local-rtc 1
```

18. 安装git

```
	git config --global user.email "lizw_0304@163.com"
    git config --global user.name "lizhiwei0304"
```

19.安装pycharm

```
	mv pycharm-2019.1.3/ /home/lizhiwei/opt
```

然后打开～/.bashrc

```
	gedit ~/.bashrc
```

20.安装截屏软件shutter

```
	sudo add-apt-repository ppa:shutter/ppa
	sudo apt-get update
	sudo apt-get install shutter
```

然后打开设置，设置开启的快捷键

![](/home/lizhiwei/Documents/ubuntu配置/0_装机软件/20150711180746764.png)

添加成功的状态

![](/home/lizhiwei/Documents/ubuntu配置/0_装机软件/20150711181142303.png)

单击右侧的禁用，然后快速按下Ctrl+Alt+A，如下图。然后利用Ctrl + Alt + A,测试OK.

# ps:

1. 里面的快捷键命令用：shutter -s 或者shutter –select

2. 截取当前活动窗口：shutter -a （a表示active）

3. 截取拖拉区域：shutter -s （s是select之意），拖拉出矩形区域后按Enter。

21.安装Kazam

```
	sudo apt-get install kazam
```

22.vscode安装与ROS插件相关

###### 从官网下载并安装

https://code.visualstudio.com/

1. 中文模式。在vs code左侧选择Extenxions，输入chinese，安装简体中文包。
2. ros插件上。我选择了MS的预览版。
   网上人多选了ajshort的版本，但是这个版本已经deprecated. 并且被MS版兼并，虽然MS版还不是很完善。
3. c++配置。在Extenxions里面，输入c++，安装c/c++ 及 C++ Intellisense 这两个。
4. 配置CMakeLists.txt文件语法高亮。在Extenxions里面，输入txt，安装Txt Syntax。
5. 配置msg, srv, action语法高亮。在Extenxions里面，输入msg，安装Msg Language Support。

参考链接：https://blog.csdn.net/MSNH2012/article/details/100512253

###### 创建工作空间及功能包

1. 如果已经有工作空间，可以通过`打开文件夹`选项进行打开。
2. 如果要新建工作空间，可以先`创建文件夹`输入文件夹名称，例如:test，点击确定。
3. 然后再创建文件夹`src`.
4. 点击"终端"–>“新建终端”,在终端中输入"catkin_make"，系统会自动在test文件夹下创建 “build”, "devel"文件夹和其他配置文件。

在`新建工作空间`时，会在test目录下自动生成一个`.vscode`文件夹，其内自动有2个`.json`文件。`c_cpp_properties.json`和`setting.json`
如果没有生成，重启vscode试试。
或者通过按Ctrl + Shift + P,输入c/c++: edit configurations(JSON), 手动生成.

另外，记得把新建的工作空间source一下。
查看工作空间情况

```bash
$ echo $ROS_PACKAGE_PATH
```

###### 功能包

`右键`点击"src"文件夹，右键弹出选项中，点击"Create Catkin Package"，输入包的名称ros_test，按Enter确认，输入包的依赖“std_msgs roscpp”，空格隔开，按Enter确认。系统自动创建CMakeLists.txt及package.xml文件。
也可通过按`Ctrl + Shift + P`,输入`ros:Create Catkin Package`配置功能包。

注：没有在创建工作空间时的两个.json文件，是无法生成功能包的。可能会没反应或报错如下：

> 命令"ROS: Create Catkin Package"导致错误 (command ‘ros.createCatkinPackage’……

###### 运行节点

1. 启动roscore：通过按`Ctrl + Shift + P`,输入`ros:start core`启动roscore。
2. 运行节点：通过按`Ctrl + Shift + P`,输入`ros:run a rose executable`，依次输入对应的package及节点，参数。或者直接下终端`rosrun ………………`

###### 断点调试配置

在未配置过调试前，没有`launch.json`文件。通过`Ctrl + Shift + D`，下拉添加配置，自动生成该文件。断点调试有如下几种方式，这里主要讲`c/c++ gdb启动`：

###### c/c++ gdb启动

先说一下，使用这种调试方法，不需要先运行节点。
该方式会生成`launch.json`:

```json
        {
            "name": "(gdb) 启动", //修改此处
            "type": "cppdbg",
            "request": "launch",
            "program": "输入程序名称，例如 ${workspaceFolder}/a.out", //修改此处
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
```

- [ ] 需要修改其中`program`为需要调试的可执行程序。更改为编译后的可执行文件的路径下的文件（需要二进制文件），对应ROS通过catkin_make生产可执行文件的路径通常在`/devel/lib/`下，后面跟上你设置好的package名和可执行文件名例如：

```json
"program": "${workspaceFolder}/devel/lib/ros_test/talker"
```

- [ ] 需要注意，有些教程用了`${workspaceRoot}/devel/lib/ros_test/talker`， 自己看哪个可行。


- [ ] *另外，这里的`"request": "launch"`，系统也提示我可以用`"request": "attach"`模式，但是我变成attach后，系统又提示我无法识别了。。*

然后:

1. 启动roscore：通过按`Ctrl + Shift + P`,输入`ros:start core`启动roscore。
2. 设置断点，运行调试
3. 如果系统像没有断点一样运行，需要配置一下。在`CMakeLists.txt`中，`project`后添加参数`SET(CMAKE_BUILD_TYPE Debug)`,然后重新catkin_make:

```cmake
cmake_minimum_required(VERSION 2.8.3)
project()
```

`SET(CMAKE_BUILD_TYPE Debug)`

或者是catkin_make在编译功能包时，添加catkin_make的参数

```bash
$ catkin_make -DCMAKE_BUILD_TYPE=Debug
```

如果工作空间下由多个功能包，可以在编译时添加`-DCATKIN_WHITELIST_PACKAGES`编译指定功能包

```bash
$ catkin_make -DCMAKE_BUILD_TYPE=Debug -DCATKIN_WHITELIST_PACKAGES="package1;package2"
```

另外，如果开始断点调试时，出现报错：

> poll failed with error Interrupted system call

解决方法是:
打开~/.gdbinit（如果没有这个文件则自己新建一个同名文档），然后添加一下三行即可。

```
set target-async 1
set pagination off
set non-stop on
```

该报错参考链接：
https://blog.csdn.net/ABC_ORANGE/article/details/102665792
