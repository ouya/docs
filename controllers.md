# Controllers

One of the huge advantages of the `Forge TV` console is that gamers get a standard `Android TV` controller!  The `Serval` controller has:
- four digital buttons (A, B, X, and Y)
- a four direction digital pad (D-Pad)
- two analog joysticks (LS, RS)
- two digital bumper buttons (L1, R1)
- two analog triggers (L2, R2) 
- two digital buttons that activate when the joysticks are pushed straight down (L3, R3)
- a previous/select button (SELECT)
- a power button (MODE)
- a next/start button (START)
- a back button (BACK)
- a home button (HOME)

Since controller interfaces are so crucial, we've done some work to make your life easier.

## Controller Constants

The **OuyaController** class contains OUYA-specific constants for buttons and axes. A small selection are shown below.

```java
public static final int BUTTON_O; //A on the Serval
public static final int BUTTON_U; //X on the Serval
public static final int BUTTON_Y; //Y on the Serval
public static final int BUTTON_A; //B on the Serval
```
## Input using OuyaController

The first thing that must be done is to initialize the OuyaController system, this should be done at the start of your application like so:

```java
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    OuyaController.init(this);
}
```

If you want the extra flexibility of querying the controller state at any time, you can use the **OuyaController** class.
In this case, your activity should forward the **onKeyDown**, **onKeyUp**, or **onGenericMotionEvent** calls to **OuyaController**:

```java
@Override
public boolean onKeyDown(int keyCode, KeyEvent event) {
    return OuyaController.onKeyDown(keyCode, event) || super.onKeyDown(keyCode, event);
}

@Override
public boolean onKeyUp(int keyCode, KeyEvent event) {
    return OuyaController.onKeyUp(keyCode, event) || super.onKeyUp(keyCode, event);
}

@Override
public boolean onGenericMotionEvent(MotionEvent event) {
    return OuyaController.onGenericMotionEvent(event) || super.onGenericMotionEvent(event);
}
```

Once **OuyaController** is getting the events, you can then get an instance of the class in one of two ways: device id, or player number. Then it's simple to query the button or axis values.

**NOTE:** We recommend using the provided deadzone value when querying thumbstick values, as shown in the example.

**Example:**

```java
OuyaController c = OuyaController.getControllerByDeviceId(deviceId);
OuyaController c = OuyaController.getControllerByPlayer(playerNum);

float axisX = c.getAxisValue(OuyaController.AXIS_LS_X);
float axisY = c.getAxisValue(OuyaController.AXIS_LS_Y);
if (axisX * axisX + axisY * axisY < OuyaController.STICK_DEADZONE * OuyaController.STICK_DEADZONE) {
  axisX = axisY = 0.0f;
}
boolean buttonPressed = c.getButton(OuyaController.BUTTON_O);
```
### Per-frame state querying

Another way the controller wrappers help to make game development easier are these optional methods to help you track per-frame button state changes.
```java
if (c.buttonChangedThisFrame(OuyaController.BUTTON_O)) {
    if (c.buttonPressedThisFrame(OuyaController.BUTTON_O)) {
        startShooting();
    }
    if (c.buttonReleasedThisFrame(OuyaController.BUTTON_O) {
        stopShooting();
    }
}
```

Of course the ODK needs some help from you to let it know when a new frame is started.  Thus you'll need to call this method at start of each frame (before doing any input handling):
```java
OuyaController.startOfFrame();
```
Note that it's possible for a button to be both pressed and released within a single frame.
## Input using traditional InputEvents

It's totally acceptable to handle input via the standard **onKeyDown**, **onKeyUp**, or **onGenericMotionEvent** methods.  If handling input in this way, the controller ID can be queried through **OuyaController.getPlayerNumByDeviceId()** as shown below.

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
            oButtonDown(player);
            
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
    updatePlayerInput(player, LS_X, LS_Y, RS_X, RS_Y, L2, R2);
    
    return true;
}
```
## The Legacy Menu Button

The legacy `OUYA` controller has a `OuyaController.BUTTON_MENU` where this button might not be available on `ALL` controllers. If you want to maximize your game's controller compatibility, it's recommended that you avoid mapping this button with gameplay.

## Controller Images

Games commonly need images to show controller graphics for pairing screens or showing controller schemes. Individual button images/names can be queried at runtime to provide maximal support for different devices (see the [OUYA Everywhere](ouya-everywhere.md) documention for more info).
