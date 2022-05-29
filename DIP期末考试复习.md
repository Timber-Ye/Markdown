# Digital Image Processing


## DIP-1 Introduction&Fundamentals
***
### What is DIP

    An image may be defined as a two-dimensional function, f(x,y), where x and y are spatial coordinates,and the amplitude of f at any pair of coordinates (x,y) is called the intensity (亮度) or gray (灰度) level of the image at that point.

> DIGITAL IMAGE(数字图像): x, y, and f are (1)finite  (2)discrete quantities(离散量)

> PIXEL(像素): DIGITAL IMAGES are made up of limited quantities of elements. We call these elements PIXELS. Pixel values typically represent gray levels, colors, opacities

Common image formats include:
+ 1 channel (通道) per point (B&W or Grayscale)
+ 3 channels per point (Red, Green, and Blue)
+ 4 channels per point (Red, Green, Blue, and “Alpha”,a.k.a. Opacity)

For most of this course we will focus on `grey-scale` images

> DIP: processing digital images by means of a digital computer(借助数字计算机处理数字图像)
>>Two major tasks:(1)Improvement of image quality for human interpretation (2)Processing of image data for storage,transmission, display and representation for automatic machine perception

    DIP&CV:
    + DIP: human is the final explainer
    + Computer vision: computer is the final explainer
    + Computer vision system needs the support of DIP
    + DIP to computer vision is a process from low level to high level processing.
***
### Human vision system

Retina(视网膜) is covered with two types of light receptors(光感受器): cones(锥状体, **responsible for color vision**) and rods(杆状体, **sensitive to low levels of illumination**)
![DIP1-1](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP1-1.jpg)

>对所有可见光波长相对平衡地反射的物体，对观察者而言是白色的。

>在有限一个波长范围内反射时，物体会呈现特定的颜色。

Three basic quantities to describe the quality of a chromatic light source:
+ Radiance(发光强度)
+ Luminance(光通量)
+ Brightness(亮度)
***
### Image acquisition(图像获取)

> Sampling(采样): digitizing the coordinate values
> Quantization(量化): digitizing the amplitude values

`ppi&dpi`
***
### Resolution(分辨率)
> **Spatial resolution** refers to the smallest
discernable(可分辨) detail in an image

Spatial resolution: M*N

> **Intensity level resolution** refers to the number of intensity levels used to represent the image

Gray resolution: L
***
### Sampling and Quantization(采样与量化)
***
### Basic Relationships Between Pixels

+ 领域
  + 四邻域 $N4$

  + 八邻域 $N8$

    ![DIP1-2](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP1-2.png)

+ 邻接
  + 4邻接: 如果$q$在集合$N_4(p)$中,则具有$V$中数值的两个元素$p和q$是4邻接的.
  + 8邻接: 如果$q$在集合$N_8(p)$中,则具有$V$中数值的两个元素$p和q$是8邻接的.
  + m邻接(混合邻接): 如果

    + $q$在集合$N_4(p)$中 
    + 或者$q$在集合$N_D(p)$中, 且$N_4(p) \cap N_4(q)$中没有来自$V$中的像素

    则$V$中数值的两个元素$p和q$是m邻接的.

+ 通路: A sequence of adjacent pixels.
    + close path
    + `4-path`, `8-path`, `m-path`

+ 连通集和连通域

  + 令S是图像中的一个像素子集。如果S的全部像素之间存在一个通路，则可以说两个像素p和q在S中是`连通`的。
  + 对于任何像素p，S中连通到该像素的像素集称为S的`连通分量`。
  + 如果S中仅有一个连通分量，则称集合S为`连通集`。
  + 令R是图像中的一个像素子集.若R是连通集,则称R为一个区域.两个区域,若它们能通过4-邻接或者8-邻接联合形成一个连通集,则称这两个区域为`邻接区域`
  + 区域R的`边界`是这样的点的集合,这些点与R的补集中的点邻近.

+ 距离度量
$$
\begin{aligned}
&D(p, q) \geq 0 (D(p, q)=0, if\ p=q) \\
&D(p, q) = D(q, p)\ and \\
&D(p, z) \leq D(p, q) + D(q, z) 
\end{aligned}
$$

+ 城市街区距离($D_4$距离)
  ![DIP1-3](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP1-3.png)
+ 棋盘距离($D_8$距离)
  ![DIP1-4](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP1-4.png)

