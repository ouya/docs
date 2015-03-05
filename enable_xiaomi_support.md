# Enabling Xiaomi Support

While being included in the Xiaomi market is currently an invite-only process, making your app Xiaomi-Enabled is a relatively simple affair. To ensure your app/game is ready for Xiaomi, be sure to update to the latest OUYA-Everywhere  plugin and publish to the OUYA Store targeting the OUYA. After passing review, your app/game is ready for Xiaomi. 

## Xiaomi Target Hardware

* [MiBox(Enhanced)](http://dev.xiaomi.com/doc/p=3838/index.html) [[XiaomiShop]](http://www.xiaomishop.com/116-original-mi-box-proenhanced-3rd-quad-core-smart-tv-4k-hd-box.html)

## Update to the latest ODK

Download latest ODK from the OUYA [Developer portal](http://devs.ouya.tv) and setup your game to use it.

Engine specific details:

* [Java](java.md#releases)
* [Unity](ouya-everywhere-unity/ouya-everywhere-unity.md#releases)

## Review the OUYA Everywhere Documentation ##

* Be sure to review the [OUYA Everywhere Documentation](ouya-everywhere.md) for your engine.
The next initialization steps may integrate differently for your particular engine.

Engine specific details:

* [OUYA Everywhere on Construct 2](construct_2.md#ouya-everywhere)
* [OUYA Everywhere on Corona](corona.md#ouya-everywhere)
* [OUYA Everywhere on HTML5](html5.md#ouya-everywhere)
* [OUYA Everywhere on Java](java.md#ouya-everywhere)
* [OUYA Everywhere on Marmalade](marmalade.md#ouya-everywhere)
* [OUYA Everywhere on MonoGame](ouya-everywhere-monogame/ouya-everywhere-monogame.md#ouya-everywhere)
* [OUYA Everywhere on Unity](ouya-everywhere-unity/ouya-everywhere-unity.md#ouya-everywhere)
* [OUYA Everywhere on Unreal](unreal.md#ouya-everywhere)

## Xiaomi Libraries

* Download and extract [SDK_MIBOX_2.0.1](https://ouya-sdks.s3.amazonaws.com/xiaomi/SDK_MIBOX_2.0.1.zip)
* Place **SDK_MIBOX_2.0.1.jar** with the libraries of your game
* Place **MiGameCenterSDKService.apk** in your <game>/assets directory.

Engine specific details:

* [Java](java.md#xiaomi-libraries)
* [Unity](ouya-everywhere-unity/ouya-everywhere-unity.md#xiaomi-libraries)

## Xiaomi required permissions

Xiaomi's SDK requires several additional permissions in `AndroidManifest.xml` in order to work.
```java
	<uses-permission android:name="com.xiaomi.sdk.permission.PAYMENT"/>
    <uses-permission android:name="android.permission.GET_TASKS"/>
    <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
```

Engine specific details:

* [Java](java.md#xiaomi-required-permissions)
* [Unity](ouya-everywhere-unity/ouya-everywhere-unity.md#xiaomi-required-permissions)

## Receipt Checking ##

* If your game has in-app-purchases, be sure to check for receipts when the application starts.
This is important to verify entitlement purchases for premium games and demo games with paid unlock.

## Xiaomi Initialization

The Xiaomi market requires a special application key and ID.  Contact the [OUYA team](mailto:xiaomisupport@ouya.tv) to get these values.

Additionally, non-OUYA markets require being pre-informed about all possible product IDs that might be used during the game's run.  This is so the SDK can do any transforms that are specific to the market that the game is connecting to.

The following initialization strings must be set.

* `tv.ouya.developer_id` - The developer UUID available after signing into the [developer portal](http://devs.ouya.tv)
 
* `signing key` - The signing key is created when a game entry is entered in the developer portal. Each game entry in the games list has a signing key available for download.
		
* `tv.ouya.xiaomi_app_id` - The Xiaomi Application Identifier

* `tv.ouya.xiaomi_app_key` - The Xiaomi Application Key

* `tv.ouya.product_id_list` - The list of entitlements available for purchase within the app/game

Engine specific details:

* [Java](java.md#xiaomi-initialization)
* [Unity](ouya-everywhere-unity/ouya-everywhere-unity.md#xiaomi-initialization)

## Disable Xiaomi Screensaver

* The Xiaomi screensaver should be disabled while your game is running.

Engine specific details:

* [Java](java.md#disable-xiaomi-screensaver)
* [Unity](ouya-everywhere-unity/ouya-everywhere-unity.md#disable-xiaomi-screensaver)

## Create a Xiaomi-specific icon

The application icon on Xiaomi is a little unusual.  It will exist in the xhdpi resolution, but it is not a standard Android icon.

* **res/drawable-xhdpi**:
	* File must be named "ouya_xiaomi_icon.png"
    * 284x160, 32-bit
    * 15px rounded corners 
    * Hero element
    	* Size 284x110
    * Game name
    	* Bottom 50px
    	* Background should be a solid color
    	* No actual text - will be inserted by system at runtime.
    * NOTE: Do NOT update your AndroidManifest.xml to reference ouya_xiaomi_icon - you only need to place the asset in the correct location.
    * See [this example](res/game_tile_alt.png)

	![this example](res/game_tile_alt.png)

Engine specific details:

* [Java](java.md#create-a-xiaomi-specific-icon)
* [Unity](ouya-everywhere-unity/ouya-everywhere-unity.md#create-a-xiaomi-specific-icon)

## Xiaomi requires a .psd image.

* Xiaomi also requires a 800x800 image of the protagonist character(s), or something showing what the game is about.

* Must be in PSD format.

* Should be sent directly to the [OUYA team](mailto:xiaomisupport@ouya.tv)

## Localization Resources

Xiaomi requires that the app/game has been localized for `Simplified Chinese`. 

Engine specific details:

* [Java](java.md#localization-resources)
* [Unity](ouya-everywhere-unity/ouya-everywhere-unity.md#localization-resources) 

## Submission

Once the above changes have been made, your updated APK should be submitted via the OUYA Developer portal.  You'll then need to email OUYA at [xiaomisupport@ouya.tv](mailto:xiaomisupport@ouya.tv) to let us know it's ready.  Once you've done that, we'll take a snapshot of the in-app-purchase products that you have and send them to Xiaomi.  Please make sure that you've created any necessary in-app-purchase products **BEFORE** telling us your submission is ready.  This email is also when you should send OUYA your protagonist PSD file.

## TL;DR
	* Update to the latest ODK.
	* Add files to your game project - Xiaomi SDK and Xiaomi's game service apk.
	* Add required permissions to your manifest.
	* Update OuyaFacade's initialization bundle.
	* Create new icon per Xiaomi's requirements
	* Create 800x800 PSD of protagonist.
	* Submit updated apk to OUYA's dev portal
	* Email OUYA to let us know it's ready; include your protagonist PSD.

## That's it!

Wasn't that easy?  :)

## Questions?

If it actually wasn't that easy and you have questions, please don't hesitate to email us at [xiaomisupport@ouya.tv](mailto:xiaomisupport@ouya.tv).
