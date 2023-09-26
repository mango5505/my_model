# USCT.preprocessing.matchTemplate

python, process the raw data of ultrasound equipment to obtain the intima region of interest

### Parameters
| Parameter | Type | Description |  Default  |
|:----------|:-----|:------------|:----------|
|scr_path|str|path of jpg/png/... format 3 channel image|required|
|templ_path|str|path of jpg/png/... format 3 channel image|required|
|draw_site|bool|Whether to display the positioning diagram|required|

### Returnss
| Type | Description |
|:-----|:------------|
|numpy.ndarray| 1 channel image data|

### Example

```http
读入原始图像路径，显示原始图像：
scr_path='\xxxxxxx\scr.png'
img1 = cv2.imread(scr_path)
cv2.imshow('raw image', img)
cv2.waitKey(0)
```
![raw image](https://raw.githubusercontent.com/mango5505/my_model/main/3.JPG?token=GHSAT0AAAAAACGFRKLV5UZYEPUDEMGQ2NLMZISSANA "raw image")
```http
读入模板路径，显示模板图像：
templ_path='\xxxxxxx\template.png'
img2 = cv2.imread(templ_path)
cv2.imshow('template image', img2)
cv2.waitKey(0)
```
![template image](https://raw.githubusercontent.com/mango5505/my_model/main/44.png?token=GHSAT0AAAAAACGFRKLUO3TNDYP6TYCVBNMWZISSBBA "template image")
```http
使用matchTemplate函数进行ROI定位，显示定位结果和ROI图像：
result=matchTemplate(scr_path=scr_path,templ_path=templ_path,draw_site=Ture)
plt.subplot(121)
plt.imshow(result[0], cmap="gray")
plt.subplot(122)
plt.imshow(result[1], cmap="gray")
plt.show()
```
![result](https://raw.githubusercontent.com/mango5505/my_model/main/location.jpg?token=GHSAT0AAAAAACGFRKLVWVSBRHQ44VGQE4CCZISSFEQ "result")


