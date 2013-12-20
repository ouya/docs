## Construct 2 Engine

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/Construct2

### Forums

@OUYA - (Construct 2 on OUYA Forums) - http://forums.ouya.tv/categories/construct2-on-ouya<br/>

## Guide

### Examples

<a target=_blank href="https://github.com/ouya/ouya-sdk-examples/tree/master/Construct2/VirtualController">[VirtualControllers]</a> - Maps OUYA controllers to virtual controllers.

### Resources

Construct 2 - https://www.scirra.com/construct2

Supported Platforms - https://www.scirra.com/manual/168/supported-platforms

PhoneGap - https://build.phonegap.com/

CocoonJS - http://ludei.com/

### Construct 2

Construct 2 is an visually programable engine that publishes HTML5.

Publishing to the OUYA requires that you package the generated HTML into PhoneGAP or CocoonJS.

Make your own <a target=_blank href="https://www.scirra.com/tutorials/352/plugins-roll-your-own-with-the-javascript-sdk">[JavaScript plugins]</a> with the JavaScript SDK.

#### PhoneGap

PhoneGap has a <A target=_blank href="http://docs.phonegap.com/en/edge/guide_platforms_android_plugin.md.html#Android%20Plugins">[plugin API]</a> that allows adding custom HTML tags with callbacks to interface HTML5 with Java for two-way communication.

With PhoneGap, you can customize your Android.manifest and embed your custom plugins to work with your HTML5 from Construct 2.

#### Android-Chromium-View

<a target=_blank href="https://github.com/davisford/android-chromium-view">[android-chromium-view]</a> - To achieve hardware acceleration on HTML5 you'll want an accelerated webview.

You can check hardware acceleration with <a target=_blank href="chrome://gpu">[chrome://gpu]</a>.

```
Graphics Feature Status
Canvas: Hardware accelerated
Compositing: Hardware accelerated on all pages and threaded
3D CSS: Hardware accelerated
CSS Animation: Accelerated and threaded
WebGL: Hardware accelerated
WebGL multisampling: Hardware accelerated
Flash 3D: Hardware accelerated
Flash Stage3D: Hardware accelerated
Flash Stage3D Baseline profile: Hardware accelerated
Texture Sharing: Hardware accelerated
Video Decode: 
Video: 
```

#### Android-Chromium

<a target=_blank href="https://github.com/davisford/android-chromium">[android-chromium]</a> - An even faster HTML5 accelerated WebView fork of Chromium.

##### Ubuntu

```
git clone https://github.com/davisford/android-chromium
cd android-chromium
```

Create local.properties file with a path to the Android SDK.

```
sdk.dir=../../Downloads/android-studio/sdk/
```

Be sure to check the github readme page and make sure you have the dependencies installed.

```
Tools\*
Extras\Android Support Repository
Extras\Android Support Library
Extras\Google Respository
```

Build the APK.

```
./gradlew clean
./gradlew build
./gradlew build
```

./gradlew clean && ./gradlew build && ./gradlew build

Get the compiled APKS.

```
mkdir apks
cp chromium/android-webview/build/apk/android-webview-debug-unaligned.apk apks
cp chromium/content-shell/build/apk/content-shell-debug-unaligned.apk apks
cp chromium/testshell/build/apk/testshell-debug-unaligned.apk apks
```

cp chromium/android-webview/build/apk/android-webview-debug-unaligned.apk apks && cp chromium/content-shell/build/apk/content-shell-debug-unaligned.apk apks && cp chromium/testshell/build/apk/testshell-debug-unaligned.apk apks

#### Crosswalk Project

https://crosswalk-project.org/

#### CocoonJS

##### Setup

1. Create yourself a free developer account on https://ludei.com
2. Install CocoonJS Launcher on OUYA. Go to dev portal (create a project) and click 'Compile Launcher' under COMPILATION. You'll get an email in a few mins with zip. Unzip it and `adb install` debug apk of CocoonJS Launcher.


##### Hello World

1. Create an empty project directory

2. Make index.html:
```html
<html>
<body>
 <canvas/>
 <script>
   var canvas = document.getElementsByTagName('canvas')[0];
   var ctx = canvas.getContext('2d');
   ctx.fillStyle = 'white';
   ctx.font = '6px Arial';
   ctx.fillText('Hello World', 10, 10);
 </script>
 </body>
</html>
```

3. Zip it: `zip -r hello_world.zip *`

4. Copy it to OUYA: `adb push hello_world.zip /mnt/sdcard/`

5. Now go to OUYA / MAKE / SOFTWARE and open CocoonJS Launcher

> Pro tip: Use real mouse to work in CocoonJS Launcher as it does not listen to gamepad and using OUYA's touch pad is a real pain.

6. If this is your first time, you'll need to log in to your free https://ludei.com/ account. Also, click cogwheel at the top right and make sure DEBUG option is checked and ORIENTATION is Landscape.

7. Click 'YOUR APP', you should see hello_world.zip in 'ZIPS IN SD CARD'. Run it. You'll see 'Hello World' in a really blurry font.

> Pro tip: To quickly reload an app after you've made changes click 'fps:NN' box at the top left, click 'Actions' and 'Reload'.

##### Development cycle

1. Edit your html/js/css files locally and test them in browser
2. `zip -r <project-name>.zip *`
3. `adb push <project-name>zip /mnt/sdcard/`
On OUYA first time:
1. Open Launcher
2. Go to 'YOUR APP'
3. Open <project-name>.zip
On OUYA when your app is already open:
1. Click 'FPS:nn' in the top-left corner, 'ACTIONS', 'RELOAD'.

##### APK compilation
1. `zip -r <project-name>.zip *`
2. Go to dev portal and 'Compile Project'
3. Upload your zip
4. Select 'Ouya' in 'Compile project for'
5. Agree to their 'Upload Conditions'
6. Submit

You'll get email in a few minutes with zip. Two apks inside.

