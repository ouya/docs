# Razer Target Hardware

The `Forge TV` runs the `Android 5.0` OS.

* [Forge TV](http://www.razerzone.com/gaming-systems/razer-forge-tv) - Spec details

The `Razer Serval` controller is supported by `OE-Input` on `Forge TV`.

* [Razer Serval Controller](http://www.razerzone.com/gaming-controllers/razer-serval) - Spec Details

The OUYA Controller is supported by `OE-Input` on `Forge TV`.

## FAQ

* An upcoming `Forge TV` OTA update will include `KL mapping files` which adds support for many controllers.

* An upcoming `Forge TV` OTA update will include the `OUYA Framework` and `store launcher` which makes `OE-Input` possible.

* The `review team` can now test `OE-Input` on `Forge TV` given a download link to the game build.

* Be sure to update 3rd party libraries related to input to ensure `Forge TV` compatibility. (`InControl`)

## OUYA Everywhere

Applications and games should use the latest `OE plugin` for their particular game engine to publish in the [developer portal](http://devs.ouya.tv).

## List of Engines That Support Forge TV

The latest `OE-Input` adds support for many controllers on the `Forge TV` device. 

* [Corona](corona.md)
* [GameMaker](game-maker.md)
* [Java](java.md)
* [MonoGame](mono-game.md)
* [Unity](unity.md)

## Button Images

[Button images](https://github.com/ouya/docs/blob/master/ouya-everywhere.md#controller-images) vary on each device. Games should use the `OE API` to get the correct button images at run-time. The submission review process checks to see that the correct button images are used.

![Button images](forge_tv/image_1.png)

## Controller Image

The `RazerVirtualController` example includes controller images for the `Razer Serval Controller`. The `RazerVirtualController` image resources can be found within the [ouya-sdk-examples](https://github.com/ouya/ouya-sdk-examples/tree/master/Android/RazerVirtualController).

![Serval Image](ouya-everywhere-android-java/image_3.png)