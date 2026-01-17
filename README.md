# Debloat HyperOS Phones with Android Debug Bridge (ADB)

This repository contains a list of [Android Debug Bridge (ADB)](https://developer.android.com/studio/command-line/adb) commands to remove bloatware from HyperOS Android. Debloating helps remove unwanted system apps to improve performance, battery life, and privacy.

The lists are categorized into three different profiles:
*   **Safe:** Removes only the most common and non-essential bloatware. Very low risk of causing issues.
*   **Balanced:** Aims for a good balance between debloating and retaining features. Some apps you might use could be removed.
*   **Agressive:** Removes a large number of apps, including some that provide certain system features. This is for users who want a minimal system, but it has a higher chance of breaking some functionalities.

>⚠️**USE AT YOUR OWN RISK!** While these commands are generally safe and reversible, debloating can have unintended consequences. It might break system functionalities or cause instability. Always back up your important data before proceeding. It's highly recommended to read through the list you choose to use and comment out any apps you want to keep.

---

## Prerequisites

1.  **Android Debug Bridge (ADB):** You need ADB installed on your computer.
    *   **Recommended:** [Minimal ADB and Fastboot](https://xdaforums.com/t/tool-minimal-adb-and-fastboot-2-9-18.2317790/) from XDA forums for a quick setup.
    *   **Official:** Download the official [Android SDK Platform Tools](https://developer.android.com/studio/releases/platform-tools).

2.  **Enable Developer Options on your phone:**
    *   Go to `Settings > About phone`.
    *   Tap on `MIUI version` (or `OS version` on HyperOS) multiple times until you see the message "You are now a developer!".

3.  **Enable USB Debugging:**
    *   Go to `Settings > Additional settings > Developer options`.
    *   Enable `Developer options`.
    *   Enable `USB debugging`.
    *   (Optional but recommended) Enable `Install via USB` if you plan to sideload apps.

---
## Instructions
Search for a Minimal ADB installer : https://xdaforums.com/t/tool-minimal-adb-and-fastboot-2-9-18.2317790/
Further reading on ADB can be found here: https://developer.android.com/studio/command-line/adb#Enabling.

1.  **Connect your phone** to your computer with USB debugging enabled. 

2.  **Verify the ADB connection** by opening a terminal and running the following command.
    ```shell
    adb devices
    ```
    You should see the device name listed as a "device". If it says "unauthorized", your device should prompt you to accept USB debugging from your computer. Tap "Allow".
    
3.  **Enter your device's shell** with the following command:
    ```shell
    adb shell
    ```

4.  **Choose and run a debloat profile.** Select one of the files (`debloat_safe.txt`, `debloat_balanced.txt`, `debloat_agressive.txt`). It is **highly recommended** to review the list of apps inside the file first. 

5.  Once in the shell, you can copy and paste the commands from your chosen file to remove the packages. For example, a command from the list would look like this:
    ```shell
    pm uninstall -k --user 0 com.miui.calculator
    ```

---

## Useful Commands

### Understanding the Debloat Commands

This repository uses the `pm uninstall` command, which removes the application for the current user (`user 0`).

*   `pm uninstall -k --user 0 <package_name>`
    *   This command uninstalls the app for the current user (`user 0`). The `-k` flag keeps the app's data and cache. This makes the uninstallation reversible. This is the recommended and safest method.

*   `pm disable-user --user 0 <package_name>`
    *   This command disables the app for the current user. The app is not removed, just disabled. It's a less aggressive alternative to uninstalling.

*   `cmd package uninstall --user 0 <package_name>`
    *   This is another way to uninstall a package. In some cases, it can be more "permanent" but is still generally reversible.

### How to Find a Package Name

To find the package name of an app you want to remove, you can use the following command from within the `adb shell`:

```shell
pm list packages | grep '<query>'
```
Replace `<query>` with a part of the app's name (e.g., to find the calculator app, you could use `grep 'calculator'`).

### How to Restore a Removed App

If you accidentally removed an app you need, you can reinstall it with the following command from your computer's terminal:

```shell
adb shell pm install-existing --user 0 <package_name>
```
For example, to restore the Mi Calculator:
```shell
adb shell pm install-existing --user 0 com.miui.calculator
```

---

## Troubleshooting & Common Fixes

If you use the aggressive debloat list, some features might break. Here are some common fixes to restore essential functionalities by running these commands from your computer's terminal.

*   **To re-enable Wallpapers:**
    ```shell
    adb shell pm install-existing --user 0 com.android.wallpaper.livepicker
    adb shell pm install-existing --user 0 com.android.wallpapercropper
    adb shell pm install-existing --user 0 com.miui.miwallpaper
    ```

*   **To fix Android Auto issues:**
    ```shell
    adb shell pm install-existing --user 0 com.google.android.projection.gearhead
    ```

*   **To fix Bluetooth functionality:**
    ```shell
    adb shell pm install-existing --user 0 com.miui.miservice
    adb shell pm install-existing --user 0 com.xiaomi.bluetooth
    ```

---

## Contributing

You are welcome to contribute to this project! If you find an app that can be safely removed, or if you have improvements for the lists, please open an issue or a pull request.

## License

This project is open source and available under the [MIT License](LICENSE).
