# Debloat HyperOS Phones with Android Debug Bridge (ADB)

This repository contains a list of [Android Debug Bridge (ADB)](https://developer.android.com/studio/command-line/adb) commands to aggressively bloatware from HyperOS Android devices that don't normally display an option to remove them.


Running all commands listed in [commands.txt](./commands.txt) will disable almost all HyperOS apps.
Use at your own risk and read over all commands to make sure you don't take out something you need.
While these commands cannot harm your device, there may be situations where your phone gets into a crash-loop and the easiest fix is to boot the device into "Recovery Mode" and perform a factory reset.

> ⚠️ Research each package before running the command to disable it. Some apps have hidden dependencies.

## Instructions
Search for a Minimal ADB installer : https://xdaforums.com/t/tool-minimal-adb-and-fastboot-2-9-18.2317790/
Further reading on ADB can be found here: https://developer.android.com/studio/command-line/adb#Enabling.

1. Install Minimal ADB
2. On your phone, enable Android's "Developer Options"
3. In "Developer Options", turn on "USB Debugging"
4. Connect your HyperOS Android phone to your computer with USB debugging enabled. Verify that adb sees your device and the daemon is running with the following command
    - `adb devices`
    - You should see the device name listed as a "device".
    - At this point your device should prompt you to accept USB debugging from your computer. Tap "Allow".
    
5. Enter your device's shell with the following command
    - `adb shell`
6. Once in the device's shell, copy and paste all desired commands from [commands.txt](./commands.txt) to remove the package.

## Other
Once in your device's shell, you can use the following command to list installed packages by name.
 - `pm list package | grep '<package name>'`

For example, to list all installed packages with Facebook in their name, you'd type,
 - `pm list package | grep 'facebook'`

## Reinstall deleted apps
Use :
 - `pm install-existing --user 0 <package name>`
  
For example to reinstall Sound Quality and Effects, 
 - `pm install-existing --user 0 com.sec.android.app.soundalive`
 - `pm install-existing --user 0 com.sec.android.app.myfiles`

