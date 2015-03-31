## Corona Engine

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/Corona

### Forums

@OUYA - (Corona on OUYA Forums) - http://forums.ouya.tv/categories/corona-on-ouya<br/>
@Corona - (Corona Forums) - http://forums.coronalabs.com/forum/627-ouya/<br/>

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
 <td>Quick Corona IAP Setup (4:29)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=mrgYvagw1EE" target="_blank">
<img src="http://img.youtube.com/vi/mrgYvagw1EE/0.jpg" alt="Quick Corona Enterprise IAP Setup" width="240" height="180" border="10" /></a></td>
</tr>
<tr>
 <td>Corona SDK Install - Windows (1:30)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=ab-IhPCdkY8" target="_blank">
<img src="http://img.youtube.com/vi/ab-IhPCdkY8/0.jpg" alt="Corona SDK Installation - Windows" width="240" height="180" border="10" /></a></td>
 <td></td>
</tr></table>

### Examples

Examples are included at the base GIT path.

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/blob/master/Corona/VirtualController"><b>Virtual Controller</b></a> - Maps OUYA controllers to multiple virtual controllers

####Corona Enterprise Examples

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Corona/InAppPurchases"><b>InAppPurchases</b></a> - Adds Java/LUA hooks for in-app-purchases in Corona Enterprise

####Corona Pro Examples

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/blob/master/Corona/Submission/ouya/docs/ouya/index.markdown"><b>InAppPurchasesPlugin</b></a> - Uses ouya plugin for in-app-purchases for Mac and Windows (required Corona PRO)

### Resources

Corona SDK - http://coronalabs.com/products/corona-sdk/<br/>
Corona SDK Daily Builds - http://developer.coronalabs.com/downloads/daily-builds<br/>
Corona Enterpise Daily Builds - http://developer.coronalabs.com/downloads/enterprise-daily-builds<br/>
Corona Resources - http://coronalabs.com/resources/<br/>
Mastering Corona SDK - http://masteringcoronasdk.com/<br/>
Corona Editor - http://coronalabs.com/blog/2013/12/11/corona-editor-is-now-1-0/<br/>

### Building

#### Mac

The Corona Enterprise SDK goes into the subfolder of your Mac's Application folder

Corona Enterprise includes sample applications:
```
Applications/CoronaEnterprise/Samples/
```

##### Authorize

Initially when setting up Corona Enterprise, the license needs to be authorized via the command-line.

```
/Applications/CoronaEnterprise/Corona/mac/bin/CoronaBuilder.app/Contents/MacOS/CoronaBuilder authorize your_email your_password
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

### Audience
The ouya-sdk-examples are targeted towards Corona Enterprise users intending to publish to the OUYA platform.

### Supported Platforms
Building Corona Enterpise examples that use Android java requires the Mac platform.
Samples that use the [OUYA Plugin for Corona](https://github.com/ouya/ouya-sdk-examples/blob/master/Corona/Submission/ouya/docs/ouya/index.markdown) will build on Mac or Windows.

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

Subscribe to OUYA-Everywhere input events.

lua
```
if nil ~= ouyaSDK and nil ~= ouyaSDK.asyncLuaOuyaInitInput then
	ouyaSDK.asyncLuaOuyaInitInput(inputs.onGenericMotionEvent, inputs.onKeyDown, inputs.onKeyUp);
end
```


Axis events come in with the onGenericMotionEvent callback.
playerNum is the controller number 0 through 3. (Zero-based)
Axis is the axis number for the event.
Val is the floating input value for the axis.

lua
```
inputs.onGenericMotionEvent = function (playerNum, axis, val)
end
```

Button events come in with the onKeyDown and onKeyUp events.
PlayerNum is the controller number 0 through 3. (Zero-based).
Button is the button number for the event.

lua
```
inputs.onKeyDown = function (playerNum, button)
end

inputs.onKeyUp = function (playerNum, button)
end
```

Button numbers can be used to detect the menu button press.

lua
```
    if (button == OuyaController.BUTTON_MENU) then
    end
```

Use `OuyaController` buttons to deteremine which button was pressed in the event.

lua
```
	if (button == OuyaController.BUTTON_DPAD_LEFT) then
    end
