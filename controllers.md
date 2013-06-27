## Controllers

One of the huge advantages of the OUYA console is that gamers get to use a *real* controller!  The OUYA controller has:
- four digital buttons (O, U, Y, and A)
- a four direction digital pad (D-Pad)
- two analog joysticks (LS, RS)
- two digital bumper buttons (L1, R1)
- two analog triggers (L2, R2) 
- two digital buttons that activate when the joysticks are pushed straight down (L3, R3)
- a touchpad, configured to behave as a mouse input (Touchpad)
See the interface-guidelines for a link to button images.

**Note:** The analog triggers also act as digital buttons, with a threshold of 0.5 for the analog value.

Since controller interfaces are so crucial, we've done some work to make your life easier.

### Controller Images

Games commonly need images to show controller graphics for pairing screens or showing controller schemes.

Several common sized graphics are provided to be used in game.

The largest image you would ever display in game is 1080p (1920x1080) or 720p (1280x720).

High resolution images are intended to be used as a source image to be rescaled before using in-game.

You'll find controller images in [[OUYA-Images.zip]] (https://s3.amazonaws.com/ouya-docs/OUYA-Images.zip)

```
Pairing Controller:
- Image: controller_pair_graphic_lit.png

Side of Controller:
- Image: OUYA-070_256x256.png
- Image: OUYA-070_512x512.png
- Image: OUYA-070_640x480.png
- Image: OUYA-070_1024x1024.png
- Image: OUYA-070_1280x720.png
- Image: OUYA-070_1920x1080.png
- Image: Hires\OUYA-070_6048x4032.png

Back of Controller:
- Image: OUYA-118_256x256.png
- Image: OUYA-118_512x512.png
- Image: OUYA-118_640x480.png
- Image: OUYA-118_1024x1024.png
- Image: OUYA-118_1280x720.png
- Image: OUYA-118_1920x1080.png
- Image: Hires\OUYA-118_6048x4032.png
```

##### Constants

The **OuyaController** class contains OUYA-specific constants for buttons and axes. A small selection are shown below.

```java
public static final int BUTTON_O;
public static final int BUTTON_U;
public static final int BUTTON_Y;
public static final int BUTTON_A;
```

The first thing that must be done is to initialize the OuyaController system, this should be done at the start of your application like so:

```java
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    OuyaController.init(this);
}
```

Now we can proceed!  It's totally acceptable to handle input via the standard **onKeyDown**, **onKeyUp**, or **onGenericMotionEvent** methods.  If handling input in this way, the controller ID can be queried through **OuyaController.getPlayerNumByDeviceId()** as shown below.

```java
@Override
public boolean onKeyDown(final int keyCode, KeyEvent event){
    //Get the player #
    int player = OuyaController.getPlayerNumByDeviceId(event.getDeviceId());       
    boolean handled = false;
    
    //Handle the input
    switch(keyCode){
        case OuyaController.BUTTON_O:
            //You now have the key pressed and the player # that pressed it
            //doSomethingWithKey();
            handled = true;
            break;
    }
    return handled || super.onKeyDown(keyCode, event);
}

@Override
public boolean onGenericMotionEvent(final MotionEvent event) {
    //Get the player #
    int player = OuyaController.getPlayerNumByDeviceId(event.getDeviceId());    
    
    //Get all the axis for the event
    float LS_X = event.getAxisValue(OuyaController.AXIS_LS_X);
    float LS_Y = event.getAxisValue(OuyaController.AXIS_LS_Y);
    float RS_X = event.getAxisValue(OuyaController.AXIS_RS_X);
    float RS_Y = event.getAxisValue(OuyaController.AXIS_RS_Y);
    float L2 = event.getAxisValue(OuyaController.AXIS_L2);
    float R2 = event.getAxisValue(OuyaController.AXIS_R2);
    
    //Do something with the input
    //updatePlayerInput(player, LS_X, LS_Y, RS_X, RS_Y, L2, R2);
    
    return true;
}
```

##### The Menu/System Button

A Menu/Pause function has been added to the OUYA button on the controller. Instead of bringing up the system menu when pressed once, a single press will now send an `OuyaController.BUTTON_MENU` (`KEYCODE_MENU`) keycode to the app, emulating a normal menu button.
To bring up the system menu you may now double-tap the OUYA button, or long press the OUYA button on newer controllers (dev kit controllers do not have this second method).

This new MENU function provides an easy and consistent way for games and apps to map a pause or context menu to a button on our controller.

*NOTE:* Because the menu button press is emulated, the up and down events happen at once. This means you won't be able to detect it being pressed using the passive anytime state querying. It's much more reliable to detect the menu press in your onKeyUp/Down code.

##### Distinguishing between Analog Joystick and Touchpad

Both the analog joystick and the touchpad states are read using **onGenericMotionEvent**.  To distinguish between them you can query the input source:

