## Interface Guidelines

Games can use the OUYA controller buttons however they wish, but we recommend the mappings below for the **O**, **U**, **Y**, and **A** buttons. If you are running a stock Android application that is not OUYA-aware, this is also how the buttons will be mapped to the standard Android navigation buttons on the navigation bar for a typical device.
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


###The safe area
(Coped from: https://developers.google.com/tv/web/docs/optimization_guide#new-kind-of-screen)

Televisions have a safe central display area surrounded by a small amount of screen space that can vary in size. If you place graphics or text outside the safe area, they may not be visible. to be sure that users can see all your interface elements lay out your pages with flexible layouts. At the very least, include at least a 10 percent margin at each resolution:

 - 1280x720 resolution. Recommended size is 1152x648.
 - 1920x1080 resolution. Recommended size is 1728x972.
