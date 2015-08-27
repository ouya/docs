# Put Your OUYA Game on `Razer Cortex TV`

`OUYA Publishing` is in the process of testing games in the `OUYA Store` for compatibility on `Forge TV`.

Some games are perfectly functional, whereas other games may have issues with input, and other games may have cosmetic issues with button images.

This document will help you put your `OUYA Game` on `Razer Cortex TV` (the storefront for the Forge TV) by making your game `OUYA Everywhere` compliant.

## Table of Contents

[Audience](forge_tv.md#audience)

[Specifications](forge_tv.md#specifications)

[Notes](forge_tv.md#notes)

[Engines That Support Forge TV](forge_tv.md#engines-that-support-forge-tv)

[Step 1. Upgrade the OUYA-Everywhere Plugin](forge_tv.md#step-1-upgrade-the-ouya-everywhere-plugin)

[Step 2. Fix Button Images](forge_tv.md#step-2-fix-button-images)

[Controller Image](forge_tv.md#controller-image)

[ADB](forge_tv.md#adb)

## Audience

`OUYA Developers` wanting to publish to the `Forge TV` console.

## Specifications

The `Forge TV` runs the `Android 5.0` OS (API 22).

* [Forge TV](http://www.razerzone.com/gaming-systems/razer-forge-tv) - Spec details

The `Razer Serval` controller is supported by [`OUYA-Everywhere`](ouya-everywhere.md) on `Forge TV`.

* [Razer Serval Controller](http://www.razerzone.com/gaming-controllers/razer-serval) - Spec Details

The OUYA Controller is supported by [`OUYA-Everywhere`](ouya-everywhere.md) on `Forge TV`.

## Notes

* An upcoming `Forge TV` OTA update will include new support for many controllers.

* An upcoming `Forge TV` OTA update will also include the `OUYA Framework` and `store launcher` which makes [`OUYA-Everywhere`](ouya-everywhere.md) possible.

* The `review team` can now test [`OUYA-Everywhere`](ouya-everywhere.md) on `Forge TV` given a download link to the game build.

* Be sure to update 3rd party libraries related to input to ensure `Forge TV` compatibility. (i.e. `InControl`*)

## Engines That Support Forge TV

The latest [`OUYA-Everywhere`](ouya-everywhere.md) adds support for many controllers on the `Forge TV` device. The following engines have `Forge TV` support and more will be added to this list:

* [Construct 2](construct_2.md)
* [Cordova](cordova.md) - `Accelerated HTML5 wrapper`
* [Corona](corona.md)
* [GameMaker](game-maker.md)
* [Java](java.md)
* [Marmalade](marmalade.md)
* [MonoGame](mono-game.md#forge-tv)
* [Unity](unity.md)
	* *InControl -- If you use InControl for Unity3D, be sure to update your InControl library. For more information, read the [InControl documentation on their website.](http://www.gallantgames.com/pages/incontrol-ouya)
* [Unreal](unreal.md#forge-tv)

## Step 1. Upgrade the OUYA-Everywhere Plugin

Applications and games should use the latest [`OUYA-Everywhere`](ouya-everywhere.md) Plugin for their particular game engine to publish in the [developer portal](http://devs.ouya.tv).

Locate the appropriate engine documentation from the above list in order to the newest plugin.

## Step 2. Fix Button Images

[Button images](https://github.com/ouya/docs/blob/master/ouya-everywhere.md#controller-images) vary on each device. Games can use [`OUYA-Everywhere`](ouya-everywhere.md) to get the correct button images at run-time. The submission review process checks to see that the correct button images are used.

Developers can also design custom button images, as long as  images reflect `Forge TV` A,B,X,Y formatting (see below).

![Button images](forge_tv/image_1.png)

## Controller Image

The `RazerVirtualController` example includes controller images for the `Razer Serval Controller`. The `RazerVirtualController` image resources can be found within the [ouya-sdk-examples](https://github.com/ouya/ouya-sdk-examples/tree/master/Android/RazerVirtualController).

![Serval Image](ouya-everywhere-android-java/image_3.png)

## ADB

The [Razer Developer Portal](http://developer.razerzone.com/forge-tv/developer-setup/) will take you through the setup of `Forge TV` for ADB debugging. Debugging and side-loading apps to the `Forge TV` requires a male-to-male USB cable.