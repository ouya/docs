# Cordova

The [`Cordova`](https://cordova.apache.org/) engine provides a hardware-accelerated wrapper around HTML5.

## Resources

* [Cordova Android Platform Guide](http://cordova.apache.org/docs/en/5.0.0/guide_platforms_android_index.md.html)

* [Cordova Command-Line-Interface Guide](http://cordova.apache.org/docs/en/5.0.0/guide_cli_index.md.html#The%20Command-Line%20Interface)

* [Cordova Android Plugins](http://cordova.apache.org/docs/en/5.0.0/guide_platforms_android_plugin.md.html#Android%20Plugins)

## Cordova Command-Line Interface

1) Before the `Cordova` projects can be created, be sure to install the [`Cordova` Command-Line Interface](http://cordova.apache.org/docs/en/5.0.0/guide_cli_index.md.html#The%20Command-Line%20Interface).

2) Install [`Node.js`](https://nodejs.org/).

On Windows, `Node.js` can be installed from an `MSI` installer and be sure to add 'Node and npm' to the path.

![Node.js installer](cordova/image_3.png)

3) Be sure to install a `GIT` source-control client. `Cordova` will report errors if `git` is missing from the path.

![Missing git](cordova/image_5.png)

4) Open a new terminal to install `Cordova` globally.

On Mac:

```
sudo npm install -g cordova
```

On Windows:

```
npm install -g cordova
```

![Install Cordova](cordova/image_4.png)

## Examples

* Be sure to update to the latest version of `Android Studio`.

### Virtual Controller

The [Virtual Controller](https://github.com/ouya/ouya-sdk-examples/tree/master/Cordova/VirtualController) example shows 4 images of the OUYA Controller which moves axises and highlights buttons when the physical controller is manipulated.

![image_1.png](cordova/image_1.png)

1) The initial `Cordova` project was created with the command-line from the `Construct2` folder.

```
cordova create VirtualController tv.ouya.examples.cordova.virtualcontroller VirtualController
```

2) `Android` support is added to the `Cordova` project with the following command-line from the `Cordova/VirtualController` folder.

```
cordova platform add android
```

3) Use the `Cordova` command-line to add the `cordova-plugin-ouya-sdk` plugin.

```
cordova plugin add https://github.com/ouya/cordova-plugin-ouya-sdk.git#master
```

4) To build and run the `Virtual Controller Example` run the following command from the `Cordova/VirtualController` folder.

```
cordova run android
```

5) Manually copy `plugins\cordova-plugin-ouya-sdk\src\android\MainActivity.java` to `platforms\android\src\tv\ouya\examples\cordova\virtualcontroller\MainActivity.java` and edit the package name to be `tv.ouya.examples.cordova.virtualcontroller`. `Cordova` auto-configs cannot replace `XML` nodes making this manual one-off necessary.

```
package tv.ouya.examples.cordova.virtualcontroller;
```

6) The example [`HTML5` contents](https://github.com/ouya/ouya-sdk-examples/blob/master/Cordova/VirtualController/www/index.html) are placed within the `Cordova\VirtualController\www` folder so that the above command will copy the files to the `Android` platform.

7) `Cordova` projects can import into Android Studio as a `Gradle` project.

![Import Android Studio](cordova/image_6.png)

8) The example project can be imported from the `Cordova/VirtualController/platforms/android` folder.

![Import IAP](cordova/image_8.png)

### In-App-Purchases

The [In-App-Purchases](https://github.com/ouya/ouya-sdk-examples/tree/master/Cordova/InAppPurchases) example shows making purchases, checking receipts, adjusting the safe area, and exiting the app.

![image_1.png](cordova/image_2.png)


1) The initial `Cordova` project was created with the command-line from the `Cordova` folder.

```
cordova create InAppPurchases tv.ouya.examples.cordova.inapppurchases InAppPurchases
```

2) `Android` support is added to the `Cordova` project with the following command-line from the `Cordova/InAppPurchases` folder.

```
cordova platform add android
```

3) Use the `Cordova` command-line to add the `cordova-plugin-ouya-sdk` plugin.

```
cordova plugin add https://github.com/ouya/cordova-plugin-ouya-sdk.git#master
```

4) To build and run the `In-App-Purchases Example` run the following command from the `Cordova/InAppPurchases` folder.

```
cordova run android
```

