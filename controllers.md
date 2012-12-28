## Controllers

One of the huge advantages of the OUYA console is that gamers get to use a *real* controller!  The OUYA controller has:
- four digital buttons (O, U, Y, and A)
- a four direction digital pad (D-Pad)
- two analog joysticks (LS, RS)
- two digital buttons that activate when the joysticks are pushed straight down (L3, R3)
- two digital bumper buttons (L1, R1)
- two analog triggers (L2, R2) 
- a touchpad, configured to behave as a mouse input (Touchpad)

**Note:** The analog triggers also act as digital buttons, with a threshold of 0.5 for the analog value.

Since controller interfaces are so crucial, we've done some work to make your life easier.

##### Constants

The **OuyaController** class contains OUYA-specific constants for buttons and axes. A small selection are shown below.
```java
public static final int BUTTON_O;
public static final int BUTTON_U;
public static final int BUTTON_Y;
public static final int BUTTON_A;
```

With these constants in hand, it's totally acceptable to handle input via the standard **onKeyDown**, **onKeyUp**, or **onGenericMotionEvent** methods.

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

Now it's simple to query the button or axis values:

```java
float axisX = c.getAxisValue(OuyaController.AXIS_LSTICK_X);
float axisY = c.getAxisValue(OuyaController.AXIS_LSTICK_Y);
boolean buttonPressed = c.getButton(OuyaController.BUTTON_O);
```

With this, you can focus on making a great game instead of on input handling.
