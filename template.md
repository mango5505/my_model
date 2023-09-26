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
读入原始图像路径，显示原始图像
scr_path='\xxxxxxx\scr.png'
img1 = cv2.imread(scr_path)
cv2.imshow('raw image', img)
cv2.waitKey(0)
```
![raw image](https://raw.githubusercontent.com/mango5505/my_model/main/3.JPG?token=GHSAT0AAAAAACGFRKLVJQXI5L7PLHGXKBTGZISRNEQ "raw image")
```http
读入模板路径，显示模板图像
templ_path='\xxxxxxx\template.png'
img2 = cv2.imread(temple_path)
cv2.imshow('template image', img2)
cv2.waitKey(0)
```
```http
result=matchTemplate(\scr_path='xxxxxxx\scr.png',templ_path='\xxxxxxx\template.png',draw_site=False)
```


