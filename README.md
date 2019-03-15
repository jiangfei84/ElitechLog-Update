# 前言
macOS 开发中使用**Sparkle**进行自更新，目前百度搜索到的资料几乎都是老版本的，英文不太好，折腾了好久，在此记录下，写个教程方便后来人使用
# 环境
* mac os High 
* Sierra(10.13.6)
Xcode Version 10.1 (10B61)
* sparkle 1.12.3

# 新建MacApp

新建Mac os app

# Sparkle集成
推荐使用cocopods 
```
pod 'Sparkle'

#Using Sparkle (1.21.3)
```
#具体流程

 1. 新增menuItem并连线
![自动更新连线](https://upload-images.jianshu.io/upload_images/1126761-8364b274bfa514f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 打开pod文件夹如图位置
![屏幕快照 2019-03-11 上午9.36.18.png](https://upload-images.jianshu.io/upload_images/1126761-df585f165c733b66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. 生成key并填入程序info.plist【访问钥匙串需要同意权限】

```
##进入bin目录后
lm-majinlideMacBook-Air:bin zhoucan$ generate_keys 
```
![屏幕快照 2019-03-11 上午9.40.45.png](https://upload-images.jianshu.io/upload_images/1126761-ad812f5d7dd4e00d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 用python开启本地文件夹服务器
```
  python -m http.server 或者 
  python -m SimpleHTTPServer

默认开启http://0.0.0.0:8000/

```
![http://0.0.0.0:8000/已开启](https://upload-images.jianshu.io/upload_images/1126761-2f2933ae21dca287.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4.1 提前设置【SUFeedURL http://0.0.0.0:8000/MacApp.xml】在info.plist添加更新网址，并打开ATS {本地是http，所以还是打开吧}
5. 把Mac App修改verison和build 改到2.0版本并打包导出 MacApp.app
6. 压缩MacApp.zip
7. 签名并生成appcast.xml
![签名生成xml](https://upload-images.jianshu.io/upload_images/1126761-8ad44ea0ad219e9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
8.xml文件和zip移动至如图所示位置
![本地文件夹服务器](https://upload-images.jianshu.io/upload_images/1126761-2cfce8cbe3b0577a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
以下是MacApp.xml的内容
```
<?xml version="1.0" standalone="yes"?><rss xmlns:sparkle="http://www.andymatuschak.org/xml-namespaces/sparkle" version="2.0">
<channel>
<title>MacApp</title>
<item>
<title>2.0</title>
<pubDate>周一, 11 3月 2019 09:44:51 +0800</pubDate>
<sparkle:minimumSystemVersion>10.13</sparkle:minimumSystemVersion>
<enclosure url="http://0.0.0.0:8000/MacApp.zip" sparkle:version="2.0" sparkle:shortVersionString="2.0" length="5056434" type="application/octet-stream" sparkle:edSignature="BT7y5cdBzgElBgkFrFQdBEjmT+fyaeWV0WTCILLcWCTLRzLwzZVONUA/uBKgsn/qKOtFCKaF9a7O7v9nBiJxDg=="/>
</item>
</channel></rss>

```
9. 检查更新
![手动更新](https://upload-images.jianshu.io/upload_images/1126761-c48f08a62395eafa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

10. 打开就检查有没有新版本
![代码更新](https://upload-images.jianshu.io/upload_images/1126761-8f6ba8d49d9a3a29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

11. 基本成功
![弹出更新](https://upload-images.jianshu.io/upload_images/1126761-3f1f2a977262320a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



