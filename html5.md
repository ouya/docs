# HTML5 #

### Downloads

[Example Source](https://github.com/ouya/ouya-sdk-examples/tree/master/Html5)

### Forums

[Forge TV on Razer Forums](https://insider.razerzone.com/index.php?forums/razer-forge-tv.126/)

[Html5 on OUYA Forums](http://forums.ouya.tv/categories/html5-on-ouya)

## Dependencies ##

[JDK7](setup.md) for [apktool](https://code.google.com/p/android-apktool/)

[JDK6](setup.md) for repackaging

[Android SDK Setup](setup.md)

# Getting Started #

<table border=1>
 <tr>
 <td>HTML5 Controller and IAP Support (6:57)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=JnJQa7XDNAI" target="_blank">
<img src="http://img.youtube.com/vi/JnJQa7XDNAI/0.jpg" alt="HTML5 Controller and IAP Support" width="240" height="180" border="10" /></a>
 </td>
 <td></td>
 </tr>
</table>

## Icons ##

Icons are placed in the drawable resource folders, using the same locations specified in the [content review guidelines](https://github.com/ouya/docs/blob/master/content-review-guidelines.md#user-content-icons).

## Chromium ##

[`Chromium`](chromium.md) provides hardware acceleration for WebGL on rendering and video decoding.

HTML5 apps/games can be embedded into a Chromium apk.

## Prebuild configurations ##

Rather than using a Linux build machine to create `Chromium` apks, several prebuilt configurations are available for easy customization.

Customization scripts are available for Windows and Mac. Windows scripts have the 'cmd' extension and 'sh' for Mac.

## Streaming ##

`Chromium` can point to an external URL and stream in content, however, this type of setup requires an Internet connection.

## Web Archive ##

An HTML5 website can be wrapped into a zip archive and embedded into the `Chromium` app.

The archive of the website is placed in the application as a raw resource.

When the app launches, the files within the archive are extracted to the application's data folder.

Use the [Web Archive](https://github.com/ouya/ouya-sdk-examples/tree/master/Html5/WebArchive) prebuilt configuration of `Chromium`.

### ```1_Decode``` ###

1_decode will extract the prebuilt configuration for customization.

Decode uses the `apktool`, to extract the encoded contents from the prebuilt apk.

`JDK7` must be installed for `apktool` to function.

### Customize ContentShell ###

##### `package identifier` ######

The first file to customize is the AndroidManifest.xml where the package identifier for your app must be set. The package identifier must match the game identifier that was created in the developer portal.

Location: `ContentShell/AndroidManifest.xml`

```xml
<manifest package="YOUR_PACKAGE_IDENTIFIER">...
```

The application name is specified in the string resources under the id `app_name`.

Location: `ContentShell/res/values/strings.xml`

```xml
<string name="app_name">YOUR_APP_NAME</string>
```

##### `icons` #####

All apps will need to customize the icons which have the same dimensions as specified in the [content review guidelines](https://github.com/ouya/docs/blob/master/content-review-guidelines.md#user-content-icons).

Location: `ContentShell/res/drawable/app_icon.png`

Location: `ContentShell/res/drawable-xhdpi/ouya_icon.png`

The web archive is a zip file which is renamed to be compatible as an Android resource. The web archive must be all lowercase and is renamed with the `jar` extension.

Location: `ContentShell/res/raw/web_archive.jar`

The `web_archive.jar` should be replaced with your `HTML5` application.

##### `raw_web_archive` #####

The name of the archived raw resource is specified in the string resources under the id `raw_web_archive` leaving off the file extension.

Location: `ContentShell/res/values/strings.xml`

```xml
<string name="raw_web_archive">web_archive</string>
```

##### `local_start_page` #####

The web archive will include an html start page which is specified in the string resources under the id `local_start_page`.

Location: `ContentShell/res/values/strings.xml`

```xml
<string name="local_start_page">/index.html</string>
```

##### `splash screen` #####

When the app launches, a custom splash screen can be turned on and used.

Location: `ContentShell/res/drawable/splash_screen.png`

Use of the splash screen is optional. The splash screen can be enabled which is specified in the string resources under the id `use_splash_screen`.

Location: `ContentShell/res/values/strings.xml`

```xml
<string name="use_splash_screen">true</string>
```

### ```2_Build``` ###

2_build again uses `apktool` repackages your ContentShell customization and rebuilds the APK.

### ```3_Align``` ###

3_align must use `zipalign` to rejigger package so that it can be used by the Android Package Manager.

`zipalign` needs to be in your path.

### ```4_Sign``` ###

4_sign uses the `jarsigner` to certify your game/app with your `keystore`. `You must create a keystore before publishing in the OUYA Store.` The default sign script comes with a debug keystore that will not be accepted in the final publishing process. The debug keystore is perfectly fine to use during development. The keystore is used by the auto-updater and ensures a 3rd party is not monkeying with your app data. After creating a keystore for your game, make a backup of your keystore as it must be used in all future updates of the app/game. If the keystore is ever lost, users will need to uninstall your game in order to get updates. We recommend emailing yourself a copy of the keystore. After making the keystore, customize the sign script to use your keystore, password, and alias.

`jarsigner` requires `JDK6` to package properly. You may need to customize the script to explicitly point at the `JDK6` version of `jarsigner`.

### 5. Install ###

5_install uses `adb` to install the repackaged app/game on your connected OUYA device.

`adb` needs to be in your path.

The OUYA needs to be connected via micro-usb or over the network and listed in `adb devices`.

## Examples ##

### Virtual Controller ###

The [Virtual Controller](https://github.com/ouya/ouya-sdk-examples/blob/master/Html5/VirtualController/web_archive/index.html) example shows 4 images of the OUYA Controller which moves axises and highlights buttons when the physical controller is manipulated.

![Virtual Controller](https://raw.githubusercontent.com/ouya/docs/master/html5/image_1.png)

### In-App-Purchases ###

The [In-App-Purchase](https://github.com/ouya/ouya-sdk-examples/tree/master/Html5/InAppPurchases/web_archive/index.html) example uses the ODK to access gamer info, purchasing, and receipts.

![In-App-Purchases](https://raw.githubusercontent.com/ouya/docs/master/html5/image_2.png)

# API #

## Input Events ##

Your HTML5 page can set callbacks in order to receive events from ContentShell for Axis and Button events.

### Axis Events ###

The Integer PlayerNum is zero based `(0, 1, 2, 3)` and indicates which controller had the axis event.

The Axis is the integer Axis Number.

```javascript
      var OuyaController = {
		AXIS_LS_X: 0,
		AXIS_LS_Y: 1,
		AXIS_RS_X: 11,
		AXIS_RS_Y: 14,
		AXIS_L2: 17,
		AXIS_R2: 18};
```

Val is the float value of the input for that axis.


```
function onGenericMotionEvent(playerNum, axis, val) {
   if (axis == OuyaController.AXIS_LS_X) {
   }
   if (axis == OuyaController.AXIS_LS_Y) {
   }
   if (axis == OuyaController.AXIS_RS_X) {
   }
   if (axis == OuyaController.AXIS_RS_Y) {
   }
   if (axis == OuyaController.AXIS_L2) {
   }
   if (axis == OuyaController.AXIS_R2) {
   }
}
```

### Button Events ###

The Integer PlayerNum is zero based `(0, 1, 2, 3)` and indicates which controller had the axis event.

The Integer Button is the button in the event.

```javascript
      var OuyaController = {
		BUTTON_O: 96,
		BUTTON_U: 99,
		BUTTON_Y: 100,
		BUTTON_A: 97,
		BUTTON_L1: 102,
		BUTTON_R1: 103,
		BUTTON_L3: 106,
		BUTTON_R3: 107,
		BUTTON_DPAD_UP: 19,
		BUTTON_DPAD_DOWN: 20,
		BUTTON_DPAD_RIGHT: 22,
		BUTTON_DPAD_LEFT: 21,
		BUTTON_MENU: 82};
```

The key down event indicates a button was just pressed.

```
function onKeyDown(playerNum, button) {
   if (button == OuyaController.BUTTON_O) {
   }
   if (button == OuyaController.BUTTON_U) {
   }
   if (button == OuyaController.BUTTON_Y) {
   }
   if (button == OuyaController.BUTTON_A) {
   }
   if (button == OuyaController.BUTTON_L1) {
   }
   if (button == OuyaController.BUTTON_R1) {
   }
   if (button == OuyaController.BUTTON_L3) {
   }
   if (button == OuyaController.BUTTON_R3) {
   }
   if (button == OuyaController.BUTTON_DPAD_UP) {
   }
   if (button == OuyaController.BUTTON_DPAD_DOWN) {
   }
   if (button == OuyaController.BUTTON_DPAD_RIGHT) {
   }
   if (button == OuyaController.BUTTON_DPAD_LEFT) {
   }
   if (button == OuyaController.BUTTON_MENU) {
   }
}
```

The key up event indicates a button was just released.

```
function onKeyUp(playerNum, button) {
   if (button == OuyaController.BUTTON_O) {
   }
   if (button == OuyaController.BUTTON_U) {
   }
   if (button == OuyaController.BUTTON_Y) {
   }
   if (button == OuyaController.BUTTON_A) {
   }
   if (button == OuyaController.BUTTON_L1) {
   }
   if (button == OuyaController.BUTTON_R1) {
   }
   if (button == OuyaController.BUTTON_L3) {
   }
   if (button == OuyaController.BUTTON_R3) {
   }
   if (button == OuyaController.BUTTON_DPAD_UP) {
   }
   if (button == OuyaController.BUTTON_DPAD_DOWN) {
   }
   if (button == OuyaController.BUTTON_DPAD_RIGHT) {
   }
   if (button == OuyaController.BUTTON_DPAD_LEFT) {
   }
   if (button == OuyaController.BUTTON_MENU) {
   }
}
```

### Plugin Methods ###

The plugin methods are defined on the OuyaSDK object.

```javascript
      var OuyaSDK = new Object();
```

##### InitOuyaPlugin #####

The developer id is displayed in the [developer portal](http://devs.ouya.tv) and is used to initialize the OUYA Plugin.

Avoid invoking IAP methods until after the OUYA Plugin has been initialized.

Upon success, the onSuccess callback will be invoked.

If the initialization fails, the onFailure callback will be invoked.

```javascript
      // Plugin method definition
      OuyaSDK.initOuyaPlugin = function(jsonData, onSuccess, onFailure) {
        OuyaSDK.initValues = jsonData;
        OuyaSDK.onSuccess = onSuccess;
        OuyaSDK.onFailure = onFailure;
        OuyaSDK.method = "initOuyaPlugin";
      };

      // Prepare the plugin initialization values
      var data = Array();
      data[0] =
      {
        'key': 'tv.ouya.developer_id',
        'value': '310a8f51-4d6e-4ae5-bda0-b93878e5f5d0'
      };
      var jsonData = JSON.stringify(data);

      // Invoke the plugin method
      OuyaSDK.initOuyaPlugin(jsonData, onSuccessInitOuyaPlugin, onFailureInitOuyaPlugin);
```

## Xiaomi Initialization

[Back to general info](enable_xiaomi_support.md#user-content-xiaomi-initialization)

`initOuyaPlugin` supports additional strings to make the game compatible with OUYA Everywhere devices.

* `tv.ouya.developer_id` - The developer UUID can be found in the [developer portal](http://devs.ouya.tv) after logging in.

* `com.xiaomi.app_id` - The Xiaomi App Id is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `com.xiaomi.app_key` - The Xiaomi App Key is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `tv.ouya.product_id_list` - The product id list is a comma separated list of product ids that can be purchased in the game.

```javascript
      // Plugin method definition
      OuyaSDK.initOuyaPlugin = function(jsonData, onSuccess, onFailure) {
        OuyaSDK.initValues = jsonData;
        OuyaSDK.onSuccess = onSuccess;
        OuyaSDK.onFailure = onFailure;
        OuyaSDK.method = "initOuyaPlugin";
      };

      // Prepare the plugin initialization values
      var data = Array();

      data[0] =
      {
        'key': 'tv.ouya.developer_id',
        'value': '00000000-0000-0000-0000-000000000000'
      };

      data[1] =
      {
        'key': 'com.xiaomi.app_id',
        'value': '0000000000000'
      };

      data[2] =
      {
        'key': 'com.xiaomi.app_key',
        'value': '000000000000000000'
      };

      data[3] =
      {
        'key': 'tv.ouya.product_id_list',
        'value': 'long_sword,sharp_axe,cool_level,awesome_sauce'
      };

      var jsonData = JSON.stringify(data);

      // Invoke the plugin method
      OuyaSDK.initOuyaPlugin(jsonData, onSuccessInitOuyaPlugin, onFailureInitOuyaPlugin);
```

##### RequestGamerInfo ######

The gamer info contains the username and gamer uuid.

The onSuccess callback receives the gamer info.

The onFailure callback receives an error code and error message.

The onCancel callback invokes if the request was cancelled.

```javascript
      // Plugin method definition
      OuyaSDK.requestGamerInfo = function(onSuccess, onFailure, onCancel) {
        OuyaSDK.onSuccess = onSuccess;
        OuyaSDK.onFailure = onFailure;
        OuyaSDK.onCancel = onCancel;
        OuyaSDK.method = "requestGamerInfo";
      };

      // Invoke the plugin method
      OuyaSDK.requestGamerInfo(onSuccessRequestGamerInfo, onFailureRequestGamerInfo, onCancelRequestGamerInfo);
```

##### RequestProducts #####

The product info contains the description price information.

The product parameter is passed an array of product identifiers.

The onSuccess callback receives an array of product details.

The onFailure callback receives an error code and error message.

The onCancel callback invokes if the request was cancelled.

```javascript
      // Plugin method definition
      OuyaSDK.requestProducts = function(products, onSuccess, onFailure, onCancel) {
        OuyaSDK.products = products;
        OuyaSDK.onSuccess = onSuccess;
        OuyaSDK.onFailure = onFailure;
        OuyaSDK.onCancel = onCancel;
        OuyaSDK.method = "requestProducts";
      };

      // Invoke the plugin method
      var products = Array("YOUR_PRODUCT_1", "YOUR_PRODUCT_2", "YOUR_PRODUCT3");
      OuyaSDK.requestProducts(products, onSuccessRequestProducts, onFailureRequestProducts, onCancelRequestProducts)
```

##### RequestPurchase #####

The purchable parameter is the product identifier being purchased.

The onSuccess callback indicates successful purchase.

The onFailure callback indicates the purchase failed.

The onCancel callback indicates the purchase was cancelled.

```javascript
      // Plugin method definition
      OuyaSDK.requestPurchase = function(purchasable, onSuccess, onFailure, onCancel) {
        OuyaSDK.purchasable = purchasable;
        OuyaSDK.onSuccess = onSuccess;
        OuyaSDK.onFailure = onFailure;
        OuyaSDK.onCancel = onCancel;
        OuyaSDK.method = "requestPurchase";
      };

      // Invoke the plugin method
      var purchasable = "YOUR_PRODUCT_ID";
      OuyaSDK.requestPurchase(purchasable, onSuccessRequestPurchase, onFailureRequestPurchase, onCancelRequestPurchase);
```

##### RequestReceipts #####

The onSuccess callback receives an array of receipts that the gamer purchased from the developer.

The onSuccess callback receives an error code and error message about why the request failed.

The onCancel callback indicates the request was cancelled.

```javascript
      // Plugin method definition
      OuyaSDK.requestReceipts = function(onSuccess, onFailure, onCancel) {
        OuyaSDK.onSuccess = onSuccess;
        OuyaSDK.onFailure = onFailure;
        OuyaSDK.onCancel = onCancel;
        OuyaSDK.method = "requestReceipts";
      };

      // Invoke the plugin method
      OuyaSDK.requestReceipts(onSuccessRequestReceipts, onFailureRequestReceipts, onCancelRequestReceipts);
```

##### SetSafeArea #####

The safe area is explained in detail in the [content-review-guidelines](https://github.com/ouya/docs/blob/master/content-review-guidelines.md#user-content-safe-zone).

The safe area for HTML5 apps can be adjusted with setSafeArea.

The amount parameter adds full padding with 0.0 and no padding with 1.0.

The onSuccess callback indicates the safe area was set.

The onFailure callback indicates the request to set the safe area failed.

```javascript
      // Plugin method definition
      OuyaSDK.setSafeArea = function(amount, onSuccess, onFailure) {
        OuyaSDK.safeAreaAmount = amount;
        OuyaSDK.onSuccess = onSuccess;
        OuyaSDK.onFailure = onFailure;
        OuyaSDK.method = "setSafeArea";
      }

      // Invoke the plugin method
      var safeAreaAmount = 0.0; //full border padding
      OuyaSDK.setSafeArea(safeAreaAmount, onSuccessSetSafeArea, onFailureSetSafeArea);
```

##### Shutdown #####

Shutdown provides a way to exit the HTML5 application.

The onSuccess callback indicates the shutdown is about to happen.

The onFailure callback indicates the request to shutdown failed.

```javascript
      // Plugin method definition
      OuyaSDK.shutdown = function(onSuccess, onFailure) {
        OuyaSDK.onSuccess = onSuccess;
        OuyaSDK.onFailure = onFailure;
        OuyaSDK.method = "shutdown";
      }

      // Invoke the plugin method
      OuyaSDK.shutdown(onSuccessShutdown, onFailureShutdown);
```

# Debugger

Several tools are available to enhance your debugging experience on the OUYA.

* [Google Canary](https://www.google.com/intl/en/chrome/browser/canary.html) is used to debug HTML5 apps while they run on the OUYA.

* [ADB Add-On](https://chrome.google.com/webstore/detail/adb/dpngiggdglpdnjdoaefidgiigpemgage) is a Add-On for Google Canary that enables HTML5 debugging of Chromium apps on the OUYA.
