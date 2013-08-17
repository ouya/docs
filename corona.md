## Corona Engine

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/Corona

### Developer Support Hangouts

2013:

<table border=1>
<tr><td>OUYA Hangout July 15th (1:00:00)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=zWYVYmk6luc" target="_blank">
<img src="http://img.youtube.com/vi/zWYVYmk6luc/0.jpg" alt="OUYA Hangout July 15th," width="240" height="180" border="10" /></a></td> 
<td></td></tr></table>

## Guide

### Examples

Examples are included at the base GIT path.

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/blob/master/Corona/VirtualController"><b>Virtual Controller</b></a> - Maps OUYA controllers to multiple virtual controllers

####Corona Enterprise Examples

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Corona/InAppPurchases"><b>InAppPurchases</b></a> - Adds Java/LUA hooks for in-app-purchases in Corona Enterprise

### Resources

Corona SDK - http://coronalabs.com/products/corona-sdk/<br/>
Corona Video Tutorials - http://coronalabs.com/resources/videos/<br/>

### Building

(Currently testing on Mac)

put the Corona Enterprise SDK into a subfolder of your Mac's Application folder

working folder:<br/>
<b><i>Applications/CoronaEnterprise/Samples/SimpleLuaExtension/android</b></i><br/>

set environment variable: ANDROID_SDK<br/>

generate local.properties:<br/>
<b><i>android update project --path .</b></i>

edit the build script to add paths to the includes:<br/>
<b><i>Applications/CoronaEnterprise/Corona/android/lib/Corona/build.xml</b></i><br/>
<pre>
&lt;property file="${user.dir}/local.properties" /&gt;
&lt;property file="${user.dir}/ant.properties" /&gt;
&lt;import file="${user.dir}/custom_rules.xml" optional="true" /&gt;
</pre>

create ant.properties<br/>
<b><i>key.alias=androiddebugkey</b></i><br/>
<b><i>key.store=debug.keystore</b></i><br/>

copy the debug keystore to the sample android folder<br/>
<b><i>cp ~/.android/debug.keystore .</b></i>

build the sample:<br/>
<b><i>./build.sh ~/android/android-sdk-macosx /Applications/CoronaEnterprise</b></i><br/>
			when prompted enter the debug keystore and alias password "android"<br/>
			
install the sample:<br/>
<b><i>adb install path/to/apk</b></i>

###Recommended Tools

Outlaw IDE - http://outlawgametools.com/outlaw-code-editor-and-project-manager/

#Full Document

### Authors
Tim Graupmann (tim@tagenigma.com)

### Audience
The ouya-sdk-examples are targeted towards Corona Enterprise users intending to publish to the OUYA platform.

### Supported Platforms
The Corona examples were created using the Mac platform.

### Introduction
Welcome to the Corona Enterprise development club. These examples provide a quick way to add OUYA controller and in-app-purchase support to your game.

### Corona Simulator

The following examples apply to the Corona Simulator version.

Build for Android/OUYA with Command+Shift+B which will produce an APK that you can install with adb commands.

```
adb install -r /path/to/Example.apk
```

Corona scripts are Lua scripts.

#### build.settings

The Corona Simulator doesn't have direct access to the Android.manifest, where build.settings gives you access to modify settings indirectly.

Add the intent filter so that your game appears in the OUYA Play Category.

```
settings =
{
   android =
   {
      mainIntentFilter =
      {
         categories = { "tv.ouya.intent.category.GAME" },
      }
   },
}
```

Set the orientation to landscape left.

```
settings =
{
	orientation = {
		default = "landscapeLeft", 
		supported = { "landscapeLeft", },
	},
}
```

#### Config.lua

Corona is designed to scale up, not down. So you want to specify 720p which will scale up to 1080p.

```
application =
{
	content =
	{
		width = 1280,
		height = 720,
		scale = "letterbox", 
	},
} 
```

#### Virtual Controller

Open the Corona Simulator at the path - ouya-sdk-examples/Corona/VirtualController/

### Corona Enterprise

The following examples apply to the Corona Enterprise version.