![DIP1-5](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP1-5.png)

### Key stages in DIP

![DIP1-6](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP1-6.png)

***
## DIP-2 Image Enhancement(PointProcessing)
> Two broad categories:
>>1 Spatial domain techniques
>>2 Frequency domain techniques

***
### Basic Spatial Domain Image Enhancement
$$g(x, y)=T[f(x, y)]$$
其中，$T$是在点$(x, y)$的邻域上定义的一种算子.

假如$g$仅仅取决于点$(x, y)$处的$f$值, 即$T$所在的邻域大小为$1×1$, 则有:
$$s = T(r)$$
$s$和$r$分别表示$g$和$f$在任意点$(x, y)$处的$f$值

POINT PROCESSING TECHNIQUES:
![DIP2-1](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP2-1.png)
***
### Thresholding(阈值)
particularly useful for segmentation
***
### Logarithmic transformation
$$s=clog(1+r)$$
将输入中范围较窄的底灰度值映射为输出中较宽范围的灰度值.
拓展图像中的暗像素的值,压缩更高灰度级的值.
***
### Power law transforms
$$s=c×r^\gamma$$
![DIP2-2](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP2-2.png)
***
### Gray level slicing
`分段线性变换函数`
![DIP2-3](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP2-3.png)
***
### Bit plane slicing

+ Higher-order bits usually contain most of the significant visual information
+ Lower-order bits contain subtle details
***
### Image subtraction

$$g(x, y)=f(x, y)-h(x, y)$$
归一化:
+ Add 255 and then divide by 2
+ $y=x-min(x); z=y*255/max(y)$.
***
### Image averaging
消除均值为零的噪声
***
### Histogram Equalization

#### Image Histograms
    
    The histogram of an image shows us the distribution of gray levels in the image. It is useful in image processing, especially in enhancement and segmentation.

PS:
1. It doesn’t include the location information of pixels.
2. Histogram information reveals that image is under-exposed or over-exposed.(3)Evenly spaced histogram means **the high contrast**.

#### HISTOGRAM EUQALIZATION
$$
s=T(r)=\int_{0}^{r}p_r(\omega)d\omega$$
***
## DIP-3 Image Enhancemen(SpatialFiltering)
***
### Basis
+ 线性滤波器
  + 一般来说，使用大小为$m×n$的滤波器对大小为$M×N$的图像进行空间线性滤波可以表示为:
  $$
  g(x,y)=\Sigma_{s=-at}^{a}\Sigma_{t=-b}^{b}w(s,t)f(x+s,y+t)
  $$
  其中,$m$和$n$为奇数,$a=(m-1)/2, b=(n-1)/2$

  + 图像边缘像素缺失的解决办法
    + 修改代码,在边缘滤波时自动忽略缺失的像素(降低程序效率)
    + 在原图像外面一圈加层纯白或者纯黑的衬底
    + 复制边缘像素
    + 不处理边缘像素

+ 非线性滤波器
***
### Smoothing spatial filters
    Used for blurring (removal of small details prior to large object extraction, bridging small gaps in lines) and noise reduction.

  >Also called averaging filter:
  >> + Simply average all of the pixels in a neighbourhood around a central value
  >> + Especially useful in removing noise from images
  >> + Also useful for highlighting gross features

PS:
  + noise reduction
  + blur edges(reduce sharp transitions)
  + reduce irrelevant details to obtain gross features

>顺序统计滤波器是非线性空间滤波器

+ `Min`: Set the pixel value to the minimum in the neighbourhood.  SALT NOISE
+ `Max`: Set the pixel value to the maximum in the
neighbourhood.   PEPPER NOISE
+ `Median`: The median value of a set of numbers is the midpoint value. Median filter are particularly effective in the presence of **impulse noise**, also called **salt and pepper noise**.
+ `Adaptive Median`: the filter size changes depending on the characteristics of the image

***
### Sharpening
Sharpening spatial filters seek to highlight fine detail
+ Remove blurring (模糊) from images
+ Highlight edges

`1st Derivative`
$$\frac{\partial f}{\partial x}=f(x+1)-f(x)$$

`2nd Derivative`
$$\frac{\partial^2 f}{\partial^2 x}=f(x+1)+f(x-1)-2f(x)$$

**Comparison**:

![DIP3-1](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP3-1.png)

+ $1^{st}$: thicker edges & stronger response to gray level step
+ $2^{nd}$:stronger response to fine detail & double response at step changes

