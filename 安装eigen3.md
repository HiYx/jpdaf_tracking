# 1. apt-get安装

## 1.1 执行安装命令

打开终端窗口，输入如下命令：

```
sudo apt-get install libeigen3-dev
```

## 1.2 修改安装路径

一般情况下，eigen会默认安装到`/usr/include`或者`/usr/local/include/`下的`eigen`目录下，需要执行复制命令，讲`Eigen`文件夹及其内容复制到`/usr/inlcude/`或者`/usr/local/include/`下：

```
# /usr/include
sudo cp -r /usr/include/eigen3/Eigen /usr/include
# /usr/local/include
sudo cp -r /usr/local/include/eigen3/Eigen /usr/local/include 
```

关于命令的说明：
 因为eigen3  被默认安装到了usr/local/include里了（或者是usr/include里，这两个都差不多，都是系统默认的路径），在很多程序中include时经常使用#include <Eigen/Dense>而不是使用#include  <eigen3/Eigen/Dense>所以要做下处理，否则一些程序在编译时会因找不到Eigen/Dense而报错。上面指令将usr/local/include/eigen3文件夹中的Eigen文件递归地复制到上一层文件夹（直接放到/usr/local/include中，否则系统无法默认搜索到 -> 此时只能在CMakeLists.txt用include_libraries(绝对路径了)）

## 1.3 测试

（1）编写文件-myeigen.c

```cpp
#include <iostream>
#include <Eigen/Dense>
using Eigen::MatrixXd;
int main()
{
MatrixXd m(2,2);
m(0,0) = 3;
m(1,0) = 2.5;
m(0,1) = -1;
m(1,1) = m(1,0) + m(0,1);
std::cout << m << std::endl;
}
```

（2）编译

```
g++ myeigen.c -o myeigen
```

（4）运行

```
./myeigen
```

（5）结果

```
3   -1
2.5  1.5
```

# 2. 源码安装

## 2.1 下载安装包

安装包下载网址：
 http://eigen.tuxfamily.org/index.php?title=Main_Page
 在该网站中，可以下载任意版本对应的文件

## 2.2 解压缩

## 2.3 安装

```
cd eigen-eigen-5a0156e40feb
mkdir build
cmake ../
sudo make install
```

## 2.4 修改安装路径

```
sudo cp -r /usr/local/include/eigen3 /usr/include 
```