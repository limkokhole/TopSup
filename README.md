
# 答题辅助
这两天冲顶大会直播答题 APP 突然火了起来，简单写了一个辅助答题脚本。使用文字识别搜索，只能增加准确率，保证不了全对。

![](./resources/screenshot.png)

灵感来自：
> [微信跳一跳辅助 ](https://github.com/wangshub/wechat_jump_game)
> [程序员如何玩转《冲顶大会》？](https://livc.io/blog/204)

## 具体做法

1. ADB 获取手机截屏
```
adb shell screencap -p /sdcard/screenshot.png
adb pull /sdcard/screenshot.png .
```
2. OCR 识别题目文字
用的是谷歌 [Tesseract](https://github.com/madmaze/pytesseract) 

3. 调用浏览器百度搜索
![](./resources/result.png)
## 使用步骤
### Android
1. 安装 ADB
下载地址：https://adb.clockworkmod.com/
安装完后插入安卓设备且安卓已打开 USB 调试模式，终端输入 `adb devices` ，显示设备号则表示成功。我手上的机子是坚果 pro1，第一次不成功，使用 [handshaker](https://www.smartisan.com/apps/handshaker) 加载驱动后成功，也可以使用豌豆荚之类的试试。
2. 安装 python 3
3. 安装 pytesseract
命令行：`pip install pytesseract`
4. 安装 谷歌 Tesseract
**Windows下链接**
*推荐使用安装版在安装时选择增加中文简体语言包*
安装版：
https://digi.bib.uni-mannheim.de/tesseract/tesseract-ocr-setup-3.05.01.exe
免安装版：
https://github.com/parrot-office/tesseract/releases/download/3.5.1/tesseract-Win64.zip
*免安装版需要下载[中文语言包](https://github.com/tesseract-ocr/tesseract/wiki/Data-Files)*
**其他系统**：
https://github.com/tesseract-ocr/tesseract/wiki
5. 修改 `GetTitleTessAndroid` 代码相应目录信息（默认安装则无需修改）
```
# tesseract 路径
pytesseract.pytesseract.tesseract_cmd = 'C:\\Program Files (x86)\\Tesseract-OCR\\tesseract'
# 语言包目录
tessdata_dir_config = '--tessdata-dir "C:\\Program Files (x86)\\Tesseract-OCR\\tessdata"'
```
7. 运行脚本
`python GetTitleTessAndroid.py`
会自动识别文字并打开浏览器

**注： 可以用 `GetImgTool.py` 调整题目截取位置**
若屏幕分辨率不同，请在 GetTitleTessAndroid.py 中自行修改代码即可
```
# 切割题目位置，左上角坐标和右下角坐标
region = img.crop((50, 350, 1000, 560)) # 坚果 pro1
#region = img.crop((75, 315, 1167, 789)) # iPhone 7P
```

### IOS
需要安装 WDA 进行截图，参考 https://testerhome.com/topics/7220,其他步骤相同。

`python GetTitleTessIos.py`

## 其他文件说明

GetTitleBaiduAndroid.py 和 GetTitleBaiduIos.py 为 [livc](https://livc.io/blog/204) 文章代码修改来的，需要在[百度平台](https://cloud.baidu.com/product/ocr)上申请 API Key 和 Secret Key 即可使用，无需下载其他文字识别包，以上代码安卓版本都测试过了。


## 总结

有了 ADB 截图，怕是各种小辅助都可以玩了。python 写小脚本真的很方便。