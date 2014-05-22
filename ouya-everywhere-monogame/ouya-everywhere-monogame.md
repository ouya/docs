[Back to OUYA-Everywhere overview](https://github.com/ouya/docs/blob/1.0.12/ouya-everywhere.md)


OUYA-Everywhere INPUT Documentation for the MonoGame Game Engine

# Audience #

This document is for MonoGame developers that want to use the OUYA SDK to access API functions and publish to the OUYA Android Console.

# Overview #

This document covers OUYA-Everywhere input, getting the console button names and console button images.

# Intro #

The MonoGame API is very similar to the Java version with the JNI bindings most of the class names and methods are the same.

Accessing Button Names

MonoGame has an interface to retrieve button names.

```
OuyaController.ButtonData buttonData;
buttonData = OuyaController.getButtonData(OuyaController.BUTTON_O);
if (null == buttonData)
{
	return;
}
if (null == buttonData.buttonName)
{
	return;
}
string buttonName = buttonData.buttonName;
```

Accessing Button Images

MonoGame has an interface to retrieve button images as Texture2D images.

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

Activity Setup

MonoGame is very similar to the Java version. The easy option is to extend OuyaActivity. Or call the static methods on OuyaInputMapper in a custom Activity.

```
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