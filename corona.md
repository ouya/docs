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

<table border=1>
<tr>
 <td>Corona SDK on OUYA Intro (5:01)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=_Jd8lG-A4-U" target="_blank">
<img src="http://img.youtube.com/vi/_Jd8lG-A4-U/0.jpg" alt="Corona SDK on OUYA Intro" width="240" height="180" border="10" /></a></td>
 <td>Corona Script Editors (6:05)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=iIg_c1qCPs4" target="_blank">
<img src="http://img.youtube.com/vi/iIg_c1qCPs4/0.jpg" alt="Corona Script Editors" width="240" height="180" border="10" /></a></td>
</tr>
<tr>
 <td>Corona SDK Virtual Controller (19:11)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=DmlL9dGNebc" target="_blank">
<img src="http://img.youtube.com/vi/DmlL9dGNebc/0.jpg" alt="Corona SDK Virtual Controller" width="240" height="180" border="10" /></a></td>
 <td>Corona Simulator Builds (3:05)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=hGPdeoPcirU" target="_blank">
<img src="http://img.youtube.com/vi/hGPdeoPcirU/0.jpg" alt="Corona Simulator Builds" width="240" height="180" border="10" /></a></td>
</tr>
<tr>
 <td>Quick Corona Input Setup (3:00)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=qk5sS25ebxI" target="_blank">
<img src="http://img.youtube.com/vi/qk5sS25ebxI/0.jpg" alt="Quick Corona Input Setup" width="240" height="180" border="10" /></a></td>
 <td>Corona Enterprise Quick IAP Setup (4:29)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=mrgYvagw1EE" target="_blank">
<img src="http://img.youtube.com/vi/mrgYvagw1EE/0.jpg" alt="Corona Enterprise Quick IAP Setup" width="240" height="180" border="10" /></a></td>
</tr></table>

### Examples

Examples are included at the base GIT path.

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/blob/master/Corona/VirtualController"><b>Virtual Controller</b></a> - Maps OUYA controllers to multiple virtual controllers

####Corona Enterprise Examples

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Corona/InAppPurchases"><b>InAppPurchases</b></a> - Adds Java/LUA hooks for in-app-purchases in Corona Enterprise

### Resources

Corona SDK - http://coronalabs.com/products/corona-sdk/<br/>
Corona SDK Daily Builds - http://developer.coronalabs.com/downloads/daily-builds<br/>
Corona Enterpise Daily Builds - http://developer.coronalabs.com/downloads/enterprise-daily-builds<br/>
Corona Resources - http://coronalabs.com/resources/<br/>
Mastering Corona SDK - http://masteringcoronasdk.com/<br/>

### Building

#### Mac

The Corona Enterprise SDK goes into the subfolder of your Mac's Application folder

Corona Enterprise includes sample applications:
```
Applications/CoronaEnterprise/Samples/
```

I had to edit the build script to add paths to the ant includes:<br/>
```
Applications/CoronaEnterprise/Corona/android/lib/Corona/build.xml
```

<pre>
&lt;property file="${user.dir}/local.properties" /&gt;
&lt;property file="${user.dir}/ant.properties" /&gt;
&lt;import file="${user.dir}/custom_rules.xml" optional="true" /&gt;
</pre>

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

<img src="http://d3j5vwomefv46c.cloudfront.net/photos/large/801589767.png?1376936189"/>

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

#### main.lua

Main creates the virtual controller sprites.
```
globals.controllers =
{
	ui.createController(1, 150, 500, 2, 2),
	ui.createController(2, 150, 1200, 2, 2),
	ui.createController(3, 850, 500, 2, 2),
	ui.createController(4, 850, 1200, 2, 2)
};
```

And the axis and button events are registered and handled by the inputs.lua script.
```
-- Add the key event listener.
Runtime:addEventListener( "key", inputs.onKeyEvent )

-- Add the axis event listener.
Runtime:addEventListener( "axis", inputs.onAxisEvent )
```

#### globals.lua

