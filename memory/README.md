## 如何粗略的計算image被讀進memory之後的size是多大

1. 用Android系統提供的API去計算
```
/**
 * returns the bytesize of the give bitmap
 */
public static int byteSizeOf(Bitmap bitmap) {
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
        return bitmap.getAllocationByteCount();
    } else if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.HONEYCOMB_MR1) {
        return bitmap.getByteCount();
    } else {
        return bitmap.getRowBytes() * bitmap.getHeight();
    }
}
```
[來源](http://stackoverflow.com/questions/2407565/bitmap-byte-size-after-decoding)

2. 粗略的手動計算
```
size = width * height * 4 for RGBA_8888
     = width * height * 2 for RGB_565
```
原理為：一張圖片的解析度為 width * height pixels，每個pixel所佔的空間為：
```
  (8 + 8 + 8 + 8 = 24 bits = 3 bytes), for RGBA_8888
  (5 + 6 + 5 = 16 bits = 2 bytes), or RGB_565
```
  因此所佔的所有空間為：pixel數 * 每個pixel所佔的空間：
```
  size = width * height * 4 for RGBA_8888
       = width * height * 2 for RGB_565
```
[來源](http://stackoverflow.com/questions/11461061/why-bitmap-size-is-bigger-in-memory-than-on-disk-in-android)  
[來源](http://androidprofessionals.blogspot.tw/2013/02/bitmap-out-of-memory-android.html)


### 什麼是RGBA_8888, RGB_565
RGBA_8888 代表有8 bit是用來存放紅色, 8 bit存放綠色, 8 bit存放藍色, 還有8 bit存放透明值
RGBX_8888 的X則代表忽略原本圖片的透明值，並且把透明值設為255，因此透明值仍然還是會佔8 bit的空間
RGB_888 則代表沒有透明值的資訊

Android預設的圖片格式為PixelFormat.OPAQUE，是不包含Alpha值
[來源](http://stackoverflow.com/questions/32348053/what-is-pixelformat-rgbx-888)


## 常用的解析度
SD (Standard Definition: 720x480 (480p)  
HD (High Definition): 1280x720(720p)  
Full HD (Full High Definition(): 1920x1080(1080p)  
2K (DCI (Digital Cinema Initiatives): 2048x1080(2k)  
4K Ultra HD(4k UHD): 3840x2160(4k)  
8K Ultra HD (8k UHD): 7680x4320(8k)

## Android裝置螢幕解析度
Nexus 5:  
1080×1920 pixel resolution (1080p)  
Cameras: 8 MP rear camera with optical image stabilization (OIS)  
1.3 MP front camera  

Nexus 5X:  
1080x1920 pixel resolution (423ppi)  
12.3 MP rear camera with f/2.0 lens and IR laser-assisted autofocus  
5 MP front camera with f/2.0 lens  

0.9 MP(0.9百萬畫素)的相機，拍出來的解析度為 1280x720 = 921,600 ~ 0.9MP  
2 MP(2百萬畫素)的相機，拍出來的解析度為 1920x1080 = 2,073,600 ~ 2mp  
5 MP(5百萬畫素)的相機，拍出來的解析度為 2560x1920 = 4,915,200 ~ 5MP
18 MP(18百萬畫素)的相機，拍出來的解析度為 5184x3456 = 17,915,904 ~ 18MP

## 為什麼1080p比1080i好
1080p和1080i的解析度都是1920x1080，他們差在畫面的呈現方式：  
1080p是每個frame都會出現所有的畫面  
1080i的第一個frame只會出現奇數行的畫面，下一個frame呈現偶數行的畫面  
然後利用視覺暫留的方式，讓使用者覺得畫面滿正常的  
理論上，以畫面的流暢度上面來說1080p > 1080i，但是人眼察覺不出來  
不過人眼必須利用視覺暫留的方式，來觀賞1080i的影片，因此眼睛容易感到疲倦  
[來源](http://www.flag.com.tw/book/cento-5105.asp?bokno=FT160&id=460)


## PPI
PPI或做ppi，  
是pixels per inch的縮寫，  
意即「每英吋多少像素」。  
基本上，  
不管是螢幕解析度也好、印刷品解析度也好，  
我們口中所說的「解析度」，  
絕大多數都是指ppi。  


我們一般印刷品需求的300dpi，  
指的是要求每英吋要有300個像素，  
這樣子來想大概就能理解了。  

## DPI

DPI或做dpi，  
是dots per inch的縮寫，  
意即「每英吋多少點」。  
確實，  
一個像素可以當作一個「點」來看，  
然而這裡的「點」，  
指的是「印刷點」。  
或者更精確地說，  
是一滴油墨或者墨水滴在紙上時的大小。


4800 DPI = 1英吋(長度)有4800個點 (這個點指的是實際上印出來的油墨點)  
一個實際的油墨點的大小為 1/4800 英吋  

300 PPI = 1英吋有300個 pixel  
一個實際的pixel的大小為 1/300 英吋  

如果要印出300 PPI中的1個點的話(也就是1個pixel)  
就需要1/300 / 1/4800 = 16個油墨點  
排成16x16的矩陣來模擬這個點

DPI和PPI嚴格說起來是兩個不同的單位  


如同一個影像大小不變，因要輸出之解析度改為 350 dpi。  
換算出來的尺寸就是 (3000 / 350) x (2000 / 350) = 8.5 英吋(inche) x 5.7 英吋(inche)。  
如換算成 CM，(8.5 inche x 2.54 cm) x (5.7 inche x 2.54 cm) = 21 cm x 14 cm。  
這是此影像檔最適當的列印尺寸大約 21 cm x 14 cm，超過此尺寸就會造成影像失真模糊之情況。  

看 dpi 吧  
96 DPI 時, 1 Inch = 96 Pixels , 1 CM = 1/2.54 Inch = 96/2.54 Pixels = 37.7953 Pixels

[來源](http://home.gamer.com.tw/creationDetail.php?sn=2342652)

## 計算每台Android同時app memory能夠容納最大的圖片數量
待續...
