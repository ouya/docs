# Cordova

The [`Cordova`](https://cordova.apache.org/) engine provides a hardware-accelerated wrapper around HTML5.

## Resources

* [Cordova Android Platform Guide](http://cordova.apache.org/docs/en/5.0.0/guide_platforms_android_index.md.html)

* [Cordova Command-Line-Interface Guide](http://cordova.apache.org/docs/en/5.0.0/guide_cli_index.md.html#The%20Command-Line%20Interface)

* [Cordova Android Plugins](http://cordova.apache.org/docs/en/5.0.0/guide_platforms_android_plugin.md.html#Android%20Plugins)

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

## Examples

* Be sure to update to the latest version of `Android Studio`.

### Virtual Controller

The [Virtual Controller](https://github.com/ouya/ouya-sdk-examples/tree/master/Cordova/VirtualController) example shows 4 images of the OUYA Controller which moves axises and highlights buttons when the physical controller is manipulated.

![image_1.png](cordova/image_1.png)

The initial `Cordova` project was created with the command-line from the `Cordova` folder.

```
cordova create VirtualController tv.ouya.examples.cordova.virtualcontroller VirtualController
```

`Android` support is added to the `Cordova` project with the following command-line.

```
cordova platform add android
```

To build and run the `Virtual Controller Example` run the following command from the `Cordova/VirtualController` folder.

```
cordova run android
```

After running the above command, the project can be imported into `Android Studio` using the `Cordova\VirtualController\platforms\android` folder.

### In-App-Purchases

The [In-App-Purchases](https://github.com/ouya/ouya-sdk-examples/tree/master/Cordova/InAppPurchases) example shows making purchases, checking receipts, adjusting the safe area, and exiting the app.

![image_1.png](cordova/image_2.png)

The initial `Cordova` project was created with the command-line from the `Cordova` folder.

```
cordova create InAppPurchases tv.ouya.examples.cordova.inapppurchases InAppPurchases
```

`Android` support is added to the `Cordova` project with the following command-line.

```
cordova platform add android
```

To build and run the `In-App-Purchase Example` run the following command from the `Cordova/InAppPurchases` folder.

```
cordova run android
```

After running the above command, the project can be imported into `Android Studio` using the `Cordova\InAppPurchases\platforms\android` folder.
