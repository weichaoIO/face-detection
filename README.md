
[toc]

---

#<center>**Eclipse 导入 OpenCV 工程之 face-detection**</center>

---

#***TODO***

- [] 

---

#**Reference**

* [eclipse导入曾经删除过的项目时 提示工作空间内该项目仍然存在](http://blog.csdn.net/xiabo851205/article/details/8234828 "http://blog.csdn.net/xiabo851205/article/details/8234828")

---

#**准备工作**

##**下载并安装 OpenCV 的 Andorid SDK**

>官方网址：http://opencv.org/downloads.html

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/01.png)

解压缩。

比如解压缩到 D:\OpenCV-3.2.0-android-sdk

##**下载并安装 NDK**

>官方网址：https://developer.android.com/ndk/downloads/index.html

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/02.png)

解压缩。

比如解压缩到 D:\android-ndk

---

#**导入工程**

##**打开 Eclipse 并开始导入工程**

>File -> Import...

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/03.png)

##**选择项目类型为 Android**

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/04.png)

##**选择导入的 OpenCV 工程**

导入 OpenCV 的 lib 和 face-detection 工程。

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/05.png)

###**非正常情况**

####**工作空间中不存在该项目却无法导入**

#####**Question**

>Select at least one project

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/06.png)

#####**Why**

以前导入过这个工程，没有删除干净。删除这个工程后，Eclipse 还有记忆。

#####**Answer**

刷新工作空间，在 Package Explorer 中右键，选择 Refresh，删除不存在的工程。

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/07.png)

---

#**配置工程**

##**配置 lib**

###**类未找到**

####**Question**

>The import android.hardware.camera2 cannot be resolved

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/08.png)

####**Why**

Project Build Target 版本过低，换成更高的版本。

####**Answer**

>右键工程 -> Properties

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/09.png)

选择较高版本，比如 Android 7.1.1。

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/10.png)

##**配置 face-detection**

###**lib 未找到**

####**Question**

>The import org.opencv.* cannot be resolved

####**Why**

lib 导入 Eclipse 后文件位置发生了变化。

####**Answer**

重新设置 lib 引用。

>右键工程 -> Properties

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/11.png)

删除旧的 lib 引用，并应用。

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/12.png)

添加新的 lib 引用，并应用。

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/13.png)

###**ndk-build.cmd 未找到**

####**Question**

>Program "/ndk-build.cmd" is not found in PATH

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/14.png)

####**Why**

默认的 C/C++ build command 不可用。

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/15.png)

####**Answer**

#####**方法1**

修改 C/C++ build command。

>右键工程 -> Properties

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/16.png)

修改为 NDK 的安装路径。

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/17.png)


#####**方法2**

添加环境变量 NDKROOT，值为 NDK 目录。

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/18.png)

rebuild 工程。

###**OpenCV.mk 未找到**

####**Question**

>make: *** No rule to make target `../../sdk/native/jni/OpenCV.mk'.  Stop.

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/19.png)

####**Why**

默认的 OpenCV.mk 路径不可用。

####**Answer**

修改路径。

打开 Android.mk。

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/20.png)

源代码：

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/21.png)

修改为：

    LOCAL_PATH := $(call my-dir)
    
    include $(CLEAR_VARS)
    
    #OPENCV_CAMERA_MODULES:=off
    #OPENCV_INSTALL_MODULES:=off
    #OPENCV_LIB_TYPE:=SHARED
    ifdef OPENCV_ANDROID_SDK
      ifneq ("","$(wildcard $(OPENCV_ANDROID_SDK)/OpenCV.mk)")
        include ${OPENCV_ANDROID_SDK}/OpenCV.mk
      else
        include ${OPENCV_ANDROID_SDK}/sdk/native/jni/OpenCV.mk
      endif
    else
      include D:\OpenCV-3.2.0-android-sdk\sdk\native\jni\OpenCV.mk
    endif
    
    LOCAL_SRC_FILES  := DetectionBasedTracker_jni.cpp
    LOCAL_C_INCLUDES += $(LOCAL_PATH)
    LOCAL_LDLIBS     += -llog -ldl
    
    LOCAL_MODULE     := detection_based_tracker
    
    include $(BUILD_SHARED_LIBRARY)

---

#**测试**

##**安装对应平台的 OpenCV Manager**

文件所在目录：D:\OpenCV-3.2.0-android-sdk\apk

##**安装并运行 face-detection**

运行成功。

![](https://github.com/weichao66666/face-detection/raw/master/README.md-images/22.png)

---














