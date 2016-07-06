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
