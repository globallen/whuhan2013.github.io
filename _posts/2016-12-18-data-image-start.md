---
layout: post
title: 数字图像处理入门
date: 2016-12-18
categories: blog
tags: [图像处理]
description: 数字图像处理入门
---

**什么是数字图像**     
一幅图像可以定义为一个二维函数f(x,y)， 这里的x 和y是空间坐标， 而在任意坐标价(x,y) 处的幅度f被称为这一坐标位置图像的亮度或灰度。 “灰度” 通常是用来表示黑白图像亮度的术语.      
彩色图像是由独立的图像组合而形成的。 例如， 在 RGB 彩色系统中， 幅彩色图像是由称为红、 绿、 蓝原色图像的3 幅独立的单色（或分量）图像组成的。 因此， 许多为黑白图像处理开发的技术也适用于彩色图像处理， 方法是分别处理3幅独立的分量图像即可。 彩色图像处理将在第6章讲解。    
图像在x和y坐标， 以及在幅度上是连续的。 要将这样的 幅图像转换成数字形式， 要求对坐标和幅度进行数字化。 将坐标值数字化称为取样， 将幅值数字化称为量化。 因此， 当x、y分量及幅值f都是有限且离散的量时， 我们称图像为数字图像。    

**坐标约定**    
取样和量化的结果是实数矩阵。 本书采用两种主要方法来表示数字图像。 假设对一幅图像 f(x,y）进行采样后，可得到一幅M行、 N 列的图像，我们称这幅图像的大小是M×N。 相应的 值是离散的。 为使符号清晰和方便起见，这些离散的些标都取整数。 在很多图像处理书籍中， 图像的原点被定义为（x,y)=(O，0)的。图像中沿着第 1 行的下一坐标点为(x, y)=(O,1）。 符号（0,1）用来表示沿着第 1 行的第 2 个取样。当图像被取样时，并不意味着在物理坐标中存在实际值。图 1-2（时 显示了这一坐标约定。 注意x是从 0 到 M一I 的整数，y是从 0 到 N-I 的整数。  

图像处理工具箱中表示数组使用的坐标约定与前面描述的坐标约定有两处不同。 首先，工 具箱用（r,c） 而不是（x,y）来表示行与列。然而，坐标顺序与前面讨论的是一样的。 在这种情况下， 坐标对（a,b）的第 1 个元素表示行，第 2 个元素表示列。其次，这个坐标系统的原点在（r,c)=(l,1) 处。 因此，r 是从 1 到M的整数，c 是从 1 到N的整数。 图 1-2(b）说明了这一坐标约定。
图像处理工具箱文档引用图 1-2(b）中的坐标作为像素坐标。       

工具箱还使用另一种较少用的 坐标约定，称为空间坐标，以x表示列，以y表示行，这与我们使用的变量x和y相反。 在本书中，除少量例外情况，将不使用工具箱的空间坐标    

![](https://raw.githubusercontent.com/whuhan2013/myImage/master/dataImage/p1.png)      

**图像的矩阵表示**     
![](https://raw.githubusercontent.com/whuhan2013/myImage/master/dataImage/p2.png)   

**图像的输入输出和显示**     
可以使用函数imread将图像读入MATLAB环境，imread的基本语法是：    
imread （’filename’｝         
此处，filename是含有图像文件全名的字符串（包括任何可用的扩展名）。例如语句    

```
f = imread('Fig0101.tif');
```

将图像fig0101读取到图像数组f中。注意，单引号（’）是用来界定filename文件名字符串的，而命令行结尾处的分号在MATLAB中用于禁止输出。假如命令行中未包括分号，MATLAB将显示这一命令行指定的运算结果。当在MATLAB命令行窗口中出现提示符（》）时，表明命令行的开始      

使用imshow函数在MATLAB桌面显示图像，imshow的基本语法是：imshow(f)   
其中f是图像数组     

图1-1显示了在屏幕上的输出。注意，图窗编号出现在最终得到的图窗的左上部。如果另一
幅图像q随后用imshow来显示，MATLAB就用新图像取代

```
》figure,imshow(g) 
使用imwrite函数将图像写入当前日录，imwrite的基本语法如下：   
imwrite(f，’filename') 
```   

**类和图像类型**     
虽然使用的是整数坐标， 但 MATLAB 中的像素值（亮度）并未限制为整数。 表 1-1 列出了 MATLAB 和图像处理工具箱为描述像素值而支持的各种类。 表中的前 8 项是数值型的数据类，第 9 项称为字符类， 最后一项称为逻辑类。
uint8 和 logical 类广泛用于图像处理， 当以 TIFF 或 JPEG 图像文件格式读取图像时，会用到这两个类。 这两个类用1个字节表示每个像素。某些科研数据源， 比如医学成像， 要求提供超出 uint8 的动态范围：针对此类数据， 会采用 uint16 和 int16 类。 这两个类为每个矩阵元素使用2 个字节。针对计算灰度的操作， 比如傅立叶变换（见第 3 章）， 使用 double 和single 浮点类。 双精度浮点数每个数组元素使用8 个宇节， 而单精度浮点数使用 4 个字节。尽管工具箱支持 int8 、 uint32 和 int32 类， 但在图像处理中并不常用    
![](https://raw.githubusercontent.com/whuhan2013/myImage/master/dataImage/p3.png)   

工具箱支持4种图像类型     

- 灰度图像    
- 二值图像  
- 索引图像    
- rgb图像

大多数单色图像的处理运算都是通过二值图像或灰度图像来进行的， 所以我们首先重点研究这两种图像。 索引图像和RGB彩色图像将在第6章中讨论。    


**1. 灰度图像**     
灰度图像是数据矩阵， 矩阵的值表示灰度浓谈。 当灰度图像的元素是 uint8 或 uintl6
类时， 它们分别具有范围为［0,255］或［0,65535］的整数值。如果图像是 double 或 single 类，
值就是浮点数（见表 1-1 的前两项）.double 或 single 灰度图像的值通常被归一化标定为［0,1]范围内， 但也可以使用其他范围的值。

**2. 二值图像**      
二值图像在 MATLAB 中具有非常 特殊的意义。 二值图像是取值只有 0 和 l 的逻辑数组。
因而， 只包含 0 和 l 数据类的数组， 比如 uint8，在MATLAB 中就不认为是二值图像。 用
logical 函数可以把数值数组转换为二值图像。 因此， 如果 A 是由 0 和 1 构成的数值数组，就可以使用下列语句创建逻辑数组 B:   
B = logical (A)      
如果 A 中含有除了。和 1 之外的其他元素， 使用 logical 函数就可以将所有非 0 值变换为逻辑 l ，而将所有 0 值变换为逻辑 0。也可以使用关系和逻辑算子得到逻辑数组。可使用函数is logical 来测试数组是否为逻辑类.     

![](https://raw.githubusercontent.com/whuhan2013/myImage/master/dataImage/p4.png)   