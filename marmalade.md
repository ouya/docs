## Marmalade

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade

### Forums

@OUYA - (Marmalade on OUYA Forums) - http://forums.ouya.tv/categories/marmalade-on-ouya<br/>

## Guide

### Examples

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade/VirtualController"><b>Virtual Controller</b></a> - Maps OUYA controllers to multiple virtual controllers

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade/MarmaladeODK"><b>Marmalade ODK Extension</b></a> - OUYA Controller and In-App-Purchase extension for Marmalade

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade/InAppPurchases"><b>InAppPurchases</b></a> - In-App-Purchase uses the ODK extension

### Resources

@Marmalade - (Forums) - https://developer.madewithmarmalade.com/develop/announce-and-discuss

Marmalade - https://www.madewithmarmalade.com/downloads

Learn Marmalade - http://developer.madewithmarmalade.com/learn

Marmalade Docs - http://docs.madewithmarmalade.com/

Using Jars on Marmalade - http://docs.madewithmarmalade.com/native/extensions/edktutorials/androidlibtutorial.html

BlocSlot 2D Puzzle Game Example - http://docs.madewithmarmalade.com/native/examplesandtutorials/gameexamples/blocslot.html

### Setup

Install <a target=_blank href="https://www.madewithmarmalade.com/free-trial">[Marmalade]</a>.

Activate the Marmalade HUB by entering your license data, which also sets up your development/build environment.

Be sure to checkout the <a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Marmalade">[Marmalade ouya-sdk-examples]</a> from github.

#### Windows

Switch to your preferred Marmalade SDK version by running 's3eConfig.exe' found in the desired version 'C:\Marmalade\7.0\s3e\bin' folder.

You may need to reboot for the environment changes to take effect.

The examples can be compiled in Visual Studio.

To support compiling in Visual Studio, install the <a target=_blank href="https://developer.nvidia.com/tegra-resources">[Tegra Android Development Pack]</a> and join the <a target=_blank href="https://developer.nvidia.com/registered-developer-programs">[Tegra Registered Developer Program]</a> to get access to the download.

* Note: In Marmalade projects if you ever see the compile error 'The builds tools for "v110_wp80" cannot be found', set the 'Platform Toolset' to 'Visual Studio 2012 (v110)'.

```
Open the project properties for the configurations

 GCC ARM Release/Debug and change under
 
 Configuration Properties->General->Platform Toolset
 
 to "Visual Studio 2012 (v110)"
```

### Marmalade ODK Extension

The Marmalade ODK Extension is a native wrapper around the ODK which makes the Java library accessible to Marmalade application code.

#### Building the Marmalade Extension

Eventually the Marmalade Extension will be part of the Marmalade SDK and building will not be necessary.

To build the extension from source switch to the extension folder 'Marmalade\MarmaladeODK' in the Marmalade examples.

##### Windows

In Windows explorer, right-click 'ODK.s4e' and click 'Build Android Extension' in the context menu.

Double-click 'ODK_android_java.mkb' to build the Java source.

Double-click 'ODK_android.mkb' to build the Native source.

### In App Purchase Example

To build the example switch to the folder 'Marmalade\InAppPurchases' in the Marmalade examples.

##### Windows

Double-click 'InAppPurchases.mkb' which will open the project in Visual Studio.

In Visual Studio, set the build target to 'GCC ARM Debug' and build the project.

In Visual Studio, set the build target to 'GCC X86 ANDROID Release' and build and run which will launch the deploy tool.

The deploy tool will generate an Android package, and using adb commands you'll be able to install on the OUYA.

```
adb install -r the.apk
```

