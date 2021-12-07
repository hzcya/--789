以前使用VScode主要是用做C语言的学习和开发，后来发现VScode配置cpp挺麻烦，所以很长一段时间之中我都是弃用VScode而使用CodeBlocks进行C语言开发的。近期因为学习有关ROS开发的课程所以重新启用了VScode希望用的舒服，学的开心。现将一些使用方法和环境配置的方式写成文章留作备份。

0x1 VScode介绍:

全称 Visual Studio Code，是微软出的一款轻量级代码编辑器，免费、开源而且功能强大。它支持几乎所有主流的程序语言的语法高亮、智能代码补全、自定义热键、括号匹配、代码片段、代码对比 Diff、GIT 等特性，支持插件扩展，并针对网页开发和云端应用开发做了优化。软件跨平台支持 Win、Mac 以及 Linux。

图片

(图片来源网络)

0x2 VScode下载:

VScode:https://code.visualstudio.com/docs?start=true

因为我使用的环境是Ubuntu 18.04的系统环境，所以在这里直接下载的是deb的包，下载后在Ubuntu里面自行安装软件包即可。x64版本



0x3 VScode插件:

ROS开发所需要的VScode插件:ROS,C/C++.Python.中文简体，CMake Tools.

图片

0x4 基本配置:

终端下新建工作空间：

mkdir -p demo/src
图片

进入工作空间的src目录:

cd /src
在当前目录下编译工作空间:

catkin_make
启动VScode

code .
快捷键编译工作空间ctrl + shift + B 调用编译，选择:catkin_make:build

图片

修改tasks.json文件

{    "version": "2.0.0",    "tasks": [        {            "label": "catkin_make:debug", //代表提示的描述性信息            "type": "shell",  //可以选择shell或者process,如果是shell代码是在shell里面运行一个命令，如果是process代表作为一个进程来运行            "command": "catkin_make",//这个是我们需要运行的命令            "args": [],//如果需要在命令后面加一些后缀，可以写在这里，比如-DCATKIN_WHITELIST_PACKAGES=“pac1;pac2”            "group": {"kind":"build","isDefault":true},            "presentation": {                "reveal": "always"//可选always或者silence，代表是否输出信息            },            "problemMatcher": "$msCompile"        }    ]}
0x5 创建功能包:

在src文件夹下create catkin package创建功能包，Package name确定功能包名称，Dependencies确定功能包所需要的编译依赖环境。

图片

图片

0x6 C++编译实现:

在src功能包下新建cpp文件

#include "ros/ros.h"int main(int argc, char *argv[]){    setlocale(LC_ALL,"");    //执行节点初始化    ros::init(argc,argv,"Hello ROS");    //输出日志    ROS_INFO("Hello ROS!!!哈哈哈哈哈哈哈哈哈哈");    return 0;}
开启代码提示:

修改 .vscode/c_cpp_properties.json 设置

 "cppStandard": "c++17"
输出汉字:

setlocale(LC_ALL, "");
0x7 CMakeLists:

add_executable(节点名称  src/C++源文件名.cpp)target_link_libraries(节点名称  ${catkin_LIBRARIES})
