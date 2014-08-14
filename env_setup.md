# Environment Setup
## Step 1: Install Android SDK
Follow the instructions [found here](https://developer.android.com/sdk/installing/index.html) to install the latest version of the Android SDK
## Step 2: Install the necessary packages
Developing for the OUYA requires a small set of packages to be installed. In the Android SDK Manager, install the latest revisions of the following packages:

 * **Tools**
   * Android SDK Tools
   * Android SDK Platform-tools
   * Android SDK Build-tools
 * **Android 4.1.2 (API 16)**
   * SDK Platform
   
   
**NOTE:** Windows 8 users, you will need to disable driver signature verification to install the unsigned Android driver.  
[See here](windows8.md) for instructions.
  
## Step 2: Add Android SDK to your PATH
To use **adb** or other Android tools from the command line, you'll need to add the build and platform tools folders to your system's PATH environment variable. After setting the PATH, you may need to close your terminal window or log out for the changes to take effect.

### Setting PATH on Mac/Linux

1. Create or edit the shell startup file `~/.bash_profile`
2. Add the following line:  
   `PATH=$PATH:<SDK>/platform-tools:<SDK>/tools`  
   replacing each `<SDK>` with the path to your Android SDK folder
4. Save and close the file  

### Setting PATH in Windows

1. Open **Advanced System Settings**, usually found by **Right click My Computer > Properties**
2. In the **Advanced** tab, click the **Environment Variables** button
3. Under System Variables, select **PATH** and click the **Edit** button
4. Add the following text to the end of the value field:  
   `;<SDK>\platform-tools;<SDK>\tools`  
   replacing each `<SDK>` with the path to your Android SDK folder
5. Click **OK**

## Step 3: Install driver for OUYA Console

For ADB to be able to connect with the console, you'll need to modify some system and Android SDK files.

### For Mac Users:

1. Create or edit the file `~/.android/adb_usb.ini`
2. Add the following line:  
   `0x2836`
3. Save and close the file

## For Windows Users:

1. Edit the file `<SDK>\extras\google\usb_driver\android_winusb.inf`
2. Locate the `[Google.NTx86]` and `[Google.NTamd64]` sections
3. Within both sections, append the following lines:

        ;OUYA Console  
        %SingleAdbInterface% = USB_Install, USB\VID_2836&PID_0010  
        %CompositeAdbInterface% = USB_Install, USB\VID_2836&PID_0010&MI_01  
4. Save and close the file  
5. Create or edit the file `%USERPROFILE%\.android\adb_usb.ini`  
   (`%USERPROFILE%` is typically a folder such as `C:\Users\MyName\`)
6. Add the following line:  
   `0x2836`
7. Save and close the file
8. With your OUYA connected via USB cable, open the Device Manager (Right click **My Computer > Properties > Device Manager**)
9. In Device Manager, select **Portable Devices > OUYA Console**
10. Right-click and choose **Update Driver Software...**
11. Choose **Browse my computer for driver software**
12. Choose **Let me pick from a list of device drivers on my computer**
13. Select **All devices** and click **Next**
14. Click **Have Disk** and browse to `<SDK>\extras\google\usb_driver`
15. Choose **ADB Composite Device**
16. Accept the unsigned driver
