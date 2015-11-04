## Construct 2 Engine

### Downloads

[Example Source](https://github.com/ouya/ouya-sdk-examples/tree/master/Construct2)

### Forums

[Forge TV on Razer Forums](https://insider.razerzone.com/index.php?forums/razer-forge-tv.126/)

[Construct 2 on OUYA Forums](http://forums.ouya.tv/categories/construct2-on-ouya)

[Construct 2 on Scirra Forums](https://www.scirra.com/forum/)

## Getting Started

<table border=1>

 <tr>
 <td>Intro to Construct 2 (19:40)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=HXf0Gp_i9ew" target="_blank">
<img src="http://img.youtube.com/vi/HXf0Gp_i9ew/0.jpg" alt="Intro to Construct 2 on OUYA" width="240" height="180" border="10" /></a>
 </td>
 <td></td>
 </tr>
</table>

### Resources

Construct 2 - https://www.scirra.com/construct2

Supported Platforms - https://www.scirra.com/manual/168/supported-platforms

### Construct 2

Construct 2 is an visually programable engine that publishes HTML5.

## Setup ##

### Windows ###

1) Install [Construct 2](https://www.scirra.com/construct2).

2) Install your license file into the `C:\Program Files\Construct 2\` folder.

3) Install the `Construct 2` [exporter changes](https://github.com/ouya/ouya-sdk-examples/tree/master/Construct2/Program%20Files/Construct%202/exporters/) into your `Construct 2` exporters folder.

4) Restart Construct 2

## Publishing ##

Publishing to `Cortex TV` requires that you package the generated `HTML5` from `Construct 2` into an Android wrapper, [`Cordova`](cordova.md). The wrapper provides accelerated `WebGL` and accelerated video decoding.

1) Use the menu `File->Export project...` item.

![file export](construct_2/image_46.png)

2) Select `HTML5 website` and click `Next` button.

![export type](construct_2/image_41.png)

3) Specify the `HTML5` output folder and click `Next` button (this should be the `Cordova` www folder).

![export destination](construct_2/image_42.png)

4) Select `Normal style` and click the `Export` button.

![export style](construct_2/image_45.png)

5) Return to `Construct 2` or open the destination `www` folder.

![destination folder](construct_2/image_44.png)

6) Before the `Cordova` scripting can be run, be sure to install the [`Cordova` Command-Line Interface](cordova.md#cordova-command-line-interface).

7) Run the `package.cmd` script in the root of the project which runs `cordova run android`.

![package command](construct_2/image_43.png)

8) The script builds the `Android` app, installs, and runs on the connected `Android` device.

![package command](construct_2/image_47.png)  

# Setup

Before you can use the `OuyaSDK` be sure to create a game in the [developer portal](http://devs.ouya.tv) and download the signing key.

The IAP example places the signing key in `Construct2\InAppPurchases\platforms\android\assets\key.der` which gets packaged when running `ouya-sdk-examples\Construct2\InAppPurchases\package.cmd` after exporting `HTML5` to `Construct2\InAppPurchases\www`. 

## Testing

You won't be charged when making IAP purchases of your own game.
Be sure to log into `Cortex TV` with the same developer account that the game is created for in the [developer portal](http://devs.ouya.tv).
You can reverse purchases in the [purchase page](https://devs.ouya.tv/developers/products) of the [developer portal](http://devs.ouya.tv) to test refund scenarios or testing first-time purchases.
Look for the `View and remove your purchases of your products` link on the bottom of the [purchase page](https://devs.ouya.tv/developers/products) to refund purchases of your own game.

# `OuyaSDK` API

To access the `OUYA SDK`, insert an `OuyaSDK` object into your layout.
The `OuyaSDK` object has to be in the layout for the `OuyaSDK` events to fire.

Right click the `Layout` and select `Insert New Object`.

![Insert Object](construct_2/image_38.png)

Select `OuyaSDK` and click `Insert`.

![Insert](construct_2/image_3.png)

## Initialize the OUYA Plugin

Interacting with the `Cortex TV` SDK can be done via the `event sheet`.

### Start of Layout

Add an event `System\On start of layout`.

![start of layout](construct_2/image_5.png)

Add actions `addInitOuyaPluginValues` and `initOuyaPlugin` to the `On start of layout` event.

![init actions](construct_2/image_6.png)

### `Developer UUID`

The `Developer UUID` is available in the [developer portal](http://devs.ouya.tv).
You must be logged in to see your `Developer UUID`.

![init actions](construct_2/image_39.png)

### `addInitOuyaPluginValues`

`addInitOuyaPluginValues` has `key` and `value` arguments.
Add a `key` of "tv.ouya.developer_id".
Add a `value` using your `Developer UUID`.

![init actions](construct_2/image_7.png)

The `initOuyaPlugin` action will invoke initializing the OUYA Plugin given the values added with `addInitOuyaPluginValues`.

![Add Init Values](construct_2/image_4.png)

### Init Events

`Initialize OUYA Plugin on Success` will be invoked if the OUYA Plugin was initialized.

`Initialize OUYA Plugin on Failure` will be invoked if the OUYA Plugin failed to initialize.

* After the success callback, the other IAP actions can be invoked.

* After the failure callback, be sure to reinvoke the `InitOuyaPlugin` action to ensure the `OUYA` plugin successfully initializes.

![Init Plugin](construct_2/image_8.png)

Actions can be added to the init events.

![Add Event](construct_2/image_9.png)

The `onFailure` event provides error code and error message fields.

* `OuyaSDK.errorCodeOnFailureInitOuyaPlugin` - The failure `error code` as a number

* `OuyaSDK.errorMessageOnFailureInitOuyaPlugin` - The failure `error message` as a string

## Xiaomi Initialization

[Back to general info](enable_xiaomi_support.md#xiaomi-initialization)

`addInitOuyaPluginValues` supports additional strings to make the game compatible with OUYA Everywhere devices.

* `tv.ouya.developer_id` - The developer UUID can be found in the [developer portal](http://devs.ouya.tv) after logging in.

* `com.xiaomi.app_id` - The Xiaomi App Id is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `com.xiaomi.app_key` - The Xiaomi App Key is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `tv.ouya.product_id_list` - The product id list is a comma separated list of product ids that can be purchased in the game.

![image alt text](construct_2/image_40.png)

## Request Gamer Info

The gamer info includes the gamer's username and unique identifier. Add an `OuyaSDK\Request Gamer Info` action.

![Gamer Info](construct_2/image_10.png)

`Request Gamer Info` has 3 events for `on Success`, `on Failure`, and `on Cancel`.

![Request Gamer Info](construct_2/image_12.png)

The `Gamer Info` fields are available in the `onSuccess` event.

`OuyaSDK.GamerInfoUsername` returns the string for the gamer's username which can be used to display in a `Text` object. 

`OuyaSDK.GamerInfoUuid` returns the string for the gamer's unique identifier.

![GamerInfo Uuid](construct_2/image_11.png)

The `onFailure` event provides error code and error message fields.

* `OuyaSDK.errorCodeOnFailureRequestGamerInfo` - The failure `error code` as a number

* `OuyaSDK.errorMessageOnFailureRequestGamerInfo` - The failure `error message` as a string

## Request Products

Requesting products gets the details about the Product created in the [developer portal](http://devs.ouya.tv).

<pre>
{
    "currencyCode": "USD",
    "originalPrice": 2.99,
    "localPrice": 2.99,
    "description": "",
    "name": "Sharp Axe",
    "developerName": "Sample Developer",
    "identifier": "sharp_axe",
    "percentOff": 0
}
</pre>

Add the action `OuyaSDK\Request Products`.

![Request Products](construct_2/image_13.png)

`Request Products` takes a string which is a comma separated list of product ids.
You can pass a comma separated list `"a,b,c,d,e,f"` or a single product `"my_awesome_sauce"`.

![CSV List](construct_2/image_14.png)

The IAP example waits for a button press before invoking the `Request Products` action.
![Button](construct_2/image_15.png)

`Request Products` has 3 events for `on Success`, `on Failure`, and `on Cancel`.

![Request Products](construct_2/image_17.png)

`Request Products on Success` gets a list of product details.
`OuyaSDK.ProductsLength` returns the count of products returned. 

![Insert Object](construct_2/image_16.png)

Retrieving product details uses an index from 0 to (`OuyaSDK.ProductsLength` - 1).
Create a global `ProductIndex` used to get the product details.

![Insert Object](construct_2/image_20.png)

Use the `Set` action to set the `ProductIndex` to start at the beginning of the products list.

![Insert Object](construct_2/image_22.png)

Add a `Repeat` event to iterate over each of the returned products. 

![Insert Object](construct_2/image_18.png)

The count will be `OuyaSDK.ProductsLength` times.

![Insert Object](construct_2/image_19.png)

All the `OuyaSDK.GetProducts*` accessors take the `ProductIndex` to return the Product item's details.

`OuyaSDK.GetProductsIdentifier(ProductIndex)` - Returns a string of the product identifier

`OuyaSDK.GetProductsName(ProductIndex)` - Returns a string of the product name

`OuyaSDK.GetProductsDescription(ProductIndex)` - Returns a string of the product description

`OuyaSDK.GetProductsLocalPrice(ProductIndex)` returns a float for the local price of the product.

Increment the `ProductIndex` with a `Set` action after looking up the data for each product.

![Insert Object](construct_2/image_21.png)

The `onFailure` event provides error code and error message fields.

* `OuyaSDK.errorCodeOnFailureRequestProducts` - The failure `error code` as a number

* `OuyaSDK.errorMessageOnFailureRequestProducts` - The failure `error message` as a string

## Request Purchase

Requesting a purchase requires the Product entitlement or consumable was created in the [developer portal](http://devs.ouya.tv).

Add the action `OuyaSDK\Request Purchase`.

![Insert Object](construct_2/image_23.png)

`Request Purchase` has 3 events for `on Success`, `on Failure`, and `on Cancel`.

![Insert Object](construct_2/image_24.png)

The `onFailure` event provides error code and error message fields.

* `OuyaSDK.errorCodeOnFailureRequestPurchase` - The failure `error code` as a number

* `OuyaSDK.errorMessageOnFailureRequestPurchase` - The failure `error message` as a string

## Request Receipts

A receipt indicates that the gamer has purchased your entitlement.
After querying the receipts list, iterate through the items to check if your entitlement `product_id` is found to unlock the full game or feature.

<pre>

{
    "gamer": "2927b3d9-e940-077a-8f68-af923f52f5d9",
    "uuid": "be05dfcdc4eb0d50",
    "generatedDate": "Thu Jan 01 00:00:00 GMT 1970",
    "localPrice": 0.99,
    "identifier": "cool_level",
    "currency": "USD",
    "purchaseDate": "Tue Nov 11 01:36:12 GMT 2014"
}

</pre>

Add the action `OuyaSDK\Request Receipts`.

![Insert Object](construct_2/image_25.png)

`Request Receipts` has 3 events for `on Success`, `on Failure`, and `on Cancel`.

![Insert Object](construct_2/image_34.png)

`Request Receipts` on Success gets a list of receipt details. `OuyaSDK.ReceiptsLength` returns the count of receipts returned. 

![Insert Object](construct_2/image_30.png)

Retrieving receipts details uses an index from 0 to (`OuyaSDK.ReceiptsLength` - 1). Create a global ReceiptIndex used to get the receipt details.

![Insert Object](construct_2/image_35.png)

Use the `Set` action to set the ReceiptIndex to start at the beginning of the receipts list.

![Insert Object](construct_2/image_36.png)

Add a `Repeat` event to iterate over each of the returned receipts. 

![Insert Object](construct_2/image_18.png)

The count will be `OuyaSDK.ReceiptsLength` times.

![Insert Object](construct_2/image_37.png)

All the `OuyaSDK.GetReceipts*` accessors take the `ReceiptIndex` to return the Receipt item's details.

`OuyaSDK.GetReceiptsIdentifier(ReceiptIndex)` - Returns a string of the product identifier

`OuyaSDK.GetReceiptsGeneratedDate(ReceiptIndex)` - Returns a string of the generated date

`OuyaSDK.GetReceiptsLocalPrice(ReceiptIndex)` returns a float for the local price of the receipt.

Increment the `ReceiptIndex` with a `Set` action after looking up the data for each product.

![Insert Object](construct_2/image_33.png)

The `onFailure` event provides error code and error message fields.

* `OuyaSDK.errorCodeOnFailureRequestReceipts` - The failure `error code` as a number

* `OuyaSDK.errorMessageOnFailureRequestReceipts` - The failure `error message` as a string

## Set Safe Area

Add the action `OuyaSDK\Set Safe Area`.

![Insert Object](construct_2/image_26.png)

The IAP example uses a floating-point `SafeAreaAmount` global variable that adjusts the safe area amount.

![Insert Object](construct_2/image_29.png)

The `Set Safe Area` action takes a `SafeAreaAmount` floating-point number. Safe area amounts range from 0.0 with full border to 1.0 with border. 

![Insert Object](construct_2/image_28.png)

`Set Safe Area` has 2 events for `on Success`, and `on Failure`.

![Insert Object](construct_2/image_31.png)

The `onFailure` event provides error code and error message fields.

* `OuyaSDK.errorCodeOnFailureSetSafeArea` - The failure `error code` as a number

* `OuyaSDK.errorMessageOnFailureSetSafeArea` - The failure `error message` as a string

## Shutdown

Add the action `OuyaSDK\Shutdown`.

![Insert Object](construct_2/image_27.png)

`Shutdown` has 2 events for `on Success`, and `on Failure`.

![Insert Object](construct_2/image_32.png)

The `onFailure` event provides error code and error message fields.

* `OuyaSDK.errorCodeOnFailureShutdown` - The failure `error code` as a number

* `OuyaSDK.errorMessageOnFailureShutdown` - The failure `error message` as a string

# Examples

The examples are `capx` files which are complete projects that depend on installing the `OuyaSDK` Construct 2 plugin.

## `Virtual Controller` ##

The [Virtual Controller](https://github.com/ouya/ouya-sdk-examples/tree/master/Construct2/VirtualController) example shows 4 images of the OUYA Controller which moves axises and highlights buttons when the physical controller is manipulated. The Virtual Controller example includes support for [OUYA-Everywhere](ouya-everywhere.md).

![Virtual Controller Example](construct_2/image_1.png)

### Building HTML5 with Construct 2

Open the `VirtualController.capx` from the `Construct2\VirtualController` example folder.

### Virtual Controller Wrapped In Cordova

1) The initial `Cordova` project was created with the command-line from the `Construct2` folder.

```
cordova create VirtualController tv.ouya.examples.construct2.virtualcontroller VirtualController
```

2) `Android` support is added to the `Cordova` project with the following command-line from the `Construct2/VirtualController` folder.

```
cordova platform add android
```

3) Use the `Cordova` command-line to add the `cordova-plugin-ouya-sdk` plugin.

```
cordova plugin add https://github.com/ouya/cordova-plugin-ouya-sdk.git#master
```

4) To build and run the `Virtual Controller Example` run the following command from the `Construct2/VirtualController` folder.

```
cordova run android
```

5) Manually copy `plugins\cordova-plugin-ouya-sdk\src\android\MainActivity.java` to `platforms\android\src\tv\ouya\examples\construct2\virtualcontroller\MainActivity.java` and edit the package name to be `tv.ouya.examples.construct2.virtualcontroller`. `Cordova` auto-configs cannot replace `XML` nodes making this manual one-off necessary.

```
package tv.ouya.examples.construct2.virtualcontroller;
```

6) When building in `Construct 2` and exporting to `HTML5` be sure to output to the `Construct2\VirtualController\www` folder so that the above command will pick up the files.

## `In-App-Purchases` ##

The [In-App-Purchases](https://github.com/ouya/ouya-sdk-examples/tree/master/Construct2/InAppPurchases) example shows making purchases, checking receipts, adjusting the safe area, and exiting the app.

![IAP Example](construct_2/image_2.png)

### Building HTML5 with Construct 2

Open the `InAppPurchases.capx` from the `Construct2\InAppPurchases` example folder.

### In-App-Purchases Wrapped In Cordova

1) The initial `Cordova` project was created with the command-line from the `Construct2` folder.

```
cordova create InAppPurchases tv.ouya.examples.construct2.inapppurchases InAppPurchases
```

2) `Android` support is added to the `Cordova` project with the following command-line from the `Construct2/InAppPurchases` folder.

```
cordova platform add android
```

3) Use the `Cordova` command-line to add the `cordova-plugin-ouya-sdk` plugin.

```
cordova plugin add https://github.com/ouya/cordova-plugin-ouya-sdk.git#master
```

4) To build and run the `In-App-Purchases Example` run the following command from the `Construct2/InAppPurchases` folder.

```
cordova run android
```

5) Manually copy `plugins\cordova-plugin-ouya-sdk\src\android\MainActivity.java` to `platforms\android\src\tv\ouya\examples\construct2\inapppurchases\MainActivity.java` and edit the package name to be `tv.ouya.examples.construct2.inapppurchases`. `Cordova` auto-configs cannot replace `XML` nodes making this manual one-off necessary.

```
package tv.ouya.examples.construct2.inapppurchases;
```

6) When building in `Construct 2` and exporting to `HTML5` be sure to output to the `Construct2\InAppPurchases\www` folder so that the above command will pick up the files.
