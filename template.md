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
img = cv2.imread(scr_path)
cv2.imshow('raw image', img)
cv2.waitKey(0)
```
```http
result=matchTemplate(\scr_path='xxxxxxx\scr.png',templ_path='\xxxxxxx\template.png',draw_site=False)
```


