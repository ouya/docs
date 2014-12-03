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

* First you need an [Unreal subscription](http://unrealengine.com) ($19/month + 5%)

* Link your Github account with your Unreal account per the [wiki instructions](https://wiki.unrealengine.com/GitHub_Setup#GitHub_Account)

* Only after linking your Github account will you have access to the [UE4 Source](https://github.com/EpicGames/UnrealEngine).

## Resources

* Get the [Unreal Engine](https://www.unrealengine.com/what-is-unreal-engine-4) - Unreal Engine and Source, All for $19 per month + 5% of Revenue

* [Unreal Forum Post](https://forums.unrealengine.com/showthread.php?51910-Getting-Started-Android-Development-References-Documentation-Feedback) - Unreal - Tracking OUYA Plugin process and requests for info

* [UE4 Video Playlist](https://wiki.unrealengine.com/Category:Epic_Video_Playlists) - Unreal - Epic Video Playlists

* [UE4 Programming Playlist](https://wiki.unrealengine.com/Introduction_to_UE4_Programming_Playlist) - Unreal - Introduction to UE4 Programming Playlist

* [Android Quick Start](https://docs.unrealengine.com/latest/INT/Platforms/Android/GettingStarted/index.html) - Unreal - Documentation steps for getting started with Unreal

* [Android Reference Guide](https://docs.unrealengine.com/latest/INT/Platforms/Android/Reference/index.html) - Unreal - Environment variable setup for Android publishing

* [Blueprints](https://docs.unrealengine.com/latest/INT/Engine/Blueprints/UserGuide/Types/ClassBlueprint/index.html) - Unreal - Visual Scripting

* [Introduction to Paper2D](https://docs.unrealengine.com/latest/INT/Videos/Player/index.html?series=PLZlv_N0_O1gauJh60307mE_67jqK42twB&video=w5I3ljBJKa0) - Unreal - Intro to 2D sprites

## Build UE4

* Download the [4.5-OUYA branch](https://github.com/ouya/UnrealEngine/tree/4.5-OUYA) to get the `OUYA fork` of the `Unreal Engine`

```
git clone -b 4.5-OUYA https://github.com/ouya/UnrealEngine
Cloning into 'UnrealEngine'...
remote: Counting objects: 243979, done.
remote: Compressing objects: 100% (53/53), done.
remote: Total 243979 (delta 8), reused 13 (delta 6)
Receiving objects: 100% (243979/243979), 291.48 MiB | 2.50 MiB/s, done.
Resolving deltas: 100% (172998/172998), done.
Checking connectivity... done.
Checking out files: 100% (26995/26995), done.
```

* Download `Required_1of2.zip` and `Required_2of2.zip` from the [4.5.X Releases](https://github.com/EpicGames/UnrealEngine/releases) and unpack in the `UnrealEngine` checkout folder.

![Unzip here](unreal/image_1.png)

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

![Check Flow](unreal/image_57.png)

The `BP_Controller` class blueprint provides a custom event named `Update Controller Sprite` which takes sprite parameter references in order to toggle visibility. The custom event first sets the parameters in variables for cleaner flow organization.

![Check Flow](unreal/image_55.png)

The `Ouya Get Button` event is used to get the current state of each controller button.

![Check Flow](unreal/image_51.png)

The menu button detection uses `Ouya Get Button Down` to catch the pressed event and then uses a `Timer Delegate` to clear the highlighted `Menu Button` after a second.

![Check Flow](unreal/image_52.png)

The `Ouya Get Axis` event is used to get the axis value for a given controller axis.

![Check Flow](unreal/image_58.png)

For the `Left Stick` and `Right Stick`, the input is rotated to match the camera angle. The axis sprites are also moved in the rotated direction using the axis input.

![Check Flow](unreal/image_59.png)

The `level blueprint` passes sprite actor references from the scene to the class blueprint. The `OuyaSDK` and `OuyaController` are also passed to the `Update Controller Sprite` custom event.  

![Check Flow](unreal/image_53.png)

The `Scene Outliner` shows all the `Sprite Actor` objects that make up a controller in a subfolder. The highlighted buttons and axis sprites are hidden by default. The left and right stick sprites are marked as `Movable` in the details tab.

![Check Flow](unreal/image_54.png)

The `level blueprint` shows mapping all the `Scene Outliner` sprites to the `Update Controller Sprite` custom event.

![Check Flow](unreal/image_56.png)
