# "WebRTC Native Stack is considered as a single chain as close to the Hardware Abstraction Layer (HAL) as possible."


**Getting Started**
------
This repository involves a **Step by Step Guide to Build Android App based on WebRTC Native Stack.** As we all know, Android Programs run into Dalvik Virtual Machine. Native Development tool (NDK) allows users to execute some of the program using native code languages such as `C/C++`.

------
# NDK:

[NDK (Native Development Kit)](https://developer.android.com/ndk/guides) includes the following features such as;

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

Drawback: There is one of the major side effect of JNI is, *The Program is no longer Cross-Platform*. To make your program Cross-Platform, you must recompile the native language part in different system environments. The program is no longer absolutely secure, and *improper* use of native code may cause the entire program to crash. As a general rule, you should focus your local methods on a few classes. This reduces the coupling between `JAVA` and `C`.

 In fact, Java and C/C++ are static type languages, the main reason that java and C cannot communicate is the problem of data types. JNI plays a role in data conversion. The corresponding relationship is shown in the following table:
 
 ![JNI Types and Data Sturctures](https://github.com/mail2chromium/Android-Native-Development-For-WebRTC/blob/master/types.png)

-----

 # NDK and JNI Workaround:
 
To put it simply, NDK is an extension toolkit developed by JNI. It uses NDK to compile corresponding local [static or shared libraries](https://stackoverflow.com/questions/2649334/difference-between-static-and-shared-libraries) that can be run for different mobile devices (`ARM, x86 ...`). In order to improve the sorting efficiency and use `C/C ++` in android, here are different steps for this process are: 

1. SDK development (write java code, write code containing native keywords to start JNI)([Native Methods](http://journals.ecs.soton.ac.uk/java/tutorial/native/index.html)) .

2. JNI development (according to the JNI coding specification, write a local interaction with java Code, the role is to talk about the `C/C++` data type conversion into Java can recognize).

3. `C/C++` development (write business logic, or call the local API or library provided by the NDK to complete the development of specific functions).

4. NDK compilation ( Write the .mk file, compile and debug, modify the .mk file, and optimize the compilation results for the specific platform (`ARM/X86`).

5. SDK compilation and packaging.

-----

# Installation and Configurations

Here we start with;

1. Download Source Code:

As we know that WebRTC implementation is done in `C/C++` languages. It needs to integrate with android application via Java Native Interface(JNI). WebRTC source available at [Github WebRTC Source Code](https://github.com/JumpingYang001/webrtc.git) as well as official WebRTC [download page](http://webrtc.github.io/webrtc-org/native-code/development/).

2. Setup Android NDK:

To integrate WebRTC code via JNI, first I needs setup android NDK. Download NDK from [here](https://developer.android.com/ndk/downloads) and extract it. Main thing to consider here is, make sure NDK path is being setup in environment variables in windows i.e. [see](https://subscription.packtpub.com/book/application_development/9781849691505/1/ch01lvl1sec09/setting-up-an-android-ndk-development-environment-in-windows). 

To test whether ndk-build is working or not, check these commands in the terminal like;

```
  ndk-stack
  ndk-build --help
```

**Example:**

Before digging into WebRTC Native Stack, If you don't have a basic understanding of NDK development, 
I must recommend you to follow this tutorial i.e [Hello-JNI](https://github.com/android/ndk-samples/tree/master/hello-jni).

-----

# WebRTC with Android:

Following are the steps that I have followed to integrate WebRTC source code with my android application. 

1. [Creating an Android Project](https://developer.android.com/training/basics/firstapp/creating-project)

2. Importing WebRTC Source to Android Project:

I have a seperated directory called `webrtc` inside the jni directory and added WebRTC source code for it. WebRTC source code can be cloned from [github](https://github.com/JumpingYang001/webrtc.git) or download via official [download page](http://webrtc.github.io/webrtc-org/native-code/development/).

![Android WebRTC Project Structure](https://github.com/mail2chromium/Android-Native-Development-For-WebRTC/blob/master/structure.png)

Make sure your project structure follow the same hierarchy as given above.

-----

3. Write a Java class that uses native code of `C/C++`:

I have maintained and updated `Apm.java` in my Android Project as given here;

![Android WebRTC Project Structure](https://github.com/mail2chromium/Android-Native-Development-For-WebRTC/blob/master/apm.png)

I have declared the all the native instance method, via keyword native which denotes that this method is implemented in another language. A native method does not contain a body. These native methods shall be found in the native library loaded. Here are the list of native methods inculded inside the `Apm.java` class;

```
    // apm constructor
    private native boolean nativeCreateApmInstance(boolean aecExtendFilter, boolean speechIntelligibilityEnhance, boolean delayAgnostic, boolean beamforming, boolean nextGenerationAec, boolean experimentalNs, boolean experimentalAgc);
    private native void nativeFreeApmInstance();
    private native int high_pass_filter_enable(boolean enable);
    
    // aec and aecm
    private native int aec_enable(boolean enable);
    private native int aec_set_suppression_level(int level); //[0, 1, 2]
    private native int aec_clock_drift_compensation_enable(boolean enable);
    private native int aecm_enable(boolean enable);
    private native int aecm_set_suppression_level(int level); //[0, 1, 2, 3, 4]
    private native int ns_enable(boolean enable);
    private native int ns_set_level(int level); // [0, 1, 2, 3]
    
    // agc
    private native int agc_enable(boolean enable);
    private native int agc_set_target_level_dbfs(int level); //[0,31]
    private native int agc_set_compression_gain_db(int gain); //[0,90]
    private native int agc_enable_limiter(boolean enable);
    private native int agc_set_analog_level_limits(int minimum, int maximum); // limit to [0, 65535]
    private native int agc_set_mode(int mode); // [0, 1, 2]
    private native int agc_set_stream_analog_level(int level);
    private native int agc_stream_analog_level();
    
    // vad 
    private native int vad_enable(boolean enable);
    private native int vad_set_likelihood(int likelihood);
    private native boolean vad_stream_has_voice();
    
    // Capture and render audio stream
    private native int ProcessStream(short[] nearEnd, int offset); //Local data// 16K, 16Bits, 10ms
    private native int ProcessReverseStream(short[] farEnd, int offset); // Remote data // 16K, 16Bits, 10ms
    private native int set_stream_delay_ms(int delay);
    
    // resampling
    private native boolean SamplingInit(int inFreq, int outFreq, long num_channels);
    private native int SamplingReset(int inFreq, int outFreq, long num_channels);
    private native int SamplingResetIfNeeded(int inFreq, int outFreq, long num_channels);
    private native int SamplingPush(short[] samplesIn, long lengthIn, short[] samplesOut,long maxLen, long outLen);
    private native boolean SamplingDestroy();

```

The static initializer invokes `System.loadLibrary()` to load the native library "webrtc_apms" (which contains all native methods) during the class loading. It will be mapped to " `libwebrtc_apms.dll` " in Windows; or " `libwebrtc_apms.so` " in (Unix/Mac-OS X). This library shall be included in **libs** directory automatically after you run this command in terminal i.e. `ndk-build`. The program will throw a UnsatisfiedLinkError if the library cannot be found in runtime.

-----

4. Compile the Java Program Apm.java & Generate the C/C++ Header File:

Starting from JDK 8, you should use "javac -h" to compile the Java program AND generate C/C++ header file called `com_webrtc_audioprocessing_Apm.h` as follows:

```
javac -h . Apm.java
```

The "-h dir" option generates C/C++ header and places it in the directory specified (in the above example, '.' for the current directory).

Before JDK 8, you need to compile the Java program using javac and generate C/C++ header using a dedicated javah utility, as follows. The javah utility is no longer available in JDK 10.

```
javac Apm.java
javah ApmJNI 
```

Now place this header file (`com_webrtc_audioprocessing_Apm.h`) inside the following i.e. `src/main/jni/`


5. WebRTC JNI Apm

By using those generated library files and header files I have written JNI based C functions to **preprocess and postprocess** the audio data. These JNI files prefixed with `com_webrtc_audioprocessing_Apm` which is the java package path of the JNI wrappers.

![Android WebRTC Project Structure](https://github.com/mail2chromium/Android-Native-Development-For-WebRTC/blob/master/jni_files.png)


 





























