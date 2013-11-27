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
./gradlew clean && ./gradlew build && ./gradlew build
```

Get the compiled APKS.

```
cp chromium/android-webview/build/apk/android-webview-debug-unaligned.apk . && cp chromium/content-shell/build/apk/content-shell-debug-unaligned.apk . && cp chromium/test-shell/build/apk/testshell-debug-unaligned.apk .
```

#### Crosswalk Project

https://crosswalk-project.org/
