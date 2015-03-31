## OUYA Everywhere

The OUYA Everywhere initiative delivers OUYA to gamers wherever they play - whether or not they bought a box from us.  We're the open guys, right?  So why lock OUYA in a box? Even a beautiful one?  OUYA is about games and game developers, not about the way you get it.

### Update the ODK

* It's important to keep your `ouya-sdk.jar` updated to take advantage of `OUYA Everywhere - Input` to make your game compatible with OUYA, MOJO, and Xiaomi consoles. The `ouya-sdk.jar` is provided in the `ODK` download in the [developer portal](http://devs.ouya.tv) and is updated with each `OTA` update.

### Engines

OUYA Everywhere has engine-specific documentation for supported engines.

* [OUYA Everywhere on Construct 2](construct_2.md#ouya-everywhere)
* [OUYA Everywhere on Corona](corona.md#ouya-everywhere)
* [OUYA Everywhere on HTML5](html5.md#ouya-everywhere)
* [OUYA Everywhere on Java](java.md#ouya-everywhere)
* [OUYA Everywhere on Marmalade](marmalade.md#ouya-everywhere)
* [OUYA Everywhere on MonoGame](ouya-everywhere-monogame/ouya-everywhere-monogame.md#ouya-everywhere)
* [OUYA Everywhere on Unity](unity.md#ouya-everywhere)
* [OUYA Everywhere on Unreal](unreal.md#ouya-everywhere)

### Overview

In order to help games be as portable as possible there are a set of new APIs available:

- [Device Identification](#user-content-device-identification)
- [Controller Images](#user-content-controller-images)
- [Controller Input](#user-content-controller-input)
- [Store Identification](#user-content-store-identification)

## Device Identification

While most games will work smoothly across all different devices, sometimes it may be necessary to specifically identify what hardware the game is running on.
The simplest way to tell if the game is running on an OUYA-supported device is:

```java
    public boolean isRunningOnOUYASupportedHardware();
```

This will return true on an officially support device, such as the OUYA console or the M.O.J.O.  On other devices it will return false.  This method will work correctly as support for new devices are added in the future.

For most purposes, the above method should suffice.  However for games that require more precise device identification, this method also exists:

```java
    public DeviceHardware getDeviceHardware();

    public enum DeviceEnum {
            OUYA,
            MOJO,
            XIAOMI,
            UNKNOWN,
    };
    public static class DeviceHardware {
        public boolean isSupported();
        public String deviceName();
        public DeviceEnum deviceEnum();
    };
```

This will provide more specific device information:

- whether it is officially supported
- the user-visible device name
- an enum for games to do hardware-specific checks against

For the corresponding device enum value, it is possible for this to be UNKNOWN, and
yet isSupported() to return true.  This can happen if new hardware support has
been added, yet this game has been compiled against an older ODK which doesn't
have an enum entry for the new hardware.


## Controller Images

As new console systems are added, it can become cumbersome for each game to manage the images for the varied controller buttons.  To alleviate this issue, there is the following API:

```java
    static public ButtonData getButtonData(int ouyaButton);

    static public class ButtonData {
        public Drawable buttonDrawable;
        public String buttonName;
    }
```

Simply pass in the button to query (eg: OuyaController.BUTTON\_O or OuyaController.BUTTON\_MENU) and a ButtonData class will be returned.  The returned Drawable will be a 160x160px image (which you can rescale if you like) that you can show in your UI.  The buttonName string will be the localized name of the button (eg: the name for OuyaController.BUTTON\_O will be "O" on the OUYA and "A" on the M.O.J.O.).
If you pass in a button value for which there is no data, buttonName and buttonDrawable will be null.

```java
    String buttonName = "O";
    final OuyaController.ButtonData buttonData = OuyaController.getButtonData(OuyaController.BUTTON_O);
    if (buttonData != null && buttonData.buttonName != null) {
        buttonName = buttonData.buttonName;
    }
    final String message = "Press " + buttonName + " to get started!";
```

All current OuyaController.BUTTON_* contants have valid button data (including ones for the D-pad).  Very useful for tutorials or button legends.

Using this API will keep your game looking correct as new console hardware support is added -- without any recompilations!


## Controller Input

One common issue with Android games is supporting different controller hardare.  We've created a new API which will remap input from various controller manufacturers to the standard OUYA button layout.  The remapping logic is provided by the OUYA Framework, and will be constantly updated to add support for more and more controllers.

The easiest way to take advantage of this is by simply extending from the OuyaActivity class in the ODK.  This will do a couple things automatically for you:

- remap controller input to the standard OUYA layout
- update OuyaController status

This is achieved by:

```java
    import tv.ouya.console.api.OuyaActivity;  

    public class MyGameActivity extends OuyaActivity {
        ...
    }
```

Of course, if you happen to override onCreate/onDestroy/dispatchKeyEvent/dispatchGenericMotionEvent, then be sure to call OuyaActivity's base methods.

With this in place, you can now handle events in onKeyDown/Up/onGenericMotionEvent and not need to worry about what controller hardware the gamer may be using:

```java
  @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        boolean handled = false;

        // Check the keycode itself
        if (keyCode == OuyaController.BUTTON_O)) {
          doFunThing();
          handled = true;
        }

        // Check state via the OuyaController methods -- the OuyaActivity
        // class automatically updates OuyaController for you as well!
        OuyaController c = OuyaController.getControllerByDeviceId(event.getDeviceId());
        if (c != null) {
          processLeftStick(
            c.getAxisValue(OuyaController.AXIS_LS_X),
            c.getAxisValue(OuyaController.AXIS_LS_Y));
        }

        return handled || super.onKeyDown(keyCode, event);
    }
```

If you are unable to extend the OuyaActivity (eg: the game engine you are using requires you to extend their activity class), you can still leverage the input remapping, but will need to call the methods manually.  They are:

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {    
      OuyaInputMapper.init(this);
      ...
    }

    @Override
    protected void onDestroy() {
      OuyaInputMapper.shutdown(this);
      ...
    }
  
    @Override
    public boolean dispatchKeyEvent(KeyEvent keyEvent) {
      boolean handled = false;
      if (OuyaInputMapper.shouldHandleInputEvent(keyEvent)) {
        handled = OuyaInputMapper.dispatchKeyEvent(this, keyEvent);
      }
      ...
      return handled || super.dispatchKeyEvent(keyEvent);
    }
  
    @Override
    public boolean dispatchGenericMotionEvent(MotionEvent motionEvent) {
      boolean handled = false;
      if (OuyaInputMapper.shouldHandleInputEvent(motionEvent)) {
        handled = OuyaInputMapper.dispatchGenericMotionEvent(this, motionEvent);
      }
      ...
      return handled || super.dispatchGenericMotionEvent(motionEvent);
    }
```


## Store Identification

For developers who choose to use a single apk across multiple storefronts (OUYA, Google Play, Amazon, etc), it can be important to identify which system the game was installed from -- especially now that some OUYA-supported platforms also support Google Play.  One way to detect where your APK was installed from is by using <a href="http://developer.android.com/reference/android/content/pm/PackageManager.html#getInstallerPackageName(java.lang.String)">PackageManager.getInstallerPackageName</a>:
```java
    final String installedFrom = getPackageManager().getInstallerPackageName(getPackageName());
    if ("com.android.vending".equals(installedFrom)) {
      // From Google Play
    } else if ("com.amazon.venezia".equals(installedFrom)) {
      // From Amazon
    } else if (OuyaFacade.getInstance().isRunningOnOUYASupportedHardware()) {
      // From OUYA
    }
```
Note that if you install your development builds via `adb install mygame.apk` then PackageManager.getInstallerPackageName will return null.
The OUYA installer package name has a few different possibilities based, which is why suggest using the OuyaFacade.isRunningOnOUYASupportedHardware method.