$\therefore$ 二阶微分在增强细节方面要比一阶微分好得多, 并且在执行上容易得多

#### Laplacian 拉普拉斯算子

$$
\nabla^2f=\frac{\partial^2f}{\partial^2x}+\frac{\partial^2f}{\partial^2y}
$$

$$
\frac{\partial^2f}{\partial^2x} = f(x+1,y)+f(x-1,y)-2f(x,y)
$$

$$
\frac{\partial^2f}{\partial^2y} = f(x,y+1)+f(x,y-1)-2f(x,y)
$$

$$
\therefore \nabla^2f=[f(x+1,y)+f(x-1,y)+f(x,y+1)+f(x,y-1)-4f(x,y)]
$$

Enhanced:
$$
g(x,y)=f(x,y)-\nabla^2f
$$
$$
g(x,y)=5f(x,y)-f(x+1,y)-f(x-1,y)-f(x,y+1)-f(x,y-1)
$$
![DIP3-2](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP3-2.png)

![DIP3-3](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP3-3.png)

#### Sobel Operators(索贝尔算子)

![DIP3-4](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP3-4.png)

***
## DIP-4 Image Enhancement(FilteringInTheFrequencyDomain)
两个变量的傅里叶变换：
$$
F[f(x,y)]=F(u,v)=\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}f(x,y)e^{-j2\pi(ux+vy)}dxdy
$$

$$
F^{-1}[F(x,y)]=f(x,y)=\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}F(u,v)e^{j2\pi(ux+vy)}dudv
$$

![DIP4-1](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP4-1.png)

Properties:
+ $f(x,y)(-1)^{x+y}\Longleftrightarrow F(u-M/2, y-N/2)$
+ Rotation:
  ![DIP4-2](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP4-2.png)

+ Average value:
$$F(0,0)=\frac{1}{MN}\Sigma_{x=0}^{M-1}\Sigma_{y=0}^{N-1}f(x,y)
$$ 

***
### Steps of Filtering in the Frequency Domain

Steps:
+ Multiply the input image by $(-1)^{x+y}$ to center the transform.
+ Compute $F(u,v)$ the DFT of the image in (1).
+ Multiply $F(u,v)$ by a filter function $H(u,v)$.
+ Compute the inverse DFT of the result.
+ Obtain the real part of the result in (4).
+ Multiply the result in (5) with $(-1)^{x+y}$.

![DIP4-3](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP4-3.png)

>In Fourier Transform:

>> Low frequencies: the general gray level

>> High frequencies: for detail, such as edges and noises

***
### Some Basic Frequency Domain Filters

**基于高斯函数的滤波器:**
![DIP4-6](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP4-6.png)
![DIP4-7](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP4-7.png)


#### Image Smoothing

Low Pass Filter:

![DIP4-4](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP4-4.png)

Smoothing is achieved in the frequency domain by **dropping out the high frequency** components

+ 2D ideal low pass filter (ILPF)
+ Butterworth lowpass Filters
+ Gaussian Lowpass Filters

#### Image Sharpening

High Pass Filter:

$$
H_{hp}(u,v)=1-H_{lp}(u,v)
$$
![DIP4-5](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP4-5.png)

## DIP-5 Image Restoration

### What is Image Restoration?

    Image restoration attempts to restore images that have been degraded

+ Identify the degradation process and attempt to reverse it
+ Similar to image enhancement, but more objective

### A Model of Image Degradation and Restoration

$$
g(x,y)=h(x,y)\ast f(x,y)+\eta(x,y)
$$

$$
G(u,v)=H(u,v)F(u,v)+N(u,v)
$$

$h(x,y)$ is the degradation function(退化函数), 图像复原的目的从$g(x,y)$中获得原图像$f(x,y)$的估计值$\hat f(x,y)$

### Noise Models

+ 高斯噪声:arises due to factors such as electronic
circuit noise and sensor noise caused by poor illumination and/or high temperature.
+ 瑞利噪声:noise phenomena in range imaging(深度成像)
+ 伽马噪声, 指数噪声:noise phenomena in laser imaging
+ 脉冲噪声(椒盐噪声):found in situations where quick transients, such as faulty switching
+ 均匀噪声

### Periodic Noise

Periodic is easy to obeserve in frequency domain
so, processing method: **In frequency domain**

### Estimating the Degradation Function(h(u,v))

