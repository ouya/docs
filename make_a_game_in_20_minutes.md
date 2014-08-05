##MAKE A GAME IN 20 MINUTES

###**Things You’ll Need Before You Start:**
1. OUYA Console
2. Windows 7 PC
3. Micro USB Cable
4. TV with HDMI input
5. Internet connection

Watch the video to follow along: https://vimeo.com/102651617

##**Getting Set Up**

**Install JDK**
- Go to: http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html
- Download JDK 6 (32bit / x86) [requires an Oracle account]
- Install the JDK and JRE using the downloaded installer, following the onscreen prompts and keep the default settings.


**Install Android SDK**
- Download the standalone SDK (not ADT/Studio bundle) from: http://developer.android.com/sdk/index.html
- Run the SDK tools installer, noting the location of the SDK folder, keep the default settings again


**Set up Android SDK** 
- Open the Android SDK Manager
- Click “Deselect All” if you don’t want to download unnecessary tools (?)
- Check and install: 
- The latest Build-tools (highest revision number) 
- Android 4.1.2 API 16 SDK Platform
- Extras > Google USB Driver
- Accept any terms and conditions
- Close both windows when complete


**Setup OUYA adb**
- Open Device Manager (Start Menu - type Device Manager)
- Connect your OUYA console to your PC via the MICRO USB port (not the regular USB), also connect your console to your TV. Then power on the console.
- Under OTHER DEVICES, OUYA will pop up
- Right click OUYA > Update Driver Software…
- Click the Browse My Computer option
- Click the “let me pick” option
- Select “Show All Devices” and click Next
- Click the “Have Disk…” button
- Browse to the folder where you installed the Android SDK and select:
<sdk>/extras/google/usb_driver/android_winusb.inf 
- Choose OK
- Select the “Android Composite ADB Interface” model from the list and click Next
- If there’s a warning about compatibility, just click Yes
- Close the windows when finished

**Download the Demo Game package**
- Download the latest release unitypackage from here: https://github.com/ouya/unity-2d-demo/releases/

**Install Unity**
- Download and install Unity following the installation prompts: http://unity3d.com/unity/download

##**Getting The Demo Running**
- Create a new Unity Project
- In the new project dialog, select your project folder and select “2D” in the defaults drop down
- Once in the editor go to Assets > Import Package > Custom Package…
- Navigate to your downloads folder and find the Demo package from github
- Import the demo package, and when a window pops up, leave all assets selected
- Once imported, go to File > Build Settings
- Select Android and hit Switch Platform
- Finally Click Build and Run to save and launch the apk
- If prompted, browse to the Android SDK folder
- Your game should automatically begin to play on your OUYA console that’s still plugged in to your TV


**Making Tweaks in Unity**
- In the project tree (one of the several panes labeled with a tab), open Scenes > Level

**Changing Jump Height**
- In the project tree, select “Assets > Prefabs > Characters > hero”
- In the Inspector, find the PLAYER CONTROL (SCRIPT) section, and change the Jump Force field from 1000 to 1200	

**Changing Enemy HP:**
- In project tree, select “Assets > Prefabs > Characters > enemy2”
- In the Inspector, find the ENEMY (SCRIPT) section and change the HP field from 2 to 3

**Changing Enemy Speed:**
- In project tree, select “Assets > Prefabs > Characters > enemy1”
- In the Inspector, find the ENEMY (SCRIPT) section and change the Move Speed field from 6 to 8

**Changing Bus Sprite**
- In project tree, select “Assets > Prefabs > Environment > Bus”
- In the Inspector, find the SPRITE RENDERER section, and click the circle next to the Sprite field where it says “Bus”
- Double click the new Bus sprite (with alien passengers) in the window that pops up

**Changing Background Image**
- In Hierarchy tab, select “backgrounds > env_bg”
- In the Inspector, find the SPRITE RENDERER section, and click the circle next to the Sprite field labeled env_bg
- Double click the night time background in the window that pops up

**Changing Music**
- In the Hierarchy tab, select “music”
- In the Inspector, find the AUDIO SOURCE section and click the circle next to the Audio Clip field labeled MainTheme
- Select the new music from the dialog

- Finally, go File > Build and Run to launch your modified game.
- The game will automatically begin to play on your OUYA console that should still be running the first version of the game.
- Check out the differences!

**Creating APK for publishing**
- Create your keystore
- Go to Edit > Project Settings > Player
- In the Android tab, expand the “Publishing Settings” section
- Check the box for Create New Keystore
- Click “Browse Keystore” to select the location of your keystore (DO NOT LOSE THIS FILE OR PASSWORD)
- Choose a password and type it into the Keystore password and Confirm password text boxes
- In the Key > Alias dropdown box, select Create a new key
- In the new key dialog, fill out all of the fields with your information. You may choose a new password or use the same one as the keystore file.
- Select CREATE KEY when you’re done.
- Return to the PUBLISH SETTINGS section, and select your newly created alias from the Key -> Alias dropdown box, and type in the password you’ve chosen.

**Changing the package name**
- Open the OUYA panel by going to Window > Open OUYA Panel
- Change the bundle ID by selecting the Bundle Identifier text and entering your package name
- A warning message will appear.
- To fix this, hit the “Sync Bundle ID” button to update the package name of all your files

**Build APK**
- Open File > Build Settings
- Click the Build button to save your APK


**Create a Developer Profile and Upload your game**
- Go to https://devs.ouya.tv and create or log in to your developer account
- Go to https://devs.ouya.tv/developers/edit and fill out your Developer Profile, and Marketplace Agreement
- If you don’t have a product, you don’t need to fill out the Payment Info or Tax Documents
- Go to Games -> Add Game
- Fill in the pertinent info about your game:
1. Title
2. Description
3. Support Email
4. apk file
5. Screenshots
6. etc
- Save your game info
- The game will continue to upload to the server, with a message saying “Still Verifying APK…”
- When that is complete, a Submit For Review button will appear and you’ll be good to go.
