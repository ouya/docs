## Adobe Air Engine

### Downloads
Open source, clone https://github.com/ouya/ouya-sdk-examples/tree/master/AdobeAir

### Forums

@OUYA - (Adobe Air on OUYA Forums) - http://forums.ouya.tv/categories/adobe-air-on-ouya<br/>

## Guide

### Videos

* Packaging ANE/SWC/ZIP extensions in FlashDevelop - https://vimeo.com/32551703

* Android Native Extensions - Part 1 - http://gotoandlearn.com/play.php?id=148

* Android Native Extensions - Part 2 - http://gotoandlearn.com/play.php?id=149

### Resources

* Adobe Air SDK - http://www.adobe.com/devnet/air/air-sdk-download.html

* FlashDevelop - http://www.flashdevelop.org/

* Quick Guide to Creating and Using SWC - http://dev.tutsplus.com/tutorials/quick-guide-to-creating-and-using-swcs--active-1211

* ANEBUILDER - http://as3breeze.com/anebuilder/

* ANE-Wizard - https://github.com/freshplanet/ANE-Wizard

# Building ANE

ANE Extensions wrap Java libraries so they can be invoked from Adobe Air ActionScript.

## Build JAR

The Java JAR needs to be compiled first before it can be wrapped in an ANE Extension.

[Gradle](https://github.com/ouya/ouya-sdk-examples/tree/master/AdobeAir/OuyaNativeExtension/Build) will compile the Java Extension code into `AirOuyaPlugin.jar`.

```
gradlew clean build copyJar copyNativeArmeabi copyNativeArmeabiV7a copyNativeArmeabiX86
```

The `AirOuyaPlugin.jar` will output to the [jar](https://github.com/ouya/ouya-sdk-examples/tree/master/AdobeAir/OuyaNativeExtension/jar) extension folder.

The `jar` folder also contains `FlashRuntimeExtensions.jar` (from the AdobeAirSDK) and `ouya-sdk.jar` (from the ODK).

## Build ANE

[build_ane.cmd](https://github.com/ouya/ouya-sdk-examples/blob/master/AdobeAir/OuyaNativeExtension/build_ane.cmd) will package the `OuyaNativeExtension.ane` on Windows adding `JDK` to the path and customizing the `AIR_SDK` path to the `AdobeAirSDK`.

## Using ANE

The `OuyaNativeExtension.ane` should be used as a library after placing in the following folders in a FlashDevelop project.

```
lib\OuyaNativeExtension.ane
extension\release\OuyaNativeExtension.ane
```

For the debug extension folder, extract the contents of `OuyaNativeExtension.ane` into a subfolder named `OuyaNativeExtension.ane`.

```
extension\debug\OuyaNativeExtension.ane\catalog.xml
extension\debug\OuyaNativeExtension.ane\library.swf
```

### Community Supported Examples

Head on over to GaslightGames implementation for:<br/>

- Controllers

- In App Purchases

https://github.com/gaslightgames


Using Adobe AIR to create OUYA games:<br/>
http://zehfernando.com/2013/using-adobe-air-to-create-ouya-games/