+ Observation

+ Experimentation
>Imaging an impulse using the same system settings.
$$
H(u,v)=\frac{G(u,v)}{A}
$$
+ Mathematical modeling
Motion Blur:
$$
H(u,v)=\frac{T}{\pi(ua+ub)}sin[\pi(ua+vb)]e^{-j\pi(ua+vb)}
$$

### 逆滤波

 **Weiner Filtering**

## DIP-6 Morphological Image Processing

### What is morphology?

    The basic ideal of Morphology is to use a special structuring element to measure or extract the corresponding shape or characteristics in the input images for further image analysis and object recognition.

  > The mathematical foundation of morphology is the set theory

  > 结构元素(SE):一幅图像中感兴趣特性所用的小集合或子图像

+ FIT(匹配): for each of SE pixels set to 1, the corresponding image pixel is also 1
+ Hit(击中): at least one of SE's pixels set to 1 the corresponding image pixel is also 1
+ Miss

### Dilation and Erosion

+ Dilation $f\oplus s$
  $$
  A\oplus B={x|[(\hat{B}_x)\cap A \neq \varnothing]}
  $$

  $\hat{B}$是结构元素,膨胀就是要和$A$hit上.
  ![DIP6-1](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP6-1.png)

  Dilation can:

  + repair breaks;
  + repair intrusions;
  + enlarge objects

+ Erosion $f\ominus s$
  $$
  A\ominus B={x|(B)_x \subseteq A}
  $$

  Erosion can:

  + Split apart joined objects
  + Strip away extrusions
  + Shrink objects

### Opening and Closing

+ Opening $f\circ s=(f\ominus s)\oplus s$

> 先腐蚀再膨胀

  + $A\circ B$是$A$的子集
  + $(A\circ B)\circ B=A\circ B$

            generally smoothes the contour of an image, breaks thin connections,eliminates protrusions （突出物）.

+ Closing $f\bullet s=(f\oplus s)\ominus s$

> 先膨胀再腐蚀

  + $A$是$A \bullet b$的子集
  + $(A\bullet B)\bullet B=A \bullet B$

              smoothes sections (缺口) of contours, but it generally fuses breaks, holes, gaps, etc.
### Hit or Miss Transform

> Morphological hit-or-miss transform is a basic tool for shape detection

$$
A\circledast B=(A\ominus X)\cap[A^c\ominus(W-X)]
$$

![DIP6-2](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP6-2.png)
>X and (W-X) where W is window slightly larger than X

>Our goal is to find X in the image

## DIP-6 Morphological Algorithms