```
    
#### Virtual Controller

![Virtual Controller Example](https://raw.githubusercontent.com/ouya/docs/master/corona/image_1.png)

Open the Corona Simulator at the path - ouya-sdk-examples/Corona/VirtualController/


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

![In-App-Purchase Example](https://raw.githubusercontent.com/ouya/docs/master/corona/image_2.png)

#### libs/ouya-sdk.jar

The OUYA ODK library used by Corona.

#### src/.../CoronaApplication.java

Contained within CoronaRuntimeEventHandler defines Lua fuction interfaces that allow Corona Lua scripts to invoke Java methods.

#### src/.../CoronaOuyaActivity.java

The activity is used to load the signing key resource and initialize the CoronaOuyaPlugin for in-app-purchases.

#### src/.../AsyncLuaOuyaRequestGamerInfo.java

This provides the Lua interface to Java to fetch the gamer uuid. 

#### src/.../AsyncLuaOuyaRequestProducts.java

This provides the Lua interface to Java to request the product details. 

#### src/.../AsyncLuaOuyaRequestPurchase.java

This provides the Lua interface to Java to request a purchase. 

#### src/.../AsyncLuaOuyaRequestReceipts.java

This provides the Lua interface to Java to request receipts. 

#### src/.../Callbacks*.java

The callback methods extract the arguments from the Lua calling method and returning the results to Lua after the service is invoked. Each callback implements onSuccess, onFailure, and onCancel methods.

```
CallbacksRequestGamerInfo.java
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
Corona/callbacksRequestGamerInfo.lua
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
local json = require "json"
if nil ~= ouyaSDK and nil ~= ouyaSDK.initOuyaPlugin then
	local data = {
	[1] = {
	    ["key"] = "tv.ouya.developer_id",
	    ["value"] = "310a8f51-4d6e-4ae5-bda0-b93878e5f5d0"
	}};
	local jsonData = json.encode(data);
	ouyaSDK.initOuyaPlugin(callbacksInitOuyaPlugin.onSuccess, callbacksInitOuyaPlugin.onFailure, jsonData);
end
```

The main script sets up the basic button and text layout of the application. The key input listener is also registered here.

#### Corona/ui.lua

The UI script handles displaying the products and receipts using the response data from the in-app-purchase service.

## Xiaomi Initialization

[Back to general info](enable_xiaomi_support.md#xiaomi-initialization)

`initOuyaPlugin` supports additional strings to make the game compatible with OUYA Everywhere devices.

* `tv.ouya.developer_id` - The developer UUID can be found in the [developer portal](http://devs.ouya.tv) after logging in.

* `com.xiaomi.app_id` - The Xiaomi App Id is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `com.xiaomi.app_key` - The Xiaomi App Key is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `tv.ouya.product_id_list` - The product id list is a comma separated list of product ids that can be purchased in the game.

```
local json = require "json"
if nil ~= ouyaSDK and nil ~= ouyaSDK.initOuyaPlugin then
	local data = {

	[1] = {
	    ["key"] = "tv.ouya.developer_id",
	    ["value"] = "00000000-0000-0000-0000-000000000000"
	},

	[2] = {
	    ["key"] = "com.xiaomi.app_id",
	    ["value"] = "0000000000000"
	},

	[3] = {
	    ["key"] = "com.xiaomi.app_key",
	    ["value"] = "000000000000000000"
	},

	[4] = {
	    ["key"] = "tv.ouya.product_id_list",
	    ["value"] = "long_sword,sharp_axe,cool_level,awesome_sauce"
	}};

	local jsonData = json.encode(data);
	ouyaSDK.initOuyaPlugin(callbacksInitOuyaPlugin.onSuccess, callbacksInitOuyaPlugin.onFailure, jsonData);
end
```

## Disable Xiaomi Screensaver

[Back to general info](enable_xiaomi_support.md#disable-xiaomi-screensaver)

* The screensaver should be disabled while your game is running. By using the OUYA-Corona plugin, the screensaver will automatically be disabled.

## Create a Xiaomi-specific icon

[Back to general info](enable_xiaomi_support.md#create-a-xiaomi-specific-icon)

The `Corona` [documentation](http://docs.coronalabs.com/daily/guide/distribution/buildSettings/index.html#ouya) has info about how to include `OUYA` and `Xiaomi` store icons. 

* The `Icon-ouya.png` should be 732x412 and placed into your Corona project fodler.

* The `Icon-ouya-xiaomi.png` should be 284x160 and placed into your Corona project fodler.
