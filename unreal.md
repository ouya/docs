# Unreal Engine

## Forums #

* [Unreal on OUYA Forums](http://forums.ouya.tv/categories/unreal-on-ouya)

* [UnrealEngine Forums](https://forums.unrealengine.com/forum.php)

## Getting Started #

<table border=1>
 <tr>
 <td>UE4 on OUYA (28:49)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=lK7qGgeuI74" target="_blank">
<img src="http://img.youtube.com/vi/lK7qGgeuI74/0.jpg" alt="OUYA Support for UE4 with Tappy Chicken" width="240" height="180" border="10" /></a></td>
 <td>Customize Tappy Chicken (2:50)<br/>
<a href="http://www.youtube.com/watch?feature=player_embedded&v=ALWtpYGSomE" target="_blank">
<img src="http://img.youtube.com/vi/ALWtpYGSomE/0.jpg" alt="Customize Tappy Chicken" width="240" height="180" border="10" /></a></td>
 </tr>
</table>

## Quick Start

* First you need an [Unreal](http://unrealengine.com) account

* Link your Github account with your Unreal account per the [wiki instructions](https://wiki.unrealengine.com/GitHub_Setup#GitHub_Account)

* Only after linking your Github account will you have access to the [UE4 Source](https://github.com/EpicGames/UnrealEngine).

* After linking your Github account, you should have access to the OUYA Branches

* [4.8-OUYA branch](https://github.com/tgraupmann/UnrealEngine/tree/4.8-OUYA) for the OUYA Fork of UE4

* [4.7-OUYA branch](https://github.com/tgraupmann/UnrealEngine/tree/4.7-OUYA) for the OUYA Fork of UE4

* [4.6-OUYA branch](https://github.com/tgraupmann/UnrealEngine/tree/4.6-OUYA) for the OUYA Fork of UE4

* Quick Link: [Building the OUYA Fork of UE4](#build-ue4)

* Quick Link: [Getting Started in the UE4 Editor](#ue4-editor)

* Quick Link: [Deploying to the OUYA](#deployment)

* Quick Link: [Tappy Chicken Example](#tappy-chicken)

* Quick Link: [Virtual Controller Example](#virtual-controller)

* Quick Link: [In-App-Purchase Example](#in-app-purchases)

### FAQ

* If you run and the screen is just all black or weird it could be a few issues:

1. Did you compile for `Development Editor/Win64`, `Development Client/Android`, and run `copy_client_for_game.cmd`?

2. Did you disable Mobile HDR lighting?

3. Did you add a `Camera` to your scene and set a default `Level`?

4. Did you install the specific version of the Tegra Android Development Pack? 
A specific version of TADP needs to be installed which is found in the `Engine\Extras\Android\` folder.

![TADP installer](unreal/image_138.png)

## Resources

* Get the [Unreal Engine](https://www.unrealengine.com/what-is-unreal-engine-4) - Unreal Engine and Source, All for 5% of Revenue after more than $3k gross revenue

* [Unreal Forum Post](https://forums.unrealengine.com/showthread.php?51910-Getting-Started-Android-Development-References-Documentation-Feedback) - Unreal - Tracking OUYA Plugin process and requests for info

* [UE4 Video Playlist](https://wiki.unrealengine.com/Category:Epic_Video_Playlists) - Unreal - Epic Video Playlists

* [UE4 Programming Playlist](https://wiki.unrealengine.com/Introduction_to_UE4_Programming_Playlist) - Unreal - Introduction to UE4 Programming Playlist

* [Android Quick Start](https://docs.unrealengine.com/latest/INT/Platforms/Android/GettingStarted/index.html) - Unreal - Documentation steps for getting started with Unreal

* [Android Reference Guide](https://docs.unrealengine.com/latest/INT/Platforms/Android/Reference/index.html) - Unreal - Environment variable setup for Android publishing

* [Blueprints](https://docs.unrealengine.com/latest/INT/Engine/Blueprints/UserGuide/Types/ClassBlueprint/index.html) - Unreal - Visual Scripting

* [Introduction to Paper2D](https://docs.unrealengine.com/latest/INT/Videos/Player/index.html?series=PLZlv_N0_O1gauJh60307mE_67jqK42twB&video=w5I3ljBJKa0) - Unreal - Intro to 2D sprites

* [Unreal Chat](http://webchat.freenode.net/?channels=%23unrealengine) - #UnrealEngine on Freenode

* [C# for Unreal Engine](http://mono-ue.github.io/) - Write gameplay, AI, UI elements, and more  with C#

* [Zeef Resource Page](http://unreal-engine-4.zeef.com) - Tons of Unreal resource links

* [Master Unreal Blueprints](https://forums.unrealengine.com/showthread.php?10221-BOOK-Blueprints-Master-the-Art-of-Unreal-Engine-4-Blueprints) - Books and other resources for mastering Blueprints

* [Syncing Remote Fork](https://help.github.com/articles/syncing-a-fork/) - As EpicGames releases branch updates, the fork needs to be synced

### Training and Jams

* [Training Streams](https://forums.unrealengine.com/showthread.php?53457-Twitch-NEW!-Training-Stream-Spline-and-Spline-Mesh-Components-Dec-9-2014) happen Tuesdays for Q&A training sessions.

* Weekly [Unreal Twitch Stream](http://www.twitch.tv/unrealengine)

* [Game Jams](https://forums.unrealengine.com/showthread.php?53417-DECEMBER-GAME-JAM-Theme-Announced-on-Dec-11th-Twitch-Stream!&highlight=game+jam) - Epic hosts monthly game jams.

* [Answer Hub](https://answers.unrealengine.com/index.html) is the best way to get your questions about UE4 answered. 

### Documentation

* Coding Documentation: [Delegates](https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/Delegates/index.html)

* Coding Documentation: [Functions](https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/Reference/Functions/index.html)

* Coding Documentation: [Properties/Core Data Types](https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/Reference/Properties/index.html)

* [Artificial Intelligence](https://docs.unrealengine.com/latest/INT/Gameplay/AI/index.html) - AI Overview and Behavior Tree Quick Start Guide

* [JNI Spec](http://docs.oracle.com/javase/6/docs/technotes/guides/jni/spec/functions.html) - UE4 to Android communication uses the JNI specification

* [BLUI](https://forums.unrealengine.com/showthread.php?58192-PLUGIN-BLUI-Open-Source-HTML5-JS-CSS-HUD-UI-Release-1-0!) - BLUI is an open-source HTML renderer to make fancy UIs by embedding a browser into UE4    

## Build UE4

* Download the [4.6-OUYA branch](https://github.com/tgraupmann/UnrealEngine/tree/4.6-OUYA) to get the `OUYA fork` of the `Unreal Engine`

```
git clone -b 4.6-OUYA https://github.com/tgraupmann/UnrealEngine
Cloning into 'UnrealEngine'...
remote: Counting objects: 243979, done.
remote: Compressing objects: 100% (53/53), done.
remote: Total 243979 (delta 8), reused 13 (delta 6)
Receiving objects: 100% (243979/243979), 291.48 MiB | 2.50 MiB/s, done.
Resolving deltas: 100% (172998/172998), done.
Checking connectivity... done.
Checking out files: 100% (26995/26995), done.
```

* *Important*: The 4.6 release introduces a new system for downloading binary dependencies - just run the 'Setup' script in the root of your UE folder to get started. See the [README](https://github.com/EpicGames/UnrealEngine/blob/4.6.0-release/README.md) or [forum post](https://forums.unrealengine.com/showthread.php?51998-GitHub-dependency-zips-and-latest-preview-snapshots) for more information.

![Generate project files](unreal/image_1.png)

* Follow the [Build the Engine](https://wiki.unrealengine.com/GitHub_Setup#Buld_the_Engine) steps to generate the Visual Studio solution.
 
![Generate project files](unreal/image_2.png)

* Open the `UE4.sln` in Visual Studio

![Open Visual Studio](unreal/image_3.png)

* Add `Solution Configurations` and `Solutions Platforms` to the Visual Studio Toolbar to easily target the `UE4` build platforms. 

![Options for toolbar](unreal/image_4.png)

* Build the `Development Editor` on the `Win64` platform which will build the `UnrealEngine\Engine\Binaries\Win64\UE4Editor.exe` editor application.

![Build UE4](unreal/image_5.png)

* Build the `Development Client` on the `Android` platform which will build the `UnrealEngine\Engine\Binaries\Android\UE4Client-armv7-es2.so` native library.

![Build Android](unreal/image_6.png)

* Run the script `UnrealEngine\Engine\Binaries\Android\copy_client_for_game.cmd` to copy the native library as the `UE4Game-armv7-es2.so` dependency to use when building BluePrint projects.

![Copy dependency](unreal/image_7.png) 

* Launch the `UE4` editor from `UnrealEngine\Engine\Binaries\Win64\UE4Editor.exe`.

![Launch editor](unreal/image_8.png)

# `UE4 Editor`

* Create a `New Project` as a `Blueprint` project in the `Unreal Project Browser` to publish to `Android` the fastest. Select `No Starter Content` to reduce the file size. Enter a location of an empty folder to place the project and give it a name. Click `Create Project`.

![New Project](unreal/image_9.png)

* Use the `File->New Level` menu item to create a new level.

![New Project](unreal/image_39.png)

* Choose an `Empty Level` to start fresh.

![New Project](unreal/image_40.png)

* Use the `Object Browser` and search for `OUYA` to add the `OuyaController` and `OuyaSDK` actors to the level.

![Object Browser](unreal/image_41.png)

* Use the `File->Save As` menu item to save the level.

![Object Browser](unreal/image_42.png)

* Enter a name for the level and click `Save`.

![Object Browser](unreal/image_43.png)

* Open the `Level Blueprint`.

![Level Blueprint](unreal/image_11.png)

* With `OuyaSDK` selected in the `Scene Outliner`, `Right-Click` to add a reference in the `Level Blueprint`.

![Add OuyaSDK](unreal/image_12.png)

* With `OuyaController` selected in the `Scene Outliner`, `Right-Click` to add a reference in the `Level Blueprint`.

![Add OuyaController](unreal/image_13.png)

* Click the `Compile` button to update the latest `Blueprint` changes after adding the `OuyaSDK` and `OuyaController` object references to the `Level Blueprint`.

![Compile Blueprint](unreal/image_14.png)

* `Right-Click` on the `Event Graph` to add an `Event Tick` to the `Level Blueprint`. The event adds an update event to the flow. 

![Event Tick](unreal/image_15.png)

* `Right-Click` on the `Event Graph` while the `OuyaSDK` object in the `Scene Outliner` is selected to add `Ouya Get Any Button Down` to the `Level Blueprint`. The event checks if any controller has a `pressed` event for the `button` parameter. 

## OUYA-Everywhere Input

![Any Button Down](unreal/image_16.png)

* `Right-Click` on the `Event Graph` while the `OuyaController` object in the `Scene Outliner` is selected to add `Get BUTTON O` to the `Level Blueprint`. The event gets the `KeyCode` for the `BUTTON_O` on the OUYA Controller. 

![Button KeyCode](unreal/image_17.png)

* `Right-Click` on the `Event Graph` while the `OuyaSDK` object in the `Scene Outliner` is selected to add `Ouya Clear Button States` to the `Level Blueprint`. The event clears any detected `pressed` and `released` states so the next `Update Tick` can detect the next events.

![Clear Button States](unreal/image_18.png)

* Click the `Compile` button to update the latest `Blueprint` changes after adding a set of events that will detect a `pressed` event for the given `button` for any `OuyaController`.

![Compile Blueprint](unreal/image_19.png)

* Click the `Play` button to verify the flow is functioning properly to troubleshoot any issues.

![Check Flow](unreal/image_20.png)

## Deployment

* Before building for `Android` check your `Project Settings` in the `Unreal Editor`.

![Project Settings](unreal/image_22.png)

* Check `Use OBB in APK` in the `Packaging` settings to output a single `APK`.

![Use OBB](unreal/image_48.png)

* Uncheck `Mobile HDR` in the `Rendering` settings.

![Mobile HDR](unreal/image_47.png)

* Be sure to select the default level by clicking the `Game Default Map` drop down and selecting your default level in the `Maps & Modes` settings page.

![Maps & Modes](unreal/image_49.png)

* Click `Android` in the `Platforms` section.  You may need to click `Configure Now` to configure the project for the `Android` platform.

![Configure Now](unreal/image_44.png)

* Set the `Orientation` to `Landscape` for the TV.

![Orientation](unreal/image_24.png)

* Click the `Open Manifest Folder` button to customize the manifest.

![Open Manifest](unreal/image_25.png)

* Be sure to check `Package game data inside .apk?` which was added in the `4.7 update`. 

![Open Manifest](unreal/image_136.png)

* The Android settings now auto-generate the `AndroidManifest.xml` in the `4.7 update`.

![Open Manifest](unreal/image_137.png)

* Edit the `AndroidManifest.xml` in a `Text-Editor`.

![intent-filter](unreal/image_26.png)

* Add the `intent-filter` so the game will appear in the `Play` section in the `OUYA Launcher`.

<pre>
&lt;category android:name="tv.ouya.intent.category.GAME" /&gt;
</pre>

![Intent Filter](unreal/image_27.png)

* Build for `Tegra 3` devices using the `File->Package Project->Android->Android (DXT)` menu item.

![DXT](unreal/image_28.png)

* Browse for an empty folder or use the previous path to output the `APK` from the build process.

![APK](unreal/image_29.png)

* Click `Show Output Log` to watch for any packaging errors that may occur while building the `APK`.

![Output Log](unreal/image_30.png)

* A `Blueprint` only project should build within a few minutes versus a `Code` project which will take much longer.

![Blueprint](unreal/image_31.png)

* Run the `Install_ProjectName_Development.bat` script to install to the connected `OUYA`.

![Install](unreal/image_32.png)

* Generally the install takes 1 second per MB and prints `Success` when the install has completed.

![Success](unreal/image_33.png)

## Examples

### Tappy Chicken

`Tappy Chicken` is a complete example project in the `Unreal Launcher`. The complete project can be installed within the `MarketPlace` in the `Complete Projects` section.

* Double-Click the `BP_MainGame` blueprint to open the `Event Graph` of the `Class Blueprint`.
 
![MainGame BluePrint](unreal/image_37.png) 

* Add a `Custom Event` named `OUYA_PLAY` that simulates clicking on the `PLAY` button at the start of the game. 

![Play Button](unreal/image_34.png)

* Add a `Custom Event` named `OUYA_TOUCH` that simulates "tapping anywhere" at the start of the game. 

![Tap Anywhere](unreal/image_35.png)

* Add a `Custom Event` named `OUYA_FLAP` that simulates flapping the chicken during gameplay.

![Flap Chicken](unreal/image_36.png)

* Compile the blueprint changes.

* Open the `Level Blueprint`.

![Level Blueprint](unreal/image_38.png)

The following event flow adds the custom events needed to play `Tappy Chicken` on the OUYA. If the `BUTTON_O` pressed event is detected on `Any` OUYA Controller then the custom events will be invoked for `OUYA_PLAY`, `OUYA_TOUCH`, and `OUYA_FLAP`. `OUYA Clear Button States` clears the detected pressed and released events so they can be detected in the next update frame.

![Check Flow](unreal/image_21.png)

* Compile the blueprint changes.

* Backup the changes with the `File->Save All` menu item.

### Virtual Controller

The [Virtual Controller](https://github.com/ouya/ouya-sdk-examples/tree/master/Unreal/VirtualController) example shows 4 images of the OUYA Controller which moves axises and highlights buttons when the physical controller is manipulated.

![Screenshot](unreal/image_57.png)

The `Level Blueprint` has a `Setup Camera` step that sets the `Camera Actor` as the view target since for this example the camera will remain in a fixed position.

![Camera Setup](unreal/image_62.png)

The `BP_Controller` class blueprint provides a custom event named `Update Controller Sprite` which takes sprite parameter references in order to toggle visibility. The custom event first sets the parameters in variables for cleaner flow organization.

![Custom Event](unreal/image_55.png)

The `Ouya Get Button` event is used to get the current state of each controller button.

![Get Button](unreal/image_51.png)

The menu button detection uses `Ouya Get Button Down` to catch the pressed event and then uses a `Timer Delegate` to clear the highlighted `Menu Button` after a second.

![Clear States](unreal/image_52.png)

One issue with the timer is that we need to pass which menu sprite should be hidden and delegate timers don't have parameters.

![Clear States](unreal/image_60.png)

Since we can't pass a delegate parameter, we use an array to store the menu sprite references to clear sprite visibility after the timer completes. Before calling the timer, we add the sprite actor reference to the array. When the delegate fires, all the sprite actor references are hidden and then the array is cleared.

![Clear States](unreal/image_61.png)

The `Ouya Get Axis` event is used to get the axis value for a given controller axis.

![Get Axis](unreal/image_58.png)

For the `Left Stick` and `Right Stick`, the input is rotated to match the camera angle. The axis sprites are also moved in the rotated direction using the axis input.

![Rotate Input](unreal/image_59.png)

The `level blueprint` passes sprite actor references from the scene to the class blueprint. The `OuyaSDK` and `OuyaController` are also passed to the `Update Controller Sprite` custom event.  

![Level Blueprint](unreal/image_53.png)

The `Scene Outliner` shows all the `Sprite Actor` objects that make up a controller in a subfolder. The highlighted buttons and axis sprites are hidden by default. The left and right stick sprites are marked as `Movable` in the details tab.

![Scene Outliner](unreal/image_54.png)

The `level blueprint` shows mapping all the `Scene Outliner` sprites to the `Update Controller Sprite` custom event.

![Level Blueprint](unreal/image_56.png)

### In-App-Purchases

The [In-App-Purchases](https://github.com/ouya/ouya-sdk-examples/tree/master/Unreal/InAppPurchases) example shows making purchases, checking receipts, adjusting the safe area, and exiting the app.

![Screenshot](unreal/image_63.png)

The `IAP` example exposes the request purchase dialog.

![Screenshot](unreal/image_70.png)

The `OuyaSDK` provides methods to access In-App-Purchases:

* AddInitOuyaPluginValues - Use to set the `Developer Id`

* InitOuyaPlugin - Initialize the `OuyaSDK` to invoke IAP calls

* RequestGamerInfo - Get the gamer's `username` and `uuid`

* RequestProducts - Get the `Product` details
 
* RequestPurchase - Purchase a `Product`

* RequestReceipts - Verify the gamer has purchased the application

* SetSafeArea - Adjust the safe area to control the border order

* Shutdown - Shutdown/Exit the application

![Screenshot](unreal/image_64.png)

* Delegates for `onSuccess`, `onFailure`, and `onCancel` parameters are created by using `Custom Events`. The red box on the top left of a `Custom Event` will connect to a `Delegate` parameter.

![Screenshot](unreal/image_72.png)    

## Add Init Ouya Plugin Values

* Invoking `Add Init Ouya Plugin Values` has 2 delegates for `onSuccess` and `onFailure`.
`Add Init Ouya Plugin Values` takes two String inputs for `Key` and `Value`.
`Key` accepts `tv.ouya.developer_id` with the `Value` being your `developer id` from the [developer portal](http://devs.ouya.tv).

![Screenshot](unreal/image_67.png)

* Be sure to set your `DEVELOPER_ID` from the [Developer Portal](http://devs.ouya.tv).

![Screenshot](unreal/image_65.png)

* Upon success or failure, the `Add Init Ouya Plugin Values` callbacks will be invoked.
`onSuccess` provides no arguments. `onFailure` receives an integer `ErrorCode` and string `Error Message` about the failure.
Upon success, the `InitOuyaPlugin` can be invoked.

![Screenshot](unreal/image_71.png)

## Xiaomi Initialization

[Back to general info](enable_xiaomi_support.md#xiaomi-initialization)

`addInitOuyaPluginValues` supports additional strings to make the game compatible with OUYA Everywhere devices.

* `tv.ouya.developer_id` - The developer UUID can be found in the [developer portal](http://devs.ouya.tv) after logging in.

* `com.xiaomi.app_id` - The Xiaomi App Id is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `com.xiaomi.app_key` - The Xiaomi App Key is provided by the content team, email `officehours@ouya.tv` to obtain your key.

* `tv.ouya.product_id_list` - The product id list is a comma separated list of product ids that can be purchased in the game.

![image alt text](unreal/image_135.png)

## Init Ouya Plugin

* `Init Ouya Plugin` has 2 delegates for `onSuccess` and `onFailure`.
Be sure to invoke `Add Init Ouya Plugin Values` successfully before invoking `Init Ouya Plugin`.

![Screenshot](unreal/image_73.png)

* Upon success or failure, the `Init Ouya Plugin` callbacks will be invoked.
`onSuccess` provides no arguments. `onFailure` receives an integer `ErrorCode` and string `Error Message` about the failure.
Upon success, the other `OuyaSDK` methods can be invoked.

![Screenshot](unreal/image_74.png)

## Request Gamer Info

* `Request Gamer Info` has 3 delegates for `onSuccess`, `onFailure`, and `onCancel`.

![Screenshot](unreal/image_75.png)

* Upon success, failure, or cancel, the `Request Gamer Info` callbacks will be invoked.
`onSuccess` provides a `Gamer Info` result object. `onFailure` receives an integer `ErrorCode` and string `Error Message` about the failure. `onCancel` receives no arguments.

![Screenshot](unreal/image_76.png)

* The `Gamer Info` object has `Username` and `Uuid` fields that can be accessed.

![Screenshot](unreal/image_77.png)

## Request Products

* `Request Products` has 3 delegates for `onSuccess`, `onFailure`, and `onCancel`.

![Screenshot](unreal/image_66.png)

* Before invoking `Request Products` be sure to create a `string array` of `product identifiers`.

![Screenshot](unreal/image_78.png)

* Upon success, failure, or cancel, the `Request Products` callbacks will be invoked.
`onSuccess` provides an `Ouya Product` result array. `onFailure` receives an integer `ErrorCode` and string `Error Message` about the failure. `onCancel` receives no arguments.

![Screenshot](unreal/image_79.png)

* The example iterates through the `Ouya Product` array to get the details for each `Ouya Product` object.

![Screenshot](unreal/image_80.png)

* Several `Ouya Product` fields are available. The example uses a highlight mechanism to select one of the returned `Ouya Product` object's `identifier` for the `Request Purchase` button.

![Screenshot](unreal/image_81.png)

## Request Purchase

* `Request Purchase` has 3 delegates for `onSuccess`, `onFailure`, and `onCancel`.

![Screenshot](unreal/image_82.png)

* The example uses the `Result Products` array variable which is `set` in the `onSuccessRequestProducts` callback. Since the `purchasable` parameter for `Request Purchase` takes a `string` argument, you can hardcode the value, pass a string, or use an `array element` like the example.

![Screenshot](unreal/image_84.png)

* Upon success, failure, or cancel, the `Request Purchase` callbacks will be invoked.
`onSuccess` provides an `Ouya Purchase Result` result object. `onFailure` receives an integer `ErrorCode` and string `Error Message` about the failure. `onCancel` receives no arguments.

![Screenshot](unreal/image_83.png)

## Request Receipts

* `Request Receipts` has 3 delegates for `onSuccess`, `onFailure`, and `onCancel`.

![Request Receipts](unreal/image_85.png)

* Upon success, or failure, or cancel, the `Request Receipts` callbacks will be invoked.
`onSuccess` provides an `Ouya Receipt` result array. `onFailure` receives an integer `ErrorCode` and string `Error Message` about the failure. `onCancel` receives no arguments.

![Receipt callbacks](unreal/image_86.png)

* The example iterates through the `Ouya Receipt` array to get the details for each `Ouya Receipt` object.

![Display Receipts](unreal/image_87.png)

* Several `Ouya Receipt` fields are available including the `identifier` which games can check for if a `entitlement` was purchased.

![Display Receipt](unreal/image_88.png)

## Shutdown

* `Shutdown` has 2 delegates for `onSuccess` and `onFailure`.

![Shutdown](unreal/image_89.png)

* Upon success or failure, the `Shutdown` callbacks will be invoked.

![Shutdown](unreal/image_90.png)

# Community Content

The [Community Content](https://github.com/ouya/ouya-sdk-examples/tree/master/Unreal/CommunityContent) example shows how to interact with the Community Content API from blueprints.

## Success Callbacks

* The examples use a `Status` text field to display the current status.
The `setTextStatus` custom event is reused as a helper to display the status. 

![Success callback](unreal/image_97.png)

## Failure Callbacks

* Most failure callbacks have an `errorCode` and `errorMessage` which are displayed in status text field for the examples. The `setErrorTextStatus` custom event can be reused to simplify the failure callbacks.

![Failure callback](unreal/image_96.png)

## Get OUYA Content

* Before interacting with the Community Content API, get a reference to the `OUYA Content` actor.

* Upon success, or failure the `Get OUYA Content` callbacks will be invoked.

* `onSuccess` receives a reference to the `OuyaContent` actor.

* `onFailure` receives an `errorCode` and `errorMessage` details about the failure.

![Get OUYA Content](unreal/image_92.png)

## Initialize OUYA Content

* `Init` has 2 delegates for `onContentInitialized` and `onContentDestroyed`.
The `onContentInitialized` delegate will be called with `OuyaContent` has been initialized.
The `onContentDestroyed` delegate will be called with `OuyaContent` has been destroyed.
* `OuyaContent` should be initialized before invoking other `Community Content` methods.

![Initialize OuyaContent](unreal/image_95.png)

## Create OUYA Mod

* `CreateOuyaMod` creates a local `Community Content` record which you can use for editing and publishing.

* Upon success or failure the `CreateOuyaMod` callbacks will be invoked.

* `onSuccess` receives a reference to the `OuyaMod` actor.

* `onFailure` receives an `errorCode` and `errorMessage` details about the failure.

![Create OuyaMod](unreal/image_93.png)

## Delete OUYA Mod

* Upon success, or failure, the `Delete` callbacks will be invoked.

* `onSuccess` receives the `Ouya Mod` object that was deleted.

* `onFailure` receives an `Ouya Mod` object, an integer `ErrorCode` and string `Error Message` about the failure.

![Delete](unreal/image_125.png)

## Download OUYA Mod

* Upon download complete, download progress, or download failure, the `Download` callbacks will be invoked.

* `onComplete` receives the `Ouya Mod` object that was downloaded.

* `onProgress` receives the `Ouya Mod` object that is downloading and an `integer` progress.

* `onFailure` receives the `Ouya Mod` object that failed to download.

![Download](unreal/image_126.png)

## Edit OUYA Mod

* Upon success, or failure, the `Edit OuyaMod` callbacks will be invoked.

* `onSuccess` receives a reference to the `OuyaModEditor` and `OuyaMod` actors.

* `onFailure` receives the associated `OuyaMod` actor, an `errorCode` and `errorMessage` details about the failure.

![Edit OuyaMod](unreal/image_98.png)

## Flag OUYA Mod

* The `Flag` function will open the dialog to `Flag` the content item for review.

![Flag OuyaMod](unreal/image_94.png)

## Get Category

* `Get Category` on the `OuyaMod` actor gets the `string` category field.

![Get Category](unreal/image_99.png)

## Get Description

* `Get Description` on the `OuyaMod` actor gets the `string` description field.

![Get Description](unreal/image_100.png)

## Get Filenames

* `Get Filenames` on the `OuyaMod` actor gets an array of filename `string` objects.

![Get Filenames](unreal/image_101.png)

## Get Meta Data

* `Get Meta Data` on the `OuyaMod` actor gets the `string` meta data field.

![Get MetaData](unreal/image_102.png)

## Get Installed OUYA Content

* Upon success, or error, the `Get Installed OUYA Content` callbacks will be invoked.

* `onSuccess` receives a reference to an array of `OuyaMod` actors, and the `Integer` count of installed items.

* `onError` receives an `errorCode` and `errorMessage` details about the failure.

![Installed Content](unreal/image_127.png)

## Get Published OUYA Content

* Upon success, or error, the `Get Published OUYA Content` callbacks will be invoked.

* `onSuccess` receives a reference to an array of `OuyaMod` actors, and the `Integer` count of published items.

* `onError` receives an `errorCode` and `errorMessage` details about the failure.

![Published Content](unreal/image_128.png)

## Get Rating Average

* `Get Rating Average` on the `OuyaMod` actor gets the `float` rating average field.

![Get RatingAverage](unreal/image_103.png)

## Get Rating Count

* `Get Rating Count` on the `OuyaMod` actor gets the `integer` rating count field.

![Get RatingCount](unreal/image_104.png)

## Get Screenshots

* Upon success, or failure, the `Get Screenshots` callbacks will be invoked.

* `onSuccess` provides an `Ouya Mod` object and `Ouya Mod Screenshot` result array.

* `onFailure` receives an `Ouya Mod` object, an integer `ErrorCode` and string `Error Message` about the failure.

![Get Screenshots](unreal/image_91.png)

## Get Tags

* `Get Tags` on the `OuyaMod` actor gets an array of tag `string` objects.

![Get Tags](unreal/image_105.png)

## Get Text File

* `Get Text File` on the `OuyaMod` actor passes a `FString` *filename* argument and returns a `FString` of the file contents.

![Get Text File](unreal/image_132.png)

## Get Title

* `Get Title` on the `OuyaMod` actor gets the `string` title field.

![Get Title](unreal/image_106.png)

## Get User Rating

* `Get User Rating` on the `OuyaMod` actor gets the `float` user rating field.

![Get UserRating](unreal/image_107.png)

## Is Downloading

* `Is Downloading` on the `OuyaMod` actor gets the `boolean` is downloading field.

![Is Downloading](unreal/image_108.png)

## Is Flagged

* `Is Flagged` on the `OuyaMod` actor gets the `boolean` is flagged field.

![Is Flagged](unreal/image_109.png)

## Is Installed

* `Is Installed` on the `OuyaMod` actor gets the `boolean` is installed field.

![Is Installed](unreal/image_110.png)

## Is Published

* `Is Published` on the `OuyaMod` actor gets the `boolean` is published field.

![Is Published](unreal/image_111.png)

## Rate OUYA Mod

* The `Rate` function will open the dialog to `Rate` the content item by the user.

![Rate OuyaMod](unreal/image_112.png)

## OUYA Mod Editor Add Screenshot

* The `Add Screenshot` function on `OuyaModEditor` will add the `UTexture2D` to the `OuyaMod` actor being edited.

![Add Screenshot](unreal/image_130.png)

## OUYA Mod Editor Add Tag

* The `Add Tag` function on `OuyaModEditor` will add the `string` tag to the associated `OuyaMod` actor being edited.

![Edit AddTag](unreal/image_113.png)

## OUYA Mod Editor Delete Filename

* The `Delete Filename` function on `OuyaModEditor` will delete `string` filename associated `OuyaMod` actor being edited.

![Delete Filename](unreal/image_114.png)

## OUYA Mod Editor New Text File

* The `New Text File` function on `OuyaModEditor` will create a `string` filename associated `OuyaMod` actor being edited and the contents of the file are passed as a `string` .

![New Text File](unreal/image_115.png)

## OUYA Mod Editor Remove Screenshot

* The `Remove Screenshot` function on `OuyaModEditor` will remove the `OuyaModScreenshot` from the `OuyaMod` actor being edited.

![Remove Screenshot](unreal/image_116.png)

## OUYA Mod Editor Remove Tag

* The `Remove Tag` function on `OuyaModEditor` will remove the `string` tag from the `OuyaMod` actor being edited.

![Remove Tag](unreal/image_117.png)

## OUYA Mod Editor Save

* The `Save` function on `OuyaModEditor` will save the associated `OuyaMod` actor being edited.

![Save OuyaMod](unreal/image_122.png)

## OUYA Mod Editor Set Category

* The `Set Category` function on `OuyaModEditor` will set the `string` category on the `OuyaMod` actor being edited.

![Set Category](unreal/image_118.png)

## OUYA Mod Editor Set Description

* The `Set Description` function on `OuyaModEditor` will set the `string` description on the `OuyaMod` actor being edited.

![Set Description](unreal/image_119.png)

## OUYA Mod Editor Set Meta Data

* The `Set Meta Data` function on `OuyaModEditor` will set the `string` meta data on the `OuyaMod` actor being edited.

![Set MetaData](unreal/image_120.png)

## OUYA Mod Editor Set Title

* The `Set Title` function on `OuyaModEditor` will set the `string` title on the `OuyaMod` actor being edited.

![Set Title](unreal/image_121.png)

## Publish OUYA Mod

* Upon success, or failure, the `Publish` callbacks will be invoked.

* `onSuccess` receives the `Ouya Mod` object that was published.

* `onFailure` receives an `Ouya Mod` object, an integer `ErrorCode` and string `Error Message` about the failure.

![Publish](unreal/image_123.png)

## Unpublish OUYA Mod

* Upon success, or failure, the `Unpublish` callbacks will be invoked.

* `onSuccess` receives the `Ouya Mod` object that was unpublished.

* `onFailure` receives an `Ouya Mod` object, an integer `ErrorCode` and string `Error Message` about the failure.

![Unpublish](unreal/image_124.png)

## GetImage

* Invoking `GetImage` on the `OuyaModScreenshot` actor returns a `UTexture2D` image.

![Get Image](unreal/image_133.png)

## GetThumbnail

* Invoking `GetThumbnail` on the `OuyaModScreenshot` actor returns a `UTexture2D` image.

![Get Thumbnail](unreal/image_134.png)