5) Manually copy `plugins\cordova-plugin-ouya-sdk\src\android\MainActivity.java` to `platforms\android\src\tv\ouya\examples\cordova\inapppurchases\MainActivity.java` and edit the package name to be `tv.ouya.examples.cordova.inapppurchases`. `Cordova` auto-configs cannot replace `XML` nodes making this manual one-off necessary.

```
package tv.ouya.examples.cordova.inapppurchases;
```

6) The example [`HTML5` contents](https://github.com/ouya/ouya-sdk-examples/blob/master/Cordova/InAppPurchases/www/index.html) are placed within the `Cordova\InAppPurchases\www` folder so that the above command will copy the files to the `Android` platform.

7) `Cordova` projects can import into Android Studio as a `Gradle` project.

![Import Android Studio](cordova/image_6.png)

8) The example project can be imported from the `Cordova/InAppPurchases/platforms/android` folder.

![Import IAP](cordova/image_7.png)

# API #

## HTML5

To interact with Cordova plugins, first add `Cordova` includes in the `head`.

```
    <script type="text/javascript" src="cordova.js"></script>
    <script type="text/javascript" src="js/index.js"></script>
```

### Input

Register the `Cordova` input hooks to receive `HTML5` input.

```
	function onLoad() {

      	if (cordova.exec == undefined) {
      		onFailure(0, "Wait for plugin to load!");
			setTimeout(function(){ onLoad() }, 1000);
      		return;
      	}

		cordova.exec(
			function(jsonData) {
				var jsonObject = JSON.parse(jsonData);
				var playerNum = jsonObject.playerNum;
				var axis = jsonObject.axis;
				var val = jsonObject.val;
				//console.log("HTML5 CallbackOnGenericMotionEvent playerNum="+playerNum+" axis="+axis+" val="+val);
				onGenericMotionEvent(playerNum, axis, val);
			},
			function(err) {
				console.error("HTML5 setCallbackOnGenericMotionEvent Failed: "+err);
			},
			"OuyaSDK", "setCallbackOnGenericMotionEvent", [ "" ]);

		cordova.exec(
			function(jsonData) {
				var jsonObject = JSON.parse(jsonData);
				var playerNum = jsonObject.playerNum;
				var button = jsonObject.button;
				//console.log("HTML5 CallbackOnKeyUp playerNum="+playerNum+" button="+button);
				onKeyUp(playerNum, button);
			},
			function(err) {
				console.error("HTML5 setCallbackOnKeyUp Failed: "+err);
			},
			"OuyaSDK", "setCallbackOnKeyUp", [ "" ]);

		cordova.exec(
			function(jsonData) {
				var jsonObject = JSON.parse(jsonData);
				var playerNum = jsonObject.playerNum;
				var button = jsonObject.button;
				//console.log("HTML5 CallbackonKeyDown playerNum="+playerNum+" button="+button);
				onKeyDown(playerNum, button);
			},
			function(err) {
				console.error("HTML5 setCallbackOnKeyDown Failed: "+err);
			},
			"OuyaSDK", "setCallbackOnKeyDown", [ "" ]);
	}
```

### OnLoad

Wait for the document to load before making calls to the `Cordova` API, by using the `body` to register the `onload` callback.

```
  <body onload="onLoad()">
``` 

## Input Events ##

Your HTML5 page can set callbacks in order to receive events from `Cordova` for Axis and Button events.

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

The Integer `PlayerNum` is zero based `(0, 1, 2, 3)` and indicates which controller had the axis event.

The Integer `Button` is the button in the event.

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
      	if (cordova.exec == undefined) {
      		onFailure(0, "Wait for plugin to load!");
      		return;
      	}
		cordova.exec(
			function(status) {
				console.log("HTML5 initOuyaPlugin: SUCCESS status="+status);
				if (status == "WAIT") {
					//console.log("**********************HTML5 initOuyaPlugin: Waiting for result...");
				} else if (status == "SUCCESS") {
					onSuccess();
				} else {
					onFailure(0, "Unknown success state!");
				}
			},
			function(jsonObject) {
			    var errorCode = jsonObject.errorCode;
			    var errorMessage = jsonObject.errorMessage;
				console.error("HTML5 initOuyaPlugin: FAILURE errorCode="+errorCode+" errorMessage="+errorMessage);
				onFailure(errorCode, errorMessage);
			},
			"OuyaSDK", "initOuyaPlugin", [ jsonData ]);
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

