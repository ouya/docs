**Windows 8**

Windows 8 will only install signed drivers by default.  This driver signature verification must be disabled in order to install the unsigned Google Android driver.  Disabling the driver signature verification requires restarting the computer, and you won't be able to read this page while going through the process.  To make it easier, note down these steps on a piece of paper first.

1. Open the Settings Charm and select **Change PC Settings**.

2. Select the **General** category.  In the **Advanced startup** section, select **Restart now**.

3. Select the **Troubleshoot** tile.

4. Select **Advanced options**.

5. Select **Start-up settings**.

6. Select **Restart**.

7. Press the relevant number key for **Disable driver signature verification**.

8. Windows will now restart.

Now proceed with the driver install procedure as described in [setup](setup.md).  Once the driver is installed, you can re-enable driver signature verification by repeating these same steps.


**Windows 8.1**

Installing the Windows 8.1 update will remove the unsigned driver.  You must repeat the process to disable driver signature verification and update the driver again.  The process to disable driver signature verification has also changed slightly in Windows 8.1

1. Open the Settings Charm and select **Change PC Settings**.

2. Select the **Update and recovery** category.  Select **Recovery**, and in the **Advanced startup** section select **Restart now**.

3. Continue with step 3 above in the Windows 8 section.