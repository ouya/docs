## Corona Engine

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/Corona

### Developer Support Hangouts

2013:
[<a href="http://www.youtube.com/watch?v=zWYVYmk6luc" target="_blank">Jul 15</a>]&nbsp;

## Guide

### Examples

Examples are included at the base GIT path.

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/blob/master/Corona/VirtualController"><b>Virtual Controller</b></a> - Maps OUYA controllers to multiple virtual controllers

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