Globals defines the set of global controller objects handles in other scripts.

#### helpers.lua

These are some helper lua scripts for creating and fading sprites.

#### inputs.lua

The axis and button events pass through to the controller sprites to highlight button presses and move the virtual axises.

#### ui.lua

This script handles creation of the virtual controller sprites and loads the texture sprite objects.

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

If you update the Android SDK path and generate local.properties be sure to readd the properties above.
```
android update project --path .
```

### In-App-Purchases

<img src="http://d3j5vwomefv46c.cloudfront.net/photos/large/801590366.png?1376936440"/>

#### libs/ouya-sdk.jar

The OUYA ODK library used by Corona.

#### libs/gson-2.2.2.jar

The Google JSON encoding library.

#### src/.../CoronaApplication.java

Contained within CoronaRuntimeEventHandler defines Lua fuction interfaces that allow Corona Lua scripts to invoke Java methods.

#### src/.../CoronaOuyaActivity.java

The activity is used to load the signing key resource and initialize the CoronaOuyaPlugin for in-app-purchases.

#### src/.../AsyncLuaOuyaFetchGamerUUID.java

This provides the Lua interface to Java to fetch the gamer uuid. 

#### src/.../AsyncLuaOuyaRequestProducts.java

This provides the Lua interface to Java to request the product details. 

#### src/.../AsyncLuaOuyaRequestPurchase.java

This provides the Lua interface to Java to request a purchase. 

#### src/.../AsyncLuaOuyaRequestReceipts.java

This provides the Lua interface to Java to request receipts. 

#### src/.../AsyncLuaOuyaSetDeveloperId.java

This provides the Lua interface to Java to set the developer id used by in-app-purchases. 

#### src/.../Callbacks*.java

The callback methods extract the arguments from the Lua calling method and returning the results to Lua after the service is invoked. Each callback implements onSuccess, onFailure, and onCancel methods.

```
CallbacksFetchGamerUUID.java
CallbacksRequestProducts.java
CallbacksRequestPurchase.java
CallbacksRequestPurchase.java
CallbacksRequestReceipts.java
```

#### src/.../CoronaOuyaFacade.java

The Corona Ouya Facade is an interface between the callbacks above and the OuyaFacade which directly interacts with the in-app-purchase service. The "Async" methods pass the LuaState to the "Callbacks" and then invoke the "CoronaOuyaPlugin" which invokes the "OuyaFacade". The "CoronaOuyaFacade" services complete and invoke the "Callbacks" and the results are returned to Lua.

#### src/.../CoronaOuyaPlugin.java

The Corona Ouya Plugin wraps invoking methods on the OuyaFacade. The CoronaOuyaPlugin provides the Java listeners for the OuyaFacade callbacks.

#### src/.../IOuyaActivity.java

This is an interface that holds static references and accessors for easy access between the SDK classes.

#### Corona/callbacks*.lua

When the CoronaOuyaFacade listener hits the onSuccess, onFailure, or onCancel callback the corresponding Lua callback is called.
```
Corona/callbacksFetchGamerUUID.lua
Corona/callbacksRequestProducts.lua
Corona/callbacksRequestPurchase.lua
Corona/callbacksRequestReceipts.lua
```

#### Corona/globals.lua

Store all the Lua script globals in one place.

#### Corona/helpers.lua

These are some helper lua scripts for creating buttons and fading sprites.

#### Corona/inputs.lua

This lua script provides the input handlers and navigation through the main menu. When the main buttons are clicked this script invokes the Java interfaces and supplies the Lua callbacks for the onSuccess, onFailure, and onCancel callbacks.

#### Corona/main.lua

Be sure to set your developer id from the developer portal.

```
local DEVELOPER_ID = "310a8f51-4d6e-4ae5-bda0-b93878e5f5d0";
```

The main script sets up the basic button and text layout of the application. The key input listener is also registered here.

#### Corona/ui.lua

The UI script handles displaying the products and receipts using the response data from the in-app-purchase service.
