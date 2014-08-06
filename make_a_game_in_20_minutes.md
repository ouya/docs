##MAKE A GAME IN 20 MINUTES

###**Things You’ll Need Before You Start:**
- OUYA Console
- Windows 7 PC
- Micro USB Cable
- TV with HDMI input
- Internet connection

Watch the video to follow along: https://vimeo.com/102651617

##**Getting Set Up**

**Install JDK**

1. Go to: http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html
1. Download JDK 6 (32bit / x86) [requires an Oracle account]
1. Install the JDK and JRE using the downloaded installer, following the onscreen prompts and keep the default settings.


**Install Android SDK**

1. Download the standalone SDK (not ADT/Studio bundle) from: http://developer.android.com/sdk/index.html
1. Run the SDK tools installer, noting the location of the SDK folder, keep the default settings again


**Set up Android SDK** 

1. Open the Android SDK Manager
1. Click “Deselect All” if you don’t want to download unnecessary tools (?)
1. Check and install: 
1. The latest Build-tools (highest revision number) 
1. Android 4.1.2 API 16 SDK Platform
1. Extras > Google USB Driver
1. Accept any terms and conditions
1. Close both windows when complete


**Setup OUYA adb**

1. Open Device Manager (Start Menu - type Device Manager)
1. Connect your OUYA console to your PC via the MICRO USB port (not the regular USB), also connect your console to your TV. Then power on the console.
1. Under OTHER DEVICES, OUYA will pop up
1. Right click OUYA > Update Driver Software…
1. Click the Browse My Computer option
1. Click the “let me pick” option
1. Select “Show All Devices” and click Next
1. Click the “Have Disk…” button
1. Browse to the folder where you installed the Android SDK and select:
<sdk>/extras/google/usb_driver/android_winusb.inf 
1. Choose OK
1. Select the “Android Composite ADB Interface” model from the list and click Next
1. If there’s a warning about compatibility, just click Yes
1. Close the windows when finished

**Download the Demo Game package**

1. Download the latest release unitypackage from here: https://github.com/ouya/unity-2d-demo/releases/latest

**Install Unity**

1. Download and install Unity following the installation prompts: http://unity3d.com/unity/download

##**Getting The Demo Running**

1. Create a new Unity Project
1. In the new project dialog, select your project folder and select “2D” in the defaults drop down
1. Once in the editor go to Assets > Import Package > Custom Package…
1. Navigate to your downloads folder and find the Demo package from github
1. Import the demo package, and when a window pops up, leave all assets selected
1. Once imported, go to File > Build Settings
1. Select Android and hit Switch Platform
1. Finally Click Build and Run to save and launch the apk
1. If prompted, browse to the Android SDK folder
1. Your game should automatically begin to play on your OUYA console that’s still plugged in to your TV


**Making Tweaks in Unity**

1. In the project tree (one of the several panes labeled with a tab), open Scenes > Level

**Changing Jump Height**

1. In the project tree, select “Assets > Prefabs > Characters > hero”
1. In the Inspector, find the PLAYER CONTROL (SCRIPT) section, and change the Jump Force field from 1000 to 1200	

**Changing Enemy HP:**

1. In project tree, select “Assets > Prefabs > Characters > enemy2”
1. In the Inspector, find the ENEMY (SCRIPT) section and change the HP field from 2 to 3

**Changing Enemy Speed:**

1. In project tree, select “Assets > Prefabs > Characters > enemy1”
1. In the Inspector, find the ENEMY (SCRIPT) section and change the Move Speed field from 6 to 8

**Changing Bus Sprite**

1. In project tree, select “Assets > Prefabs > Environment > Bus”
1. In the Inspector, find the SPRITE RENDERER section, and click the circle next to the Sprite field where it says “Bus”
1. Double click the new Bus sprite (with alien passengers) in the window that pops up

**Changing Background Image**

1. In Hierarchy tab, select “backgrounds > env_bg”
1. In the Inspector, find the SPRITE RENDERER section, and click the circle next to the Sprite field labeled env_bg
1. Double click the night time background in the window that pops up

**Changing Music**

1. In the Hierarchy tab, select “music”
1. In the Inspector, find the AUDIO SOURCE section and click the circle next to the Audio Clip field labeled MainTheme
1. Select the new music from the dialog
1. Finally, go File > Build and Run to launch your modified game.
1. The game will automatically begin to play on your OUYA console that should still be running the first version of the game.
1. Check out the differences!

**Creating APK for publishing**

1. Create your keystore
1. Go to Edit > Project Settings > Player
1. In the Android tab, expand the “Publishing Settings” section
1. Check the box for Create New Keystore
1. Click “Browse Keystore” to select the location of your keystore (DO NOT LOSE THIS FILE OR PASSWORD)
1. Choose a password and type it into the Keystore password and Confirm password text boxes
1. In the Key > Alias dropdown box, select Create a new key
1. In the new key dialog, fill out all of the fields with your information. You may choose a new password or use the same one as the keystore file.
1. Select CREATE KEY when you’re done.
1. Return to the PUBLISH SETTINGS section, and select your newly created alias from the Key -> Alias dropdown box, and type in the password you’ve chosen.

**Changing the package name**

1. Open the OUYA panel by going to Window > Open OUYA Panel
1. Change the bundle ID by selecting the Bundle Identifier text and entering your package name
1. A warning message will appear.
1. To fix this, hit the “Sync Bundle ID” button to update the package name of all your files

**Build APK**

1. Open File > Build Settings
1. Click the Build button to save your APK


**Create a Developer Profile and Upload your game**

1. Go to https://devs.ouya.tv and create or log in to your developer account
1. Go to https://devs.ouya.tv/developers/edit and fill out your Developer Profile, and Marketplace Agreement
1. If you don’t have a product, you don’t need to fill out the Payment Info or Tax Documents
1. Go to Games -> Add Game
1. Fill in the pertinent info about your game:
  1. Title
  2. Description
  3. Support Email
  4. apk file
  5. Screenshots
  6. etc
1. Save your game info
1. The game will continue to upload to the server, with a message saying “Still Verifying APK…”
1. When that is complete, a Submit For Review button will appear and you’ll be good to go.
