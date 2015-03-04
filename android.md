## Android Java and Native Applications

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/Android

## Guide

###Tegra Toolkit###

The Tegra Toolkit is a large toolset which includes the following:

<b>Visual Studio Templates</b> - Adds compiling and debugging Android Java and Android Native Applications

<b>Android NDK</b> - Development kit for compiling "C" native applications

<b>Android SDK</b> - Software development kid for compiling Android applications

<b>NVidia NDK modules</b> - for hardware accelerated development

<b>Eclipse</b> - A common IDE for integrated Java development

<b>NVidia Samples</b> - A large set of hardware accelerated examples in Java and Native Applications

### Examples

Examples are included at the base GIT path.

#### External Storage

* Note: In the game details, be sure to move to external storage so the app can access the external storage drive.

<b>External Storage</b> - Using external storage example as a Java Activity. https://github.com/ouya/ouya-sdk-examples/tree/master/Android/ExternalStorageExample

<b>External Storage Native</b> - Using external storage example as a C++ Native Activity. External storage is accessed via JNI calls. https://github.com/ouya/ouya-sdk-examples/tree/master/Android/ExternalStorageExampleNative

#### General

<b>Native In-App-Purchases</b> - https://github.com/ouya/ouya-sdk-examples/tree/master/Android/InAppPurchasesNative

<b>Multiple Activities</b> - Switch between Java activities within the same application - https://github.com/ouya/ouya-sdk-examples/tree/master/Android/MultipleActivities

<b>Multiple Activities Native</b> - Switch between Java activities and a native activity in the same application - https://github.com/ouya/ouya-sdk-examples/tree/master/Android/MultipleActivitiesNative

<b>Multiple Activities WebView</b> - Switch between Java activities with a browser in the same application - https://github.com/ouya/ouya-sdk-examples/tree/master/Android/MultipleActivitiesWebView

<b>Set Resolutions</b> - Switch between view size resolutions - https://github.com/ouya/ouya-sdk-examples/tree/master/Android/SetResolutions

<b>Sound Mixer</b> - Detect volume changes - https://github.com/ouya/ouya-sdk-examples/tree/master/Android/SoundMixer

<b>Safe Area</b> - Adjust the safe area with a slider - https://github.com/ouya/ouya-sdk-examples/tree/master/Android/SafeAreaExample

### Resources

<b>Android NDK</b> - http://developer.android.com/tools/sdk/ndk/index.html

<b>Android SDK</b> - http://developer.android.com/sdk/index.html

<b>Java JDK 7</b> - http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html

<b>Java JDK 6</b> - http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html

<b>NVIDIA Nsight Visual Studio Edition</b> - https://developer.nvidia.com/nvidia-nsight-visual-studio-edition

<b>Tegra Developer Pack</b> - https://developer.nvidia.com/tegra-resources

<table border="1"><tr><td>
<i>*Be sure to register to get access to the Tegra Developer Pack downloads.</i><br/>
<b>Tegra Registered Developer Program</b> - https://developer.nvidia.com/registered-developer-programs<br/>
</td></tr></table>

Note:

There is also an issue with nSight and the default install of the NVIDIA Toolkit, which can be fixed by the following two options. This fixes a compile error for ANT related to Java/NDK projects.

```
First/easiest option to try:
     1. Edit NVPACK\android-sdk-windows\build-tools\<each version>\dx.bat
     2. Change "set defaultMx=-Xmx1024M" to "set defaultMx=-Xmx512M"
     3. Save and exit
     4. Restart Visual Studio
     5. Rebuild
Second option:
      1. Run NVPACK\android-sdk-windows\SDKManager.exe
      2. Click "deselect all"
      3. If Tools : "Android SDK Build Tools Rev 19.0.1" is installed, check it for deletion
      4. If Tools : "Android SDK Build Tools Rev 19" is installed, check it for deletion
      5. Click "delete packages"
      6. Click Tools : "Android SDK Build Tools Rev 18.1.1"
      7. Click "Install packages"
      8. Follow the steps to take the license and install the package
```

<b>Android:Drawables</b> - A list of the built-in Android drawables - http://androiddrawableexplorer.appspot.com/

<b>Unity-VS</b> - http://code.google.com/p/vs-android/

#### Docs

FAQ: Why didn't FindClass find my class? - http://developer.android.com/training/articles/perf-jni.html#faq_FindClass

JNI Local Reference Changes in ICS - http://android-developers.blogspot.com/2011/11/jni-local-reference-changes-in-ics.html

Java Programming Tutorial Java Native Interface (JNI) - http://www3.ntu.edu.sg/home/ehchua/programming/java/JavaNativeInterface.html

Visual Studio Project Properties - http://docs.nvidia.com/tegra/index.html#Nsight_Tegra_Projects.html%3FTocPath%3DTegra%20Android%20Documentation%7CHow%20to%20Develop%20Tegra%20Android%20NDK%20Applications%20Using...%7CNsight%20Tegra%2C%20Visual%20Studio%20Edition%7C_____2

#### Including JARS

Within the Visual Studio project settings you'll find Configuration Properties->Ant Build->Additional Dependencies.

Here you can add:

```
JAR Directories: libs
```

and

```
JAR Dependencies: gson-2.2.2.jar;ouya-sdk.jar
```

### Environment Variables:

####Windows

SdkRootPath=C:\NVPACK\android-sdk-windows

NdkRootPath=C:\NVPACK\android-ndk-r8e

JdkRootPath=C:\NVPACK\jdk1.6.0_24

AntRootPath=C:\NVPACK\apache-ant-1.8.2

NVSamplesPath=C:\NVPACK\TDK_Samples\tegra_android_native_samples_v10p12

####NVidia Samples

The NVidia samples have an additional include directory.

```
$(NVSamplesPath)/libs/jni
```

The NVidia samples have an additional library directory.

```
$(NVSamplesPath)/libs/jni/nv_obj/$(Configuration)
```