[Back to general info](enable_xiaomi_support.md#xiaomi-initialization)

`initOuyaPlugin` supports additional strings to make the game compatible with OUYA Everywhere devices.

* `tv.ouya.developer_id` - The developer UUID can be found in the [developer portal](http://devs.ouya.tv) after logging in.

* `com.xiaomi.app_id` - The Xiaomi App Id is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `com.xiaomi.app_key` - The Xiaomi App Key is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `tv.ouya.product_id_list` - The product id list is a comma separated list of product ids that can be purchased in the game.

```javascript
      // Plugin method definition
      OuyaSDK.initOuyaPlugin = function(jsonData, onSuccess, onFailure) {
      	if (cordova.exec == undefined) {
      		onFailure(0, "Wait for plugin to load!");
      		return;
      	}
		cordova.exec(
			function(status) {
				console.log("HTML5 initOuyaPlugin: SUCCESS status="+status);
				if (status == "WAIT") {
					//console.log("**********************HTML5 initOuyaPlugin: Waiting for result...");
				} else if (status == "SUCCESS") {
					onSuccess();
				} else {
					onFailure(0, "Unknown success state!");
				}
			},
			function(jsonObject) {
			    var errorCode = jsonObject.errorCode;
			    var errorMessage = jsonObject.errorMessage;
				console.error("HTML5 initOuyaPlugin: FAILURE errorCode="+errorCode+" errorMessage="+errorMessage);
				onFailure(errorCode, errorMessage);
			},
			"OuyaSDK", "initOuyaPlugin", [ jsonData ]);
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
      	if (cordova.exec == undefined) {
      		onFailure(0, "Wait for plugin to load!");
      		return;
      	}
		cordova.exec(
			function(jsonObject) {
				//console.log("HTML5 requestGamerInfo: SUCCESS");
				onSuccess(jsonObject);
			},
			function(jsonObject) {
			    var errorCode = jsonObject.errorCode;
			    var errorMessage = jsonObject.errorMessage;
				console.error("HTML5 requestGamerInfo: FAILURE errorCode="+errorCode+" errorMessage="+errorMessage);
				onFailure(errorCode, errorMessage);
			},
			"OuyaSDK", "requestGamerInfo", [ ]);
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
      	if (cordova.exec == undefined) {
      		onFailure(0, "Wait for plugin to load!");
      		return;
      	}
		cordova.exec(
			function(jsonObject) {
				//console.log("HTML5 requestProducts: SUCCESS: products="+jsonObject.toString());
				onSuccess(jsonObject);
			},
			function(jsonObject) {
			    var errorCode = jsonObject.errorCode;
			    var errorMessage = jsonObject.errorMessage;
				console.error("HTML5 requestProducts: FAILURE errorCode="+errorCode+" errorMessage="+errorMessage);
				onFailure(errorCode, errorMessage);
			},
			"OuyaSDK", "requestProducts", [ products ]);
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
      	if (cordova.exec == undefined) {
      		onFailure(0, "Wait for plugin to load!");
      		return;
      	}
		cordova.exec(
			function(jsonObject) {
				//console.log("HTML5 requestPurchase: SUCCESS: identifier="+jsonObject.productIdentifier);
				onSuccess(jsonObject.productIdentifier);
			},
			function(jsonObject) {
			    var errorCode = jsonObject.errorCode;
			    var errorMessage = jsonObject.errorMessage;
				console.error("HTML5 requestPurchase: FAILURE errorCode="+errorCode+" errorMessage="+errorMessage);
				onFailure(errorCode, errorMessage);
			},
			"OuyaSDK", "requestPurchase", [ purchasable ]);
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
      	if (cordova.exec == undefined) {
      		onFailure(0, "Wait for plugin to load!");
      		return;
      	}
		cordova.exec(
			function(jsonArray) {
				//console.log("HTML5 requestReceipts: SUCCESS: jsonObject="+jsonArray.toString());
				onSuccess(jsonArray);
			},
			function(jsonObject) {
			    var errorCode = jsonObject.errorCode;
			    var errorMessage = jsonObject.errorMessage;
				console.error("HTML5 requestReceipts: FAILURE errorCode="+errorCode+" errorMessage="+errorMessage);
				onFailure(errorCode, errorMessage);
			},
			"OuyaSDK", "requestReceipts", [ ]);
      };

      // Invoke the plugin method
      OuyaSDK.requestReceipts(onSuccessRequestReceipts, onFailureRequestReceipts, onCancelRequestReceipts);
```

##### SetSafeArea #####

The safe area is explained in detail in the [content-review-guidelines](https://github.com/ouya/docs/blob/master/content-review-guidelines.md#safe-zone).

The safe area for HTML5 apps can be adjusted with setSafeArea.

The amount parameter adds full padding with 0.0 and no padding with 1.0.

The onSuccess callback indicates the safe area was set.

The onFailure callback indicates the request to set the safe area failed.

```javascript
      // Plugin method definition
      OuyaSDK.setSafeArea = function(amount, onSuccess, onFailure) {
      	if (cordova.exec == undefined) {
      		onFailure(0, "Wait for plugin to load!");
      		return;
      	}
		cordova.exec(
			function() {
				console.log("HTML5 setSafeArea: SUCCESS");
				onSuccess();
			},
			function(jsonObject) {
			    var errorCode = jsonObject.errorCode;
			    var errorMessage = jsonObject.errorMessage;
				console.error("HTML5 setSafeArea: FAILURE errorCode="+errorCode+" errorMessage="+errorMessage);
				onFailure(errorCode, errorMessage);
			},
			"OuyaSDK", "setSafeArea", [ amount ]);
      }

      // Invoke the plugin method
      var safeAreaAmount = 0.0; //full border padding
      OuyaSDK.setSafeArea(safeAreaAmount, onSuccessSetSafeArea, onFailureSetSafeArea);
```

##### GetDeviceHardware #####

Device identification is part of [OUYA-Everywhere](ouya-everywhere.md#device-identification). HTML5 apps can use device specific game logic and media using the name from getDeviceHardware. 

The onSuccess callback receives the name of the device hardware.

The onFailure callback indicates the request to get the device hardware failed.

```javascript
      // Plugin method definition
      OuyaSDK.getDeviceHardware = function(onSuccess, onFailure) {
      	if (cordova.exec == undefined) {
      		onFailure(0, "Wait for plugin to load!");
      		return;
      	}
		cordova.exec(
			function(jsonObject) {
				//console.log("HTML5 getDeviceHardware: SUCCESS");
				onSuccess(jsonObject.deviceHardware);
			},
			function(jsonObject) {
			    var errorCode = jsonObject.errorCode;
			    var errorMessage = jsonObject.errorMessage;
				console.error("HTML5 getDeviceHardware: FAILURE errorCode="+errorCode+" errorMessage="+errorMessage);
				onFailure(errorCode, errorMessage);
			},
			"OuyaSDK", "getDeviceHardware", [ ]);
      }

      // Invoke the plugin method
      OuyaSDK.getDeviceHardware(onSuccessGetDeviceHardware, onFailureGetDeviceHardware);
```

The onSuccess callback receives the `String` name of the device hardware. The `device name` can be used to show an alternate set of buttons, for example.

```javascript
      function onSuccessGetDeviceHardware(name) {
      	deviceHardware = name;
      	lblStatus = "received device hardware="+deviceHardware;
      	redraw = true;
      }
```

##### Shutdown #####

Shutdown provides a way to exit the HTML5 application.

The onSuccess callback indicates the shutdown is about to happen.

The onFailure callback indicates the request to shutdown failed.

```javascript
      // Plugin method definition
      OuyaSDK.shutdown = function(onSuccess, onFailure) {
      	if (cordova.exec == undefined) {
      		onFailure(0, "Wait for plugin to load!");
      		return;
      	}
		cordova.exec(
			function() {
				console.log("HTML5 shutdown: SUCCESS");
				onSuccess();
			},
			function(jsonObject) {
			    var errorCode = jsonObject.errorCode;
			    var errorMessage = jsonObject.errorMessage;
				console.error("HTML5 shutdown: FAILURE errorCode="+errorCode+" errorMessage="+errorMessage);
				onFailure(errorCode, errorMessage);
			},
			"OuyaSDK", "shutdown", [ ]);
      }

      // Invoke the plugin method
      OuyaSDK.shutdown(onSuccessShutdown, onFailureShutdown);
```
