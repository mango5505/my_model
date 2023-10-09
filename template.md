# USCT.preprocessing.MatchTemplate

### 数字医学影像的模板匹配感兴趣区域定位算法

作者：李婧  
时间：2023/10/7  
版本：1.0


> __*function*__  
USCT.preprocessing.MatchTemplate(
    scr_path, templ_path, prepro_flag=True, visual_flag=Flase
)

&emsp;&emsp;本函数适用于为医疗设备扫查获得的数字影像提取自定义的感兴趣区域。具体地，医疗设备直接扫查获得原始图像，在同类原始图像中由使用者根据期望截取其中感兴趣区域作为模板图像；算法线性遍历原始图像，寻找与模板图像匹配度最高的区域，作为感兴趣区域输出。函数设置预处理标志位，选择输入原始图像是否包含医疗设备界面信息，需要进行预处理步骤；设置最优匹配位置坐标可视化标志位，选择是否将感兴趣区域在原始图像中以方框标示同作为结果输出。

### 参数
&emsp;&emsp;**scr_path:** &emsp; *string*  
&emsp;&emsp;&emsp;&emsp;&emsp;原始图像路径文本，原始图像要求为三通道png、jpg等静态图像格式文件  
&emsp;&emsp;**templ_path:** &emsp; *string*  
&emsp;&emsp;&emsp;&emsp;&emsp;模板图像路径文本，模板图像要求为三通道png、jpg等静态图像格式文件       
&emsp;&emsp;**prepro_flag:** &emsp; *bool*   
&emsp;&emsp;&emsp;&emsp;&emsp;预处理标志位，真为需要预处理，假为不需要，默认为真。  
&emsp;&emsp;**visual_flag:** &emsp; *bool*    
&emsp;&emsp;&emsp;&emsp;&emsp;最优匹配位置坐标可视化标志位，真为显示，假为不显示，默认为假。  
### 返回值
&emsp;&emsp;**result:** &emsp; *numpy.ndarray*   
&emsp;&emsp;&emsp;&emsp;&emsp;使用模板图像在原始图像中进行模板匹配，定位最优匹配位置后，切割出的匹配结果图像，即原始图像中与模板图像最接近的图像区域，图像输出为单通道二维数组。如最优匹配位置坐标可视化标志位置真，感兴趣区域在原始图像中位置以方框标示图一同返回。

### 算法介绍

#### 背景
&emsp;&emsp;本函数适用于为医疗设备扫查获得的数字影像提取自定义的感兴趣区域。以在超声数字影像中应用为例，临床中常使用超声设备获得颈动脉、心血管和颅内血管等血管影像，从而达到疾病诊断的目的。血管超声影像显示血管内中膜是否增厚、有无斑块形成、斑块形成的部位、大小、是否有血管狭窄及狭窄程度、有无闭塞等详细情况。计算机对血管超声影像进行算法处理时，相较于对直接分割测量内中膜厚度，在整张超声影像中定位感兴趣区域再进行处理可以降低计算时间并提高算法的鲁棒性。使用模板匹配的定位方法，使用者根据关注的感兴趣区域自行选择模板，达到在整张图像中定位期望的图像区域的目的。
#### 算法详解

*预处理*<br>
&emsp;&emsp;在某些场景下，医疗设备扫查后直接导出的数字影像包含了病理图像之外的设备界面元素，这将干扰后续计算机的图像处理分析结果，在此情况下，算法添加预处理标志位，提供预处理功能，预处理详细处理步骤：<br>
&emsp;步骤一：灰度化图像<br>
&emsp;步骤二：自适应阈值<br>
&emsp;步骤三：Canny边缘识别<br>
&emsp;步骤四：形态学整形，形成只包含主轮廓边缘的二值图像<br>
&emsp;步骤五：目标轮廓选择<br>
&emsp;步骤六：剪裁输出<br>
<div  align="center"><img decoding="async" src="https://raw.githubusercontent.com/mango5505/my_model/main/1.jpg" width="60%" div align=center/><br>预处理前后示意图</div>

