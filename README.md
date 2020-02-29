# "WebRTC Native Stack is considered as close to the Hardware Abstraction Layer (HAL) as possible."


**Getting Started**
------
This repository involves a **Step by Step Guide to Build Android App based on WebRTC Native Stack.** As we all know, Android Programs run into Dalvik Virtual Machine. Native Development tool (NDK) allows users to execute some of the program using native code languages such as `C/C++`.

------
# NDK:

[Native Development kit](https://developer.android.com/ndk/guides) includes the following features such as;

- Tool and build files needed to generate a native code base from `C/C++`.
- Embed consistent native libraries in application packages files (.apk files) that can be deployed on Android devices.
- Supports a number of native system header files and libraries for the Android platform.

Here you can select & download the latest or lagecy NDK package for your development platform i.e. [NDK Download](https://developer.android.com/ndk/downloads)

------
# Why NDK?

NDk finds its best use to squeeze extra performance out to device, to achieve low latency or run computationally intensive applications on android platform. Here are some other reasons why we use NDK in our development;

- Easy to port libraries written in `C/C++` can be easily reused on other embedded platforms.
- Code protection, because the java code of apk can be easily decompiled, and the `C/C++` library is difficult to reverse.
- Call third-party `C/C++` libraries in the NDK, because most open source libraries are written in `C/C++` code,
such as WebRTC, TarsosDSP, Ffmpeg, OpenCV, and a lot of other machine learning stuff etc. 

------
# What is JNI?

[JNI (Java Native Interface)](https://docs.oracle.com/javase/8/docs/technotes/guides/jni/index.html) provides several APIs to implement the communication between Java and other languages (mainly `C/C++`). 

Drawback: There is one of the major side effect of JNI is, *The Program is no longer Cross-Platform*. To make your program Cross-Platform, you must recompile the native language part in different system environments. The program is no longer absolutely secure, and *improper* use of native code may cause the entire program to crash. As a general rule, you should focus your local methods on a few classes. This reduces the coupling between JAVA and C.

 In fact, Java and C/C++ are static type languages, the main reason that java and C cannot communicate is the problem of data types. JNI plays a role in data conversion. The corresponding relationship is shown in the following table:
 
 ![JNI Types and Data Sturctures](https://github.com/mail2chromium/Android-Audio-Processing-Using-WebRTC/blob/master/Content.png)

 
 





