### Boundary extraction
$$
\beta(A=A-(A\ominus B)
$$

![DIP7-1](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP7-1.png)

### Region filling
$$
X_k=(X_{k-1}\oplus B)\cap A^c
$$
where X0 is the starting point inside the boundary, B is a simple structuring element and Ac is the complement of A

![DIP7-2](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP7-2.png)

### Extraction of connected components
$$
X_k=(X_{k-1}\oplus B)\cap A
$$


### Thinning/Thicking
+ 根据击中-击不中变换来定义细化的基本运算：
$$
A \otimes B= A-(A\circledast B)
$$

通常用一个结构元序列将细化定义为

$$
A\otimes {B}=((...((A\otimes B^1)\otimes B^2)...)\otimes B^n)
$$

![DIP7-3](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP7-3.png)


+ 粗化的基本运算定义为
$$
A\odot B=A \cup (A\circledast B)
$$

粗化的实际算法: the usual procedure is to thin the background, and then complement the result.
![DIP7-4](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP7-4.png)

### Skeletons

$$
S(A)=\bigcup_{k=0}^{K}S_k(A)
$$

$$
S_k(A)=(A\ominus kB)-(A\ominus kB)\circ B
$$

>K是A被腐蚀为空集之前的最后一个步骤

![DIP7-5](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP7-5.png)

### Summary

+ 5 Basic Structuring Elements

![DIP7-6](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP7-6.png)

## DIP8 Segmentation

    The purpose of image segmentation is to partition an image into meaningful regions with respect to a particular application

>Segmentation algorithms generally are based on of 2 basic properties of intensity values:

>>`Discontinuity`: to partition an image based on abrupt changes in intensity (such as edges) (分)

>> `Similarity`:to partition an image into regions that are similar according to a set of predefined criteria. (合)

### Detection Of Discontinuities

>We typically find discontinuities using **masks and correlation**

+ Points

  Can be achieved by using the mask:

  ![DIP8-1](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP8-1.png)

  Compared with `The Laplacian`
 
+ Lines
  The masks below will extract lines that are **one pixel** thick and running in a particular direction:

  ![DIP8-2](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP8-2.png)

  Apply every masks on the image. If at a certain point in the image:
  $$
  |R_i| \ge |R_j|
  $$
  for all $j\neq i$, that point is said to be more likely associated with a line in the direction of mask $i$

+ Edges

  ![DIP8-3](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP8-3.png)
  ![DIP8-4](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP8-4.png)

  Conclusion:
  + The $1^{st} derivative$ can be used to detect the presence of an edge at a point in an image
  + The sign of the $2^{nd} derivative$can be used to determine whether an edge pixel lies **on the dark or light side of an edge**

  > Derivative based edge detectors are extremely sensitive to noise

  **image smoothing should be serious consideration prior** to the use of derivatives in applications where noise is likely to be present

  + Gradient Direction

    ![DIP8-5](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP8-5.png)

    ![DIP8-7](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP8-7.png)    


    > Sobel masks have slightly superior noise suppression characteristics which is an important issue when dealing with derivatives

    To avoid the turbulence of noise, it is better to smooth first:

    ![DIP8-6](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP8-6.png)
  
    $$
    \frac{\partial^2}{\partial x^2}(h\ast f)=(\frac{\partial^2}{\partial x^2}h)\ast f
    $$

  + Laplacian combined with smoothing:`The Laplacian of Gaussian(LoG)`

    $$
    \nabla^2h(r)=-[\frac{r^2-\sigma^2}{\sigma^4}]e^{-\frac{r^2}{2\sigma^2}}
    $$

  + Optimal Edge Detection: Canny
    >边缘点位于图像被高斯平滑后的梯度值的极大值点
    + 高斯滤波
    + 计算梯度的幅值和方向
    + 非极大值抑制
    + 滞后阈值
    + 边缘连接

      ![DIP8-8](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP8-8.png)

### Detection Of Similarities

## DIP9 Segmentation-2

### Edge Linking

Local Processing:
+ A point in the predefined neighborhood of
(x,y) is linked to the pixel at (x,y) if both
**magnitude and direction** (i.e., angle) criteria
are satisfied.

+ the process is repeated at every location in
the image

+ A record must be kept

+ Simply by assigning a different gray level to
each set of linked edge pixels

Global Processing：
+ Edge Detection
+ Model fitting：Hough Transform


  ![DIP9-1](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP9-1.png)

### Thresholding

T depends on：
+ **Global Threshold**: Only $f(x,y)$(only on gray-level values)

  T is calculated as follows:
  + Set an `initial estimate` for T( **Average \ Median** grey level).
  + Compute the **average grey levels** $\mu_1$ and $\mu_2$ of two groups of pixels
  + New Threshold value:
    $$T=\frac{\mu_1+\mu_2}{2}$$

  + Repeat until...

  也可以使用大津法计算全局阈值T:
  + try all t to find the maximum ICV:

    $$
    ICV=p_0(t)\times(MO-M)^2+p_b(t)\times(MB-M)^2
    $$
    > The probability of group **object** is $p_0(t)$, mean is MO

    > The probability of group background is $p_b(t)$,mean is MB

+ `Local Threshold`: Both$f(x,y)$and $p(x,y)$(denotes some lcal property of this point)

+ `Adaptive Threshold`:On $f(x,y)$, $p(x,y)$, $x$ and $y$
  $$
  T=T[x,y,p(x,y),f(x,y)]
  $$

>An approach to handling situations in
which single value thresholding will not
work is to **divide an image into sub images and threshold these individually**

>Since the threshold for each pixel depends
on its location within an image， this
technique is said to **adaptive**

### Region Based Segmentation

Region Based Segmentation: R is an image, segmentation is a process that **partitions R into n sub-regions** $R_1, R_2, …, R_n$ such that:

![DIP9-2](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP9-2.png)

### Region Growing

      As name implies, region growing is a procedure that groups pixels or subregions into larger regions based on predefined criteria.

![DIP9-3](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP9-3.png)

### Region Splitting and Merging

> Split each region which is not satisfy the criteria.

> Merge two neighbor region based on criteria.

![DIP9-4](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP9-4.png)

## DIP10 Color Image Processing

### Color Fundamentals

`Cones`:
+ 65% --sensitive to red light
+ 33% --sensitive to green light
+ 2% --sensitive to blue light(**but the blue cones are the most sensitive**)

> Primary colors: RED; GREEN; BLUE

PS: It is not mean that these three
fixed RGB components can **generate all spectrum colors**

![DIP10-1](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP10-1.png)

>FIGURE OUT the difference of the primary colors of **light** and the primary colors of **pigments**.

+ Brightness


+ Hue


+ Saturation

### Color Models

> The purpose of color model is to facilitate the specification
of colors in some standard

+ RGB Image(Hardware Oriented **for screen**)


+ CMY / CMYK(**For printer**)

Cyan(青), magenta(深红), and yellow(黄) are the secondary (二次色) colors of light. Or the primary colors of pigments.

![DIP10-2](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP10-2.png)

K: True Black(In practice, combining CMY colors for printing produces a muddy (灰蒙蒙）looking black)

+ HSI Models

![DIP10-3](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP10-3.png)

### Pseudo Color
> The principle of pseudo color is for human visualization and interpretation of gray scale events
in an image.


#### Intensity Slicing

![DIP10-4](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP10-4.png)
#### Gray level to color transform

![DIP10-5](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP10-5.png)

## DIP11 Color Image Processing-2

### Color Transformation

### Color Complements

> Color complements are useful for enhancing detail that is embedded in dark regions of a color image

![DIP10-6](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP10-6.png)

### Color Slicing

![DIP10-7](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP10-7.png)

### Tone and Color Corrections

![DIP10-8](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP10-8.png)

### Histogram Processing

+ Spread the color intensities uniformly


+ Leaving the colors themselves (e.g Hue) unchanged.

### Color Image Smoothing

![DIP10-9](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP10-9.png)

### Color Image Sharpening

![DIP10-10](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP10-10.png)

## DIP14 Object Recognition

### The Basic Idea

> Classification:
> +  Have labels
> 
> +  Want a rule the assign labels accurately
>
> +  Supervised learning

> Clustering:
> +  No labels
> 
> +  Groups points into clusters based on how "near" they are to one another
> 
> +  Unsupervised learning

> Classifier: Algorithm/device that separates objects into different classes

> Consists of three parts:
>  + Sensor
>
>  + Feature extractor
>
>  + Classifier

**Feature Space**:
+ Distance in feature space is related to similarity.
+ Similar objects yields similar measurement results (feature vectors)

### Theoretic Method

![DIP14-1](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP14-1.png)

PS:If two clouds that represent different classes **overlap** in feature space, then **a perfect classification does not exist**.

#### Minimum Distance Classifier

> The simplest approach: 
>
> + Compute the (Euclidean) distance between the unknown and each of the prototype vectors.
>
> + Then choose the smallest distance to make a decision

Represent each class by its `mean vector` of the patterns of that class:
$$
m_j=\frac{1}{N_j}\sum_{x\in \omega_j}X_j
$$

Determine the Euclidean distance of pattern $x$ to class $\omega_j$

$$
D'_j(X)=||X-m_j||=[(x-m_j)^T(x-m_j)]^{\frac{1}{2}}
$$

If $D_i(x)$ is the smallest distance, then assign $x$ to class $\omega_j$

Usually choose to calculate the maximum of $(x^{T})m_j-\frac{1}{2}m_j^Tm_j$

![DIP14-2](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP14-2.png)

`Feature selection`: more discriminative features which show **less overlap** between classes.

`Performance of Classifier`: 
+ The performance of a classifier should be assessed by the classification error on **an independent test set**.
+ Determine a decision boundary by **minimizing the classification error** of the training set.

#### Data Clustering

      Data clustering is a common technique for statistical data analysis, which is used in many fields, including machine learning, data mining, pattern recognition, and image analysis.

  > Cluster Analysis methods do not need training sets for learning

  >Distance Measure: select adistance measure, which determines how the similarity of two elements is calculated.

  + K-means cluster analysis methods:
    
    ![DIP14-3](https://gitee.com/yehanqiao/Markdown/raw/master/IMG/DIP/DIP14-3.png)

    >Advantages: simplicity and speed which allows it to run on large datasets.

    >Disadvantages:
    >
    >>+ does not yield the same, depend on initial random assignments
    >
    >>+ minimizes intra-cluster variance, not global