<br><br>*模板匹配*<br>

&emsp;&emsp;模板匹配是一种在较大原始图像中搜索和查找模板图像位置的方法。OpenCV附带了一个函数cv.matchTemplate（）来实现此目的。它将模板图像滑过原始图像，并在模板图像下比较原始图像，计算匹配程度，每个滑动到的位置的匹配程度对应一个灰度值，模板滑过整张原始图像返回一个灰度图像，其中每个像素表示该像素的邻域与模板的匹配程度。<br>
&emsp;&emsp;如果输入图像的大小为 （WxH），模板图像的大小为 （wxh），则输出图像的大小将为 （W-w+1， H-h+1）。根据匹配公式:<br>
\begin{gathered}
R(x',y')=\frac{\sum_{x',y'}(T(x',y')-I(x+x',y+y'))^2}{\sqrt[2]{{\sum_{x',y'}T(x',y')^2\times}\sum_{x',y'}I(x+x',y+y')^2}}
\end{gathered}
&emsp;&emsp;其中$R(x,y)$代表匹配结果矩阵，$I(x+x',y+y')$代表模板移动(x,y)位置对应的待匹配图像矩阵，$T(x',y' )$代表匹配模板矩阵。<br>
&emsp;&emsp;得到结果后，您可以查找最大值/最小值的位置。将其作为矩形的左上角，并以（w，h）作为矩形的宽度和高度。该矩形是模板区域。
<div  align="center"><img decoding="async" src="https://raw.githubusercontent.com/mango5505/my_model/main/2.jpg" width="60%" div align=center/><br>模板图像滑过原始图像</div><br>

#### 参考文献
\[1\]&emsp; OpenCV 4.6.20 *OpenCV-Python Tutorials*,Intel,USA,2000;
software available at https://github.com/opencv
### 示例

```python
##参考环境：python3.10   opencv4.6

#1.导入OpenCV、pyplot软件包，读入原始图像路径，显示原始图像：
import cv2
from matplotlib import pyplot as plt
scr_path='\xxxxxxx\scr.png'
img1 = cv2.imread(scr_path)
cv2.imshow('raw image', img)
cv2.waitKey(0)

#2.读入模板路径，显示模板图像：
templ_path='\xxxxxxx\template.png'
img2 = cv2.imread(templ_path)
cv2.imshow('template image', img2)
cv2.waitKey(0)

#3.使用MatchTemplate函数进行ROI定位，标志位设置需要预处理，需要显示匹配位置，输出结果显示定位可视化和ROI图像：
result=MatchTemplate(scr_path=scr_path,templ_path=templ_path,prepro_flag=Ture,visual_flag=Ture)
plt.subplot(121)
plt.imshow(result[0], cmap="gray")
plt.subplot(122)
plt.imshow(result[1], cmap="gray")
plt.show()
```
<div  align="center"><img decoding="async"src="https://raw.githubusercontent.com/mango5505/my_model/main/3.JPG?token=GHSAT0AAAAAACGFRKLV5UZYEPUDEMGQ2NLMZISSANA" width="60%" div align=center/><br>原始图像（示例程序1节输出）</div>
<br>
<div  align="center"><img decoding="async" src="https://raw.githubusercontent.com/mango5505/my_model/main/44.png?token=GHSAT0AAAAAACGFRKLUO3TNDYP6TYCVBNMWZISSBBA" width="30%" div align=center/><br>模板图像（示例程序2节输出）</div>
<br>
<div  align="center"><img decoding="async" src="https://raw.githubusercontent.com/mango5505/my_model/main/location.jpg?token=GHSAT0AAAAAACGFRKLVWVSBRHQ44VGQE4CCZISSFEQ" width="60%" div align=center/><br>左：最优匹配位置可视化&emsp;&emsp;右：感兴趣区域（示例程序3节输出）</div>

