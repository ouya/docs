## Interface Guidelines

Games can use the OUYA controller buttons however they wish, but we recommend the mappings below for the **O**, **U**, **Y**, and **A** buttons. If you are running a stock Android application that is not OUYA-aware, this is also how the buttons will be mapped to the standard Android navigation buttons on the navigation bar for a typical device.

We also provide a [zip file](https://d31pno3ktcq63f.cloudfront.net/assets/OUYA_Buttons.zip) with button images for use in your games.

```text
OUYA    Color    Default Function
O       green    select
U       blue     -
Y       yellow   -
A       red      back/cancel
```

Please use the naming conventions below when referring to the controller buttons in help screens for your game:
```text
O, U, Y, A
System (the home button)
D-Pad 
LS (the left joystick movement)
L1 (the left bumper)
L2 (the left trigger)
L3 (the button function of pressing the left joystick straight down)
RS
R1
R2
R3
Touchpad (not "Trackpad")
```

### TV safe area

Leave at least a 10% margin between your UI elements and the edge of the screen. This will help ensure that your content is visible across a wide range of TVs and settings.

For further tips and advice, read [Google's Designing for TV Guide](https://developers.google.com/tv/web/docs/optimization_guide).
