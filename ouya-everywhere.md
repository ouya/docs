## OUYA Everywhere

The OUYA Everywhere initiative delivers OUYA to gamers wherever they play - whether or not they bought a box from us.  We're the open guys, right?  So why lock OUYA in a box? Even a beautiful one?  OUYA is about games and game developers, not about the way you get it.

In order to help games be as portable as possible there are a set of new APIs available:

- [Device Identification](#device-identification)
- [Controller Images](#controller-images)
- [Controller Input](#controller-input)


<h3 id="device-identification">Device Identification</h3>

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


<h3 id="controller-images">Controller Images</h3>

As new console systems are added, it can become cumbersome for each game to manage the images for the varied controller buttons.  To alleviate this issue, there is the following API:

```java
    static public ButtonData getButtonData(int ouyaButton);

    static public class ButtonData {
        public Drawable buttonDrawable;
        public String buttonName;
    }
```

Simply pass in the button to query (eg: OuyaController.BUTTON\_O or OuyaController.BUTTON\_MENU) and a ButtonData class will be returned (or null if there was no data for the requested button).  The returned Drawable will be a 160px-tall button image (which you can rescale if you like) that you can show in your UI.  The buttonName string will be the localized name of the button (eg: the name for OuyaController.BUTTON\_O will be "O" on the OUYA and "A" on the M.O.J.O.).

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


####MonoGame####

MonoGame has an interface to retrieve button names.

C#
```
            OuyaController.ButtonData buttonData;
            buttonData = OuyaController.getButtonData(OuyaController.BUTTON_O);
            if (null == buttonData)
            {
                return;
            }
            string buttonName = buttonData.buttonName;
```

MonoGame has an interface to retrieve button images as Texture2D images.

C#
```
            Texture2D buttonTexture;
            OuyaController.ButtonData buttonData;
            buttonData = OuyaController.getButtonData(OuyaController.BUTTON_O);
            if (null == buttonData)
            {
                return;
            }
            BitmapDrawable drawable = (BitmapDrawable)buttonData.buttonDrawable;
            if (null == drawable)
            {
                return;
            }
            Bitmap bitmap = drawable.Bitmap;
            using (MemoryStream ms = new MemoryStream())
            {
                bitmap.Compress(Bitmap.CompressFormat.Png, 100, ms);
                ms.Position = 0;
                buttonTexture = Texture2D.FromStream(GraphicsDevice, ms);
            }
```

<h3 id="controller-input">Controller Input</h3>

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
      boolean handled = OuyaInputMapper.dispatchKeyEvent(this, keyEvent);
      ...
      return handled;
    }
  
    @Override
    public boolean dispatchGenericMotionEvent(MotionEvent motionEvent) {
      boolean handled = OuyaInputMapper.dispatchGenericMotionEvent(this, motionEvent);
      ...
      return handled;
    }
```

MonoGame is very similar to the Java version. The easy option is to extend OuyaActivity. Or call the static methods on OuyaInputMapper in a custom Activity.

```C#
using Android.OS;
using Android.Views;

namespace OuyaSdk
{
    public class CustomActivity : Microsoft.Xna.Framework.AndroidGameActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            OuyaInputMapper.init(this.Handle);
        }

        protected override void OnDestroy()
        {
            OuyaInputMapper.shutdown(this.Handle);
            base.OnDestroy();
        }

        public override bool DispatchGenericMotionEvent(MotionEvent motionEvent)
        {
            bool handled = OuyaInputMapper.DispatchGenericMotionEvent(this.Handle, motionEvent);
            ...
            return handled;
        }

        public override bool DispatchKeyEvent(KeyEvent keyEvent)
        {
            bool handled = OuyaInputMapper.DispatchKeyEvent(this.Handle, keyEvent);
            ...
            return handled;
        }
    }
}
```