* Joystick == InputDevice.SOURCE_CLASS_JOYSTICK
* Touchpad == InputDevice.SOURCE_CLASS_POINTER

Example:

```java
@Override
public boolean onGenericMotionEvent(final MotionEvent event) {
    //Get the player #
    int player = OuyaController.getPlayerNumByDeviceId(event.getDeviceId());    
    
    //Joystick
    if((event.getSource() & InputDevice.SOURCE_CLASS_JOYSTICK) != 0) {
        float LS_X = event.getAxisValue(OuyaController.AXIS_LS_X);            
        //do other things with joystick
    }
            
    //Touchpad
    if((event.getSource() & InputDevice.SOURCE_CLASS_POINTER) != 0) {
        //Print the pixel coordinates of the cursor
        Log.i("Touchpad", "Cursor X: " + event.getX() + "Cursor Y: " + event.getY());
    }
    
    return true;
}
```
##### The Left Joystick

If you do not consume motion events from the left joystick, the system will send your app dpad KeyEvents.

If you don't want your app to get these key events, return true from `onGenericMotionEvent`. Otherwise, you can differentiate these dpad events using the source field as such:
```java
public boolean onKeyDown(int keyCode, KeyEvent event) {
    if(keycode >= KeyEvent.KEYCODE_DPAD_UP && keycode <= KeyEvent.KEYCODE_DPAD_RIGHT) {
        if((event.getSource() & InputDevice.SOURCE_DPAD) != 0) {
            //Event is from actual dpad
        }
        if((event.getSource() & InputDevice.SOURCE_JOYSTICK) != 0) {
            //Event is from joystick
        }
    } else {
        //keyCode is not a dpad keycode
    }
    ...
}
```

One caveat to using the joystick as a dpad to keep in mind is that the triggering value for the dpad event is `0.5`, which causes diagonals and directionals to be inequal in size.
If your games requires 8 equally spaced directions, you will have to do your own math from the axes' values.

##### Anytime State Querying

If you want the extra flexibility of querying the controller state at any time, you can use the rest of the **OuyaController** class.
First off, this means your activity should forward the **onKeyDown**, **onKeyUp**, or **onGenericMotionEvent** calls to **OuyaController**:

```java
@Override
public boolean onKeyDown(int keyCode, KeyEvent event) {
    boolean handled = OuyaController.onKeyDown(keyCode, event);
    return handled || super.onKeyDown(keyCode, event);
}

@Override
public boolean onKeyUp(int keyCode, KeyEvent event) {
    boolean handled = OuyaController.onKeyUp(keyCode, event);
    return handled || super.onKeyUp(keyCode, event);
}

@Override
public boolean onGenericMotionEvent(MotionEvent event) {
    boolean handled = OuyaController.onGenericMotionEvent(event);
    return handled || super.onGenericMotionEvent(event);
}
```

Once **OuyaController** is getting the events, you can then get an instance of the class in one of two ways: device id, or player number:

```java
OuyaController c = OuyaController.getControllerByDeviceId(deviceId);
OuyaController c = OuyaController.getControllerByPlayer(playerNum);
```

Now it's simple to query the button or axis values (for the analog joysticks, we recommend checking against the provided deadzone value):

```java
float axisX = c.getAxisValue(OuyaController.AXIS_LS_X);
float axisY = c.getAxisValue(OuyaController.AXIS_LS_Y);
if (axisX * axisX + axisY * axisY < OuyaController.STICK_DEADZONE * OuyaController.STICK_DEADZONE) {
  axisX = axisY = 0.0f;
}
boolean buttonPressed = c.getButton(OuyaController.BUTTON_O);
```

With this, you can focus on making a great game instead of on input handling.

##### Testing Button State Changes

Another way the controller wrappers help to make game development easier are these optional methods to help you track per-frame button state changes.
```java
if (c.buttonChangedThisFrame(OuyaController.BUTTON_O)) {
    if (c.getButton(OuyaController.BUTTON_O)) {
        startShooting();
    } else {
        stopShooting();
    }
}
```

Of course the ODK needs some help from you to let it know when a new frame is started.  Thus you'll need to call this method at start of each frame (before doing any input handling):
```java
OuyaController.startOfFrame();
```

#### Analog vs Digital Trigger Data

We have deprecated the use of digital (key event) data to determine the state of the trigger buttons (L2 and R2). We now recommend using the analog axis data for more flexible use of the triggers.

To mimic the digital state using the analog data is fairly simple:
```java
public boolean isL2Down(OuyaController c) {
    return (c.getAxisValue(OuyaController.AXIS_L2) > 0.5f); //0.5 can be any threshold between 0.0 (completely released) - 1.0 (completely pressed)
}
```
Just make sure that you have your **onGenericMotionEvent** function sending its events to the OuyaController class, as described at the top of this document.
