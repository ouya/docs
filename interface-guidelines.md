## Interface Guidelines

Games can use the `Cortex TV` controller buttons however they wish, but we recommend the mappings below for the **A**, **B**, **X**, and **Y** buttons. If you are running a stock Android application that is not `Cortex TV` aware, this is also how the buttons will be mapped to the standard Android navigation buttons on the navigation bar for a typical device.

The [OUYA-Everywhere](ouya-everywhere.md#user-content-controller-images) API is available to access button images and text for use in your games.

```text
OUYA    Default Function
A       select
B       back/cancel
X       -
Y       -
```

Please use the naming conventions below when referring to the controller buttons in help screens for your game:
```text
A, B, X, Y
D-Pad 
LS (the left joystick movement)
L1 (the left bumper)
L2 (the left trigger)
L3 (the button function of pressing the left joystick straight down)
RS
R1
R2
R3
Previous/Select (SELECT)
Power (MODE)
Foward/Start (START)
Back (BACK)
Home (HOME)
```

### TV Safe Area

Leave at least a 10% margin between your UI elements and the edge of the screen. This will help ensure that your content is visible across a wide range of TVs and settings.

More information about safe area can be found in the [content review guidelines](content-review-guidelines.md#user-content-safe-zone).

For further tips and advice, read [Google's Designing for TV Guide](https://developers.google.com/tv/web/docs/optimization_guide).
