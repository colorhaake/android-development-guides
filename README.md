## Share

For basic sharing, please check the following link  
https://github.com/codepath/android_guides/wiki/Sharing-Content-with-Intents

For sharing video to Line, Uri must be `content://`  
like `content://media/external/video/media/3338`  

For normal sharing, Uri could be `file://`  
like `file:///storage/emulated/0/Movies/ABC/abc.mp4`

For how to convert file path to content uri, or to file uri, please check the following link  
[link](./share/README.md)

### Share on Android 6
The following steps would meet error  
1. install app that is share target (but not run this app before)  
2. launch another app to do share to this app (share path is `file://`)  
3. It could show toast error "can't attach empty files"

The reason is in Android 6, system is runtime permission checking. Your uri is `file://`, so the target app should have an permission of `READ_EXTERNAL_STORAGE`, but your taget app do not grant it this permission, so it would popup error

Please check the following link for more details  
[Android 6: cannot share files anymore?](http://stackoverflow.com/questions/32981194/android-6-cannot-share-files-anymore)  
[Runtime Permissions, Files, and ACTION_SEND](https://commonsware.com/blog/2015/10/07/runtime-permissions-files-action-send.html)

## Device memory limit
ASUS Zenphone 2  
dalvik.vm.heapgrowthlimit=200m  
dalvik.vm.heapsize=348m  

Xiaomi Mi 4i  
dalvik.vm.heapgrowthlimit=128m  
dalvik.vm.heapsize=256m  

Nexus 5  
dalvik.vm.heapgrowthlimit=192m  
dalvik.vm.heapsize=512m  

heapgrowthlimit是一般情況下的heap size  
heapsize是使用largeHeap後，可以用的最大heap size  

結論是小米手機heap真的很小  

## 如何粗略的計算image被讀進memory之後的size是多大
[link](./memory/README.md)

## 如何用git增加submodules
語法：  
`git submodule add <repository> [<path>]`  
`git submodule add https://github.com/facebook/xxxxx.git to/your/path`  

接著輸入`git status`  
可以看到有兩個要新增的檔案  
```
#       new file:   .gitmodules
#       new file:   your_submodules_repository
```
這兩個檔案都要commit以及push到remote repository.  
`your_submodules_repository`只是個連結而已，並不是整個資料夾裡的檔案都上去  

接著如果你的同事clone下來這個專案之後，只要輸入  
```
git submodule init // 會加submodule的資訊到專案目錄的.git/config
git submodule update
```
就會clone下來整個submodule  

## 如何將git的帳號以及密碼cached起來，因此任何和remote repository的溝通不用再次輸入帳密
原則上應該會是自動就會cached到keychain。如果真的發生一直要求輸入帳密的情形，輸入  
`git config credential.helper store`  
可以cached到local  
`git config --global credential.helper store`  
可以cached到global  
