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

#### config.lua

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

#### main.lua

The starting point for your Corona game.

Subscribe to axis events with a callback to receive axis input events.

```
-- Add the axis event listener.
Runtime:addEventListener( "axis", onAxisEvent )
```

Subscribe to key events with a callback to receive key input events.

```
-- Add the key event listener.
Runtime:addEventListener( "key", onKeyEvent )
```

#### Virtual Controller

Open the Corona Simulator at the path - ouya-sdk-examples/Corona/VirtualController/

The key event callback gives you access to the event.

```
-- Called when a key event has been received.
local function onKeyEvent( event )

end
```

The event device descriptor tells you which controller the input came from.

```
if (tostring(event.device.descriptor) == "Joystick 1") then
```

The system button can be detected by the key name. The phase indicates whether the button is up or down.
```
    --System Button / Pause Menu
    if (event.keyName == "menu" and event.phase == "up") then
        print ("menu button detected")
    end
```

For the OUYA controller, the dpads can be detected by their key names. "down", "left", "right", "up".
```
    if (event.keyName == "down") then
    if (event.keyName == "left") then
    if (event.keyName == "right") then
    if (event.keyName == "up") then
```

For the OUYA controller, the left stick and right stick buttons can be detected by key names.
```
 if (event.keyName == "leftJoystickButton") then
 if (event.keyName == "rightJoystickButton") then
```

For the OUYA controler, the "O", "U", "Y", "A" butons can be detected by key names, also.

```
 if (event.keyName == "buttonA") then -- O button
 if (event.keyName == "buttonX") then -- U button
 if (event.keyName == "buttonY") then -- Y button
 if (event.keyName == "buttonB") then -- A button
```

For the OUYA controller, the left and right bumpers are detected by key names.
```
if (event.keyName == "leftShoulderButton1") then -- Left bumper
if (event.keyName == "rightShoulderButton1") then -- right bumper
```

For the OUYA controller, the left and right triggers are detected by key name.
```
if (event.keyName == "leftShoulderButton2") then -- left trigger
if (event.keyName == "rightShoulderButton2") then -- right trigger
```

The axis event callback gives you access to the event.

```
-- Called when an axis event has been received.
local function onAxisEvent( event )

end
```

The event device descriptor tells you which controller the input came from.

```
if (tostring(event.device.descriptor) == "Joystick 1") then
```

Access values can be accessed via the normalizeValue field.

```
local valAxis = event.normalizedValue;
```

The corresponding axis can be determined by number for the OUYA Controller.

```
if (event.axis.number == 1) then -- LX
if (event.axis.number == 2) then -- LY
if (event.axis.number == 4) then -- RX
if (event.axis.number == 5) then -- RY
if (event.axis.number == 3) then -- Left Trigger
if (event.axis.number == 6) then -- Right Trigger
```

### Corona Enterprise

The following examples apply to the Corona Enterprise version.

As with Corona Simulator projects the Corona subfolder can be opened and tested in Corona Simulator to validate the Lua script part of the project.
```
ouya-sdk-examples/Corona/InAppPurchases/Corona/
```

The android portion of the project cannot be compiled in the Corona Simulator and is compiled in the terminal or command line.
```
ouya-sdk-examples/Corona/InAppPurchases/android/
```

#### AndroidManifest.xml

The standard Android configuration that can be customized for Corona. Here you can customize the CoronaApplication, CoronaActivity, and intent filters.

#### build_easy.sh

This is a convenience shell script that clears the generated and binary files. The script invokes the build script and installs the build to the connected android device.

#### install_easy.sh

This is a convenience shell script that installs the build android package.

#### debug.keystore

Final builds should use a custom keystore. The debug keystore can be used for testing builds.

#### local.properties

Local properties defines the Android SDK path and the keystore settings for the build.

```
sdk.dir=~/android/android-sdk-macosx
key.alias=androiddebugkey
key.store=debug.keystore
key.store.password=android
key.alias.password=android
```

