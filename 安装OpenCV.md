本文介绍怎样在树莓派中通过apt方式[安装opencv](https://so.csdn.net/so/search?q=安装opencv&spm=1001.2101.3001.7020)，并通过一个简单的样例说明怎样使用opencv。

相比于源码方式安装opencv，通过apt方式安装过程步骤简单些。消耗的时间也少一些。

通过apt方式安装没有自己主动生成opencv.pc文件，所以在编写makefile文件时不能直接使用pkg-config工具，而须要逐个指定opencv_core、opencv_imgproc等动态链接库。



# 1.安装opencv

# apt 安装 [opencv](https://so.csdn.net/so/search?q=opencv&spm=1001.2101.3001.7020)

```bash
sudo apt install libopencv-dev
```

查询安装的 `opencv` 版本：

```bash
pkg-config --modversion opencv
```

输出如下则证明安装成功（2022-06-01 `apt` 下载的最新 `opencv` 版本为 `3.2.0`）：

```
3.2.0
```



 安装完毕之后，opencv相关的头文件被安装到/usr/lib文件夹中，该文件夹是linux默认头文件查找路径。
 opencv的相关动态链接库被安装到/usr/lib文件夹中。这些动态链接库包含：

```undefined
【opencv_calib3d】——相机校准和三维重建
【opencv_core】——核心模块，画图和其它辅助功能
【opencv_features2d】——二维特征检測
【opencv_flann】——高速最邻近搜索
【opencv_highgui】——GUI用户界面
【opencv_imgproc】——图像处理
【opencv_legacy】——废弃部分
【opencv_ml】——机器学习模块
【opencv_objdetect】——目标检測模块
【opencv_ocl】——运用OpenCL加速的计算机视觉组件模块
【opencv_video】——视频分析组件
```

# 2.简单演示样例

【C++】——通过代码加载一张图片，通过opencv把彩色图片转换为黑白图片，并把原图和转换后的图片输出到屏幕中。

```cpp
 1 #include <opencv2/core/core.hpp>
 2 #include <opencv2/imgproc/imgproc.hpp>
 3 #include <opencv2/highgui/highgui.hpp>
 4 #include <iostream>
 5 using namespace cv;
 6 using namespace std;
 7 int main (int argc, char **argv)
 8 {
 9     Mat image, image_gray;
10     image = imread(argv[1], CV_LOAD_IMAGE_COLOR );
11     if (argc != 2 || !image.data) {
12         cout << "No image data\n";
13         return -1;
14     }
15    
16     cvtColor(image, image_gray, CV_RGB2GRAY);
17     namedWindow("image", CV_WINDOW_AUTOSIZE);
18     namedWindow("image gray", CV_WINDOW_AUTOSIZE);
19    
20     imshow("image", image);
21     imshow("image gray", image_gray);
22    
23     waitKey(0);
24     return 0;
25 }
```

【makefile】创建一个Makefile的文本文件即可，要cd进入Makefile所在目录才能make

```crystal
 1 CC = g++ 
 2 # 可运行文件
 3 TARGET = test
 4 # C文件
 5 SRCS = test.cpp
 6 # 目标文件
 7 OBJS = $(SRCS:.cpp=.o)
 8 # 库文件
 9 DLIBS = -lopencv_core -lopencv_imgproc -lopencv_highgui
10 # 链接为可运行文件
11 $(TARGET):$(OBJS)
12  $(CC) -o $@ $^ $(DLIBS)
13 clean:
14  rm -rf $(TARGET) $(OBJS)
15 # 编译规则 $@代表目标文件 $< 代表第一个依赖文件
16 %.o:%.cpp
17  $(CC) -o $@ -c $<
```

【简单说明】
 DLIBS = -lopencv_core -lopencv_imgproc -lopencv_highgui
  演示样例中使用了opencv中的核心部分、图像处理部分和GUI部分，所以依次添加opencv_core、opencv_imgproc、opencv_highgui动态链接库。该部分和和

【编译】

```go
make
```

【运行】

```bash
./test raspberry.jpg
```

可运行文件test和raspberry.jpg应在同一个文件夹中。